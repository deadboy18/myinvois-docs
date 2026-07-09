# MyInvois Malaysia — Developer Documentation

> **Community-maintained developer documentation for Malaysia's LHDN e-Invoice (MyInvois) system.**
> APIs, digital certificates, SDKs, validation errors, production playbooks — all in one place.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)
[![Docs](https://img.shields.io/badge/docs-92%20markdown%20files-blue.svg)](#-whats-inside)
[![Last verified](https://img.shields.io/badge/last%20verified-July%202026-green.svg)](#-freshness--versioning)
[![Links](https://img.shields.io/badge/links-853%2F853%20resolved-brightgreen.svg)](#)

This repository is a comprehensive, developer-focused reference for integrating
with the **MyInvois System** operated by **Lembaga Hasil Dalam Negeri Malaysia
(LHDN / IRBM)**. It covers every documented API endpoint, every document-type
schema, every known error code, digital-certificate procurement, all major
community SDKs, and a full production-operations playbook.

It is **not an official LHDN publication.** For authoritative guidance always
consult [sdk.myinvois.hasil.gov.my](https://sdk.myinvois.hasil.gov.my/) and
the [IRBM e-Invoice microsite](https://www.hasil.gov.my/en/e-invoice/). This
repo aggregates public material into a shape developers can actually work
from, fills documented gaps (the complete signature error table, SDK survey,
operations runbooks), and keeps everything cross-linked.

---

## 📚 What's inside

| Section | Purpose | Files |
|---|---|---|
| [`01-getting-started/`](01-getting-started/) | API overview, standard error response, standard headers, integration practices | 5 |
| [`02-platform-api/`](02-platform-api/) | Platform API endpoints (login × 2, document types × 3, notifications) | 7 |
| [`03-einvoice-api/`](03-einvoice-api/) | e-Invoice API endpoints (validate TIN, submit, cancel, reject, retrieve, search, QR code) | 12 |
| [`04-document-types/`](04-document-types/) | UBL schemas for invoices, credit/debit/refund notes, self-billed variants — v1.0 and v1.1 | 17 |
| [`05-code-tables/`](05-code-tables/) | Classification, countries, currencies, e-Invoice types, MSIC, payment methods, state codes, tax types, unit types | 10 |
| [`06-signature/`](06-signature/) | XAdES signature spec, creation guides for XML and JSON, mathematical mappings | 4 |
| [`07-validation/`](07-validation/) | The 8 document validators | 1 |
| [`08-faq/`](08-faq/) | Official IRBM FAQ | 1 |
| [`09-postman/`](09-postman/) | Postman collection + sandbox/production environments | 4 |
| [`10-sample-documents/`](10-sample-documents/) | 44 real XML/JSON sample payloads + 9 code-table JSONs + Digital Signature User Guide PDF | 55 |
| [`11-irbm-specific-guideline/`](11-irbm-specific-guideline/) | **Specific Guideline v4.8** as markdown — cross-border, self-billed, e-commerce, employment perks, foreign income, cybersecurity, **SVDP**, appendices | 1 |
| [`12-cookbook/`](12-cookbook/) | **Error cookbook + operations playbook + General Guideline v4.7 + community SDK survey** | 26 |
| [`release-notes/`](release-notes/) | Archived release notes | 3 |

---

## 🚀 Quick navigation by use case

### "I want to integrate MyInvois from scratch."
Start here, in this order:

1. [Getting Started → Overview](01-getting-started/overview.md) — what is MyInvois, who's in scope
2. [Cookbook → Implementation Timeline & Scope](12-cookbook/10-implementation-timeline.md) — am I required? by when?
3. [Cookbook → Sandbox Setup](12-cookbook/12-sandbox-setup.md) — register, get credentials
4. [Cookbook → Digital Certificates](12-cookbook/11-digital-certificates.md) — buy a cert (4 vendors compared)
5. [Platform API → Login as Taxpayer](02-platform-api/01-login-taxpayer.md) — authenticate
6. [Signature → Creation guide XML](06-signature/signature-creation-xml.md) / [JSON](06-signature/signature-creation-json.md) — sign your invoices
7. [e-Invoice API → Submit Documents](03-einvoice-api/02-submit-documents.md) — submit
8. [e-Invoice API → Get Submission](03-einvoice-api/06-get-submission.md) — poll for status
9. [Cookbook → Production Readiness](12-cookbook/15-production-readiness.md) — pre-launch checklist

### "My submission failed. Where do I look?"

| You got this | Go to |
|---|---|
| HTTP 4xx / 5xx | [Cookbook → HTTP Status Codes](12-cookbook/01-http-status-codes.md) |
| `errorCode: "DS3xx"` | [Cookbook → Signature Errors (full DS300–DS338 table)](12-cookbook/02-signature-errors.md) |
| `Error03` / duplicate submission | [Cookbook → Duplicate Detection](12-cookbook/09-duplicate-detection.md) |
| `Invalid Structure` | [Cookbook → Submission Errors](12-cookbook/03-submission-errors.md) |
| `CF401` / `CF403` / `CF410` | [Cookbook → Field Validation Rules](12-cookbook/06-field-validation-rules.md) |
| HTTP 429 | [Cookbook → Rate Limits](12-cookbook/04-rate-limits.md) |
| Anything weird | [Cookbook → Common Pitfalls](12-cookbook/05-common-pitfalls.md) |

### "I'm deciding what to buy/build."

| Question | Answer in |
|---|---|
| Which digital cert should I buy? | [Cookbook → Digital Certificates](12-cookbook/11-digital-certificates.md) (PosDigicert, MSC Trustgate, Rafftech, TM Applied Business compared) |
| Soft cert vs roaming cert? | [Cookbook → Digital Certificates § Soft vs Roaming](12-cookbook/11-digital-certificates.md#soft-vs-roaming-certificates) |
| Which community SDK? | [Cookbook → Community SDKs](12-cookbook/community-sdks/README.md) (PHP, Node.js, Python, C#, B4X surveyed) |
| Do I need to implement at all? | [Cookbook → Implementation Timeline](12-cookbook/10-implementation-timeline.md) |

### "I'm running this in production."

- [Production Readiness Checklist](12-cookbook/15-production-readiness.md) — 130+ items across 13 sections
- [Observability & Operations](12-cookbook/16-observability-and-operations.md) — logging schema, metrics, alerts, runbooks
- [Integration Anti-Patterns](12-cookbook/08-integration-antipatterns.md) — five things that will get you throttled
- [Validation Status Lifecycle](12-cookbook/14-validation-status-lifecycle.md) — state machine + 72-hour windows

---

## 🌐 Environment URLs (cheat sheet)

| | Production | Sandbox / Pre-production |
|---|---|---|
| **Portal** | `myinvois.hasil.gov.my` | `preprod.myinvois.hasil.gov.my` |
| **API base** | `api.myinvois.hasil.gov.my` | `preprod-api.myinvois.hasil.gov.my` |
| **Identity** | `api.myinvois.hasil.gov.my` | `preprod-api.myinvois.hasil.gov.my` |

> ⚠️ **Sandbox has lower rate limits and only 3 months of data retention** (effective Apr 2025). See [Cookbook → Sandbox Setup](12-cookbook/12-sandbox-setup.md).

---

## 🔄 How the integration flow works

```
     Your ERP / accounting system
              │
              │  1. Build UBL 2.1 document (XML or JSON)
              │  2. Sign with organisational digital certificate
              ▼
     ┌─────────────────────────┐
     │   Login as Taxpayer /   │  ← cache token for 60 min
     │   Intermediary          │
     └────────────┬────────────┘
                  │ access_token
                  ▼
     ┌─────────────────────────┐
     │  Submit Documents API   │  ← up to 100 rpm, batchable
     └────────────┬────────────┘
                  │ submissionUID
                  ▼
     ┌─────────────────────────┐
     │  Poll Get Submission    │  ← 300 rpm, use THIS not Get Recent
     │  until Valid / Invalid  │
     └────────────┬────────────┘
                  │
       ┌──────────┴──────────┐
       ▼                     ▼
    Valid                Invalid
       │                     │
       │  3. Generate QR     │ 4. Fix errors
       │  4. Share w/ buyer  │    → resubmit
       ▼                     │
    Stored 7+ years          │
    by IRBM + taxpayer       │
       │                     │
       │ 72-hour window:     │
       │ cancel (Supplier)   │
       │ reject (Buyer)      │
       ▼
    After 72h: auto-accepted.
    Adjust via credit/debit/refund note.
```

---

## 📜 Official sources (always authoritative)

| Purpose | URL |
|---|---|
| MyInvois SDK portal (APIs, schemas, codes) | <https://sdk.myinvois.hasil.gov.my/> |
| IRBM e-Invoice microsite | <https://www.hasil.gov.my/en/e-invoice/> |
| Implementation information | <https://www.hasil.gov.my/en/e-invoice/implementation-of-e-invoicing-in-malaysia/> |
| General + Specific Guidelines (latest PDFs) | <https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/> |
| SDK FAQ | <https://sdk.myinvois.hasil.gov.my/faq/> |
| Document validation rules | <https://sdk.myinvois.hasil.gov.my/document-validation-rules/> |
| Signature specification | <https://sdk.myinvois.hasil.gov.my/signature/> |
| Postman collection | <https://sdk.myinvois.hasil.gov.my/postman/> |
| Integration best practices | <https://sdk.myinvois.hasil.gov.my/integration-practices/> |
| Release notes | <https://sdk.myinvois.hasil.gov.my/release-notes/> |
| SDK contacts | <https://sdk.myinvois.hasil.gov.my/contacts/> |
| MyTax Portal (login, TIN lookup, e-Daftar) | <https://mytax.hasil.gov.my/> |
| MyInvois Portal (production) | <https://myinvois.hasil.gov.my/> |
| MyInvois Portal (sandbox) | <https://preprod.myinvois.hasil.gov.my/> |
| MCMC licensed Certificate Authorities | <https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees> |

### Digital certificate vendors

| CA | e-Invoice page | Phone |
|---|---|---|
| Pos Digicert | <https://www.posdigicert.com.my/digital-certificate-for-einvoice> | 03-8800 6009 |
| MSC Trustgate | <https://www.msctrustgate.com/e-invoice> | +603 8318 1800 |
| Rafftech (Raffcomm Technologies) | <https://www.rafftech.my/einvoice> | +603 4040 0091 |
| Telekom Applied Business | See MCMC list | via corporate channels |

### Community SDKs

| Repo | Language | Status |
|---|---|---|
| [klsheng/myinvois-php-sdk](https://github.com/klsheng/myinvois-php-sdk) | PHP | Most complete, on Packagist |
| [syukranDev/e-invoice-sdk-nodejs](https://github.com/syukranDev/e-invoice-sdk-nodejs) | Node.js | On npm |
| [ERPGulf/myinvois](https://github.com/ERPGulf/myinvois) | Python / Frappe | ERPNext app |
| [mokth/einvoice](https://github.com/mokth/einvoice) | C# / .NET | Sample apps + X509 tools |
| [pyhoon/lhdn-einvoice-api-client-b4x](https://github.com/pyhoon/lhdn-einvoice-api-client-b4x) | B4X | Cross-platform reference UI |

Detailed per-SDK writeups: [`12-cookbook/community-sdks/`](12-cookbook/community-sdks/)

### Support contacts

| Channel | Contact |
|---|---|
| General e-Invoice questions (email) | `myinvois@hasil.gov.my` |
| Helpdesk hotline | **03-8682 8000** |
| HASiL Contact Centre | 03-8911 1000 |
| SDK feedback form | <https://feedback.myinvois.hasil.gov.my> |

Full contact directory: [`contacts.md`](contacts.md)

---

## 📅 Freshness & versioning

This is a **snapshot** as of **July 2026**, including:

- **e-Invoice General Guideline v4.7** (published 7 July 2026)
- **e-Invoice Specific Guideline v4.8** (published 7 July 2026)
- **MyInvois SDK** as published at that date
- **Digital Signature User Guide** (PosDigicert Rev 1.2, July 2024)
- Community SDK versions (klsheng 1.0.14, ERPGulf 3.0.1, etc.)

### What changes frequently

LHDN actively maintains the SDK. Expect updates to:

- **Release notes** → re-check <https://sdk.myinvois.hasil.gov.my/release-notes/>
- **Code tables** (currencies, tax types, classification codes, MSIC) → re-check <https://sdk.myinvois.hasil.gov.my/codes/>
- **FAQ** (new issues and resolutions) → re-check <https://sdk.myinvois.hasil.gov.my/faq/>
- **Validation rules** (new `CF###` codes appear) → re-check <https://sdk.myinvois.hasil.gov.my/document-validation-rules/>
- **Guideline PDFs** → re-check <https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/>

### What's stable

- **API endpoint contracts** — change rarely and with notice
- **DS300–DS338 signature error codes** — sourced from PosDigicert's signed guideline
- **The 4 validation statuses** (Submitted/Valid/Invalid/Cancelled)
- **The 72-hour windows** (legislated, not implementation detail)
- **Digital certificate requirements** (mandatory fields in cert Subject)

**Before going live, always diff against the official SDK.** If you spot a
drift, please contribute — see [`CONTRIBUTING.md`](CONTRIBUTING.md).

---

## 🎯 What this documentation does (and doesn't)

### ✅ Does

- Explain every documented MyInvois API endpoint with parameters, example requests/responses, and known errors
- Catalog every known error code with remediation (DS300–DS338 complete, HTTP codes, known `CF###` codes)
- Walk through digital certificate procurement with 4 vendors' real prices
- Survey all major community SDK implementations
- Provide a production-readiness checklist
- Include 44 sample invoice payloads covering normal, foreign currency, consolidated, multi-line, and self-billed variants
- Render both IRBM guideline PDFs (General v4.7, Specific v4.8) as searchable markdown

### ⚠️ Does not

- **Enumerate every `CF###` validation rule.** LHDN does not publish a complete list. The cookbook has the community-known ones. New ones appear when LHDN adds validation.
- **Replace the official guidelines.** When PDFs get revised, this repo may be out of date by weeks or months. Cross-check the [official page](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/).
- **Grant access to anything.** You still need real credentials from MyInvois Portal, a real TIN from IRBM, and a real cert from a licensed CA.
- **Cover industry-specific catalogue PDFs** (banking, insurance, construction, etc.) in detail. The Specific Guideline touches them; dedicated industry catalogues are published separately by LHDN.
- **Offer legal or tax advice.** Integration mechanics only.

---

## 🏷 License & attribution

MIT Licensed — see [`LICENSE.md`](LICENSE.md).

This is a compilation of publicly available material. Full source attribution
in [`ATTRIBUTION.md`](ATTRIBUTION.md). Trademarks (MyInvois, IRBM, LHDN,
ERPNext, Peppol, etc.) belong to their respective owners.

---

## 🤝 Contributing

Issues and PRs welcome. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for style
guidelines. Particularly valuable contributions:

- New error codes you encountered and resolved
- Newly discovered community SDKs
- Updates when LHDN publishes guideline revisions
- Real-world integration stories / gotchas

---

## 📖 Full table of contents

<details>
<summary>Click to expand full file tree</summary>

```
.
├── README.md                                   ← you are here
├── CONTRIBUTING.md                             ← style guide, how to add pages
├── CHANGELOG.md                                ← version history
├── LICENSE.md                                  ← MIT
├── ATTRIBUTION.md                              ← sources
├── contacts.md                                 ← IRBM + CA + community contacts
│
├── 01-getting-started/
│   ├── README.md
│   ├── overview.md
│   ├── error-responses.md
│   ├── integration-practices.md
│   └── standard-headers.md
│
├── 02-platform-api/                            ← 7 files (login × 2, doc types × 3, notifications)
├── 03-einvoice-api/                            ← 12 files (submit, cancel, reject, poll, retrieve, search, QR)
├── 04-document-types/                          ← 17 UBL schemas (v1.0 + v1.1)
├── 05-code-tables/                             ← 9 code tables
├── 06-signature/                               ← XAdES spec, creation guides (XML + JSON)
├── 07-validation/                              ← 8 validators described
├── 08-faq/                                     ← official IRBM FAQ
├── 09-postman/                                 ← Postman collection + envs
├── 10-sample-documents/                        ← 44 sample payloads + code JSONs + PDF
├── 11-irbm-specific-guideline/                 ← Specific Guideline v4.8 as markdown
│
├── 12-cookbook/                                ← Error + operations playbook
│   ├── README.md
│   ├── 01-http-status-codes.md                 (HTTP codes, standard envelope)
│   ├── 02-signature-errors.md                  (★ full DS300–DS338 table)
│   ├── 03-submission-errors.md                 (validators, CF codes)
│   ├── 04-rate-limits.md                       (RPM, Retry-After)
│   ├── 05-common-pitfalls.md                   (TIN, sandbox 403, UTC, etc.)
│   ├── 06-field-validation-rules.md            (phone, email, SST, TTX)
│   ├── 07-special-characters.md                (XML/JSON escapes)
│   ├── 08-integration-antipatterns.md          (10 don't-do-this patterns)
│   ├── 09-duplicate-detection.md               (2hr window, 5 fields)
│   ├── 10-implementation-timeline.md           (who, when, exemptions)
│   ├── 11-digital-certificates.md              (4 vendors, lifecycle)
│   ├── 12-sandbox-setup.md                     (preprod walkthrough)
│   ├── 13-required-fields-reference.md         (55 fields + annexure)
│   ├── 14-validation-status-lifecycle.md       (state machine, 72hr windows)
│   ├── 15-production-readiness.md              (★ 130-item checklist)
│   ├── 16-observability-and-operations.md      (logs, metrics, alerts, runbooks)
│   ├── community-sdks/
│   │   ├── README.md                           (comparison matrix)
│   │   ├── 01-klsheng-php-sdk.md
│   │   ├── 02-mokth-csharp.md
│   │   ├── 03-erpgulf-frappe.md
│   │   ├── 04-syukrandev-nodejs.md
│   │   └── 05-pyhoon-b4x.md
│   └── irbm-general-guideline/
│       └── README.md                           (General Guideline v4.7 as markdown)
│
└── release-notes/
    ├── README.md
    ├── sdk-1-0.md
    └── sdk-beta.md
```

</details>

---

> ⭐ **Star this repo if it saved you time.** Every integrator has lost hours
> to DS326 before — may this cookbook save the next one.
