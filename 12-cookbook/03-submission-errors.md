# 03 — Submission & Validation Errors

When the Submit Documents API accepts your upload (HTTP 200) but the validators
reject individual documents, the response body will have `status: "Invalid"`
and an error chain. This file catalogs those errors by validator.

## The 8 validators (in execution order)

| # | Validator | What it checks | Typical codes |
|---|---|---|---|
| 1 | Structure Validator | Document matches the UBL 2.1 schema for its declared version | `Invalid Structure` |
| 2 | Core Fields Validator | All mandatory fields are present and well-formed | `CF###`-prefixed codes |
| 3 | Duplicated Submission Validator | Not a resubmission of a recent document | `Error03`, `DS302` |
| 4 | Signature Validator | Digital signature is valid | `DS300`–`DS338` — see [02-signature-errors.md](02-signature-errors.md) |
| 5 | Taxpayer Validator | Supplier/buyer TINs exist and were valid at issuance | `Error05`-prefixed, "TIN doesn't match" |
| 6 | Referenced Documents Validator | Referenced invoices (for credit/debit/refund notes) exist and are valid | `Error06`-prefixed |
| 7 | Code Validator | Currency codes, tax types, classification codes, MSIC codes, state codes are valid | `Error07`-prefixed, `ItemCode` errors |
| 8 | Currency Validator | Currency codes and exchange rates are consistent | (runs under Code Validator) |

## Top-level error codes by step

The outer `errorCode` reflects the validator that failed:

| Code | Step name | Meaning |
|---|---|---|
| `Error01` | Structure Validator | Document failed schema validation |
| `Error02` | Core Fields Validator | A required field is missing or malformed |
| `Error03` | Duplicated Submission Validator | Matches a recent submission |
| `Error04` | Signature Validator | Signature is invalid — drill into `innerError` for the `DS3xx` code |
| `Error05` | Taxpayer Validator | Supplier or buyer TIN problem |
| `Error06` | Referenced Documents Validator | Referenced invoice doesn't exist or was invalid |
| `Error07` | Code Validator | One of the code tables didn't match |

## Structure Validator

### `Invalid Structure`

The most common Structure Validator error. Per the official FAQ:

> The "Invalid Structure" error message usually appears due to an **incorrect
> sequence of elements** in your submission.

UBL 2.1 is order-sensitive. Element sequence must match the schema definition
exactly. Common culprits:

- `cac:AccountingSupplierParty` must come before `cac:AccountingCustomerParty`
- `cac:TaxTotal` comes before `cac:LegalMonetaryTotal`
- `cac:InvoiceLine` blocks come after all totals

**Fix:** compare your XML/JSON structure against the
[official sample](https://sdk.myinvois.hasil.gov.my/sample/) and realign the
order. The [`klsheng/myinvois-php-sdk`](https://github.com/klsheng/myinvois-php-sdk)
and its re-created UBL-Invoice package already enforce correct ordering — use
it as a reference.

### `Failed to parse input document`

Your XML or JSON couldn't be parsed at all. Checks:
- UTF-8 encoding, no BOM
- No unescaped special characters (see [07-special-characters.md](07-special-characters.md))
- Well-formed XML (closing tags match) / well-formed JSON (balanced braces)
- `Content-Type` header matches the content (`application/xml` vs `application/json`)

## Core Fields Validator

Core-field errors use `CF###` codes. LHDN updates the validation rules
periodically — the following are confirmed as of 2025.

### Known `CF` codes

| Code | Category | Meaning / fix |
|---|---|---|
| `CF321` | Supplier email | Email format / length issue on supplier |
| `CF364` | Phone format | Phone contains disallowed characters (spaces, dashes, parens) |
| `CF366` | Phone length | Phone < 8 or > 20 characters |
| `CF401` | Phone field | Phone-number validation failure (specific — see [06-field-validation-rules.md](06-field-validation-rules.md)) |
| `CF403` | Email field | Email format violation (RFC 5321/5322) or length > 320 |
| `CF410` | Email empty | Email is required but missing |

**Note:** LHDN doesn't publish the full `CF` code list. The codes above are
confirmed from third-party support documentation (ABSS, Financio). The error
message text in the response will include what's wrong — always read both
`error` (English) and `propertyPath` fields to localize.

### Example — phone number validation fix

```text
# Wrong (will fail CF364/CF401):
"+60 12-345 6789"
"(03) 1234 5678"

# Correct:
"+60123456789"
"0312345678"
```

See [06-field-validation-rules.md](06-field-validation-rules.md) for the
complete field rule set.

## Duplicated Submission Validator (`Error03` + `DS302`)

The system identifies duplicates by comparing these 5 fields using AND, within
a **2-hour rolling window**:

1. e-Invoice type and type version
2. Issuance date and time
3. Internal ID / invoice number
4. Supplier TIN (for normal e-Invoices)
5. Buyer TIN (for self-billed e-Invoices)

If **all** of these match an existing submission, you get `DS302`. See
[09-duplicate-detection.md](09-duplicate-detection.md) for a worked example.

**Do not**: retry a submission that returned HTTP 200. It was accepted. Use
`Get Submission` to check its validation status instead of resubmitting.

## Taxpayer Validator

### "TIN doesn't match" family

Three variants of this error from the FAQ:

| Scenario | Resolution |
|---|---|
| You're using `Login as Taxpayer`. | The issuer TIN in the document **must match** the TIN associated with your Client ID / Client Secret. |
| You're using `Login as Intermediary`. | The issuer TIN in the document **must match** the TIN of the taxpayer you're representing (via `onbehalfof` header). |
| Sole proprietors using `IG` prefix. | TIN validation works for `IG`-prefix TINs only if the **"Business Owner" role** is assigned in the user's MyTax profile. |

### "Issuance date time value of the document is too old and cannot be submitted"

The `IssueDate` + `IssueTime` on the document is > 72 hours in the past. You
have a 72-hour window from the time stamped on the document to submit it.

**Fix:** regenerate the document with a current timestamp. Do not back-date.

### "Document issuance date and time is in the future"

Your `IssueDate`/`IssueTime` is after the current UTC time. Two common causes:
1. Local timezone bleeding into the timestamp (e.g. Malaysia's UTC+8 emitted
   as UTC).
2. Clock skew on your server. Check NTP.

Always use **UTC with `Z` suffix**: `2025-01-15T08:30:00Z`.

## Referenced Documents Validator

Applies to credit notes, debit notes, and refund notes. The original invoice
referenced in `cac:BillingReference` → `cac:InvoiceDocumentReference` must:

- Exist in the MyInvois system
- Have been in `Valid` status at the time the credit/debit/refund note is issued
- Not be cancelled

To reference **multiple** original invoices in one credit/debit/refund note,
add **multiple** `InvoiceDocumentReference` entries — do **not** concatenate
UUIDs in a single reference. (From the FAQ: "taxpayer will need to add a new
line at Billing Reference. It cannot be inserted under the same
InvoiceDocumentReference.")

## Code Validator

Validates the enumerated codes in your document against the SDK code tables.

### "ItemCode 00 was not active when document issued"

Two possible meanings per the FAQ:
1. The **classification code** `00` isn't active / defined for this item type.
2. The **state code** `00` was retired — it's no longer accepted for API
   submissions. Use the codes from
   <https://sdk.myinvois.hasil.gov.my/codes/state-codes/>.

### Other common code errors

- **Invalid currency code** — must be ISO 4217 3-letter code. Check against
  <https://sdk.myinvois.hasil.gov.my/codes/currencies/>.
- **Invalid tax type** — tax exemption code `E` was added on 24 May 2024. If
  you're using an SDK that predates that, update it.
- **Invalid MSIC code** — must match a current MSIC 2008 classification.
- **Invalid classification code** — see
  <https://sdk.myinvois.hasil.gov.my/codes/classification-codes/>.

## Size limits

- **Individual XML/JSON document**: max **300 KB** per submission.
  - If you exceed: **minify** (remove whitespace, unnecessary formatting) —
    usually cuts XML by 20-30%.
- **Submission batch size**: check the official docs — typical batching is
  100 documents per submission call, but this is rate-limited, not schema-limited.

## Negative values & edge cases

- **Negative values are allowed** in Document Submission (returns, adjustments).
- **Special characters in invoice IDs are allowed** — no restriction.
- **Multi-buyer-email** — only one email supported per field, but the field
  allows up to 320 characters (RFC 5321 limit).
- **Multiple tax types per line** — repeat the `TaxSubtotal` element within
  the line item; do not split into multiple lines for this purpose.

## Reference links

- [Official FAQ (source for most of this page)](https://sdk.myinvois.hasil.gov.my/faq/)
- [Document Validation Rules](https://sdk.myinvois.hasil.gov.my/document-validation-rules/)
- [Code tables index](https://sdk.myinvois.hasil.gov.my/codes/)
