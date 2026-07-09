<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/02-submit-documents/ -->

# Submit Documents

This API allows taxpayer to submit one or more signed documents to MyInvois System.

[Validate taxpayer TIN](01-validate-taxpayer-tin.md)
[Cancel Document](03-cancel-document.md)

## Overview

Submit Documents API is the main API of the solution because it is used to submit one or more documents of different types to the system. Documents submitted are grouped into the submission. Then, each document that is part of the submission must be linked to and comply with data structure of the document type (see [document type versions supported](../04-document-types/README.md)).

Document submission API supports documents submitted using JSON and XML format. Taxpayer ERP system or integration module is responsible for transforming the data into JSON or XML format as per the document type structure expected by API (see [document type versions supported](../04-document-types/README.md)). Note that all documents within the submission and enclosing submission element itself needs to be in the same format. The format used should be provided through Content-type request header value.

When API is called, it performs minimal set of validations through [validation rules](../07-validation/README.md) to check that submission is of correct structure. Then, each document that is part of the submission is checked for the taxpayer issuing the documents. The structure of each document is checked that it matches the appropriate structure of the document type version used.

After synchronous validation succeeds, caller receives back the assigned submission ID and list of IDs of the documents that are accepted for further processing, matching them back to internal document IDs supplied so that callers can map them in taxpayer ERP system. These IDs can later be used to [retrieve documents](07-get-document.md), [submission information](06-get-submission.md) and learn about full validation results. If required, they can also be used to [cancel the document](03-cancel-document.md).

**Note!** When you are logged in as intermediary, permissions that are granted by the taxpayer will be applied on your profile and this will control the functionalities that you'll be able to executed on behalf of the taxpayer you are representing.

## Signature

This is REST based API that receives its document submission information in the body of the request.

Signature:
`POST /api/v1.0/documentsubmissions/`

## Rate Limit

To optimize the use of our APIs, a rate limit of 100 Requests Per Minute (RPM)/ Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call except for the one defined below:

| Header parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| Content type | String | Defines the type of the document submission format used. | application/json | Mandatory |

Body of the request contains a single `documents` value that contains one or more Document objects. Document objects of different types and even versions can have different fields supplied.

| Input parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| documents | [Document[]](#single-document-consists-of) | List of document objects submitted. List should have at least one document. The document should follow the UBL 2.1 schema based on the document type version. | Array of Documents | Mandatory |

#### Single document consists of:

| Input parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| format | String | XML, JSON | XML | Mandatory |
| document | String | The base64 of the document JSON or XML |  | Mandatory |
| documentHash | String | The hash value of the document being submitted. Hashing can be performed using algorithms like SHA256 as mentioned [here](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.sha256). |  | Mandatory |
| codeNumber | String | Document reference number used by Supplier for internal tracking purpose | INV12345, CN23456, DN34567 | Mandatory |

Document can be either:

* [Invoice](../04-document-types/README.md#invoice) - Invoice is a commercial document issued by Supplier to itemise and record a transaction with Buyer.
* [Credit Note](../04-document-types/README.md#Credit) - Credit note is the document issued by Suppliers to correct errors, apply discounts, or account for returns in a previously issued e-Invoice with the purpose of reducing the value of the original e-Invoice. This is used in situations where the reduction of the original e-Invoice does not involve return of monies to the Buyer.
* [Debit Note](../04-document-types/README.md#Debit) - Debit note is the document issued to indicate additional charges on a previously issued e-Invoice.
* [Refund Note](../04-document-types/README.md#Refund) - Refund note is the document issued by a Supplier to confirm the refund of the Buyer's payment. This is used in situations where there is a return of monies to the Buyer.
* [Self-Billed Invoice](../04-document-types/README.md#Self-Billed%20Invoice) - There are certain circumstances where another party (other than the Supplier) will be allowed to issue a self-billed e-Invoice. Kindly refer to Section 8 of the e-Invoice Specific Guideline where self-billed e-Invoice will be allowed.   
  Self-Billed Invoice refers to the self-billed e-Invoice that will be issued by the Buyer for the transaction with the Supplier, based on the circumstances as specified in Section 8 of the e-Invoice Specific Guideline.
* [Self-Billed Credit Note](../04-document-types/README.md#Self-Billed%20Credit) - Self-Billed Credit Note is issued by Buyers to correct errors, apply discounts, or account for returns in a previously issued Self-Billed e-Invoice with the purpose of reducing the value of the original Self-Billed e-Invoice. This is used in situations where the reduction of the original Self-Billed e-Invoice does not involve return of monies to the Buyer
* [Self-Billed Debit Note](../04-document-types/README.md#Self-Billed%20Debit) - Self-Billed Debit Note is the document issued by Buyers to indicate additional charges on a previously issued Self-Billed e-Invoice.
* [Self-Billed Refund Note](../04-document-types/README.md#Self-Billed%20Refund) - Self-Billed Refund Note is the document issued by Buyers to confirm the refund payment from the Supplier. This is used in situations where there is a return of monies from the Supplier to the Buyer.

## Outputs

### Successful Response

On a successful submission, API returns unique submission ID and unique IDs for all documents accepted. These IDs assigned are mapped to the internal IDs that are inside the documents so that caller ERP system can map them back in the ERP as well.

Given that the process of validation is not complete when result is returned, API returns HTTP status code `202`.

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| submissionUID | String | Unique ID of the submission. 26 Latin alphanumeric symbols. |  |
| acceptedDocuments | [acceptedDocuments[]](02-submit-documents.md#accepted-documents) | List of documents that are initially accepted (passed sync validations) together with their invoice code numbers and newly assigned IDs. |  |
| rejectedDocuments | [rejectedDocuments[]](02-submit-documents.md#rejected-documents) | List of documents that are not accepted together with their invoice code numbers and error information. |  |

#### Accepted Documents

Accepted document response consists of:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| uuid | String | Unique document ID assigned by e-Invoice. 26 Latin alphanumeric symbols. |  |
| invoiceCodeNumber | String | Document reference number used by Supplier for internal tracking purpose | INV12345, CN23456, DN34567 |

#### Rejected Documents

Rejected document response consists of:

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| invoiceCodeNumber | String | Document reference number used by Supplier for internal tracking purpose | INV12345, CN23456, DN34567 |
| error | [Error](../01-getting-started/error-responses.md#error-response-structure) | Error information detailing why the document was not accepted in this submission. |  |

#### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

Additional specialised error messages can be returned as a result of validation of documents:

| HTTP Status Code | Error Code | Description |
| --- | --- | --- |
| 400 | BadStructure | Returned when there is a structural error with the submission message. Either is not having the Documents included in a single Document tag or there are other elements that are not allowed. |
| 400 | MaximumSizeExceeded | Returned when the size of the submission exceeds allowed limit. Details including supported size in the [Error](#error) object in the body of the return message. It is expected that calling system creates smaller submissions and submit them one by one. |
| 403 | IncorrectSubmitter | Returned when submitter of the documents is trying to submit them on behalf of the other taxpayer. Also returned when intermediary is submitting documents on behalf of taxpayer, but they do not have such a permission. Details in the [Error](#error) object in the body of the return message. |
| 422 | DuplicateSubmission | Returned when an identical submission is detected based on the previous submissions sent by the same taxpayer within the past 10 minutes. Issuer can try submitting the same payload again based on the returned value in the response header `Retry-After` in seconds. Detection works on hashing the request payload and compare it with previous submissions. |

## Additional considerations

The e-Invoice file size rules are as follows:

1. 5 MB maximum submission size;
2. 100 maximum e-Invoices per submission; and
3. 300 KB maximum per e-Invoice.

No restriction on the number of lines in an e-Invoice will be imposed, as long as the rules above are met.

Minification of XML/JSON Documents

In order to optimize document size and prevent potential validation errors, we recommend implementing minification techniques for XML and JSON documents submitted via the Submit Document API. This entails the compression of whitespace and unnecessary characters within the documents, effectively reducing their overall size without altering their content or structure.

Minification serves two primary purposes:

* *Size Optimization:* By eliminating unnecessary whitespace, line breaks, and indentation, the size of XML and JSON documents can be significantly reduced. This is particularly beneficial when dealing with large datasets or documents, helping to minimize bandwidth usage and improve overall system performance.
* *Validation Error Prevention:* Minification can help mitigate the risk of encountering validation errors, such as the 1,000 lines limitation. By reducing the overall line count of the documents, the likelihood of hitting such limits is diminished, ensuring smoother processing and transmission of data.

Implementing minification as part of the document submission process not only enhances the efficiency of data transfer but also contributes to a more robust and scalable system architecture. We encourage developers to incorporate minification techniques into their workflows when utilizing the Submit Document API, thereby optimizing performance and mitigating potential validation issues.

### Mapping Internal IDs to Assigned Ones

To support mapping of internal documents managed in taxpayer ERP system to documents issued or received through MyInvois System, taxpayers can supply their internal invoice identifiers (used in ERP solution) and also internal product identifiers for the goods sold or services rendered when submitting e-Invoice to LHDNM.

When MyInvois System returns back assigned unique IDs for document submission (containing one or more documents), each individual document accepted by the solution contains assigned e-Invoice ID, long unique ID used for the creation of validation link and internal document ID (that was supplied by the submitter).

### Throttling

Given large amounts of submissions coming from multiple taxpayers, it is recommended that taxpayers submit invoices in batches, it is important that ERP systems are not calling the API too frequently. Therefore, solution leverages throttling techniques configured by administrators. See [standard headers description](../01-getting-started/standard-headers.md) to see headers that are returned in successful responses and [standard error response information](../01-getting-started/error-responses.md) for throttling error information.

## Notes

**Important !**

• The ERP system can submit multiple documents using batch processing. Each batch is able handle up to 100 documents, with a total size limit of 5 MB and a maximum document size of 300 KB. Submitting documents in batches rather than individually is recommended for efficiency.

• The ERP system should not keep re-submit the same submission repeatedly without considering the results received from the submission API. The submission APIs return the results of the initial processing, which should be reviewed before resending a submission. The system may impose limits on the usage policy of this API, which will be based on each client ID and linked to the specific ERP system using the APIs.

• The system allows for batch submissions of documents, accommodating up to 100 documents per batch, with a combined size limit of 5 MB and a maximum of 300 KB per document. Batch submissions of documents are recommended over individual submissions for increased efficiency.

• Avoid repeated submissions of the same document without considering the results from the initial submission API. The API provides immediate processing results, which should be reviewed prior to any resubmission. A threshold of 100 request per minute (RPM) for this API is advised.

• Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Validate taxpayer TIN](01-validate-taxpayer-tin.md)
[Cancel Document](03-cancel-document.md)


---
