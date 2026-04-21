<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/07-get-document/ -->

# Get Document

This API allows taxpayers to retrieve document source in XML or JSON format along with the additional tax authority metadata.

[Get Submission](06-get-submission.md)
[Get Document Details](08-get-document-details.md)

## Overview

This API allows caller to get full details of the document when requested by unique ID assigned to document by MyInvois System.

Details include original XML or JSON submission and additional tax authority metadata i.e., additional ID, and current status. As the document is submitted in XML or JSON, this API returns document information also in either XML or JSON.

Document with invalid status will not be returned. Details of the invalid document can be fetched by [Get Document Details](08-get-document-details.md) API.

## Signature

This is REST based API that takes unique ID of the document as URL parameter and returns document details that were received from the issuer and additional details added after submission and validation of the document by the tax authority.

Signature:
`GET /api/v1.0/documents/{uuid}/raw`

## Rate Limit

To optimize the use of our APIs, a rate limit of 60 Requests Per Minute (RPM)/ Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call, but the content type parameter can define the expected output format - JSON or XML.

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| uuid | String | Unique ID of the document to retrieve. | F9D425P6DS7D8IU | Mandatory |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is a single object of type `DocumentExtended`.

Document Extended contains:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| uuid | String | Unique document ID in e-Invoice | F9D425P6DS7D8IU |
| submissionUid | String | Unique ID of the submission the document was part of | HJSD135P2S7D8IU |
| longId | String | Unique long temporary Id that can be used to query document data anonymously. The long id will be returned only for valid documents | LIJAF97HJJKH 8298KHADH0990 8570FDKK9S2LSIU HB377373 |
| internalId | String | Internal ID used in submission for the document | PZ-234-A |
| typeName | String | Unique name of the document type that can be used in submission of the documents. | invoice |
| typeVersionName | String | Name of the document type version within the document type that can be used in document submission to identify document type version being submitted | 1.0 |
| issuerTin | String | TIN of issuer | C2584563200 |
| issuerName | String | Issuer company name | AMS Setia Jaya Sdn. Bhd. |
| receiverId | String | Optional: receiver registration number (can be national ID or foreigner ID). | 201901234567 |
| receiverName | String | Optional: receiver name (can be company name or personâs name) | AMS Setia Jaya Sdn. Bhd. |
| dateTimeIssued | DateTime | The date and time when the document was issued in the UTC timeformat. | 2015-02-13T13:15:10Z |
| dateTimeReceived | DateTime | The date and time when the document was submitted in the UTC timeformat. | 2015-02-13T13:15:10Z |
| dateTimeValidated | DateTime | The date and time when the document passed all validations and moved to the valid state. | 2015-02-13T13:15:10Z |
| totalExcludingTax | Decimal | Total sales amount of the document in MYR. | 10.10 |
| totalDiscount | Decimal | Total discount amount of the document in MYR. | 50.00 |
| totalNetAmount | Decimal | Total net amount of the document in MYR. | 100.70 |
| totalPayableAmount | Decimal | Total amount of the document in MYR. | 124.09 |
| status | String | Status of the document - `Submitted`, `Valid`, `Cancelled` | Valid |
| cancelDateTime | Date | Refer to the document cancellation that has been initiated by the taxpayer âissuerâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| rejectRequestDateTime | Date | Refer to the document rejection request that has been initiated by the taxpayer âreceiverâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| documentStatusReason | String | Mandatory: Reason of the cancellation or rejection of the document. | Examples of reasons: Wrong buyer details or Wrong invoice details or any other reasons as appropriate |
| createdByUserId | String | User created the document. Can be ERP ID or User Email | 1XXXXXXXX00:9e21b10c-41c4-9323-c590-95abcb6e4e4d  general.ams@supplier.com |
| document | [Document](../04-document-types/README.md) | Document string containing the original submitted raw document. Document with âinvalidâ status will not be returned. Details of the invalid document can be fetched by [Get Document Details](08-get-document-details.md) API. | Â |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

## Additional considerations

To get the unique ID that will be used to call this API, taxpayer system can first call API returning list of [recent documents](05-get-recent-documents.md).

Receiver of the documents can retrieve documents that are in âValidâ or âCancelledâ status. If document exists, and is issued to given receiver, but status is âsubmittedâ or âinvalidâ, not found code will be returned.

Issuer of the documents can retrieve documents in any status. The document validation or public URL can be created using below -

```
{envbaseurl}/uuid-of-document/share/longid
```

where {envbaseurl} need to be replaced with MyInvois portal Base URL

## Notes

**Important !**

â¢ Repeated invocation of this API to retrieve document details can lead to unnecessary system load and inefficiency. Instead, maintain a local copy of the documents. This ensures documents can be accessed when needed without frequent API calls, thereby improving system performance and reducing overhead.

â¢ Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Get Submission](06-get-submission.md)
[Get Document Details](08-get-document-details.md)


---
