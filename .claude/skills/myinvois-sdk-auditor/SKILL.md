---
name: myinvois-sdk-auditor
description: Scrape, audit, and update the myinvois-docs repository against the live LHDN MyInvois SDK portal and IRBM guideline PDFs. Use when the user asks to check for updates, sync the repo, scrape the SDK site, compare against the live portal, find what changed, or update the MyInvois documentation. Also triggers on mentions of "SDK updates", "new release notes", "check the portal", "scrape myinvois", "LHDN updates", or "guideline changes". Works with the repo at https://github.com/deadboy18/myinvois-docs or any local clone of it.
---

# MyInvois SDK Auditor

A skill for scraping the live LHDN MyInvois SDK portal and IRBM guideline PDFs,
comparing them against the myinvois-docs repository, identifying gaps, and
generating patch files to bring the repo up to date.

## Repo structure

The repo being maintained is a community developer documentation mirror of
Malaysia's LHDN e-Invoice (MyInvois) system. Key directories:

```
myinvois-docs/
├── 01-getting-started/          ← SDK "Getting Started" page
├── 02-platform-api/             ← Platform API endpoints (login, doc types, notifications)
├── 03-einvoice-api/             ← e-Invoice API endpoints (submit, cancel, reject, retrieve, search, QR)
├── 04-document-types/           ← UBL schemas for all 16 document types (v1.0 + v1.1)
├── 05-code-tables/              ← Classification, countries, currencies, MSIC, payment methods, state codes, tax types, units
├── 06-signature/                ← XAdES signature spec, creation guides
├── 07-validation/               ← Document validators
├── 08-faq/                      ← SDK FAQ (README.md) + LHDN general FAQ
├── 09-postman/                  ← Postman collection + environments
├── 10-sample-documents/         ← Sample XML/JSON payloads + code-table JSONs
├── 11-irbm-specific-guideline/  ← IRBM Specific Guideline as markdown
├── 12-cookbook/                  ← Error cookbook, operations playbook, community SDKs
│   └── irbm-general-guideline/  ← IRBM General Guideline as markdown
├── release-notes/               ← SDK release notes (sdk-1-0.md, sdk-beta.md)
├── CHANGELOG.md                 ← Repo version history
└── README.md                    ← Main readme with version badges
```

## Source URLs to check

### SDK Portal pages (primary sources)

| Page | URL | Maps to repo file(s) |
|---|---|---|
| Release notes index | https://sdk.myinvois.hasil.gov.my/release-notes/ | `release-notes/README.md` |
| SDK 1.0 Release (detailed) | https://sdk.myinvois.hasil.gov.my/sdk-1-0-release/ | `release-notes/sdk-1-0.md` |
| FAQ | https://sdk.myinvois.hasil.gov.my/faq/ | `08-faq/README.md` |
| Validation rules | https://sdk.myinvois.hasil.gov.my/document-validation-rules/ | `07-validation/README.md` |
| Getting started | https://sdk.myinvois.hasil.gov.my/start/ | `01-getting-started/overview.md` |
| State codes | https://sdk.myinvois.hasil.gov.my/codes/state-codes/ | `05-code-tables/state-codes.md` |
| Country codes | https://sdk.myinvois.hasil.gov.my/codes/countries/ | `05-code-tables/country-codes.md` |
| Currency codes | https://sdk.myinvois.hasil.gov.my/codes/currency-codes/ | `05-code-tables/currency-codes.md` |
| Tax types | https://sdk.myinvois.hasil.gov.my/codes/tax-types/ | `05-code-tables/tax-types.md` |
| Unit types | https://sdk.myinvois.hasil.gov.my/codes/unit-types/ | `05-code-tables/unit-types.md` |
| e-Invoice types | https://sdk.myinvois.hasil.gov.my/codes/e-invoice-types/ | `05-code-tables/e-invoice-types.md` |
| Payment methods | https://sdk.myinvois.hasil.gov.my/codes/payment-methods/ | `05-code-tables/payment-methods.md` |
| Classification codes | https://sdk.myinvois.hasil.gov.my/codes/classification/ | `05-code-tables/classification-codes.md` |
| MSIC codes | https://sdk.myinvois.hasil.gov.my/codes/msic-codes/ | `05-code-tables/msic-codes.md` |
| Signature | https://sdk.myinvois.hasil.gov.my/signature/ | `06-signature/README.md` |
| Integration practices | https://sdk.myinvois.hasil.gov.my/integration-practices/ | `01-getting-started/integration-practices.md` |
| Contacts | https://sdk.myinvois.hasil.gov.my/contacts/ | `contacts.md` |

### API endpoint pages

| Page | URL | Maps to |
|---|---|---|
| Login as Taxpayer | https://sdk.myinvois.hasil.gov.my/api/01-login-taxpayer/ | `02-platform-api/01-login-taxpayer.md` |
| Login as Intermediary | https://sdk.myinvois.hasil.gov.my/api/02-login-intermediary/ | `02-platform-api/02-login-intermediary.md` |
| Get Document Types | https://sdk.myinvois.hasil.gov.my/api/03-get-document-types/ | `02-platform-api/03-get-document-types.md` |
| Get Document Type | https://sdk.myinvois.hasil.gov.my/api/04-get-document-type/ | `02-platform-api/04-get-document-type.md` |
| Get Document Type Version | https://sdk.myinvois.hasil.gov.my/api/05-get-document-type-version/ | `02-platform-api/05-get-document-type-version.md` |
| Get Notifications | https://sdk.myinvois.hasil.gov.my/api/06-get-notifications/ | `02-platform-api/06-get-notifications.md` |
| Validate TIN | https://sdk.myinvois.hasil.gov.my/einvoicingapi/01-validate-taxpayer-tin/ | `03-einvoice-api/01-validate-taxpayer-tin.md` |
| Submit Documents | https://sdk.myinvois.hasil.gov.my/einvoicingapi/02-submit-documents/ | `03-einvoice-api/02-submit-documents.md` |
| Cancel Document | https://sdk.myinvois.hasil.gov.my/einvoicingapi/03-cancel-document/ | `03-einvoice-api/03-cancel-document.md` |
| Reject Document | https://sdk.myinvois.hasil.gov.my/einvoicingapi/04-reject-document/ | `03-einvoice-api/04-reject-document.md` |
| Get Recent Documents | https://sdk.myinvois.hasil.gov.my/einvoicingapi/05-get-recent-documents/ | `03-einvoice-api/05-get-recent-documents.md` |
| Get Submission | https://sdk.myinvois.hasil.gov.my/einvoicingapi/06-get-submission/ | `03-einvoice-api/06-get-submission.md` |
| Get Document | https://sdk.myinvois.hasil.gov.my/einvoicingapi/07-get-document/ | `03-einvoice-api/07-get-document.md` |
| Get Document Details | https://sdk.myinvois.hasil.gov.my/einvoicingapi/08-get-document-details/ | `03-einvoice-api/08-get-document-details.md` |
| Search Documents | https://sdk.myinvois.hasil.gov.my/einvoicingapi/09-search-documents/ | `03-einvoice-api/09-search-documents.md` |
| Search Taxpayer TIN | https://sdk.myinvois.hasil.gov.my/einvoicingapi/10-search-taxpayer-tin/ | `03-einvoice-api/10-search-taxpayer-tin.md` |
| QR Code | https://sdk.myinvois.hasil.gov.my/einvoicingapi/11-qr-code/ | `03-einvoice-api/11-qr-code.md` |

### IRBM Guideline PDFs (check for new versions)

| Document | Check URL | Maps to |
|---|---|---|
| General Guideline | https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/ | `12-cookbook/irbm-general-guideline/README.md` |
| Specific Guideline | Same page | `11-irbm-specific-guideline/README.md` |

## Audit workflow

### Step 1: Check release notes first (fastest signal)

The SDK release notes page is the single best indicator of what changed.
Compare the list of dated entries on the live page against `release-notes/sdk-1-0.md`.

```
1. web_fetch https://sdk.myinvois.hasil.gov.my/sdk-1-0-release/
2. Compare dated entries against release-notes/sdk-1-0.md
3. Any dates in live page not in the repo file = missing entries
```

### Step 2: Check FAQ for new entries

```
1. web_fetch https://sdk.myinvois.hasil.gov.my/faq/
2. Count ## headings in live page vs 08-faq/README.md
3. Look for new Q&A entries at the bottom (LHDN appends, doesn't insert)
```

### Step 3: Check IRBM guideline versions

```
1. web_search "IRBM e-Invoice guideline" site:hasil.gov.my
2. Compare version numbers against:
   - 12-cookbook/irbm-general-guideline/README.md (title line has version)
   - 11-irbm-specific-guideline/README.md (title line has version)
3. If new version found, user needs to upload the PDF for full extraction
```

### Step 4: Spot-check high-change pages

These pages change most often based on historical patterns:

```
Priority 1 (check every time):
- SDK 1.0 Release notes
- FAQ
- Guideline version numbers

Priority 2 (check if release notes mention them):
- Code tables (state codes, country codes, currency codes, unit types)
- Validation rules
- API endpoint pages (especially Submit Documents, Search TIN)

Priority 3 (rarely change):
- Signature spec
- Getting started
- Postman collection
- Document type schemas
```

### Step 5: Build patches

When differences are found:

1. Create updated files with changes applied
2. Update `CHANGELOG.md` with a new version entry
3. Update version references in `README.md` if guideline versions changed
4. Package as a zip for the user to extract and push
5. Provide exact git commands for commit, tag, and push

## Version numbering convention

The repo follows semver:
- **Major** (2.0.0) — breaking restructure of repo navigation
- **Minor** (1.2.0) — significant content update (new guideline version, new endpoint, new SDK surveyed)
- **Patch** (1.1.3) — corrections, clarifications, new FAQ entries, release note catch-ups

## Encoding fix patterns

The original scrape produced double-encoded UTF-8 (mojibake). If new content
is scraped from the SDK portal, apply these replacements before committing:

| Mojibake bytes | Correct output | Character |
|---|---|---|
| `\xc3\xa2\xc2\x80\xc2\x99` | `'` | Right single quote → apostrophe |
| `\xc3\xa2\xc2\x80\xc2\x98` | `'` | Left single quote → apostrophe |
| `\xc3\xa2\xc2\x80\xc2\x9c` | `"` | Left double quote |
| `\xc3\xa2\xc2\x80\xc2\x9d` | `"` | Right double quote |
| `\xc3\xa2\xc2\x80\xc2\xa2` | `•` (UTF-8) | Bullet point |
| `\xc3\xa2\xc2\x80\xc2\x93` | `–` (UTF-8) | En dash |
| `\xc3\x82\xc2\xa0` | ` ` (space) | Non-breaking space |
| `\xc3\x82` (bare, before space/letter) | (remove) | Stray artifact |

Apply this Python snippet to any newly scraped content:

```python
def fix_encoding(data: bytes) -> bytes:
    replacements = [
        (b'\xc3\xa2\xc2\x80\xc2\x99', b"'"),
        (b'\xc3\xa2\xc2\x80\xc2\x98', b"'"),
        (b'\xc3\xa2\xc2\x80\xc2\x9c', b'"'),
        (b'\xc3\xa2\xc2\x80\xc2\x9d', b'"'),
        (b'\xc3\xa2\xc2\x80\xc2\xa2', b'\xe2\x80\xa2'),
        (b'\xc3\xa2\xc2\x80\xc2\x93', b'\xe2\x80\x93'),
        (b'\xc3\xa2\xc2\x80\xc2\xa6', b'...'),
        (b'\xc3\x82\xc2\xa0', b' '),
    ]
    import re
    for old, new in replacements:
        data = data.replace(old, new)
    data = re.sub(rb'\xc3\x82(?=[ \n\ra-zA-Z0-9(])', b'', data)
    return data
```

## Current repo state (as of last update)

- **Repo version:** v1.1.2 (9 July 2026)
- **General Guideline:** v4.7 (published 7 July 2026)
- **Specific Guideline:** v4.8 (published 7 July 2026)
- **SDK release notes:** current through 08 July 2026
- **FAQ:** current (includes Search TIN ambiguous results entry)
- **Encoding:** all 98 markdown files clean, zero mojibake

## Important notes

- The SDK portal sometimes serves cached pages to web_fetch. If the user
  reports a newer entry visible on the live site that web_fetch doesn't return,
  ask them to copy-paste or screenshot the content.
- LHDN appends new FAQ entries at the bottom — diff from the end.
- LHDN prepends new release note entries at the top — diff from the top.
- Code table JSON files at `10-sample-documents/samples/*.json` may also
  need updating if code tables change, but these are lower priority.
- The `08-faq/lhdnm-e-invoice-general-faqs.md` file is a separate document
  from the SDK FAQ — it's the LHDN general FAQ PDF rendered as markdown.
- Always update `CHANGELOG.md` with every patch.
