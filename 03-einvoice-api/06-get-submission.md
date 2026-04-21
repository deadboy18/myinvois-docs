<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/06-get-submission/ -->

# Get Submission

This API returns information on documents submitted during a single submission by taxpayer.

[Get Recent Documents](05-get-recent-documents.md)
[Get Document](07-get-document.md)

## Overview

This API allows caller to get details of a single submission to check its processing status after initially [submitting it](02-submit-documents.md) and getting back unique submission identifier.

This API is available to submitter only as it might contain documents issued to multiple receivers.

## Signature

This is REST based API that takes unique ID of the submission as URL parameter and returns details of the submission and summary information of the documents part of the submission.

In view that one submission might contains large number of documents, list of the documents returned by this API is paged.

Signature:
`GET /api/v1.0/documentsubmissions/{submissionUid}?pageNo={pageNo}&pageSize={pageSize}`

## Rate Limit

To optimize the use of our APIs, a rate limit of 300 Requests Per Minute (RPM)/ Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| submissionUid | String | Unique ID of the document submission to retrieve. | HJSD135P2S7D8IU | Mandatory |
| pageNo | Number | Optional: number of the page to retrieve | 3 | Optional |
| pageSize | Number | Optional: number of the documents to retrieve per page. Page size cannot exceed system configured maximum page size for this API [100] | 20 | Optional |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is a single object containing the attributes as defined in the table:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| submissionUid | String | Unique document submission ID in e-Invoice | HJSD135P2S7D8IU |
| documentCount | Number | Total count of documents in submission that were accepted for processing | 234 |
| dateTimeReceived | DateTime | The date and time when the submission was received by e-Invoice. | 2015-02-13T14:20:10Z |
| overallStatus | String | Overall status of the batch processing. Values: `in progress`, `valid`, `partially valid`, `invalid` | valid |
| documentSummary | [Document Summary[]](#document-summary) | List of the retrieved batch documents in current page. | See structure. |

#### Document Summary

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
| dateTimeIssued | DateTime | The date and time when the document was issued in the UTC format. | 2015-02-13T13:15:10Z |
| dateTimeReceived | DateTime | The date and time when the document was submitted in the UTC format. | 2015-02-13T13:15:10Z |
| dateTimeValidated | DateTime | The date and time when the document passed all validations and moved to the valid state. | 2015-02-13T13:15:10Z |
| totalExcludingTax | Decimal | Total sales amount of the document in MYR. | 10.10 |
| totalDiscount | Decimal | Total discount amount of the document in MYR. | 50.00 |
| totalNetAmount | Decimal | Total net amount of the document in MYR. | 100.70 |
| totalPayableAmount | Decimal | Total amount of the document in MYR. | 124.09 |
| status | String | Status of the document - `Submitted`, `Valid`, `Invalid`, `Cancelled` | Valid |
| cancelDateTime | Date | Refer to the document cancellation that has been initiated by the taxpayer âissuerâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| rejectRequestDateTime | Date | Refer to the document rejection request that has been initiated by the taxpayer âreceiverâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| documentStatusReason | String | Mandatory: Reason of the cancellation or rejection of the document. | Examples of reasons: Wrong buyer details or Wrong invoice details or any other reasons as appropriate |
| createdByUserId | String | User created the document. Can be ERP ID or User Email | 1XXXXXXXX00:9e21b10c-41c4-9323-c590-95abcb6e4e4d  general.ams@supplier.com |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

## Additional considerations

Maximum page size allowed is defined by e-Invoice system administrators.

This API is available to issuers that are querying their submissions. This also includes authorized intermediaries (service providers) who submitted the documents on behalf of the taxpayer they represent.

## Notes

**Important !**

â¢ Use the âGet Submission APIâ to monitor the status of submissions and documents. Ensure a 3-5 second interval between requests to avoid system throttling, which may occur if too many requests are made for the same submission. These limits are based on each client ID and will be linked to the specific ERP system using the APIs.

â¢ To prevent system overload and ensure optimal functionality, it is recommend to adjust the âGet Submissionâ API calls to a polling interval of 3-5 seconds, with a maximum rate of 300 request per minute (RPM).

â¢ Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Get Recent Documents](05-get-recent-documents.md)
[Get Document](07-get-document.md)


---
