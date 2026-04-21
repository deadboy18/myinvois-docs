# 16 — Observability & Operations

If you run e-Invoice integration for any non-trivial volume, you need the
same operational rigor you'd apply to any other business-critical API.
This section is opinionated and concrete.

## Philosophy

Three things matter for e-Invoice operations:

1. **Every invoice is a financial event.** Failures are not retry-and-forget
   — they block revenue, delay payments, and create tax audit risk.
2. **IRBM is a single point of dependency.** You can't pick a different
   "provider" if you hit an issue. Your leverage is data quality + preserving
   evidence.
3. **The 72-hour clocks are real.** Being "late by a day" is a compliance
   issue, not a minor incident.

Your observability strategy should make all three visible **without** manual
log-diving.

## Structured logging

Every single API call to MyInvois should emit a structured log line with at
minimum these fields:

```json
{
  "timestamp": "2026-04-21T08:30:15Z",
  "level": "info|warn|error",
  "event": "myinvois_api_call",
  "endpoint": "submit_documents | get_submission | login | cancel | reject | ...",
  "environment": "production | sandbox",
  "http_status": 200,
  "latency_ms": 423,
  "correlation_id": "abc123-xyz",
  "submission_uid": "SUB_XXX",
  "document_uuid": "DOC_YYY",
  "error_code": null,
  "error_inner_codes": []
}
```

### Why each field matters

- **`correlation_id`**: LHDN support needs this to trace any failed request.
  **Log it on successes too** — you don't know which "success" will become a
  ticket later.
- **`environment`**: stops you chasing a phantom production bug that's
  actually happening in sandbox.
- **`error_inner_codes`**: e.g. `["Error03", "DS302"]`. These tell you what
  kind of issue it was (duplicate, signature, etc.) for dashboards without
  parsing the full error tree.
- **`latency_ms`**: MyInvois is near real-time, usually. A latency spike is
  a leading indicator of system issues.

### What to NOT log

- **Never log the full Client Secret or cert passphrase.** Even in DEBUG.
- **Never log the raw cert private key material.**
- **Careful with access tokens** — they're short-lived but still sensitive.
  Log a hash or first-6-chars prefix if you must correlate.
- **PII in invoices** — consider whether TINs, customer names, phone numbers
  should be in application logs. Structured separation (invoice data →
  database, log lines → observability) is the clean answer.

## Metrics to emit

Counter metrics (per endpoint, tagged by `environment`):

| Metric | Tags | What it tells you |
|---|---|---|
| `myinvois_api_calls_total` | `endpoint`, `status` | Call volume + success rate |
| `myinvois_errors_total` | `endpoint`, `error_code` | Which errors are hitting you |
| `myinvois_rate_limit_hits_total` | `endpoint` | Are you too aggressive? |
| `myinvois_login_calls_total` | — | Should be ≤1 per hour per process |
| `myinvois_duplicate_errors_total` | — | DS302 hits — indicates retry logic issue |

Histogram metrics:

| Metric | Tags | What it tells you |
|---|---|---|
| `myinvois_api_latency_seconds` | `endpoint` | Performance trends |
| `myinvois_time_to_valid_seconds` | — | Submit → Valid polling duration |
| `myinvois_polls_per_submission` | — | Are you polling too much / too little? |

Gauge metrics:

| Metric | Tags | What it tells you |
|---|---|---|
| `myinvois_cert_days_until_expiry` | `cert_subject` | Renewal reminder |
| `myinvois_submissions_in_progress` | — | Stuck batches |
| `myinvois_dead_letter_queue_size` | — | Manual-review backlog |

## Dashboards

At minimum, one dashboard answering these questions at a glance:

1. **Are we accepting traffic?** — successful submits per minute/hour/day
2. **Are we failing?** — failures per minute, broken out by error code
3. **Are we being rate-limited?** — 429 count by endpoint
4. **Are we stuck anywhere?** — in-progress submissions count + age histogram
5. **Are we burning auth quota?** — login calls per hour
6. **Is our cert OK?** — days until expiry

### Sample PromQL (for Prometheus users)

```promql
# Success rate (last 5m)
sum(rate(myinvois_api_calls_total{status="200"}[5m])) 
  / sum(rate(myinvois_api_calls_total[5m]))

# Error breakdown
sum by (error_code) (rate(myinvois_errors_total[15m]))

# 99th percentile latency per endpoint
histogram_quantile(0.99, 
  sum by (endpoint, le) (rate(myinvois_api_latency_seconds_bucket[5m])))

# Stuck submissions (seen as in_progress > 10 min)
(time() - myinvois_submission_start_time) > 600 
  and myinvois_submission_status == "InProgress"
```

## Alerts

These are the alerts every MyInvois integrator should have. Severity guide:
- **P1 (page oncall)** — customer revenue at risk
- **P2 (Slack alert)** — fix within hours
- **P3 (ticket)** — fix this week

| Alert | Severity | Trigger |
|---|---|---|
| Auth failure spike | **P1** | > 3 consecutive 401 responses from login endpoint. Could mean rotated secret or compromised cert. |
| Sustained 5xx rate | **P1** | > 10% 5xx rate for 5 minutes. MyInvois may be down — start queuing with evidence for the downtime concession. |
| Sustained 4xx (non-429) rate | **P2** | > 5% 4xx rate for 10 minutes. Something changed on our side or theirs. |
| 429 rate sustained | **P2** | 429 count > 10 in 5 minutes. We're over-polling or mis-configured. |
| Submissions stuck | **P2** | Any submission in `InProgress` > 30 minutes. |
| Cert expiring soon | **P2** | < 30 days to cert expiry. |
| Cert expiring imminent | **P1** | < 7 days to cert expiry. |
| Dead-letter queue growing | **P2** | DLQ size > 50 or growing > 10/hour. |
| Excess login calls | **P3** | > 5 login calls per hour per process (should be ~1). Indicates token caching bug. |
| Duplicate error spike | **P3** | DS302 hits > 10 per hour. Your submission logic is retrying on success. |

## Correlation ID discipline

Every MyInvois response carries a `correlationId` response header. **Your
code should:**

1. Read it on every response (success or failure).
2. Attach it to the log line.
3. Store it with the invoice record in your DB.
4. Return it to customers / internal users if they hit an issue (via admin UI).
5. Include it when raising a support ticket with LHDN.

```python
def log_myinvois_call(resp, endpoint):
    logger.info(
        "myinvois_api_call",
        endpoint=endpoint,
        http_status=resp.status_code,
        latency_ms=resp.elapsed.total_seconds() * 1000,
        correlation_id=resp.headers.get("correlationId"),  # ALWAYS log
        body_error_code=extract_error_code(resp),
    )
    return resp
```

## Runbooks

Every serious integration has these runbooks written BEFORE launch. Not
when an incident hits.

### Runbook: Certificate about to expire

**Trigger:** cert_days_until_expiry alert fires (< 30 days).

1. Verify expiry date against the cert file: `openssl pkcs12 -in cert.p12 -info -noout`
2. Contact your CA to initiate renewal (see contacts in your CA's docs).
3. CA verification takes 3–5 working days.
4. New cert arrives — **deploy to staging/sandbox first**, test at least one submission.
5. Rollover plan: store both old and new cert for a brief overlap; switch
   the "active" pointer atomically.
6. After switchover, monitor for DS326 / DS311 errors (new cert may have
   different RDN order or field values).
7. Archive the old cert (don't delete; you may need it for auditing old
   invoices).

### Runbook: Certificate compromised / private key leaked

**Trigger:** evidence of unauthorized access, or security team flags it.

1. **Revoke immediately** via the CA's revocation form.
2. Order replacement cert (standard 3–5 day lead time, so you may be down
   for days — more reason to detect leaks fast).
3. **Rotate Client Secrets** at MyInvois Portal too. Assume they're
   compromised together.
4. **Audit recent submissions** — are any fraudulent? Check the `correlationId`
   of suspect submissions; contact LHDN via support if you suspect any
   invoice was signed by an attacker.
5. **Incident report** — internal and, if warranted, to LHDN.
6. Review how the leak happened; update secret storage + access controls.

### Runbook: MyInvois system is down

**Trigger:** sustained 5xx / 503 across all endpoints.

1. **Confirm it's not us** — check from a second network / geography.
2. **Stop hammering.** Back off. You'll only pile up retries to no effect.
3. **Queue submissions internally** — keep them intact with their intended
   `IssueDate`.
4. **Log the outage** — start time, observed errors, correlation IDs.
5. **Monitor for recovery** — a single successful Login call is your signal.
6. **Resume gradually** — first a few test calls, then the queue.
7. **If IssueDate on queued invoices would blow past 72 hours** — contact
   LHDN support with the outage evidence. Reference the **system-downtime
   concession** (General Guideline §2.5.4).

### Runbook: Submission stuck in InProgress > 1 hour

**Trigger:** `submissions_in_progress` age histogram shows stuck items.

1. Note the `submissionUid` and `correlationId`.
2. Try `Get Submission` manually — is it still `InProgress`, or has it
   flipped to `Invalid` and we missed the signal?
3. If still `InProgress`: note down for LHDN support.
4. If `Invalid`: check the inner errors, surface to a human.
5. **Do NOT** resubmit. That creates a DS302 duplicate.
6. Submit a support ticket to `myinvois@hasil.gov.my` with:
   - `submissionUid`
   - `correlationId`
   - Your TIN
   - Timeline of attempts

### Runbook: Client Secret compromised

**Trigger:** secret leaked via any vector (logs, screenshot, ex-employee).

1. Log in to MyInvois Portal.
2. Navigate to your ERP registration → **use the alternate Client Secret**
   temporarily.
3. **Rotate (re-issue) the leaked secret**. Both secrets now new.
4. Update your production config. Rolling deploy with grace period.
5. Audit recent successful API calls — anything suspicious? Use correlation
   IDs.
6. Note: rotating secrets doesn't revoke existing access tokens. Any leaked
   token is valid for up to 60 more minutes. For maximum safety, wait out
   that window before declaring clean.

### Runbook: Batch needs cancellation but > 72 hours post-validation

**Trigger:** business realises invoice is wrong, > 72 hours since validation.

1. Confirm status via `Get Document` — it's `Valid`.
2. Cancellation API will reject (past window). Don't try.
3. Issue a **new credit/debit/refund note** referencing the original UUID.
4. Submit + validate + share with buyer as normal.
5. Track the linkage internally so you can reconcile the "net" correctly.

## Dead-letter queue (DLQ) pattern

Every integration eventually accumulates submissions that fail in unexpected
ways. Route them to a DLQ that a human can review:

```python
class SubmissionState(Enum):
    PENDING = "pending"
    SUBMITTED = "submitted"   # in MyInvois, InProgress
    VALID = "valid"
    INVALID = "invalid"
    CANCELLED = "cancelled"
    DEAD_LETTER = "dead_letter"  # needs human review

def move_to_dlq(invoice_id, reason, context):
    db.execute(
        "UPDATE invoices SET state = ?, dlq_reason = ?, dlq_context = ? "
        "WHERE id = ?",
        (SubmissionState.DEAD_LETTER, reason, json.dumps(context), invoice_id)
    )
    alert_ops_team(invoice_id, reason)
```

**Reasons to route to DLQ:**
- Submission stuck in `InProgress` for > 30 minutes
- Retried 3 times on 5xx and still failing
- Validation returned error codes we don't recognize
- Signature errors that survived a cert rotation
- Any submission where our internal state disagrees with MyInvois state

**Provide the ops team a UI** with:
- Filter by reason / error code
- Full submission payload visible
- Full response payloads visible
- Correlation IDs in a copy-friendly format
- "Retry" and "Give up and write off" actions

## On-call pages

If you run a 24/7 team, schedule expectations:

- **Oncall P1 response:** within 15 minutes, engineering action within 1 hour.
- **After-hours coverage:** necessary because invoices issued outside business
  hours still have the 72-hour clock.
- **Escalation path:** when to engage LHDN support vs. handle internally.

## Capacity planning

MyInvois rate limits (from [04-rate-limits.md](04-rate-limits.md)):

- Submit Documents: **100 rpm** per client ID
- Get Submission: **300 rpm** per client ID (use for polling)

**Capacity math:**

- If each submit takes 1 API call + ~5 polls = 6 calls per invoice
- At 100 rpm submits, your *actual* sustainable throughput is about
  **80 invoices/minute** (leaving headroom for retries and rejects)
- That's **~100,000 invoices/day** per client ID at steady-state

If your volume exceeds this:
- Talk to LHDN about a higher rate limit
- Use the intermediary model with one intermediary client ID per taxpayer
- Batch within a single submission (multiple documents per submit call)

## Reference

- [IRBM contacts](../contacts.md)
- [04-rate-limits.md](04-rate-limits.md) — the rate limit rules
- [08-integration-antipatterns.md](08-integration-antipatterns.md) — anti-patterns
- [14-validation-status-lifecycle.md](14-validation-status-lifecycle.md) — state machine
- [15-production-readiness.md](15-production-readiness.md) — full go-live checklist
