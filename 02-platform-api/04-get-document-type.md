<!-- source: https://sdk.myinvois.hasil.gov.my/api/04-get-document-type/ -->

# Get Document Type

This API allows taxpayer's ERP system to retrieve the details of single document type that contains structure definitions of the document.

[Get All Document Types](03-get-document-types.md)
[Get Document Type Version](05-get-document-type-version.md)

## Overview

This API allows taxpayer ERP system to retrieve the details of single document type that contains structure definitions of the document.

## Signature

This is REST based API that takes unique ID of the document type as URL parameter and returns document type object with additional details.

Signature:
`GET /api/v1.0/documenttypes/{id}`

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| id | Number | Unique ID of existing document type | 45 | Mandatory |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is a single object of type `DocumentType`.

Output list of Document Types contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| id | Number | Unique identifier of the document type | 45 |
| [invoiceTypeCode](../05-code-tables/e-invoice-types.md) | Number | The document type code from the possible values: `01`,`02`,`03`,`04`,`11`,`12`,`13`,`14` | 04 |
| description | String | Description of the document type | Invoice |
| activeFrom | Date | Date when the activity of the document type started | 2015-02-13T13:15:00Z |
| activeTo | Date | Optional: date when the activity of the document type ends | 2027-03-01T00:00:00Z |
| documentTypeVersions | Array of document type version summary objects | 1 or more array values | Â |
| workflowParameters | Array of workflow parameters | 1 or more array values | Â |

Array of workflow parameter contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| id | Number | Unique id of the parameter for this document type | 124 |
| parameter | String | Name of the parameter.  Can be submissionDuration or cancellationDuration or rejectionDuration | Rejection time limit in hours |
| value | Number | Parameter value | 72 |
| activeFrom | Date | Date when the activity of the parameter started | 2015-02-13T13:15:00Z |
| activeTo | Date | Optional: date when the activity of the parameter ended | 2027-03-01T00:00:00Z |

Array of document type version summary objects contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| id | Number | Unique identifier of the document type version | 454 |
| name | String | Unique name of the document type version | 1.0 |
| description | String | Description of the document type version | Invoice version 1.0 |
| activeFrom | Date | Date when the activity of the document type version started | 2015-02-13T13:15:00Z |
| activeTo | Date | Optional: date when the activity of the document type version ends | 2027-03-01T00:00:00Z |
| versionNumber | Decimal | Unique version number of the document type version | 1.1 |
| status | String | Status of the document type version - `draft`, `published`, `deactivated` | published |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

No custom error codes are provided by this API.

## Additional considerations

To get the ID used to call this API, taxpayer system needs to first call API returning list of the [document types](03-get-document-types.md).

[Get All Document Types](03-get-document-types.md)
[Get Document Type Version](05-get-document-type-version.md)


---
