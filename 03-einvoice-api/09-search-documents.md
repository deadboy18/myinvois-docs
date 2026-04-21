<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/09-search-documents/ -->

# Search Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters.

[Get Document Details](08-get-document-details.md)
[Search Taxpayer's TIN](10-search-taxpayer-tin.md)

## Overview

The Search Documents API has various enhanced filters for precise queries and provides access to large result set in a paginated way.

Search documents allows users to query the system and return list of documents that have been received or issued.

To invoke this API, users must provide values for either the `submissionDateFrom` and `submissionDateTo` filter parameters or the `issueDateFrom` and `issueDateTo` filter parameters. Users can combine these optional filters in various ways to obtain precise results.

Results will be returned in a paginated format based on the specified `pageSize`, allowing users to navigate through multiple pages of results.

**Note**: The values of the input filter parameters should remain consistent when requesting subsequent pages of results.

Days difference between `submissionDateFrom` and `submissionDateTo` filter parameters, and `issueDateFrom` and `issueDateTo` filter parameters should not exceed a configured number of days set by e-Invoice system administrators (Current configuration is 31 days).

Results list is ordered based on registration date of the submission descending (newest documents submitted into the e-Invoice system appear first).

## Signature

This is REST based that accepts the below URL filter parameters to search for documents.

Signature:
`GET /api/v1.0/documents/search?&uuid={uuid}&submissionDateFrom={submissionDateFrom}&submissionDateTo={submissionDateTo}&pageSize={pageSize}&pageNo={pageNo}&issueDateFrom={issueDateFrom}&issueDateTo={issueDateTo}&invoiceDirection={invoiceDirection}&status={status}&documentType={documentType}&searchQuery={searchQuery}`

## Rate Limit

To optimize the use of our APIs, a rate limit of 12 Requests Per Minute (RPM) / Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

URL parameters accepted:

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| uuid | String | Optional: Unique ID of the document to retrieve. | F9D425P6DS7D8IU | Optional |
| submissionDateFrom | DateTime | Mandatory (as this or the issue date and time filters): The start date and time when the document was submitted to the e-Invoice API, Time to be supplied in UTC timezone. Mandatory when âsubmissionDateToâ is provided or issueDate filters are not used. SubmissiondateFrom should be less than SubmissionToDate | 2022-11-25T01:59:10Z | Mandatory |
| submissionDateTo | DateTime | Mandatory (as this or the issue date and time filters): The end date and time when the document was submitted to the e-Invoice API, Time to be supplied in UTC timezone. Mandatory when âsubmissionDateFromâ is provided or issueDate filters are not used.SubmissiondateFrom should be less than SubmissionToDate | 2022-12-22T23:59:59Z | Optional |
| pageSize | Number | Optional: number of the documents to retrieve per page. Page size cannot exceed system configured maximum page size for this API. Default is 100. PageSize should be greaterthan 0 and less or equal to 100. PageSzie shouldnât be negative value | 100 | Optional |
| pageNo | Number | Optional: number of the page to retrieve. Typically this parameter value is derived from initial parameter less call when caller learns total amount of page of certain size | 3 | Optional |
| issueDateFrom | DateTime | Mandatory (as this or the submission date and time filters): The start date and time when the document was issued. Mandatory when âissueDateToâ is provided or submissionDate filters are not used.IssuedateFrom should be less than IssueToDate | 2021-02-25T23:55:10Z | Optional |
| issueDateTo | DateTime | Mandatory (as this or the submission date and time filters): The end date and time when the document was issued. Mandatory when âissueDateFromâ is provided or submissionDate filters are not used.IssuedateFrom should be less than IssueToDate | 2021-03-10T01:59:10Z | Optional |
| invoiceDirection | Text | Optional: direction of the document. Possible values: (`Sent`, `Received`). When not provided sent and received documents are retrieved. | Sent | Optional |
| status | Text | Optional: status of the document. Possible values: (`Valid`, `Invalid`, `Cancelled`, `Submitted`) | Valid | Optional |
| documentType | Text | Optional: Unique name of the document type. Possible values: `01` [Invoice], `02` [Credit Note], `03` [Debit Note], `04` [Refund Note], `11` [Self-billed Invoice], `12` [Self-billed Credit Note], `13` [Self-billed Debit Note], `14` [Self-billed Refund Note] | 01 | Optional |
| searchQuery | Text | Optional: Free Text can be given. Possible Search parameters: (`uuid`,`buyerTIN`,`supplierTIN`,`buyerName`,`supplierName`,`internalID`,`total` ) Special characters are not allowed | C2584563200 | Optional |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is part of a single object containing `result` structure.

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| uuid | String | Unique document ID in e-Invoice | 42S512YACQBRSRHYKBXBTGQG22 |
| submissionUID | String | Unique ID of the submission that document was part of | XYE60M8ENDWA7V9TKBXBTGQG10 |
| longId | String | Unique long temporary Id that can be used to query document data anonymously | YQH73576FY9VR57Bâ¦ |
| internalId | String | Internal ID used in submission for the document | PZ-234-A |
| typeName | String | Unique name of the document type that can be used in submission of the documents. | invoice |
| typeVersionName | String | Name of the document type version within the document type that can be used in document submission to identify document type version being submitted | 1.0 |
| issuerTin | String | TIN of issuer | C2584563200 |
| issuerName | String | Issuer company name | AMS Setia Jaya Sdn. Bhd. |
| receiverId | String | Optional: receiver registration number (can be national ID or foreigner ID). | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 |
| receiverName | String | Optional: receiver name (can be company name or personâs name) | AMS Setia Jaya Sdn. Bhd. |
| dateTimeIssued | DateTime | The date and time when the document was issued. | 2015-02-13T13:15:00Z |
| dateTimeReceived | DateTime | The date and time when the document was submitted. | 2015-02-13T14:20:00Z |
| dateTimeValidated | DateTime | The date and time when the document passed all validations and moved to the valid state. | 2015-02-13T14:20:00Z |
| totalSales | Decimal | Total sales amount of the document in MYR. | 10.10 |
| totalDiscount | Decimal | Total discount amount of the document in MYR. | 50.00 |
| netAmount | Decimal | Total net amount of the document in MYR. | 100.70 |
| total | Decimal | Total amount of the document in MYR. | 124.09 |
| status | String | Status of the document - `Submitted`, `Valid`, `Invalid`, `Cancelled` | Valid |
| cancelDateTime | Date | Refer to the document cancellation that has been initiated by the taxpayer âissuerâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| rejectRequestDateTime | Date | Refer to the document rejection request that has been initiated by the taxpayer âreceiverâ of the document on the system, will be in UTC format | 2021-02-25T01:59:10Z |
| documentStatusReason | String | Mandatory: Reason of the cancellation or rejection of the document. | Examples of reasons: Wrong buyer details or Wrong invoice details or any other reasons as appropriate |
| createdByUserId | String | User created the document. Can be ERP ID or User Email | C1XXXXXXXX00:9e21b10c-41c4-9323-c590-95abcb6e4e4d  general.ams@supplier.com |
| supplierTIN | String | TIN of issuer | C2584563200 |
| supplierName | String | Supplier company name | AMS Setia Jaya Sdn. Bhd. |
| submissionChannel | String | Channel through which document was introduced into the system | possible values: `ERP`, `Invoicing Portal`, `InvoicingMobileApp` |
| intermediaryName | String | Intermediary company name | AMS Setia Jaya Sdn. Bhd. |
| intermediaryTin | String | TIN of intermediary | IG12345678912 |
| intermediaryRob | String | ROB of intermediary | 201901234567 |
| buyerName | String | Buyer company name | AMS Setia Jaya Sdn. Bhd. |
| buyerTIN | String | Tin of buyer | C2584563200 |
| metadata | [Metadata](#metadata) | Information about the results retrieved or results matching the query | See structure |

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| totalPages | Number | Total count of pages based on the supplied (or default) page size | 23 |
| totalCount | Number | Total count of matching objects | 157 |

### Throttling and Rate limiting

To allow consistent performance for all taxpayers using e-Invoice API and prevent overuse of resources, this API has throttling limits set per taxpayer (current configuration is **1 Request every 5 Seconds**). The throttling limit is subject to change by e-Invoice system administrators.

See the [standard error response](../01-getting-started/error-responses.md) for more details when your calls to the API are throttled.

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

## Additional considerations

Days difference between `submissionDateFrom` and `submissionDateTo` filter parameters, and `issueDateFrom` and `issueDateTo` filter parameters are currently configured to 31 days. Days difference value can be changed by e-Invoice system administrators.

There are maximum number of 10,000 documents that can be returned by this endpoint. You can use the various optional filters to get the desired documents results.

Receiver of the documents can retrieve documents that are in statuses `Valid` or `Cancelled`. If document exists, and is issued to given receiver, but status is `Submitted` or `Invalid`, document will not be part of the response. Issuer of the documents can retrieve documents in any status. The document validation or public URL can be created using below -

```
{envbaseurl}/uuid-of-document/share/longid
```

where {envbaseurl} need to be replaced with MyInvois portal Base URL

## Notes

**Important !**

â¢ Search document API is designed for your manual auditing requirements and also for troubleshooting issues. This API shouldn't be used for ERP system reconciliation implementation with the MyInvois system. Excessive requests for this API may result in throttling. System may impose limits on the usage policy of this API. These limits will be based on each client ID and hence would be linked to the specific ERP system using the APIs.

â¢ Optimise search queries by applying specific filters and parameters to narrow down the results, improving the efficiency of your searches. Avoid making repetitive searches with identical criteria, and where applicable, cache the results to reduce redundant API calls.

â¢ Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Get Document Details](08-get-document-details.md)
[Search Taxpayer's TIN](10-search-taxpayer-tin.md)


---
