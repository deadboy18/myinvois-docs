<!-- source: https://sdk.myinvois.hasil.gov.my/standard-error-response/ -->

# Standard Error Response

Page describes the standard error data structure and standard error codes that APIs of e-Invoice are using. If there are specialized error code and messages for a specific API, these are defined in API definition page.

# Error Response Structure

Error response is a single JSON object that has name value pair named `error`. This object can have the elements of type `Error` as defined in the table below.

| Property | Type | Description | Value example |
| --- | --- | --- | --- |
| propertyName | String | The name of the target/subject of the error, e.g., the name of the parameter that caused the error. This might have the value null if the property path cannot be resolved for the target. | document |
| propertyPath | String | Optional: the path to the target/subject of the error, e.g., the path to the parameter that caused the error. This might have the value null if the property path cannot be resolved for the target. | $.InvoiceLineItem[\*].InvoicedQuantity.unitCode |
| errorCode | String | Error code that client must be able to handle. | Error03 |
| error | String | Human readable error message. If available, message is returned in English language. | Previous passwords may not be reused |
| errorMS | String | Human readable error message. If available, message is returned in Malay language. | Kata laluan lama tidak boleh digunakan semula |
| target | String | Optional: the target/subject of the error, e.g., parameter that caused the error. | password |
| innerError | innerError[] | Optional: list of multiple errors detected that caused overall API call to fail. Used to return all errors in one go so that they all can be corrected by the caller before calling again. Each Error object is of the same structure as defined in this table that allows composition of multiple errors received. | <JSON list of Error structures> |

Example error responses:

```
{
  "status": "Invalid",
  "error": {
    "propertyName": null,
    "propertyPath": null,
    "errorCode": "Error03",
    "error": "Duplicated Submission Validator",
    "errorMSï¼ "Penduaan Sub Validatormissionâ,
    "innerError": [
      {
        "propertyName": "document",
        "propertyPath": "document",
        "errorCode": "DS302",
        "error": "Duplicated submitted document with UUID 132YBP1HE6GHJ0XSVNW0141J10 where the values of configured attributes match with values for the document with UUID: GJCGP9RWJF7961J8XNW0141J10â,
        "errorMs": "MS-Duplicated submitted document with UUID 132YBP1HE6GHJ0XSVNW0141J10 where the values of configured attributes match with values for the document with UUID: GJCGP9RWJF7961J8XNW0141J10â,
        "innerError": null
      ï½
    ]
  },
  "name": "Step03-Duplicated Submission Validator"
},
```

# Standard HTTP Status Codes

When returning error structure standard client or server HTTP error status codes must be used.

Additional error codes are used to provide more details on top of the general HTTP Status Code. These details are returned in [error structure](#error-response-structure) that are in the body of response.

Commonly used internal error codes of the solution are defined in the table mapped to HTTP Status Code.

| HTTP Status Code | Error Code | Description |
| --- | --- | --- |
| 400 | BadRequest | Used when supplied parameters are invalid to fulfil the request. Request should not be repeated without modifying the parameters. |
| 400 | BadArgument | Returned when one of the parameters supplied is not correct. Name of the argument is provided in `target` field. |
| 401 | Unauthorized | Used then API requires token to authorize caller, but it was not provided or was malformed or the token had expired. |
| 403 | Forbidden | Used when client does not have access to the API or data requested. |
| 404 | NotFound | Used when requested object does not exist. |
| 429 | TooManyRequests | Returned by API if it called more than throttling limit allows it to be called by a single caller. `Retry-After` response header in case of this error response contains a number, indicating the seconds remaining for this client to try again. |
| 500 | InternalServerError | Server encountered error when executing client request. |
| 501 | NotImplemented | API called or the method requested is not implemented/supported. |
| 503 | ServiceUnavailable | Returned when solution is encountering internal errors that are causing it to have limited functionality. |

# Correlation Information

When error is encountered, API returns correlation ID as part of `correlationId` response header value that can be used to identify the failed request when raising an issue with support team.


---
