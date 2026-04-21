<!-- source: https://sdk.myinvois.hasil.gov.my/sample/ -->

# Sample Documents

Reference UBL 2.1 XML and JSON payloads for every document type, plus signed samples, downloadable code-table JSONs, and the official digital-signature user guide. All files live in [`samples/`](samples/).

Use these as fixtures for unit tests and integration tests, or as starting points when building your document generator.

## Signed end-to-end examples

These are fully-signed samples with a working XAdES envelope — study these when implementing signing.

| File | Format |
|---|---|
| [`one-doc-signed.xml`](samples/one-doc-signed.xml) | XML |
| [`sample-ul-invoice-2.1-signed.min.json`](samples/sample-ul-invoice-2.1-signed.min.json) | JSON |

See [`06-signature/README.md`](../06-signature/README.md) for how the signature is constructed.

## Per-document-type samples

**Version 1.0** = signature validation disabled (sandbox / dev). **Version 1.1** = signature validation enabled (production).

### Invoice

| Scenario | v1.0 XML | v1.0 JSON | v1.1 XML | v1.1 JSON |
|---|---|---|---|---|
| Basic invoice | [XML](samples/1.0-Invoice-Sample.xml) | [JSON](samples/1.0-Invoice-Sample.json) | [XML](samples/1.1-Invoice-Sample.xml) | [JSON](samples/1.1-Invoice-Sample.json) |
| Foreign currency | [XML](samples/1.0-Invoice-ForeignCurrency-Sample.xml) | [JSON](samples/1.0-Invoice-ForeignCurrency-Sample.json) | [XML](samples/1.1-Invoice-ForeignCurrency-Sample.xml) | [JSON](samples/1.1-Invoice-ForeignCurrency-Sample.json) |
| Multi-line item | [XML](samples/1.0-Invoice-MultiLineItem-Sample.xml) | [JSON](samples/1.0-Invoice-MultiLineItem-Sample.json) | [XML](samples/1.1-Invoice-MultiLineItem-Sample.xml) | [JSON](samples/1.1-Invoice-MultiLineItem-Sample.json) |
| Consolidated (monthly) | [XML](samples/1.0-Invoice-Consolidated-Sample.xml) | [JSON](samples/1.0-Invoice-Consolidated-Sample.json) | [XML](samples/1.1-Invoice-Consolidated-Sample.xml) | [JSON](samples/1.1-Invoice-Consolidated-Sample.json) |

### Credit Note, Debit Note, Refund Note

| Document type | v1.0 XML | v1.0 JSON | v1.1 XML | v1.1 JSON |
|---|---|---|---|---|
| Credit Note | [XML](samples/1.0-Credit-Note-Sample.xml) | [JSON](samples/1.0-Credit-Note-Sample.json) | [XML](samples/1.1-Credit-Note-Sample.xml) | [JSON](samples/1.1-Credit-Note-Sample.json) |
| Debit Note | [XML](samples/1.0-Debit-Note-Sample.xml) | [JSON](samples/1.0-Debit-Note-Sample.json) | [XML](samples/1.1-Debit-Note-Sample.xml) | [JSON](samples/1.1-Debit-Note-Sample.json) |
| Refund Note | [XML](samples/1.0-Refund-Note-Sample.xml) | [JSON](samples/1.0-Refund-Note-Sample.json) | [XML](samples/1.1-Refund-Note-Sample.xml) | [JSON](samples/1.1-Refund-Note-Sample.json) |

### Self-Billed (issued by buyer)

| Document type | v1.0 XML | v1.0 JSON | v1.1 XML | v1.1 JSON |
|---|---|---|---|---|
| Self-Billed Invoice | [XML](samples/1.0-Self-Billed-Invoice-Sample.xml) | [JSON](samples/1.0-Self-Billed-Invoice-Sample.json) | [XML](samples/1.1-Self-Billed-Invoice-Sample.xml) | [JSON](samples/1.1-Self-Billed-Invoice-Sample.json) |
| Self-Billed Credit Note | [XML](samples/1.0-Self-Billed-Credit-Sample.xml) | [JSON](samples/1.0-Self-Billed-Credit-Sample.json) | [XML](samples/1.1-Self-Billed-Credit-Sample.xml) | [JSON](samples/1.1-Self-Billed-Credit-Sample.json) |
| Self-Billed Debit Note | [XML](samples/1.0-Self-Billed-Debit-Sample.xml) | [JSON](samples/1.0-Self-Billed-Debit-Sample.json) | [XML](samples/1.1-Self-Billed-Debit-Sample.xml) | [JSON](samples/1.1-Self-Billed-Debit-Sample.json) |
| Self-Billed Refund Note | [XML](samples/1.0-Self-Billed-Refund-Sample.xml) | [JSON](samples/1.0-Self-Billed-Refund-Sample.json) | [XML](samples/1.1-Self-Billed-Refund-Sample.xml) | [JSON](samples/1.1-Self-Billed-Refund-Sample.json) |

## Code table data (machine-readable)

Use these as the source of truth for validating codes in your own system. They mirror the tables in [`05-code-tables/`](../05-code-tables/README.md).

- [`ClassificationCodes.json`](samples/ClassificationCodes.json)
- [`CountryCodes.json`](samples/CountryCodes.json)
- [`CurrencyCodes.json`](samples/CurrencyCodes.json)
- [`EInvoiceTypes.json`](samples/EInvoiceTypes.json)
- [`MSICSubCategoryCodes.json`](samples/MSICSubCategoryCodes.json)
- [`PaymentMethods.json`](samples/PaymentMethods.json)
- [`StateCodes.json`](samples/StateCodes.json)
- [`TaxTypes.json`](samples/TaxTypes.json)
- [`UnitTypes.json`](samples/UnitTypes.json)

## Additional Reference

- [`Digital_Signature_User_Guide.pdf`](samples/Digital_Signature_User_Guide.pdf) — official IRBM PDF walkthrough of the signing process
