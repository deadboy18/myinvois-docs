# Community SDKs — Overview

LHDN's official SDK only provides documentation and Postman collections — no
official client libraries. The Malaysian developer community has filled that
gap. This section surveys the five most prominent open-source implementations.

## At a glance

| Repo | Language | Stars* | Install | Official package registry | Best for |
|---|---|---|---|---|---|
| [klsheng/myinvois-php-sdk](https://github.com/klsheng/myinvois-php-sdk) | PHP | 98 | `composer require klsheng/myinvois-php-sdk` | [Packagist](https://packagist.org/packages/klsheng/myinvois-php-sdk) | **Most complete**. Covers all endpoints, UBL builders, digital signature. |
| [mokth/einvoice](https://github.com/mokth/einvoice) | C# / .NET | 65 | Clone the repo | — | Multiple sample apps (console, Windows), X509 + XAdES signature tools |
| [ERPGulf/myinvois](https://github.com/ERPGulf/myinvois) | Python | 21 | `bench get-app ...` | — | **If you're on ERPNext / Frappe.** Full-stack app, not a standalone SDK |
| [syukranDev/e-invoice-sdk-nodejs](https://github.com/syukranDev/e-invoice-sdk-nodejs) | Node.js | 13 | `npm install einvoice-sdk-nodejs` | [npm](https://www.npmjs.com/package/einvoice-sdk-nodejs) | JSON-first, lightweight, includes auto-retry |
| [pyhoon/lhdn-einvoice-api-client-b4x](https://github.com/pyhoon/lhdn-einvoice-api-client-b4x) | B4X | 9 | Clone the B4J/B4A/B4i project | — | Reference UI app (not a library). Desktop + Android + iOS. |

*Star counts as of April 2026 snapshot.

## Coverage matrix

Which repo implements which MyInvois endpoint:

| Endpoint | klsheng PHP | mokth C# | ERPGulf Py | syukran Node | pyhoon B4X |
|---|---|---|---|---|---|
| Login as Taxpayer | ✅ | ✅ | ✅ | ✅ | ✅ |
| Login as Intermediary | ✅ | partial | partial | ✅ | ❌ |
| Get All Document Types | ✅ | ✅ | ✅ | — | — |
| Get Document Type | ✅ | — | — | — | — |
| Get Document Type Version | ✅ | — | — | — | — |
| Get Notifications | ✅ | — | — | — | — |
| Validate Taxpayer TIN | ✅ | ✅ | ✅ | ✅ | ✅ |
| Search Taxpayer TIN | ✅ | — | — | — | — |
| Get Taxpayer from QR Code | ✅ | — | — | — | — |
| Submit Documents (JSON) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Submit Documents (XML) | ✅ | ✅ | ✅ | — | ✅ |
| Cancel Document | ✅ | ✅ | ✅ | ✅ | — |
| Reject Document | ✅ | — | — | — | — |
| Get Recent Documents | ✅ | — | ✅ | — | — |
| Get Submission | ✅ | ✅ | ✅ | ✅ | — |
| Get Document | ✅ | ✅ | ✅ | ✅ | — |
| Get Document Details | ✅ | — | ✅ | ✅ | — |
| Search Documents | ✅ | — | — | — | — |
| Generate QR Code URL | ✅ | — | ✅ | — | — |
| Digital Signature | ✅ | ✅ | ✅ | ✅ | ✅ |
| Self-Billed variants | ✅ | ✅ | ✅ | ✅ | ✅ |
| Consolidated invoices | — | — | ✅ | — | — |

**Note:** `partial` for the intermediary logins means the code exists but may
not expose the `onbehalfof` parameter convincingly — review before relying on it.

## Feature comparison

### Digital signature handling

| Repo | Approach |
|---|---|
| klsheng PHP | Full pipeline. Supports both `.crt + .key` and `.p12 / .pfx` with passphrase. Includes **DS326 workaround** (configurable RDN order). |
| mokth C# | Separate tools: `X509CertificateTool` for cert manipulation, `XmlSignatureTest` for signing, `hashingUtilities` for hashing. Modular but DIY assembly. |
| ERPGulf Python | Integrated into Frappe; handles both v1.0 (no sig) and v1.1 (with sig). |
| syukranDev Node | JSON signing only. Exposes `getCertificatesHashedParams()` helper. |
| pyhoon B4X | Uses B4X Encryption library; works but is sample-quality. |

### Error handling

| Repo | Approach |
|---|---|
| klsheng PHP | Surfaces errors to caller as exceptions/arrays. No auto-retry. |
| mokth C# | Per-app; check each sample project. Generally exception-based. |
| ERPGulf Python | Frappe error framework with audit logging. |
| syukranDev Node | **Has automatic retry on 429.** |
| pyhoon B4X | Sample-quality error display (MsgBox-style). |

### Package distribution

| Repo | Registry |
|---|---|
| klsheng PHP | Packagist (Composer) — `klsheng/myinvois-php-sdk`, 12k+ installs |
| syukranDev Node | npm — `einvoice-sdk-nodejs` |
| others | Source-only, no package published |

## Picking one

- **Building a PHP web app or service?** → klsheng PHP SDK. It's the most
  complete and has the largest user base (visible via the DS326 workaround
  being upstream).
- **Building a Node.js service?** → syukranDev. It's lightweight and handles
  rate limiting. For XML support, you'd add your own layer.
- **On Frappe / ERPNext?** → ERPGulf. Drop-in compliance app.
- **Building a .NET / C# ERP integration?** → mokth. Use the components, not
  the sample apps directly.
- **Prototyping on mobile or cross-platform desktop?** → pyhoon B4X for
  the UI; under the hood it still just speaks the MyInvois HTTP API.

## What's missing across the board

Gaps you'll likely need to fill yourself regardless of which SDK you pick:

1. **Webhook / notification handling** — MyInvois doesn't actually push
   notifications to your system; you poll `Get Notifications`. No SDK
   wraps this well.
2. **Batch state machine** — managing a queue of submissions, retrying
   failures, reconciling cancel/reject flows. Each SDK stops at "make the
   call"; you need to build the workflow.
3. **Observability** — correlation-ID logging, structured error emission,
   per-TIN rate-limit quotas. You'll want to add this.
4. **UI for error remediation** — accountants/AP staff need to see what's
   failing and fix it. No SDK provides this.
5. **Archival** — IRBM recommends 7-year retention of source documents. Your
   integration needs its own storage strategy.

## Detail pages

- [klsheng PHP SDK](01-klsheng-php-sdk.md)
- [mokth C# / .NET](02-mokth-csharp.md)
- [ERPGulf Frappe/ERPNext](03-erpgulf-frappe.md)
- [syukranDev Node.js](04-syukrandev-nodejs.md)
- [pyhoon B4X client](05-pyhoon-b4x.md)
