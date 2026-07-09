# MyInvois Error Cookbook & Community SDK Reference

> Companion docs for [MyInvois](https://sdk.myinvois.hasil.gov.my/) developers.
> This bundle fills gaps in the official SDK: **specific error codes with
> remediation**, **a survey of community-maintained SDK client libraries**
> in PHP, Node.js, Python, C#, and B4X, **the full IRBM General Guideline
> rendered as markdown**, **digital certificate procurement and lifecycle
> guidance**, and **sandbox-to-production playbook**.

Scraped and compiled from:
- Official [MyInvois SDK portal](https://sdk.myinvois.hasil.gov.my/) (FAQ, validation rules, integration practices)
- [IRBM e-Invoice General Guideline v4.6](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/) (published 7 December 2025)
- [PosDigicert's Signature Creation Guideline Rev 1.2](https://www.posdigicert.com.my/public/uploads/files/PosDigicert-Document_Signature_Creation_Guideline_Rev1.pdf) (the full DS error table)
- Community GitHub repos (see below)
- Certificate authority public pages ([PosDigicert](https://www.posdigicert.com.my/digital-certificate-for-einvoice), [MSC Trustgate](https://www.msctrustgate.com/e-invoice))
- [MCMC licensed digital-signature authorities](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees)

Community repositories surveyed:

| Repo | Language | What it is |
|---|---|---|
| [klsheng/myinvois-php-sdk](https://github.com/klsheng/myinvois-php-sdk) | PHP | Composer package, most mature community SDK |
| [syukranDev/e-invoice-sdk-nodejs](https://github.com/syukranDev/e-invoice-sdk-nodejs) | Node.js | npm package, JSON-focused |
| [ERPGulf/myinvois](https://github.com/ERPGulf/myinvois) | Python | Frappe/ERPNext app |
| [mokth/einvoice](https://github.com/mokth/einvoice) | C# | Windows/Linux console + Windows GUI app, X509 tools |
| [pyhoon/lhdn-einvoice-api-client-b4x](https://github.com/pyhoon/lhdn-einvoice-api-client-b4x) | B4X | Cross-platform (Desktop/Android/iOS) reference UI |

---

## Table of contents

### Error reference

1. [HTTP status codes & standard error response](01-http-status-codes.md) — every code MyInvois returns, with the official response envelope shape
2. [Signature errors (DS300–DS338)](02-signature-errors.md) — **the complete XAdES validation error table**, with fixes for each
3. [Submission & validation errors](03-submission-errors.md) — `Error01`–`Error07` validators, CF-prefixed core-field errors, "Invalid Structure", duplicate detection
4. [Rate limits per API](04-rate-limits.md) — RPM table, `Retry-After` handling, polling cadence
5. [Common pitfalls (FAQ-derived)](05-common-pitfalls.md) — TIN `IG` prefix, sandbox 403, 72-hour issuance window, "TIN doesn't match" errors
6. [Field validation rules](06-field-validation-rules.md) — phone, email, SST, TTX, postal-code format rules with exact character limits
7. [Special characters encoding](07-special-characters.md) — XML and JSON escape sequences
8. [Integration anti-patterns](08-integration-antipatterns.md) — the five things that will get you throttled or flagged
9. [Duplicate detection logic](09-duplicate-detection.md) — exact fields compared and the 2-hour window

### Compliance & setup

10. [Implementation timeline & scope](10-implementation-timeline.md) — who must comply, by when, exemptions, international organisations list
11. [Digital certificates](11-digital-certificates.md) — **procurement, vendor pricing (PosDigicert, MSC Trustgate, Rafftech, TM Applied Business), soft vs roaming, lifecycle**
12. [Sandbox setup](12-sandbox-setup.md) — pre-prod registration, key differences, testing sequence, go-live checklist
13. [Required fields reference (55 fields)](13-required-fields-reference.md) — every field in Appendix 1 & 2 of the General Guideline, with mandatoriness and validation
14. [Validation status lifecycle](14-validation-status-lifecycle.md) — Submitted / Valid / Invalid / Cancelled state machine + the four 72-hour windows

### Production operations

15. [Production readiness checklist](15-production-readiness.md) — **the single "am I ready to go live?" document**, synthesizing the rest of the cookbook
16. [Observability & operations](16-observability-and-operations.md) — logging, metrics, alerts, runbooks for certs/outages/stuck submissions, DLQ pattern
17. [Automated update checking with Claude](17-automated-update-checking.md) — **Claude Skill for auditing the repo against the live SDK portal and IRBM guidelines**

### Reference

- [Official IRBM contacts](../contacts.md) — support emails, phones, portals, CA contacts
- [IRBM e-Invoice General Guideline v4.7 (7 July 2026)](irbm-general-guideline/README.md) — complete markdown rendering

### Community SDKs

- [Overview & comparison](community-sdks/README.md)
- [klsheng PHP SDK](community-sdks/01-klsheng-php-sdk.md)
- [mokth C# / .NET samples](community-sdks/02-mokth-csharp.md)
- [ERPGulf Frappe/ERPNext app](community-sdks/03-erpgulf-frappe.md)
- [syukranDev Node.js SDK](community-sdks/04-syukrandev-nodejs.md)
- [pyhoon B4X reference client](community-sdks/05-pyhoon-b4x.md)

---

## Quick error triage

**"My submission was rejected — where do I look?"**

1. HTTP status **4xx / 5xx**? → [01-http-status-codes.md](01-http-status-codes.md)
2. Got a response body with `"errorCode": "DS3xx"`? → [02-signature-errors.md](02-signature-errors.md) — this is a digital-signature problem, not a data problem
3. Got `"Error03"` / `"Duplicated Submission Validator"`? → [09-duplicate-detection.md](09-duplicate-detection.md)
4. Got `"Invalid Structure"`? → [03-submission-errors.md](03-submission-errors.md) — structure validator failed, usually element sequence
5. Got `"CF401"` / `"CF403"` / `"CF410"`? → [06-field-validation-rules.md](06-field-validation-rules.md) — phone or email format
6. Got **HTTP 429**? → [04-rate-limits.md](04-rate-limits.md)
7. Got `"ItemCode 00 was not active"`? → [05-common-pitfalls.md](05-common-pitfalls.md) — state code `00` is retired

**"Non-error questions"**

- Am I required to implement e-Invoice, and when? → [10-implementation-timeline.md](10-implementation-timeline.md)
- Where do I buy a digital certificate? → [11-digital-certificates.md](11-digital-certificates.md)
- How do I set up sandbox? → [12-sandbox-setup.md](12-sandbox-setup.md)
- What fields do I need in my invoice? → [13-required-fields-reference.md](13-required-fields-reference.md)
- How do I handle the 72-hour cancellation window? → [14-validation-status-lifecycle.md](14-validation-status-lifecycle.md)
- Am I ready to go live? → [15-production-readiness.md](15-production-readiness.md)
- How do I run this in production? → [16-observability-and-operations.md](16-observability-and-operations.md)
- Who do I contact for support? → [contacts.md](../contacts.md)

---

## Environment URLs (cheat sheet)

| Environment | Portal | API |
|---|---|---|
| Production | `myinvois.hasil.gov.my` | `api.myinvois.hasil.gov.my` |
| Sandbox / Pre-prod | `preprod.myinvois.hasil.gov.my` | `preprod-api.myinvois.hasil.gov.my` |

Sandbox rate limits are **lower** than production (as of 28 Apr 2025) and data is
**retained only for 3 months** before permanent deletion. Back up anything important.

---

## How this fits into the main docs

This cookbook was built as a follow-up to the main
[`myinvois-docs`](../README.md) scrape of the official SDK site, to close out
the "error cookbook" and "community SDKs" gaps that the SDK portal doesn't
cover directly. Drop this whole folder into the main docs repo, or use it
standalone.

## License

The text in this cookbook is a derivative compilation of public documentation
from LHDN/IRBM, PosDigicert's public guideline, and the linked open-source
repositories (all MIT / ISC licensed). Released under MIT to match.
