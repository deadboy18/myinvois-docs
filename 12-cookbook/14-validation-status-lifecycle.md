# 14 — Validation Status Lifecycle & The 72-Hour Window

Once you submit an e-Invoice, it moves through a small state machine.
Understanding the states — and the 72-hour window that controls several of
them — is critical for designing your retry logic, UI flows, and
reconciliation processes.

**Source:** IRBM e-Invoice General Guideline v4.6, Sections 2.3, 2.4, 2.5.

## The four states

| State | Numeric | Meaning |
|---|---|---|
| **Submitted** | 1 | Successfully transmitted to MyInvois. **Passed immediate validations** (Structure + Core Fields) but background validations still running. |
| **Valid** | 2 | Passed **all** validations (immediate + background). Validated e-Invoice has UUID, long ID, validation link, QR code. |
| **Invalid** | 3 | Failed one or more validations. Not usable; must fix and resubmit. |
| **Cancelled** | 4 | Cancelled by the Supplier within 72 hours of validation. No longer valid; new e-Invoice required for any adjustment. |

## State transitions

```
                 [Submit Documents API]
                         │
                         ▼
                   ┌──────────┐
          pass     │          │     fail
     ┌─────────────│ Submitted│──────────────┐
     │             │  (1)     │              │
     ▼             └──────────┘              ▼
┌──────────┐                            ┌──────────┐
│  Valid   │     ◄─── 72hr ───►         │ Invalid  │
│   (2)    │     cancel                 │   (3)    │
└──────────┘     window                 └──────────┘
     │                                       │
     │ cancel within                         │ fix + resubmit as
     │ 72 hours                              │ new e-Invoice
     ▼                                       ▼
┌──────────┐
│Cancelled │
│   (4)    │
└──────────┘
```

### Key transition rules

- **Submitted → Valid**: automatic, happens in background usually within
  seconds to minutes.
- **Submitted → Invalid**: automatic if background validators fail.
- **Valid → Cancelled**: only the Supplier can do this, only within 72 hours
  of validation, with a justification.
- **Valid → Valid (past 72h)**: no further cancellation allowed.
  Adjustments require issuing new credit / debit / refund note.
- **Invalid → Valid**: impossible. An Invalid invoice stays Invalid.
  You must submit a NEW corrected invoice. The Invalid one is effectively
  a dead record.
- **Cancelled → anything**: impossible. Cancellation is terminal.

## Validation types: immediate vs background

Per the General Guideline Table 2.2, each validator runs in one of two modes:

| # | Validator | Timing |
|---|---|---|
| 1 | Structure Validator | **Immediate** |
| 2 | Core Fields Validator | **Immediate** |
| 3 | Signature Validator | **Background** |
| 4 | Taxpayer Validator | **Background** |
| 5 | Referenced Documents Validator | **Background** |
| 6 | Code Validator | **Immediate and Background** |
| 7 | Duplicate Document Validator | **Background** |

### What this means for your polling logic

- **Immediate validators** run during the `Submit Documents` call itself.
  If they fail, you get an error synchronously on submit — HTTP 400 or a
  validation error in the response body.
- **Background validators** run after the submission is accepted. The
  document enters `Submitted` state first, then moves to `Valid` or
  `Invalid` once the background checks complete.
- **You cannot short-circuit background validators.** Even if your payload
  is "definitely" correct, you still need to poll `Get Submission` until the
  document exits `Submitted`.

### Typical timing

| Batch size | Expected Submitted → Valid/Invalid |
|---|---|
| 1 document | < 5 seconds |
| 10 documents | 10–30 seconds |
| 100 documents | 1–5 minutes |

These are observed ranges, not SLAs. Spikes can happen under system load —
implement reasonable timeout logic.

## The 72-hour windows (multiple distinct ones)

The 72-hour rule appears in several places. **These are different windows.**
Keep them straight:

### Window A — `IssueDate` freshness (submission)

The **time between `IssueDate/IssueTime` on the document and when you submit**
must be **≤ 72 hours**.

| Violation | Error |
|---|---|
| `IssueDate` more than 72 hours ago | *"Issuance date time value of the document is too old and cannot be submitted"* |
| `IssueDate` in the future | *"Document issuance date and time is in the future"* |

**Practical impact:** don't batch weekly. Submit continuously.

### Window B — Buyer rejection

After validation, the **Buyer** has **72 hours** to request rejection via
MyInvois Portal or API.

- Must specify a reason (erroneous SST, wrong amount, wrong details, etc.)
- Upon request, Supplier is notified
- If Supplier agrees: Supplier cancels (which triggers Window C cancellation)
- If Supplier doesn't accept / doesn't cancel in time: the rejection request
  doesn't result in cancellation. No further recourse via rejection —
  adjustment needs a new credit/debit/refund note.

### Window C — Supplier cancellation

After validation, the **Supplier** has **72 hours** to cancel the e-Invoice
via MyInvois Portal or API.

- Must include a justification.
- Upon cancellation, Buyer is notified.
- No cancellation allowed after the 72-hour window.

### Window D — Auto-acceptance

If neither rejection nor cancellation happens within 72 hours, the e-Invoice
is **auto-accepted**. Any subsequent adjustment requires a new e-Invoice
(credit note / debit note / refund note).

### All four, visually

```
  IssueDate                Submit            Validate
  ───────────────►─────────►─────────────────────────
  [----- ≤ 72hr -----]                     │
  (Window A)                               │
                                           ▼
                                     ◄─────────────► Valid / Invalid
                                           │
                                    ┌──────┴───────┐
                             ◄──────┤ 72hr window  ├──────►
                                    │  B (buyer),  │
                                    │  C (seller), │
                                    │  D (auto)    │
                                    └──────────────┘
                                           │
                                           ▼
                              Cancel possible only here.
                              After 72hr: use credit/debit/refund
                              note for any adjustment.
```

## Programmatic detection of state

When you poll `Get Submission`, the response includes `overallStatus`:

```json
{
  "submissionUid": "ABC123...",
  "overallStatus": "Valid",
  "documentSummary": [
    {
      "uuid": "DEF456...",
      "longId": "GHI789...",
      "status": "Valid",
      "issueDate": "2025-01-15",
      "issueTime": "08:30:00Z",
      ...
    }
  ]
}
```

`overallStatus` reflects the aggregate. Each document has its own `status`:

```python
# Typical handling
resp = get_submission(submission_uid)
if resp["overallStatus"] == "InProgress":
    sleep(poll_interval)
    continue
elif resp["overallStatus"] == "Valid":
    for doc in resp["documentSummary"]:
        if doc["status"] == "Valid":
            mark_valid_in_erp(doc["uuid"])
        elif doc["status"] == "Invalid":
            mark_invalid_in_erp(doc["uuid"], doc.get("error"))
        elif doc["status"] == "Submitted":
            # Still in background validation
            queue_for_later_check(doc["uuid"])
```

**Note:** some submissions may end up in a mixed state — the overall status
shows complete but individual documents may still be `Submitted`. Always
check individual doc statuses, not just `overallStatus`.

## Retention and storage

- All validated e-Invoices are stored in **IRBM's database**.
- Despite this, **taxpayers must retain sufficient records and
  documentation** (Income Tax Act 1967).
- **Recommended retention: 7 years** (standard tax record keeping period).
- Store the source XML/JSON, the PDF visual representation, any attachments,
  and correlation IDs for audit.

## The system-downtime concession

Per Section 2.5.4 of the General Guideline v4.6:

> In cases where the MyInvois System is down due to maintenance or technical
> issues, and taxpayers are able to demonstrate their evidence of their
> e-Invoice compliance efforts, the Director General of Inland Revenue will
> evaluate this on a case-to-case basis. If the justifications are valid,
> the Director General will not take action against the taxpayers for their
> inability to comply with the e-Invoice transmission and validation
> requirements during such period.

**Practical implication:** keep your submission attempts logged. If MyInvois
goes down and you can't submit on time, you need evidence that you **tried**.
Log every `ConnectionError`, `Timeout`, 503 response, etc. with timestamps.

### What qualifies as "system down"?

LHDN's wording is broad: "disruptions, system failures, unforeseen
malfunctions or service outages (based on MyInvois System's performance
records)." There's no public status page — you'll typically see:

- Widespread HTTP 5xx / 503 responses
- Login API failing for everyone
- Industry groups / partners reporting issues simultaneously

### What to do when it happens

1. **Don't panic-retry aggressively** — that hurts more than it helps.
2. **Queue submissions internally** with their intended `IssueDate`.
3. **Log the outage window** — timestamps of failures, correlation IDs if
   any responses came back.
4. **Resume as soon as you detect success** — retry from the head of the
   queue.
5. **If `IssueDate` on queued docs would go past 72h** — contact IRBM
   support with evidence of the outage. Include the correlation IDs.

## Notification behavior

Per the General Guideline, notifications fire at these points:

| Event | Notified |
|---|---|
| e-Invoice validated | Supplier and Buyer, via email + portal |
| Buyer requests rejection | Supplier |
| Supplier cancels | Buyer |

**In sandbox, notifications may not fire** (as noted in
[12-sandbox-setup.md](12-sandbox-setup.md#email-notifications-dont-actually-fire)).
Don't build dependencies on sandbox notifications.

## Design patterns

### Pattern 1: Idempotent submission

```python
def submit_invoice(invoice_id, payload):
    # Check if we already submitted this
    existing = db.query_submission(invoice_id)
    if existing and existing.status in ("Submitted", "Valid", "Invalid"):
        return existing
    
    # Otherwise submit
    resp = api.submit_documents([payload])
    db.store_submission(invoice_id, resp.submission_uid, "Submitted")
    return resp
```

### Pattern 2: Handle the auto-accept boundary

```python
def can_still_cancel(invoice):
    validated_at = parse_datetime(invoice.validated_at)
    elapsed = datetime.now(timezone.utc) - validated_at
    return elapsed < timedelta(hours=72) and invoice.status == "Valid"

def adjust_invoice(invoice, adjustment):
    if can_still_cancel(invoice):
        # Simplest path — cancel and reissue
        api.cancel(invoice.uuid, reason=adjustment.reason)
        new_invoice = build_corrected_invoice(invoice, adjustment)
        return submit_invoice(new_invoice.id, new_invoice.payload)
    else:
        # Must use credit / debit / refund note
        note = build_adjustment_note(invoice, adjustment)
        return submit_invoice(note.id, note.payload)
```

### Pattern 3: Background polling with dead-letter queue

```python
def poll_until_complete(submission_uid, max_wait=timedelta(minutes=30)):
    deadline = datetime.now() + max_wait
    backoff = 5  # seconds
    while datetime.now() < deadline:
        resp = api.get_submission(submission_uid)
        if resp.overall_status != "InProgress":
            return resp
        sleep(backoff)
        backoff = min(backoff * 1.5, 60)  # cap at 60s
    # Exceeded max_wait — move to manual review queue
    dead_letter_queue.push(submission_uid)
    raise TimeoutError(f"Submission {submission_uid} stuck")
```

## Reference

- [IRBM e-Invoice General Guideline v4.6](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/) — Sections 2.3, 2.4, 2.5
- [Document Validation Rules (SDK)](https://sdk.myinvois.hasil.gov.my/document-validation-rules/)
- Related cookbook pages:
  - [09-duplicate-detection.md](09-duplicate-detection.md) — 2-hour duplicate window
  - [03-submission-errors.md](03-submission-errors.md) — specific validator errors
  - [04-rate-limits.md](04-rate-limits.md) — polling rate limits
