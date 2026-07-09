<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/05-get-recent-documents/ -->

# Get Recent Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters. This API will only return documents that are issued within the last 31 days.

[Reject Document](04-reject-document.md)
[Get Submission](06-get-submission.md)

## Overview

Get recent documents API allows users to query the system and return list of documents that have been recently received or issued .

System limits the number of the documents that can be received in one request by implementing paging mechanism for this API and also by not allowing to request documents that are issued more than 31 days ago.

List is ordered based on registration date of the submission descending.

## Signature

This is REST based API that takes optional URL parameters to enable paging.

Signature:
`GET /api/v1.0/documents/recent?pageNo={pageNo}&pageSize={pageSize}&submissionDateFrom={submissionDateFrom}&submissionDateTo={submissionDateTo}&issueDateFrom={issueDateFrom}&issueDateTo={IssueDateTo}&InvoiceDirection={InvoiceDirection}&status={status}&documentType={documentType}&receiverIdType={receiverIdType}&receiverId={receiverId}&issuerIdType={issuerIdType}&issuerId={issuerId}&receiverTin={receiverTin}&issuerTin={issuerTin}`

## Rate Limit

To optimize the use of our APIs, a rate limit of 12 Requests Per Minute (RPM)/ Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

URL parameters accepted:

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| pageNo | Number | Optional: number of the page to retrieve. Typically this parameter value is derived from initial parameter less call when caller learns total amount of page of certain size | 3 | Optional |
| pageSize | Number | Optional: number of the documents to retrieve per page. Page size cannot exceed system configured maximum page size for this API | 20 | Optional |
| submissionDateFrom | DateTime | Optional: The start date and time when the document was submitted to the e-Invoice API, Time to be supplied in UTC timezone. Mandatory when 'submissionDateTo' is provided | 2022-11-25T01:59:10Z | Optional |
| submissionDateTo | DateTime | Optional: The end date and time when the document was submitted to the e-Invoice API, Time to be supplied in UTC timezone. Mandatory when 'submissionDateFrom' is provided | 2022-12-22T23:59:59Z | Optional |
| issueDateFrom | DateTime | Optional: The start date and time when the document was issued. Mandatory when 'issueDateTo' is provided | 2021-02-25T23:55:10Z | Optional |
| issueDateTo | DateTime | Optional: The end date and time when the document was issued. Mandatory when 'issueDateFrom' is provided | 2021-03-10T01:59:10Z | Optional |
| InvoiceDirection | Text | Optional: direction of the document. Possible values: (`Sent`, `Received`) | Sent | Optional |
| status | Text | Optional: status of the document. Possible values: (`Valid`, `Invalid`, `Cancelled`, `Submitted`) | Valid | Optional |
| documentType | Text | Optional: Document type code. | 01 | Optional |
| receiverId | Text | Optional: Document recipient identifier. Only can be used when 'Direction' filter is set to Sent. Possible values: (Business registration number, National ID(IC), Passport Number, Army ID) | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 | Optional |
| receiverIdType | Text | Optional: Document recipient identifier type. Only can be used when 'Direction' filter is set to Sent. Possible values: (BRN, PASSPORT, NRIC, ARMY) This is mandatory in case the receiverId is provided | PASSPORT | Optional |
| issuerIdType | Text | Optional: Document issuer identifier type. Only can be used when 'Direction' filter is set to Received. Possible values: (BRN, PASSPORT, NRIC, ARMY) This is mandatory in case the issuerId is provided | PASSPORT | Optional |
| receiverTin | Text | Optional: Document recipient TIN. Only can be used when 'Direction' filter is set to Sent. | C2584563200 | Optional |
| issuerTin | Text | Optional: Document issuer identifier. Only can be used when 'Direction' filter is set to Received. | C2584563200 | Optional |
| issuerId | Text | Optional: Document issuer identifier. Only can be used when 'Direction' filter is set to Received. Possible values: (Business registration number, National ID(IC), Passport Number, Army ID) | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 | Optional |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is part of a single object containing `result` structure and `metadata` structure.

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| uuid | String | Unique document ID in e-Invoice | 42S512YACQBRSRHYKBXBTGQG22 |
| submissionUID | String | Unique ID of the submission that document was part of | XYE60M8ENDWA7V9TKBXBTGQG10 |
| longId | String | Unique long temporary Id that can be used to query document data anonymously | YQH73576FY9VR57B... |
| internalId | String | Internal ID used in submission for the document | PZ-234-A |
| typeName | String | Unique name of the document type that can be used in submission of the documents. | invoice |
| typeVersionName | String | Name of the document type version within the document type that can be used in document submission to identify document type version being submitted | 1.0 |
| issuerTin | String | TIN of issuer | C2584563200 |
| receiverId | String | Optional: receiver registration number (can be national ID or foreigner ID). | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 |
| receiverName | String | Optional: receiver name (can be company name or person's name) | AMS Setia Jaya Sdn. Bhd. |
| dateTimeIssued | DateTime | The date and time when the document was issued. | 2015-02-13T13:15:00Z |
| dateTimeReceived | DateTime | The date and time when the document was submitted. | 2015-02-13T14:20:00Z |
| dateTimeValidated | DateTime | The date and time when the document passed all validations and moved to the valid state. | 2015-02-13T14:20:00Z |
| totalSales | Decimal | Total sales amount of the document in MYR. | 10.10 |
| totalDiscount | Decimal | Total discount amount of the document in MYR. | 50.00 |
| netAmount | Decimal | Total net amount of the document in MYR. | 100.70 |
| total | Decimal | Total amount of the document in MYR. | 124.09 |
| status | String | Status of the document - `Submitted`, `Valid`, `Invalid`, `Cancelled` | Valid |
| cancelDateTime | Date | Refer to the document cancellation that has been initiated by the taxpayer "issuer" of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| rejectRequestDateTime | Date | Refer to the document rejection request that has been initiated by the taxpayer "receiver" of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| documentStatusReason | String | Mandatory: Reason of the cancellation or rejection of the document. | Examples of reasons: Wrong buyer details or Wrong invoice details or any other reasons as appropriate |
| createdByUserId | String | User created the document. Can be ERP ID or User Email | C1XXXXXXXX00:9e21b10c-41c4-9323-c590-95abcb6e4e4d  general.ams@supplier.com |
| supplierTin | String | TIN of issuer | C2584563200 |
| supplierName | String | Supplier company name | AMS Setia Jaya Sdn. Bhd. |
| submissionChannel | String | Channel through which document was introduced into the system | possible values: `ERP`, `Invoicing Portal`, `InvoicingMobileApp` |
| intermediaryName | String | Intermediary company name | AMS Setia Jaya Sdn. Bhd. |
| intermediaryTin | String | TIN of intermediary | IG12345678912 |
| intermediaryRob | String | ROB of intermediary | 201901234567 |
| buyerName | String | Buyer company name | AMS Setia Jaya Sdn. Bhd. |
| buyerTin | String | Tin of buyer | C2584563200 |
| metadata | [Metadata](#metadata) | Information about the results retrieved or results matching the query | See structure |

#### Metadata

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| totalPages | Number | Total count of pages based on the supplied (or default) page size | 23 |
| totalCount | Number | Total count of matching objects | 157 |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

## Additional considerations

This API is limited to retrieve only certain maximum number of documents and to query only certain "time window" of the documents to prevent huge queries to be executed. These maximum parameters are defined by e-Invoice system administrators.

Time window is a configurable parameter configured by Tax Authuority. Current configuration is **31 days**.

The maximum number of documents that can be returned by this endpoint is **10,000 documents**. You can use the various optional filters to get the desired documents results.

Receiver of the documents can retrieve documents that are in statuses `Valid` or `Cancelled`. If document exists, and is issued to given receiver, but status is `Submitted` or `Invalid`, document will not be part of the response. Issuer of the documents can retrieve documents in any status. The document validation or public URL can be created using below -

```
{envbaseurl}/uuid-of-document/share/longid
```

where {envbaseurl} need to be replaced with e-Invoice portal Base URL

## Notes

**Important !**

• The 'Get Recent Document' API should be used for troubleshooting issues and it was not designed for reconciliation between your ERP system and the MyInvois system. Excessive use of this API may result in throttling. The system may impose limits on the usage policy of this API. Which will be based on each client ID and will be linked to the specific ERP system using the APIs.

• Kindly note that the 'Get Recent Documents' API is intended to assist with troubleshooting issues, rather than for reconciliation between your ERP system and the MyInvois system. Excessive use of this API for purposes other than troubleshooting may result in system throttling.

• Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Reject Document](04-reject-document.md)
[Get Submission](06-get-submission.md)


---
