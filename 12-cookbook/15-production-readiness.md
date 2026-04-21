# 15 — Production Readiness Checklist

The single "am I ready to go live?" document. If you can tick everything
here, you're in good shape. If you can't, fix the gaps before pushing to
production.

This list synthesizes material from the rest of the cookbook — follow the
links into each section for the why and the how.

---

## 1. Compliance & scope

- [ ] **I know my mandatory implementation date.** I've confirmed it based on
      my FY2022 audited turnover (or YA2022 tax return revenue if no audit).
      See [10-implementation-timeline.md](10-implementation-timeline.md).
- [ ] **If I'm past my mandatory date, I'm going live ASAP.** (Being overdue
      is not a compliance-neutral state.)
- [ ] **I'm not in an exempted category** that would make this all optional.
      If I am, I've decided whether to voluntarily adopt anyway.
- [ ] **My suppliers and buyers are mapped** — which are already issuing
      e-Invoices, which aren't yet, which never will be (e.g. exempt parties).

## 2. Credentials & access

- [ ] I have **production** MyInvois credentials:
  - [ ] Production Client ID (not production-looking placeholder)
  - [ ] Two production Client Secrets (both stored securely)
  - [ ] These are **different** from my sandbox credentials
- [ ] I have **sandbox** credentials, and I can reach both environments
      independently from my code.
- [ ] **Secrets are in a proper secrets manager** (Vault, AWS Secrets Manager,
      Azure Key Vault, Google Secret Manager, or at minimum environment
      variables loaded from an out-of-repo file). Never hardcoded.
- [ ] **I know who else in my organization has these credentials** and
      rotation responsibilities are assigned.

## 3. Digital certificate

- [ ] I have a **production organizational digital certificate** from an
      [MCMC-licensed CA](11-digital-certificates.md#where-to-buy-one--licensed-malaysian-cas).
- [ ] The cert's **Subject fields** are correct:
  - [ ] CN (organization name) — matches my company name
  - [ ] C = `MY` (or my actual country if foreign)
  - [ ] E — organizational email
  - [ ] O — organization name
  - [ ] **OI — my IRBM-assigned TIN** (critical; wrong = DS311 on every submission)
  - [ ] **Serial Number — my BRN** linked to the TIN above
- [ ] **Cert expiry is > 60 days away** (or I have a renewal already in
      flight).
- [ ] The cert file (`.p12` / `.pfx`) is stored with restrictive file permissions
      (`chmod 400` or equivalent) and backed up to a secure location.
- [ ] The cert passphrase is in a secrets manager, not alongside the file.
- [ ] I have a **revocation plan** — I know which CA form to submit if the
      key leaks.
- [ ] If I'm an intermediary: I've registered as such in MyInvois Portal and
      my customers have granted me the right permissions.

## 4. Signing pipeline

- [ ] My code implements the 8-step signing pipeline correctly
      (see [02-signature-errors.md § Signing pipeline](02-signature-errors.md#signing-pipeline-8-step-summary)).
- [ ] **Canonicalization** uses `xml-c14n11` for XML / full minification for JSON.
- [ ] **Digest algorithm** is SHA-256 everywhere (not SHA-1, not SHA-512).
- [ ] **Signature algorithm** is RSA-SHA256.
- [ ] **UTC timestamp** format is `YYYY-MM-DDTHH:MM:SSZ` (no offsets, no fractional seconds).
- [ ] **X509IssuerName** RDN order matches what the cert actually emits (this
      is the DS326 footgun — [see fix](02-signature-errors.md#ds326-deep-dive)).
- [ ] I've successfully signed and submitted at least **one real sandbox
      invoice end-to-end** and it came back `Valid`.

## 5. API integration

- [ ] **Base URLs are a single config knob.** One `MYINVOIS_ENV=production`
      environment variable flips everything — URLs, credentials, possibly cert.
- [ ] **Token caching** is implemented. I never call `/connect/token` more than
      once per 60 minutes under normal load.
  - [ ] Cache respects `expires_in` from the response (not hardcoded 3600).
  - [ ] There's a refresh buffer (e.g. re-login at 58 minutes, not 59:59).
- [ ] **Rate limit handling** is implemented:
  - [ ] HTTP 429 → read `Retry-After` → sleep → retry (not tight-loop).
  - [ ] `Retry-After` parser handles both integer-seconds and HTTP-date formats.
- [ ] **5xx handling** has exponential backoff with jitter.
- [ ] **4xx handling** does NOT retry automatically (except 429). A 400 means
      fix the payload.
- [ ] **Polling** uses `Get Submission` (300 rpm), NOT `Get Recent` / `Search`
      (12 rpm each).
- [ ] **Duplicate detection** (`DS302` / `Error03`): on hit, DO NOT resubmit.
      Instead, find the existing doc and use its UUID.
- [ ] **Correlation ID** is captured from every response (success and failure)
      and logged.

## 6. Data layer

- [ ] All **55 mandatory fields** (or the applicable subset for my document
      type) are populated correctly.
      See [13-required-fields-reference.md](13-required-fields-reference.md).
- [ ] All **TINs** are validated client-side before submission — `IG` prefix
      for individuals, prefix+no-leading-zeros+trailing-zero for non-individuals.
- [ ] **Phone numbers** are normalized: digits only + optional leading `+`, no
      spaces / dashes / parentheses. Min 8, max 20 chars.
- [ ] **Emails** are RFC 5322 validated, or blank. Not `NA`, not a placeholder.
- [ ] **SST / TTX** fields use real numbers or the literal `NA`.
- [ ] **Postal codes** are 5-digit numeric for Malaysia.
- [ ] **Dates** are UTC with `Z` suffix.
- [ ] **Currency codes** are ISO 4217.
- [ ] **State codes** are the current set (NOT `00` — it's retired).
- [ ] **Classification codes** / **MSIC codes** / **Tax Types** reference
      current SDK code tables, not a stale copy.
- [ ] **Special characters** in text fields are properly escaped (XML entities
      / JSON escape sequences — see [07-special-characters.md](07-special-characters.md)).
- [ ] **UBL JSON `_` wrapping** — every scalar value is wrapped as `{"_": "value"}`.

## 7. Workflow & business logic

- [ ] **Submit path**: submit → poll `Get Submission` → act on `Valid` / `Invalid`.
- [ ] **Invalid invoices** are surfaced to a human — not silently re-tried in
      a loop.
- [ ] **Valid invoices** are persisted with: UUID, longId, validationLink,
      QR code, validation timestamp.
- [ ] **72-hour cancellation window** is enforced in code — past 72h,
      cancellation API calls will fail; adjustments must go via
      credit/debit/refund note.
- [ ] **Buyer rejection requests** from MyInvois Portal are watched (via
      `Get Notifications` polling or UI surfacing).
- [ ] **Issue-date freshness** — code refuses to submit documents where
      `IssueDate` is > 72 hours old OR in the future.
- [ ] **Duplicate prevention** — invoice numbers are unique in my system,
      combined with timestamp, so I can't accidentally resubmit the same
      (type, version, date, number, TIN) combo within 2 hours.

## 8. Error handling & observability

- [ ] **Structured logging** for every API call:
  - [ ] Request ID / correlation ID
  - [ ] HTTP status code
  - [ ] Response `errorCode` (if present)
  - [ ] Latency
  - [ ] `correlationId` response header (always — for LHDN support tickets)
- [ ] **Metrics** emitted for:
  - [ ] Submission rate (successes + failures, by error code)
  - [ ] Poll count per submission
  - [ ] Time-to-Valid distribution
  - [ ] Rate-limit hits (429 count)
  - [ ] Login calls per hour (should be ~1, definitely not 60)
- [ ] **Alerts** set up:
  - [ ] Sustained 5xx rate > 5% → oncall
  - [ ] Sustained 429 rate → oncall (you're burning quota somewhere)
  - [ ] Submissions stuck in `InProgress` > 30 min → investigate
  - [ ] Certificate expiry < 30 days → action ticket
  - [ ] Sustained authentication failures → rotate secrets, investigate
- [ ] **Dead-letter queue** for submissions that fail in ways we can't auto-handle.
- [ ] See [16-observability-and-operations.md](16-observability-and-operations.md)
      for detailed patterns.

## 9. Runbooks

I have written runbooks for:

- [ ] **Certificate about to expire** — renewal process, who to call, rollover plan
- [ ] **Certificate compromised / leaked** — revocation, secret rotation, audit trail
- [ ] **Client Secret compromised** — rotation via portal, rollover plan
- [ ] **MyInvois system down** — queuing strategy, evidence preservation for
      the system-downtime concession
      ([see 14 § system-downtime concession](14-validation-status-lifecycle.md#the-system-downtime-concession))
- [ ] **Batch stuck in `InProgress`** for > 1 hour — escalation path, how to
      raise a support ticket with the `correlationId`
- [ ] **Customer disputes a validated invoice at hour 73+** — credit/debit/refund
      note workflow
- [ ] **Supplier cancels an invoice I already recorded as valid** — notification
      handling on my side
- [ ] **Intermediary relationship lost mid-batch** — e.g. customer revokes
      permission, what happens to in-flight submissions

## 10. Storage & compliance

- [ ] **Source XML/JSON** of each submitted invoice is stored on my side
      (not just in MyInvois), with retention ≥ 7 years.
- [ ] **Visual representation (PDF)** is stored alongside, with QR code embedded.
- [ ] **Correlation IDs** are retrievable by invoice UUID for any audit ticket
      I might need to raise.
- [ ] **Archive format** is immutable or write-once — not a scratch database
      someone can `DELETE FROM` by accident.
- [ ] If I'm an intermediary: I can demonstrate per-taxpayer access boundaries
      (I'm not leaking Taxpayer A's invoices to Taxpayer B).

## 11. Operations & support

- [ ] Someone **non-engineering** (finance, AP/AR staff, ops) can answer
      "is this invoice validated?" through my UI. They don't need to SSH
      into anything.
- [ ] There's a **way to re-trigger a submission** manually via admin UI for
      the cases where auto-retry has given up.
- [ ] There's a **way to cancel an invoice** via admin UI (within the 72h window).
- [ ] The **IRBM support contacts** are documented in my runbook:
      see [contacts.md](../contacts.md).

## 12. Rollout plan

- [ ] **Canary submission** planned — submit ONE real production invoice
      first, verify everything works end-to-end, before flipping the firehose.
- [ ] **Gradual ramp** defined — not 100% of traffic on day 1.
- [ ] **Rollback plan** — if production goes catastrophically wrong, how do
      I stop? (Hint: there's no "undo" on an accepted invoice past 72h —
      the only way back is to cancel within 72h or issue a credit note.)
- [ ] **Communication plan** — who tells customers / suppliers if there's an
      issue?
- [ ] **Post-launch monitoring** — the first 72 hours after cutover, someone
      is actively watching metrics and error logs.

## 13. After launch

- [ ] **Cert renewal calendar entry** for 60 days before expiry.
- [ ] **Quarterly compliance review** — new code tables, new validation
      rules, new release notes.
- [ ] **Annual drill** — run the cert compromise runbook in dry-run mode
      to make sure it still works.
- [ ] **Feedback loop** — errors users hit in production feed back into the
      validation logic, so we catch more at submit-time and fewer at
      validator-time.

---

## Scoring guide

- **All boxes ticked** → you're ready. Ship with confidence.
- **Missed in sections 1–3** → don't ship. These are credentials / cert fundamentals.
- **Missed in sections 4–6** → don't ship. These are correctness.
- **Missed in sections 7–12** → ship with awareness. These are
  operations-quality items; a first-time launch can survive some gaps, but
  they'll bite you eventually. Schedule the fixes post-launch.
- **Missed in section 13** → not a blocker for launch; put them on the
  30-day-post-launch checklist.

## Going-live order of operations

1. Complete sections 1–6 against **sandbox** first. Run a full lap: submit,
   validate, cancel. All green.
2. Request production credentials + cert.
3. Repeat section 4–6 against **production** with ONE canary invoice. Verify.
4. Sections 7–10 should already be code-complete; validate them in production
   on the canary.
5. Ramp volume gradually (10% → 50% → 100% over days, not hours).
6. First week: daily check on metrics. First month: weekly check.

## Reference

- [10 — Implementation Timeline](10-implementation-timeline.md)
- [11 — Digital Certificates](11-digital-certificates.md)
- [12 — Sandbox Setup](12-sandbox-setup.md)
- [13 — Required Fields](13-required-fields-reference.md)
- [14 — Validation Status Lifecycle](14-validation-status-lifecycle.md)
- [16 — Observability & Operations](16-observability-and-operations.md)
- [Official IRBM contacts](../contacts.md)
