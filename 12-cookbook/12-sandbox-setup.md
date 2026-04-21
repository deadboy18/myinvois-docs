# 12 — Sandbox Setup & Pre-Production Environment

Before you go live, everything is tested in **sandbox** (also called
"pre-production" / "preprod"). This section walks through setup, the key
differences from production, and the "I'm ready to go live" checklist.

## Environment URLs (for reference)

| Component | Sandbox / Pre-prod | Production |
|---|---|---|
| Invoicing Portal | `preprod.myinvois.hasil.gov.my` | `myinvois.hasil.gov.my` |
| System API | `preprod-api.myinvois.hasil.gov.my` | `api.myinvois.hasil.gov.my` |
| Identity service | `preprod-api.myinvois.hasil.gov.my` | `api.myinvois.hasil.gov.my` |

## Registration flow

You cannot reuse production credentials in sandbox. You **register separately**
in pre-production. The flow:

1. **Log in to the pre-production MyInvois Portal** at
   <https://preprod.myinvois.hasil.gov.my/> using your MyTax credentials.
2. **Register your company** if it's not already known to preprod.
3. **Register your ERP system** via "View and Register ERP" in the portal.
4. **Obtain Client ID + Client Secret** for the registered ERP. You'll get
   **two** client secrets (for redundancy — either can be used).
5. **Test** using these credentials against `preprod-api.myinvois.hasil.gov.my`.

If you skip to production credentials in sandbox, you'll get
**`403 Forbidden`** on every Login call (the most common sandbox pitfall —
see [05-common-pitfalls.md](05-common-pitfalls.md#403-forbidden-in-sandbox)).

### Do you need a different cert for sandbox?

Technically **no** — a production-issued cert from a licensed CA will work
in sandbox. But:

- Some CAs offer **sandbox-only certs at reduced cost** for testing — ask
  your CA if you want to avoid burning your production cert's validity on
  tests.
- Some teams prefer a dedicated sandbox cert to isolate test signatures
  from any production audit trail.
- **You cannot use a self-signed cert** — sandbox enforces chain-of-trust
  validation identically to production. You'll hit `DS329`.

## Key differences from production

These trip up people who assume sandbox = production minus data.

### 1. Lower rate limits (effective 28 April 2025)

Per the [Integration Practices page](https://sdk.myinvois.hasil.gov.my/integration-practices/):

> The sandbox environment is intended for functional testing and has a **lower
> API rate limit** compared to the production environment.

LHDN doesn't publish exact sandbox RPM figures, but assume **25–50% of
production**. Don't load-test your system here — sandbox is for functional
correctness, not throughput.

### 2. 3-month data retention

> Data in the sandbox environment will be **retained for a maximum of 3
> months**, after which it will be permanently deleted. Please ensure to
> back up any important data before the retention period expires.

Practical implications:
- Don't assume last quarter's test UUIDs will still resolve.
- If you're building test-data baselines or regression suites, **archive
  them on your side** — don't depend on sandbox to keep them.
- The validation link / QR code on a sandbox-validated invoice may stop
  working after 3 months.

### 3. Sandbox may run behind production

LHDN pushes features to production first or second. If a feature seems
missing, check the release notes for sandbox-specific parity. Rare, but it
happens.

### 4. Test TINs

- Use your **real TIN** in sandbox if you registered your real company.
- For testing edge cases (e.g. "what if the buyer TIN is a sole proprietor?"),
  you can use any valid-format TIN — sandbox's TIN validation is looser.
- Do **not** use production real customer TINs for volume testing — you'll
  leak PII into sandbox even if retention is only 3 months.

## Sandbox readiness checklist (before you write code)

- [ ] Can log in to <https://preprod.myinvois.hasil.gov.my/> with MyTax
      credentials
- [ ] Your company is registered in pre-production portal
- [ ] Your ERP is registered; you have **Client ID + 2 Client Secrets**
- [ ] You have a cert from a licensed CA (or a sandbox-specific cert from
      the CA) — and you've verified its Subject fields (CN, OI, Serial Number, etc)
- [ ] The cert's OI matches the TIN associated with your sandbox Client ID
- [ ] You can successfully call `Login as Taxpayer` and get a token
- [ ] You've imported the [Postman collection](https://sdk.myinvois.hasil.gov.my/postman/)
      and made at least one call against preprod

## Functional testing sequence

Work through these in order — each one depends on the previous. Skip ahead
at your peril.

### 1. Authentication

```
POST /connect/token
  grant_type=client_credentials
  client_id=<your Client ID>
  client_secret=<your Client Secret>
  scope=InvoicingAPI
```

Expected: 200 OK with `access_token` + `expires_in: 3600`.

If 403: using production creds in sandbox, or client ID malformed.
If 401: secret wrong.

### 2. Validate a TIN

```
GET /api/v1.0/taxpayer/validate/<TIN>?idType=BRN&idValue=<BRN>
Authorization: Bearer <token>
```

Expected: 200 OK.

Try both your own TIN + BRN (should succeed) and a deliberately wrong pair
(should fail with a known error). This confirms the happy and sad paths.

### 3. Submit a sample invoice

Use the official sample at
<https://sdk.myinvois.hasil.gov.my/files/one-doc-signed.xml> as a starting
point. Replace supplier/buyer TINs with your sandbox-valid ones.

### 4. Poll for validation

Use `Get Submission` with the returned `submissionUID` until `overallStatus`
is `Valid` or `Invalid`.

### 5. Retrieve and inspect the validated invoice

`Get Document` / `Get Document Details`. Verify the `uuid`, `longId`, and
`validationLink` are returned correctly.

### 6. Generate QR code URL

Use `longId` + `uuid` per the format in the FAQ:
`{envbaseurl}/uuid-of-document/share/longid`

For sandbox: `https://preprod.myinvois.hasil.gov.my/<uuid>/share/<longid>`

Scan the generated QR with a phone → it should show the invoice details.

### 7. Cancel the invoice

`Cancel Document` with a reason. Verify the status transitions to `Cancelled`.
Note: this only works within 72 hours of validation.

### 8. Test error paths

- Submit malformed JSON → should get `400 BadRequest`.
- Submit without signature → should get signature validation error.
- Submit invoice with future `IssueDate` → should fail validation.
- Submit duplicate (same payload twice within 2 hours) → should get
  `Error03` / `DS302`.
- Exceed rate limit on Login (13+ calls / minute) → should get `429`.

If all of these behave as expected, your integration handles the common
paths.

## Common sandbox-only issues

### TIN validation accepts things it shouldn't

Sandbox is sometimes more permissive than production for TIN validation —
nonexistent TINs may pass. Don't rely on sandbox TIN validation as a
production guarantee.

### Email notifications don't actually fire

Sandbox may suppress email notifications. If you've wired "trigger on
validation email" as a test, it won't work — use the Get Submission polling
path instead.

### Submissions processed faster than production

Sandbox processing is often faster because load is lower. Your polling logic
may work in sandbox but time out in production under real load. Test with
realistic delays (e.g. mock slow responses in your code) before going live.

### Existing sandbox data disappears silently

After 3 months, your test invoices vanish. Retrieval endpoints will return
404. Handle this gracefully in any demo/test harness.

## Transitioning to production

When you're ready:

1. **Register in production** — separate registration at
   <https://myinvois.hasil.gov.my/>.
2. **Obtain production Client ID + Client Secret** (completely separate from
   sandbox).
3. **Swap base URLs and credentials** via config — don't hardcode them in
   multiple places (see [08-integration-antipatterns.md §10](08-integration-antipatterns.md)).
4. **Ensure your cert is a real production cert** (not sandbox-only).
5. **Run a canary** — submit one real invoice, verify it, check it's
   validated, check it's visible in your customer-facing UI. Fix before
   going wide.
6. **Gradually ramp volume** — don't flip the firehose on day one.

## Production readiness checklist

Before cutting production traffic:

- [ ] Production Client ID + Client Secret obtained and stored securely
- [ ] Config clearly separates sandbox and production (single env var, not
      scattered constants)
- [ ] Cert is a valid production-issued cert, expiry > 60 days away
- [ ] Token caching implemented — you're not re-authing per request
- [ ] Rate-limit handling implemented (`429` with `Retry-After` → wait, retry)
- [ ] All `correlationId` headers logged on errors
- [ ] Error handling discriminates between transport errors (HTTP 4xx/5xx)
      and validation errors (`status: "Invalid"` body)
- [ ] Duplicate-error (`DS302`) detection present; does NOT trigger retry
- [ ] You have a runbook for: cert about to expire, client secret leaked,
      MyInvois down, batch stuck in InProgress for hours
- [ ] Someone non-engineering (finance / ops) can answer "is this invoice
      validated?" through your UI — not through logs
- [ ] 7-year retention strategy for archived e-Invoices (storage + format)
- [ ] Gradual rollout plan — canary → partial volume → full cutover

## Reference

- [SDK FAQ — sandbox credentials](https://sdk.myinvois.hasil.gov.my/faq/)
- [Integration Practices → Rate Limit](https://sdk.myinvois.hasil.gov.my/integration-practices/#rate-limit)
- [Postman collection](https://sdk.myinvois.hasil.gov.my/postman/)
- [SQL.com.my sandbox setup guide](https://www.sql.com.my/e-invoice-setup-environment/)
- [Kiizen MyInvois Portal guide](https://www.kiizen.com.my/myinvois-portal-guide-to-issue-einvoice/)
