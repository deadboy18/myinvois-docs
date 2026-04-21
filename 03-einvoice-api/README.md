<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/ -->

# e-Invoice APIs

Definition of the API that taxpayer systems can leverage to deal with the actual Invoice documents.

[Platform API](../02-platform-api/README.md)

## Validate Taxpayer's TIN

This API allows taxpayer's ERP system to validate specific Tax Identification Number (TIN) before adding this number to an invoice and issuing the invoice.

[Read more](01-validate-taxpayer-tin.md)

## Submit Documents

This API allows taxpayer to submit one or more signed documents to MyInvois System.

[Read more](02-submit-documents.md)

## Cancel Document

This API allows issuer to cancel previously issued document, either self-induced cancellation or by accepting a rejection request made by the buyer.

[Read more](03-cancel-document.md)

## Reject Document

This API allows a buyer that received an invoice to reject it and request the supplier to cancel it.

[Read more](04-reject-document.md)

## Get Recent Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters. This API will only return documents that are issued within the last 31 days.

[Read more](05-get-recent-documents.md)

## Get Submission

This API returns information on documents submitted during a single submission by taxpayer.

[Read more](06-get-submission.md)

## Get Document

This API allows taxpayers to retrieve document source in XML or JSON format along with the additional tax authority metadata.

[Read more](07-get-document.md)

## Get Document Details

This API allows taxpayers to retrieve a single document's full details including validation results.

[Read more](08-get-document-details.md)

## Search Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters.

[Read more](09-search-documents.md)

## Search Taxpayer's TIN

This API allows taxpayer's ERP system to search for a specific Tax Identification Number (TIN) using the supported search parameters. The supported search parameters are either:  
  
 1) Taxpayer Name or   
 2) taxpayer, ID Type, ID Value or   
 3) ID Type, ID Value or   
 4) ID Type, ID Value, File Type, or   
 5) If all parameters are provided then the search would use an AND operator to make sure the result found matches all search parameters provided.

[Read more](10-search-taxpayer-tin.md)

## Taxpayer's QR Code

This API allows taxpayerâs ERP system to search and retrieve the information for a specific Taxpayer based on the Base64 formatted string obtained from scanning the respective QR code.

[Read more](11-qr-code.md)

[Platform API](../02-platform-api/README.md)


---
