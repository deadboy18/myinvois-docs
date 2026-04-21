<!-- source: https://sdk.myinvois.hasil.gov.my/api/03-get-document-types/ -->

# Get All Document Types

This API allows taxpayer's systems to retrieve list of document types published by the MyInvois System.

[Login as Intermediary System](02-login-intermediary.md)
[Get Document Type](04-get-document-type.md)

## Overview

The main function of the MyInvois System is to enable submission of the issued documents to tax authority. There are multiple types of documents supported by the solution and this API allows taxpayer ERP systems to retrieve their definitions through API call.

## Signature

This is REST based API that does not take additional URL parameters, yet it returns the list of active and deactivated document types that were once active.

Signature:
`GET /api/v1.0/documenttypes`

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

No additional URL or request body parameters are provided to this API.

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is part of a single object `result` that includes list of `DocumentType` elements.

Output list of Document Types contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| id | Number | Unique identifier of the document type | 45 |
| [invoiceTypeCode](../05-code-tables/e-invoice-types.md) | Number | The document type code from the possible values: `1`,`2`,`3`,`4`,`11`,`12`,`13`,`14` | 04 |
| description | String | Description of the document type | Invoice |
| activeFrom | Date | Date when the activity of the document type started | 2015-02-13T13:15:00Z |
| activeTo | Date | Optional: date when the activity of the document type ends | 2027-03-01T00:00:00Z |
| documentTypeVersions | Array of document type version summary objects | 1 or more array values | Â |

Array of document type version summary objects contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| id | Number | Unique identifier of the document type version | 454 |
| name | String | Unique name of the document type version | 1.0 |
| description | String | Description of the document type version | Invoice version 1.1 |
| activeFrom | Date | Date when the activity of the document type version started | 2015-02-13T13:15:00Z |
| activeTo | Date | Optional: date when the activity of the document type version ends | 2027-03-01T00:00:00Z |
| versionNumber | Decimal | Unique version number of the document type version | 1.1 |
| status | String | Status of the document type version - `draft`, `published`, `deactivated` | published |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

No custom error codes are provided by this API.

## Additional considerations

When document type list is returned to caller, they can query more detailed information about [document type](04-get-document-type.md) or [document type version](05-get-document-type-version.md) using returned internal identifiers.

[Login as Intermediary System](02-login-intermediary.md)
[Get Document Type](04-get-document-type.md)


---
