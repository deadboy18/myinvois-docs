# 05 — Common Pitfalls & Gotchas

A catalog of the most common "this should work but doesn't" issues, pulled
from the official MyInvois FAQ, PosDigicert's guidance, and community SDK
READMEs. If you're stuck, start here.

## TIN format rules (the #1 source of HTTP 400)

MyInvois is strict about TIN format. Getting it wrong returns
`400 BadArgument`.

### Individual TIN — prefix `IG`

- The prefix for individual TINs is now **`IG`**, replacing the old `OG` and `SG`.
- Example: `SG123456789` → now input as `IG123456789`.
- The numeric part is unchanged. Max length **14 characters including prefix**.
- For sole proprietors: the `IG` prefix plus the BRN works *only if* the user
  has the **"Business Owner" role** assigned in their MyTax profile.

### Non-individual TIN — prefixes `C`, `CS`, `D`, `F`, `FA`, `PT`, `TA`, `TC`, `TN`, `TR`, `TP`, `J`, `LE`

Non-individual TINs have **two** quirks:

1. **Remove leading zeros after the prefix:**
   - `C01234567890` → `C1234567890`
2. **Ensure trailing zero:**
   - `C123456789` (no trailing 0) → `C1234567890`
   - Non-individual TINs always end with `0`. If yours doesn't, add one.

### Quick validation before submit

```python
import re

INDIVIDUAL_TIN = re.compile(r"^IG\d{1,12}$")
NON_INDIVIDUAL_PREFIXES = {"C","CS","D","F","FA","PT","TA","TC","TN","TR","TP","J","LE"}

def tin_looks_valid(tin: str) -> bool:
    if tin.startswith("IG"):
        return bool(INDIVIDUAL_TIN.match(tin)) and len(tin) <= 14
    for prefix in NON_INDIVIDUAL_PREFIXES:
        if tin.startswith(prefix):
            rest = tin[len(prefix):]
            return rest.isdigit() and rest[0] != '0' and rest[-1] == '0'
    return False
```

This won't catch every invalid TIN (only IRBM knows what's actually registered),
but catches 90% of format errors before they hit the wire.

## 403 Forbidden in sandbox

**Most common cause:** you used **production** Client ID / Secret in **sandbox**.

The two environments are isolated. You need to:

1. Register your company in the **pre-production portal**
   (`preprod.myinvois.hasil.gov.my`).
2. Register your ERP system there — separately from production.
3. Obtain a **different** Client ID + Client Secret for the sandbox.

The production and sandbox credentials are never interchangeable.

## 401 Unauthorized

Almost always means your **access token expired**. Tokens are valid for
**60 minutes** from issue.

- Don't fetch a new token per request — you'll hit the Login rate limit (12 rpm)
  and trigger throttling.
- Do cache the token and re-login only when the cached one expires.
- Read `expires_in` from the Login response (in seconds). Don't hardcode 3600.

## The 72-hour window

Several places in MyInvois use a **72-hour rule**. Keep them straight:

| Window | What happens |
|---|---|
| Document `IssueDate` → submission | Must be within **72 hours** before submission. If older, validator rejects with "Issuance date time value of the document is too old". |
| Submission → buyer can reject | Buyer has **72 hours** after validation to request rejection. |
| Submission → supplier can cancel | Supplier has **72 hours** after validation to cancel (with justification). |
| Past 72h post-validation | Invoice is auto-accepted. Adjustments must be via new credit/debit/refund note. |

**Practical impact:** don't batch up documents and submit them in daily/weekly
runs if `IssueDate` falls behind 72 hours. Submit continuously.

## UTC timestamp formatting

All timestamps must be **UTC with `Z` suffix**:

```
Good:  2025-01-15T08:30:00Z
Bad:   2025-01-15T16:30:00+08:00       ← offsets not accepted
Bad:   2025-01-15T08:30:00.123Z        ← sub-seconds rejected by some validators
Bad:   2025-01-15T08:30:00             ← no Z
Bad:   Wed, 15 Jan 2025 08:30:00 GMT   ← HTTP date format
```

Malaysia is UTC+8. If you're generating timestamps in local time, convert
before stamping:

```python
from datetime import datetime, timezone
now_utc = datetime.now(timezone.utc).strftime("%Y-%m-%dT%H:%M:%SZ")
```

## Printing: API vs Portal

- e-Invoices submitted **via API cannot be printed from the MyInvois Portal.**
- The print feature only works for invoices submitted manually through the portal.
- Your ERP must handle printing for API-submitted invoices.

## Document versions (v1.0 vs v1.1)

Both v1.0 and v1.1 are available. Key difference:

| Feature | v1.0 | v1.1 |
|---|---|---|
| Basic submission | ✅ | ✅ |
| Digital signature validation | ❌ | ✅ |
| Available in sandbox | ✅ | ✅ |
| Recommended for new integrations | ❌ | ✅ |

Recommendation from LHDN: **use v1.1**. v1.0 is kept alive for backwards
compatibility but will eventually be retired (no official date yet).

## UBL JSON quirk: every value keyed by `_`

UBL's JSON binding requires every attribute value to be inside an object keyed
by `_`:

```json
// Wrong
{ "InvoiceTypeCode": "02" }

// Right
{ "InvoiceTypeCode": { "_": "02" } }
```

This applies to every simple-type value in the UBL JSON document. It's
counterintuitive but it's what the UBL 2.1 JSON spec requires.

## XML size > 300 KB

Hard limit is 300 KB per document. If you exceed it:

1. **Minify first** — remove whitespace, newlines, comments, unnecessary
   formatting. Typical shrink: 20–30% on a "pretty-printed" XML.
2. If still too big: split into multiple invoices. No other option.

## Two client secrets — what are they for?

Each ERP registration gets **two client secrets** that expire simultaneously.
The reason: **redundancy**. If one leaks, rotate by using the other. Both valid
at the same time, no primary/secondary distinction.

## Intermediary credentials

If you're a service provider submitting on behalf of customers:

- **Use your own** Client ID / Client Secret (not the customer's).
- Authenticate via `Login as Intermediary System`.
- Add the `onbehalfof` header with the customer's TIN.
- You need to be added as an intermediary in the customer's MyInvois portal
  settings **with appropriate permissions** (Submit, Cancel, Request Rejection,
  View — toggle them all on if you need full access).

## The "Business Owner" role for sole proprietors

For sole-proprietor TINs (starting with `IG`) to validate:
1. Go to MyTax portal
2. Check/assign the **"Business Owner"** role to the user
3. TIN validation with BRN will now work

Without this role, `Validate Taxpayer TIN` API will reject sole-proprietor
TINs even if they're real.

## `NA` for missing SST / TTX numbers

If the supplier / buyer has no SST or TTX number, use **`NA`** literally.
Don't leave empty, don't omit the element.

- SST field allows `NA`
- TTX field allows `NA`
- Email field can be blank (don't use `NA` here)

## Classification code `00` in product master

"ItemCode 00 was not active when document issued" means the item being
invoiced has classification code `00`, which is either retired or not
configured. Fix:

1. Go to your product master data.
2. Assign a valid classification code from
   <https://sdk.myinvois.hasil.gov.my/codes/classification-codes/>.
3. Resubmit.

## State code `00` is retired

Per the FAQ: "the use of state code `00` is no longer valid for issuing
e-invoices via API or Batch Upload." Use the codes from
<https://sdk.myinvois.hasil.gov.my/codes/state-codes/>.

## Correlation IDs for support tickets

Every API error response includes a `correlationId` response header. When
raising support with LHDN, **include it**. They can trace the exact request.

Log it always, even on success, for auditing.

## When in doubt: use Postman

The [Postman collection](https://sdk.myinvois.hasil.gov.my/postman/) + sample
requests lets you reproduce any call in a known-good environment. If your SDK
is sending something different from Postman and failing, the SDK is wrong.
