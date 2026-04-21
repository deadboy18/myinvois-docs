# 09 — Duplicate Detection Logic

The Duplicated Submission Validator (Step 3, returns `Error03` + inner `DS302`)
is one of the easier validators to accidentally trigger if you retry too
aggressively or have a race condition. Knowing exactly what it compares helps
you avoid it.

## The rule

An e-Invoice is flagged as a duplicate **if and only if all 5 of these fields
match** another already-processed e-Invoice within a **2-hour rolling window**.

| # | Field | Notes |
|---|---|---|
| 1 | e-Invoice type + type version | e.g. `01` v1.1 vs `01` v1.0 are considered different |
| 2 | Issuance date and time | Exact match required — down to the second |
| 3 | Internal ID / invoice number | The `cbc:ID` in the document |
| 4 | Supplier TIN | For **normal** e-Invoices |
| 5 | Buyer TIN | For **self-billed** e-Invoices |

Note the asymmetry: field 4 applies to **normal** invoices (where Supplier
issues), field 5 applies to **self-billed** invoices (where Buyer issues).

## Worked example

Invoice A submitted successfully at 10:00 UTC:
- Type: `01` / v1.1
- IssueDate/Time: `2025-01-15T09:55:00Z`
- ID: `INV-2025-0042`
- Supplier TIN: `C1234567890`
- Buyer TIN: `C0987654321`

At 10:15 UTC, invoice B is submitted:
- Type: `01` / v1.1 (same)
- IssueDate/Time: `2025-01-15T09:55:00Z` (same)
- ID: `INV-2025-0042` (same)
- Supplier TIN: `C1234567890` (same)
- Buyer TIN: `C5555555555` (**different**)

**Result: NOT a duplicate.** Even though 4 out of 5 match, field 5 differs,
and this is a normal (not self-billed) invoice so field 4's match is the one
that counts — wait, but field 4 *does* match. Let me re-read the rule...

Actually: for a **normal** invoice, fields 1–4 are the comparison set. So in
the above example, B **would** be flagged as a duplicate because fields 1–4
all match A, regardless of the buyer TIN change. Buyer TIN is only the
duplicate-key field for self-billed invoices.

### Corrected example

Same as above, but invoice B has a different `IssueDate`:
- IssueDate/Time: `2025-01-15T10:15:00Z` (different from A's 09:55:00)

**Result: NOT a duplicate.** Field 2 differs.

### Another corrected example

Invoice A is self-billed (type `11`), same fields as above. Invoice B is a
**normal** invoice (type `01`) but otherwise identical.

**Result: NOT a duplicate.** Field 1 (type) differs.

## Why this matters for retry logic

The duplicate check is **why the official guidance says not to retry on 2xx
responses** (see [08-integration-antipatterns.md](08-integration-antipatterns.md#3-dont-resubmit-on-20x-responses)).

If you submit the same document twice within 2 hours:
1. First submission succeeds.
2. Second submission gets `Error03 / DS302`.

Even if the first submission appeared to "not get through" from your side
(e.g. network error, timeout) — the server may well have received and accepted
it. Always check status via `Get Submission` before retrying.

## Why 2 hours?

It's a deliberate middle-ground:
- Long enough to catch actual duplicates from flaky retries.
- Short enough that you're not blocked forever if you legitimately need to
  reissue an invoice with the same ID.

If you **must** reissue an invoice with the same ID within 2 hours (rare),
you can either:
- Cancel the first one, then submit the new one with a different `IssueDate`/
  `IssueTime` (even 1 second later counts as "different").
- Wait out the 2-hour window.

## Safer numbering practices

To minimize accidental duplicates:

1. **Include a sequence number or UUID in the invoice ID** — unique by
   construction.
2. **Stamp `IssueTime` with current UTC to the second** — even if two invoices
   share an ID by mistake, they're unlikely to also share a timestamp.
3. **Don't reuse invoice numbers across retries.** If you want to retry a
   failed submission, resubmit **the same payload** (same ID, same
   timestamp). The system will either accept it (first real receipt) or
   reject as duplicate (already accepted).

## Detecting a DS302 in your code

```python
def is_duplicate_error(response_body):
    err = response_body.get("error", {})
    if err.get("errorCode") == "Error03":
        return True
    for inner in err.get("innerError") or []:
        if inner.get("errorCode") == "DS302":
            return True
    return False
```

When you detect it, the correct response is **not** to retry with a fresh
payload. Instead:

1. Log the duplicate event.
2. Call `Search Documents` or `Get Recent Documents` with the matching fields
   to find the already-accepted document.
3. Use its UUID for downstream work.

## Reference

- [FAQ — How does the system validate if a document is a duplicate?](https://sdk.myinvois.hasil.gov.my/faq/#how-does-the-system-validate-if-a-document-is-a-duplicate)
- [Standard Error Response — DS302 example](https://sdk.myinvois.hasil.gov.my/standard-error-response/)
