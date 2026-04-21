# 06 — Field Validation Rules

Every field in a MyInvois document has specific format rules. Violating them
produces `CF###` errors from the Core Fields Validator. This table is the
definitive reference, pulled from the official FAQ.

## Supplier / buyer information fields

| # | Field | Rules |
|---|---|---|
| i | **Supplier / Buyer Name** | Max **300** characters |
| ii | **Email address** | Must follow valid format per **RFC 5321 & RFC 5322**  •  Max **320** characters  •  **No spaces** between characters  •  May be blank if no email |
| iii | **Phone number** | Min **8**, max **20** characters  •  Only **optional `+`** allowed at the front  •  **No spaces** between digits |
| iv | **Address line 1 / 2 / 3** | Max **150** characters per line |
| v | **City name** | Max **50** characters |
| vi | **Postal code** | If country is Malaysia: max **5-digit numbers only**  •  Otherwise: max **50 alphanumeric** incl. special characters |
| vii | **SST number** | Max **35** characters  •  Only `-` (dash) and `;` (semicolon) allowed as special chars  •  Up to **2 SST numbers** separated by `;`  •  Use literal `NA` if no SST |
| viii | **TTX number** | Max **17** characters  •  Only `-` allowed as special char  •  Use literal `NA` if no TTX |

## Phone number — the details

The single most common `CF` error. Exact rules:

### Allowed
```
60123456789       ← country code included, no +
+60123456789      ← country code with +
0123456789        ← local Malaysian mobile
0312345678        ← local Malaysian landline
65...             ← Singapore country code (foreign country codes accepted)
```

### Rejected (causes CF401 / CF403 / CF410 / CF364)
```
+60 12-345 6789         ← contains spaces and dashes
(03) 1234 5678          ← contains parentheses and spaces
03-12345678             ← contains dash
+60 (12) 345-6789       ← mixed special chars
.                       ← period / dot literal
@home                   ← any non-phone text
```

### Normalization snippet

```javascript
function normalizeMalaysianPhone(raw) {
  if (!raw) return null;
  // Strip everything that's not + or digit
  let cleaned = raw.replace(/[^\d+]/g, '');
  // + can only appear at position 0
  if (cleaned.indexOf('+') > 0) {
    cleaned = cleaned.replace(/\+/g, '');
    cleaned = '+' + cleaned;
  }
  // Validate length
  if (cleaned.length < 8 || cleaned.length > 20) return null;
  return cleaned;
}
```

## Email — the details

- Must validate per RFC 5321 (transport) + RFC 5322 (message format).
- Only one email per field. Multiple comma-separated addresses are **not** supported.
- If the customer has no email, **leave it blank** (don't put `NA`, don't put
  a placeholder).
- Some portals (like ABSS) advise entering a **`;`** (semicolon) for no-email
  customers — that's a portal-specific workaround, not a MyInvois requirement.
- Max field length is 320 characters (RFC 5321 upper bound).

### Validation

Use a real RFC 5322 library if you can. Regex-only validators will reject
valid addresses (e.g. with `+` suffixes, IP-literal domains). A pragmatic
middle ground:

```python
import re

# Permissive but catches obvious bad formats
EMAIL_RE = re.compile(
    r"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$"
)

def email_ok(email: str) -> bool:
    if not email:
        return True  # blank is allowed
    if len(email) > 320:
        return False
    if ' ' in email:
        return False
    return bool(EMAIL_RE.match(email))
```

## Postal code — Malaysian vs foreign

| Country | Rules |
|---|---|
| Malaysia (`MY`) | **5 digits exactly**, numeric only. Example: `50000` |
| Any other country | Up to 50 alphanumeric chars, special chars allowed. Example: `SW1A 1AA`, `10017-1234` |

If you leave postal code blank for a Malaysian address, the validator rejects.

## SST number

Examples:

```text
Single SST:         A01-1234-56789012
Two SSTs:           A01-1234-56789012; B02-5678-90123456
No SST:             NA
```

- Max 35 characters (includes the `;` separator and spaces if you use them — but
  better to not use spaces).
- Only `-` and `;` allowed as special chars. Anything else (e.g. `/`, `.`) fails.

## TTX number (Tourism Tax)

Examples:

```text
Single TTX:    T12-3456789012345
No TTX:        NA
```

- Max 17 characters.
- Only `-` allowed as special char.
- Cannot have multiple TTX numbers (unlike SST).

## Address lines

- Up to **3 lines** supported.
- Each line max 150 chars.
- If the address is short, just use Line 1 and leave 2 and 3 empty.
- Don't concatenate the whole address into Line 1 if it would exceed 150 chars —
  split across lines.

## City & state

- City name: max 50 characters.
- State: use the code from <https://sdk.myinvois.hasil.gov.my/codes/state-codes/>.
- **State code `00` is retired** (as of a mid-2024 update). Use an actual state
  code.

## Invoice ID / number

- **No character restrictions** — the system doesn't limit special characters in
  the invoice code or number.
- This is the one place you have freedom. Use your existing invoice numbering.
- Length limit is imposed by the UBL schema, practically ~50 chars safe.

## Negative values

- Allowed in `Document Submission`. Useful for:
  - Returns / refunds
  - Adjustment line items
  - Discounts represented as negative
- The document still has to balance (sum of lines = totals).

## Multiple references in credit / debit / refund notes

- Multiple **original** invoice UUIDs referenced from a single credit/debit/refund note:
- Must use **multiple** `<cac:InvoiceDocumentReference>` blocks, not concatenated strings.
- See the FAQ section "How can I submit a single Credit / Debit / Refund Note
  that includes multiple Original e-Invoice Reference Numbers" for XML/JSON
  examples.

## Multiple tax types per line

- Repeat the `<cac:TaxSubtotal>` element within the line item's `<cac:TaxTotal>`.
- Do **not** split into multiple lines just to handle multiple tax types.

## Quick validation checklist

Before submission, confirm:

- [ ] All phone numbers are digits + optional leading `+`, no spaces/dashes/parens
- [ ] All emails validate as RFC 5321/5322 or are blank
- [ ] Malaysian postal codes are exactly 5 digits
- [ ] SST / TTX fields contain real numbers or literal `NA`
- [ ] No field exceeds its character limit
- [ ] Invoice ID is populated
- [ ] All TINs in supplier/buyer/delivery parties follow the [format rules](05-common-pitfalls.md#tin-format-rules)
- [ ] `IssueDate` + `IssueTime` is UTC with `Z` suffix and within the last 72 hours
- [ ] All classification codes, currency codes, tax types, MSIC codes are from the current SDK code tables
- [ ] No state code `00` anywhere

## Reference

- [Official FAQ → Supplier/Buyer Field Validation](https://sdk.myinvois.hasil.gov.my/faq/#what-are-the-supplierbuyer-information-field-validation)
- [Document Types](https://sdk.myinvois.hasil.gov.my/types/)
- [Code tables](https://sdk.myinvois.hasil.gov.my/codes/)
