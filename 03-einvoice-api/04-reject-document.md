<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/04-reject-document/ -->

# Reject Document

This API allows a buyer that received an invoice to reject it and request the supplier to cancel it.

[Cancel Document](03-cancel-document.md)
[Get Recent Documents](05-get-recent-documents.md)

## Overview

Document rejection is a way to quickly notify the supplier that there are some problems with the document they have issued, so that correction can be done as soon as possible. Rejection is allowed for recipient of the document only within limited time period from the time of validation of the document. This time limit is specific to document type and is returned by calling [Get Document Type API](../02-platform-api/04-get-document-type.md) and checking workflow parameters.

If allowed rejection time since validation has passed but the document is still considered incorrect, buyer need to notify issuers and they need to revert previously issued incorrect invoice through submission of a credit note/debit note/refund note (as the case may be).

Rejection is permitted within the 72-hour window from the date and time that the document was marked valid to be able to reject a document.

Rejection action for any document can happen only once.

**Note!** When you are logged in as intermediary, permissions that are granted by the taxpayer will be applied on your profile and this will control the functionalities that you'll be able to executed on behalf of the taxpayer you are representing.

## Signature

This is REST based API that receives the document ID through the URL parameter and state to be changed in request body.

Signature:
`PUT /api/v1.0/documents/state/{UUID}/state`

## Rate Limit

To optimize the use of our APIs, a rate limit of 12 Requests Per Minute (RPM) / Client ID is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| uuid | String | Unique ID of existing document. | F9D425P6DS7D8IU | Mandatory |

| Body parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| status | String | Desired status for the document. Must be rejected to reject received document. | Rejected | Mandatory |
| reason | String | Reason for rejecting the document. | Examples of reasons: Wrong buyer details or Wrong invoice details or any other reasons as the supplier deems appropriate | Mandatory |

## Outputs

| Parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| uuid | String | Unique ID of the document | F9D425P6DS7D8IU |
| status | String | Once a rejection request is received, a document is still considered "Valid" until the Seller "cancels" the document. | Requested for Rejection |
| error | [Error](../01-getting-started/error-responses.md#error-response-structure) | Error if rejection of document failed. |  |

### Successful Response

On a successful submission, this API returns `200` status code.

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

Additional specialised error messages can be returned as a result of validation of state of the document and caller:

| HTTP Status Code | Error Code | Description |
| --- | --- | --- |
| 400 | OperationPeriodOver | Returned when caller is trying to reject document when the time limit of rejection has already run out. |
| 400 | IncorrectState | Returned when caller is trying to reject the document that is not in valid state. State transition in this case is not allowed. |
| 400 | ActiveReferencingDocuments | Returned when caller is trying to reject the document that has been referenced by other documents, e.g., credit note referencing invoice. If rejection is still required, the required action is to first reject the referencing document and followed by this one. |
| 403 | Forbidden | Returned when valid taxpayer system is trying to perform operation on a document not received by them. |

## Notes

**Important !**

• Thoroughly validate rejection criteria before invoking the rejection API to avoid incorrect rejections.

• Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Cancel Document](03-cancel-document.md)
[Get Recent Documents](05-get-recent-documents.md)


---
