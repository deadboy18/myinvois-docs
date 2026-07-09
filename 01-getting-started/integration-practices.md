<!-- source: https://sdk.myinvois.hasil.gov.my/integration-practices/ -->

# Integration Practices

This page explains about the Integration Practices that will help users to integrate with the API.

[Postman](../09-postman/README.md)

# Content

The solution supports multiple integration options, but it is important to integrate following the below guidance to make sure your integration is behaving correctly and you would be able to get the most benefits of the solution.

# Polling Approach

In this integration approach, the ERP system would submit documents using one of the submission APIs and then continue to check the status of these submitted documents using one of the submission APIs and the reference of the submission that was generated from the system.

**Note!**
Please note the following key points in the above flow diagram:

[Login as Taxpayer](../02-platform-api/01-login-taxpayer.md)

* Login is performed and access tokens should be cached for the lifetime of the token (this is default to be valid for 60 minutes).
* If login fails, then issue should be investigated and resolved.

[Submit documents](../03-einvoice-api/02-submit-documents.md)

* Once invoice documents are submitted, then the ERP system should wait for a period of time depending on the size of your submission before checking with the e-invoicing system if the submission has been completed or not.

  The submission API returns a submission ID that can be used to check the status of the submission as per below.

[Get Submission](../03-einvoice-api/06-get-submission.md)

* In order to check the Submission status, use the submission UID along with Get Submission API:
* Once the submission has been completely processed then the documents are updated in your internal ERP system accordingly.

# Common Integration Anti-patterns

The following are key practices that should be avoided when implementing the integration with the e-invoicing system as these practices might impact the integrity of the data received and would increase the load over your ERP system.

1. Acquiring a new authentication token with every API call to the system.

   Received authentication tokens remain valid for a duration that is already specified in the response of the authentication API as per below.

   By default, these token are valid for 60 minutes and hence can be used by any system APIs that requires such a token while the token is still valid.
2. Getting the status of the submission by checking the status of each document submitted within the submission by any of the APIs "Get Recent Documents", "Search Documents", "Get Document" or "Get Document Details". These APIs are designed to be used for documents retrieval after submission and validation and not during the submission operation as per the above workflows.
3. Re-submitting the same submission over and over again without regards to the results received from the submission APIs.

   The submission APIs returns results of initial processing of the submission and hence these should be used and checked before the submission is sent again.

   In fact, resending the same submission should not be performed in all cases except in case of error received from the submission APIs.

   Otherwise, any of the 20x response statuses should be accepted by your system and in this case the submission shouldn't be resubmitted again as it will be automatically flagged as duplicate submission and in these cases the submission APIs might start to throttling calls being received from your ERP system.
4. The calls to "Get Recent Documents" should be in most cases be fulfilled by the more optimized API "Search Documents".

   Calling the API "Get Recent Documents" is subject to being throttled as well.
5. Calling of the authentication API on the path "/connect/token" without supplying the required parameters such as the "client\_id", "grant\_type", or "client\_secret"; would not add any value to your system as these calls will be automatically blocked by the system.

   These include cases such as sending the "client\_id" with wrong ('0' or '**YOUR\_CLIENT\_ID**') or empty values.

# Rate Limit

Rate limiting controls API usage spikes by restricting the number of calls allowed within a specified time frame. If the limit is exceeded, a "429 Too Many Requests" response status code is returned to the caller. The information below outlines the intended limits and serves as an educational resource for your API integration.

| API | Requests Per Minute (RPM)/ Client Id |
| --- | --- |
| Login as Taxpayer System | 12 |
| Login as Intermediary System | 12 |
| Submit Documents | 100 |
| Get Submission | 300 |
| Cancel Document | 12 |
| Reject Document | 12 |
| Get Document | 60 |
| Get Document Details | 125 |
| Get Recent Documents | 12 |
| Search Documents | 12 |
| Search Taxpayer's TIN | 60 |
| Taxpayer's QR Code | 60 |

**What Happens When Limits Are Exceeded?**

When the rate limit for an API endpoint is exceeded:

• The server will return a "429 Too Many Requests" status code. Please refer error response. [sdk.myinvois.hasil.gov.my/standard-error-response](error-responses.md)  
• The response will include a Retry-After header indicating how long to wait before retrying.

**Note:** Please note that the sandbox environment is intended for functional testing and has a lower API rate limit compared to the production environment, effective 28 April 2025. Additionally, data in the sandbox environment will be retained for a maximum of 3 months, after which it will be permanently deleted. Please ensure to back up any important data before the retention period expires.

# Polling Approach for Document Submission

The pseudo-code provided below outlines a process for submitting documents in base64 format via an API call (SubmitDocuments) and subsequently polling another API (GetSubmission) to check the submission status.

The submitDocuments function initiates the submission process, returning a unique submission identifier. The pollSubmissionStatus function continuously checks the status of the submission until the status becomes valid or invalid.

The main function orchestrates these steps, encoding the document, submitting it, polling for submission status, and processing them accordingly.

This structured approach facilitates efficient document submission and status handling within an application or system.

```
// Main function to orchestrate the process
function main():
// Step 1: Login as Taxpayer System
token = getTokenFromLoginEndpoint(clientId, clientSecret)
// Cache the token based on expires_in response parameter
// Step 2: Submit documents
base64Document = encodeDocumentToBase64()
requestBody = appendBase64TorequestBody()
submissionUID = submitDocuments(requestBody, token)
// Step 3: Poll for Submission Status
submissionStatus = pollSubmissionStatus(submissionUID)
// Step 4: Process further

// Define function to submit documents via SubmitDocuments API
function submitDocuments(requestBody, token):
response = callSubmitDocumentsAPI(requestBody, token)
submissionUID = response.submissionUID
return submissionUID

// Define function to poll GetSubmission API for Submission Status
function pollSubmissionStatus(submissionUID):
inProgress = true
while inProgress is true:
    submission = callDocumentSubmissionsAPI(submissionUID)
    submissionStatus = submission.overallStatus
    if submissionStatus.isInProgress:
        waitForSomeTime()            
    else:
        inProgress = false
        return submissionStatus
```

[Postman](../09-postman/README.md)


---
