<!-- source: https://sdk.myinvois.hasil.gov.my/types/ -->

# Types

The section contains the definitions of the document structures that APIs can operate within the current version of the MyInvois System.  
The solution will support UBL 2.1 standards and would support both JSON and XML as described here [UBL-2.1](http://docs.oasis-open.org/ubl/UBL-2.1.html) and here [UBL-2.1-JSON-v2.0-cnd01.](https://docs.oasis-open.org/ubl/UBL-2.1-JSON/v2.0/cnd01/UBL-2.1-JSON-v2.0-cnd01.html)  
 Please ensure to follow the right sequence of elements to match UBL 2.1 schema guidance.

## Invoice

Invoice is a commercial document issued by Supplier to itemise and record a transaction with Buyer.

* [Invoice v1.0](invoice-v1-0.md)

  v1.0 of the Invoice document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Invoice v1.1](invoice-v1-1.md)

  v1.1 of the Invoice document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Credit Note

Credit note is the document issued by Suppliers to correct errors, apply discounts, or account for returns in a previously issued e-Invoice with the purpose of reducing the value of the original e-Invoice. This is used in situations where the reduction of the original e-Invoice does not involve return of monies to the Buyer.

* [Credit Note v1.0](credit-note-v1-0.md)

  v1.0 of the Credit Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Credit Note v1.1](credit-note-v1-1.md)

  v1.1 of the Credit Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Debit Note

Debit note is the document issued to indicate additional charges on a previously issued e-Invoice.

* [Debit Note v1.0](debit-note-v1-0.md)

  v1.0 of the Debit Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Debit Note v1.1](debit-note-v1-1.md)

  v1.1 of the debit Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Refund Note

Refund note is the document issued by a Supplier to confirm the refund of the Buyer's payment. This is used in situations where there is a return of monies to the Buyer.

* [Refund Note v1.0](refund-note-v1-0.md)

  v1.0 of the Refund Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Refund Note v1.1](refund-note-v1-1.md)

  v1.1 of the refund Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Self-Billed Invoice

There are certain circumstances where another party (other than the Supplier) will be allowed to issue a self-billed e-Invoice. Kindly refer to Section 8 of the e-Invoice Specific Guideline where self-billed e-Invoice will be allowed.  
  
Self-Billed Invoice refers to the initial self-billed e-Invoice that will be issued by the Buyer.

* [Self-Billed Invoice v1.0](self-billed-invoice-v1-0.md)

  v1.0 of the Self-Billed Invoice ddocument type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Self-Billed Invoice v1.1](self-billed-invoice-v1-1.md)

  v1.1 of the Self-Billed Invoice document type is the initial version of the Invoices supported by the MyInvois System

## Self-Billed Credit Note

Self-Billed Credit Note is issued by Buyers to correct errors, apply discounts, or account for returns in a previously issued Self-Billed e-Invoice with the purpose of reducing the value of the original Self-Billed e-Invoice. This is used in situations where the reduction of the original Self-Billed e-Invoice does not involve return of monies to the Buyer.

* [Self-Billed Credit Note v1.0](self-billed-credit-note-v1-0.md)

  v1.0 of the Self-Billed Credit Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Self-Billed Credit Note v1.1](self-billed-credit-note-v1-1.md)

  v1.1 of the Self-Billed Credit Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Self-Billed Debit Note

Self-Billed Debit Note is the document issued by Buyers to indicate additional charges on a previously issued Self-Billed e-Invoice.

* [Self-Billed Debit Note v1.0](self-billed-debit-note-v1-0.md)

  v1.0 of the Self-Billed Debit Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Self-Billed Debit Note v1.1](self-billed-debit-note-v1-1.md)

  v1.1 of the Self-Billed Debit Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.

## Self-Billed Refund Note

Self-Billed Refund Note is the document issued by Buyers to confirm the refund of the Buyer's payment. This is used in situations where there is a return of monies to the Buyer.

* [Self-Billed Refund Note v1.0](self-billed-refund-note-v1-0.md)

  v1.0 of the Self-Billed Refund Note document type structure is maintained as v1.1, the only difference is that signature validation is disabled on this version. This version will be deprecated and replaced by version 1.1 at a later date that will be announced later.
* [Self-Billed Refund Note v1.1](self-billed-refund-note-v1-1.md)

  v1.1 of the Self-Billed Refund Note document type structure is maintained as v1.0, the only difference is that signature validation is enabled on this version.


---
