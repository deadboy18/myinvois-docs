<!-- source: https://sdk.myinvois.hasil.gov.my/document-validation-rules/ -->

# Document validation rules

Page describes the validation that is getting applied on every submitted document.

# Document validation

There are different kind of validations that get executed on the submitted document, below is the list of validators that run on submitted documents.

1. Structure Validator
2. Core Fields Validator
3. Signature Validator
4. Taxpayer Validator
5. Referenced Documents Validator
6. Code Validator
7. Duplicate Document Validator
8. Currency Validator

## Structure Validator

Validator that enables validation of the submitted document type version to check its structure matching what is required by the document type version. Validator supports validating both [XML](https://docs.oasis-open.org/ubl/UBL-2.1.html) and [JSON](https://docs.oasis-open.org/ubl/UBL-2.1-JSON/v2.0/cnd01/UBL-2.1-JSON-v2.0-cnd01.html) based documents. We support [UBL 2.1](https://docs.oasis-open.org/ubl/UBL-2.1.html) standards for Invoice documents.

## Core Fields Validator

Validator that is used to validate that the document contains at least the main fields that any document should have to be processed by the system

## Signature Validator

Validator that is responsible for validating the submitted document signature

## Taxpayer Validator

Validator that is used to validate if the taxpayers referenced in the submitted document are valid at the date of issuance of the document. It also performs checks on the issuer that require asynchronous processing and cannot be done synchronously in core fields validator.

## Referenced Documents Validator

Validator that is used to perform validation of submitted credit notes, debit notes and refund notes to make sure that the documents being referenced to are valid invoices at the moment of issuance of a new credit note, debit note and refund note.

## Code Validator

Code validator is used to enable validation of various codes used in submitted documents to make sure it is referenced to valid codes, for example, currency codes and tax types. Refer to the [Code](../05-code-tables/README.md) tables for more information.

## Duplicate Document Validator

Validator that tries to find documents submitted recently that are similar / identical to the document that is being / has been processed to identify cases where the same document (often) by mistake has been submitted twice or even more than twice

## Currency Validator

Validator is used to ensure correct usage of currency codes and exchange rates.


---
