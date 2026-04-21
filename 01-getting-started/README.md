# Getting Started

New to MyInvois? Read these in order.

## Contents

1. **[Overview](overview.md)** — the foundational reference: high-level architecture, document types, authentication model, and base URLs.
2. **[Standard Headers](standard-headers.md)** — common headers used across all API requests.
3. **[Error Responses](error-responses.md)** — the standard error-response format and HTTP status codes returned by the platform.
4. **[Integration Practices](integration-practices.md)** — recommended patterns for retries, rate-limit handling, token reuse, logging, and production hardening.

## Minimum Viable Integration — Checklist

Use this as a rough mental model of what your code needs to do:

- [ ] **Credentials stored securely** — `client_id`, `client_secret`, and (for prod) the signing certificate + private key
- [ ] **Token cache** — obtain an OAuth token once per hour, not per request
- [ ] **TIN validation** — validate buyer/supplier TINs before building documents
- [ ] **Document builder** — generate UBL 2.1 XML or JSON from your internal data model
- [ ] **XAdES signer** — digitally sign the document (production only)
- [ ] **Submit + map** — call `POST /documentsubmissions/`, store the returned `uuid` against your internal invoice ID
- [ ] **Status poller** — async job that polls `GET /documents/{uuid}/details` until `Valid` or `Invalid`
- [ ] **Retry + backoff** — handle rate limits, 5xx errors, token expiry
- [ ] **Cancellation path** — call `PUT /documents/state/{uuid}/state` with `cancelled` within 72h if needed
- [ ] **Rejection handling** — receive buyer rejection requests and decide whether to cancel
- [ ] **PDF + QR generator** — embed the validation QR code on buyer-facing copies
- [ ] **Consolidated invoice scheduler** — for B2C retailers: monthly consolidated e-Invoice (see [Specific Guideline §3](../11-irbm-specific-guideline/README.md))

## What order should I read the full docs in?

```
01-getting-started/overview.md
    ↓
02-platform-api/01-login-taxpayer.md      (authentication)
    ↓
04-document-types/README.md               (what to build)
    ↓
04-document-types/invoice-v1-0.md         (start with v1.0 — no signature needed)
    ↓
03-einvoice-api/02-submit-documents.md    (submit it)
    ↓
03-einvoice-api/08-get-document-details.md (poll for results)
    ↓
07-validation/README.md                   (understand why it might fail)
    ↓
06-signature/README.md                    (before moving to production / v1.1)
    ↓
01-getting-started/integration-practices.md (harden for production)
```
