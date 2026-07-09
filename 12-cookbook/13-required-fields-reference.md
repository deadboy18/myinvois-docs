# 13 — Required Fields Reference (55 Fields)

**Source:** IRBM e-Invoice General Guideline v4.7 (7 July 2026) — Appendix 1 & 2

Every e-Invoice must contain at least 55 fields grouped into 8 categories.
Some are always mandatory, some conditionally mandatory, some optional.
This is the definitive reference.

## Categories at a glance

1. **Parties** — who's transacting
2. **Supplier's Details** — registration, contact
3. **Buyer's Details** — same for buyer
4. **Address** — both supplier and buyer
5. **Contact Number** — both parties
6. **Invoice Details** — invoice-level metadata
7. **Products / Services** — line-level data
8. **Payment Info** — how it's being paid

Plus **Annexure fields** (Appendix 2) for import/export and
different-shipping-recipient cases.

## Full field list (Appendix 1 — the 55)

### Parties

| # | Field | Description |
|---|---|---|
| 1 | Supplier's Name | Business or individual issuing the e-Invoice |
| 2 | Buyer's Name | Recipient of the e-Invoice |

### Supplier's details

| # | Field | Description |
|---|---|---|
| 3 | Supplier's TIN | Assigned by IRBM |
| 4 | Supplier's Registration / Identification / Passport Number | Business: BRN (12-digit from SSM) • Malaysian individual: MyKad/MyTentera • Non-Malaysian: Passport / MyPR / MyKAS |
| 5 | Supplier's SST Registration Number | **Mandatory if SST-registered**, not applicable otherwise |
| 6 | Supplier's Tourism Tax Registration Number | **Mandatory if tourism-tax-registered** (hotel operators, online travel operators) |
| 7 | Supplier's Email | **Optional** |
| 8 | Supplier's MSIC Code | 5-digit code for business nature and activity |
| 9 | Supplier's Business Activity Description | Free text describing the business |

### Buyer's details

| # | Field | Description |
|---|---|---|
| 10 | Buyer's TIN | Assigned by IRBM |
| 11 | Buyer's Registration / Identification / Passport Number | Same rules as supplier #4 |
| 12 | Buyer's SST Registration Number | **Mandatory if SST-registered** |
| 13 | Buyer's Email | **Optional** |

### Address

| # | Field | Description |
|---|---|---|
| 14 | Supplier's Address | Registered / business / residential |
| 15 | Buyer's Address | Registered / business / residential |

### Contact number

| # | Field | Description |
|---|---|---|
| 16 | Supplier's Contact Number | Phone (office / mobile / fax) |
| 17 | Buyer's Contact Number | Phone (office / mobile / fax) |

### Invoice details

| # | Field | Description |
|---|---|---|
| 18 | e-Invoice Version | Current version (e.g. `1.0`, `1.1`) |
| 19 | e-Invoice Type | Invoice, credit note, debit note, refund note, etc. |
| 20 | e-Invoice Code / Number | Your internal tracking reference (e.g. `INV12345`) |
| 21 | Original e-Invoice Reference Number | **Mandatory where applicable** — IRBM UUID of the original invoice when issuing credit / debit / refund note |
| 22 | e-Invoice Date and Time | Current UTC datetime of issuance |
| 23 | Issuer's Digital Signature | Electronic signature using issuer's or service provider's digital certificate |
| 24 | Invoice Currency Code | ISO 4217 (e.g. `MYR`, `USD`) |
| 25 | Currency Exchange Rate | **Mandatory if non-MYR** — rate converting foreign currency to MYR |
| 26 | Frequency of Billing | **Optional** — Daily / Weekly / Biweekly / Monthly / Bimonthly / Quarterly / Half-yearly / Yearly / Others |
| 27 | Billing Period | **Optional** — e.g. `2025-01-01 – 2025-01-31` |

### Products / Services (line level, plus some invoice-level totals)

| # | Field | Description |
|---|---|---|
| 28 | Classification | General expenses / medical / donations / self-billed / etc. |
| 29 | Description of Product or Service | Free text |
| 30 | Unit Price | Price per single unit |
| 31 | Tax Type | Sales tax / service tax / tourism tax / etc. — line and invoice level |
| 32 | Tax Rate | **Mandatory where applicable** — % or prevailing rate |
| 33 | Tax Amount | Amount of tax payable — line and invoice level |
| 34 | Details of Tax Exemption | **Mandatory if exemption applied** — exemption certificate number, gazette order, etc. |
| 35 | Amount Exempted from Tax | **Mandatory if exemption applied** — total amount exempted |
| 36 | Subtotal | Amount per line item, **excluding** tax and charges, line level only |
| 37 | Total Excluding Tax | Amount payable (incl. discounts/charges) **excluding** tax — line and invoice level |
| 38 | Total Including Tax | Amount payable **including** tax — invoice level only |
| 39 | Total Net Amount | **Optional** — sum of net amount, invoice level only |
| 40 | Total Payable Amount | Total incl. tax and rounding, excl. prepayments — invoice level only |
| 41 | Rounding Amount | **Optional** — invoice level only |
| 42 | Total Taxable Amount Per Tax Type | **Optional** — invoice level only |
| 43 | Quantity | **Optional** — units of product / service |
| 44 | Measurement | **Optional** — e.g. kg, box, litre, hour |
| 45 | Discount Rate | **Optional** — % discount, line and invoice level |
| 46 | Discount Amount | **Optional** — absolute discount, line and invoice level |
| 47 | Fee / Charge Rate | **Optional** — % or rate, line and invoice level |
| 48 | Fee / Charge Amount | **Optional** — absolute, line and invoice level |

### Payment info (all optional)

| # | Field | Description |
|---|---|---|
| 49 | Payment Mode | Cash / cheque / bank transfer / credit card / debit card / e-Wallet |
| 50 | Supplier's Bank Account Number | For Buyer to make payment |
| 51 | Payment Terms | Timing and method |
| 52 | Prepayment Amount | Amount already prepaid |
| 53 | Prepayment Date | Date of prepayment received |
| 54 | Prepayment Reference Number | Identifier tracing the prepayment |
| 55 | Bill Reference Number | Supplier's internal billing reference |

## Annexure fields (Appendix 2)

These are **additional** fields required or optional under specific
circumstances.

### Mandatory Annexure fields — Import / Export of goods

| Field | Description |
|---|---|
| Reference Number of Customs Form No.1, 9, etc. | Unique identifier on the Declaration of Goods Imported |

### Optional Annexure fields — Shipping Recipient (different from buyer)

When goods are shipped to someone other than the buyer:

| Field | Description |
|---|---|
| Shipping Recipient's Name | |
| Shipping Recipient's Address | |
| Shipping Recipient's TIN | Assigned by IRBM |
| Shipping Recipient's Registration / Identification / Passport Number | Same format rules as supplier/buyer |

### Optional Annexure fields — Import / Export

| Field | Description |
|---|---|
| Incoterms | International trade rules (e.g. FOB, CIF, DDP) |
| Product Tariff Code | HS code |
| Free Trade Agreement (FTA) Information | **Export only, if applicable** |
| Authorisation Number for Certified Exporter | e.g. ATIGA number — **Export only, if applicable** |
| Reference Number of Customs Form No.2 | Declaration of Goods Exported |
| Country of Origin | Goods origin |
| Details of other charges | Additional charges with amount |

## Mandatoriness quick reference

| Always mandatory | Conditionally mandatory | Always optional |
|---|---|---|
| Supplier/Buyer Name | Supplier/Buyer SST Number (if SST-registered) | Supplier/Buyer Email |
| Supplier/Buyer TIN | Supplier Tourism Tax Number (if tourism tax registrant) | Frequency of Billing |
| Supplier/Buyer Reg/ID/Passport Number | Original e-Invoice Reference Number (for CN/DN/RN) | Billing Period |
| Supplier/Buyer Address | Currency Exchange Rate (non-MYR only) | Quantity / Measurement |
| Supplier/Buyer Contact Number | Tax Rate (where applicable) | Discount rate / amount |
| Supplier MSIC Code | Details of Tax Exemption (if applied) | Fee / charge rate / amount |
| Supplier Business Activity Description | Amount Exempted from Tax (if applied) | Payment mode / terms |
| e-Invoice Version, Type, Code, Date/Time | | Prepayment fields |
| Issuer's Digital Signature | | Bank account / Bill Ref number |
| Invoice Currency Code | | Rounding / Net Amount / Total Per Tax Type |
| Classification, Description, Unit Price | | |
| Tax Type, Tax Amount | | |
| Subtotal, Total Excluding Tax, Total Including Tax, Total Payable Amount | | |

## Common field-specific gotchas

- **TIN format** — see [05-common-pitfalls.md § TIN format rules](05-common-pitfalls.md#tin-format-rules). `IG` for individuals; remove leading / add trailing zeros for non-individuals.
- **SST number** — use literal `NA` if not SST-registered. Only `-` and `;` allowed as special chars. Max 35 chars. Up to 2 numbers separated by `;`.
- **TTX number** — use literal `NA` if not tourism-tax-registered. Only `-` allowed. Max 17 chars.
- **Email** — RFC 5321 + 5322 compliant. Max 320 chars. Blank OK.
- **Phone** — digits + optional leading `+`. No spaces/dashes/parens. Min 8, max 20 chars.
- **MSIC code** — exactly 5 digits. See <https://sdk.myinvois.hasil.gov.my/codes/msic/>.
- **Currency code** — ISO 4217. See <https://sdk.myinvois.hasil.gov.my/codes/currencies/>.
- **Tax Type** — see <https://sdk.myinvois.hasil.gov.my/codes/tax-types/>. Includes exemption code `E`.
- **Classification** — see <https://sdk.myinvois.hasil.gov.my/codes/classification-codes/>.
- **State code `00` is retired** — use real state codes.
- **`IssueDate` / `IssueTime`** — UTC with `Z` suffix, within the last 72 hours.

## Reference

- [IRBM e-Invoice General Guideline v4.7 (7 July 2026) — Appendix 1 & 2](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/)
- [Document Types (schema)](https://sdk.myinvois.hasil.gov.my/types/)
- [Code tables](https://sdk.myinvois.hasil.gov.my/codes/)
- [Field validation rules — this cookbook](06-field-validation-rules.md)
