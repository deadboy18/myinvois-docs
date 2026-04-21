<!-- source: https://sdk.myinvois.hasil.gov.my/standard-header-parameters/ -->

# Standard Header Parameters

Page describes the standard header parameters that can be used by all APIs of the e-Invoice public API except those where explicitly stated otherwise.

# Request Header Parameters

## Any API Call

These header parameters apply to all calls to all APIs if not explicitly stated otherwise.

| Header parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| Accept | String | Defines the content types that client is able to accept | application/json |
| Accept-Language | String | May contain the preferred language for response. Supported language values in MyInvois System is `en`. If supplied by caller and if supported by the API, system will try to return textual data in language preferred. If not supplied, default system language is used. | en |
| Content-type | String | Defines the type of the message. e-Invoice relies on JSON based content. | application/json |

## Authenticated Call

These header parameters apply to calls to APIs that require authentication and authorization of the callers.

| Header parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| Authorization | String | Must contain access token issued prior to this call by [identity service](../02-platform-api/01-login-taxpayer.md). | Bearer <Token value> |

# Response Header Parameters

## Any API Call

These header parameters apply to all calls to all APIs if not explicitly stated otherwise.

| Header parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| Content-type | String | Defines the type of the message returned. In e-Invoice is is primarily JSON but can also be XML and octet stream for binary content. | application/json |
| correlationId | String | Defines the unique string value that can be used to track the calls in the system that were made during a single session. Helps in resolving potential problems with integration. | JHDSJ8882POY72SG-2828 |
| X-Rate-Limit-Limit | Number | A number representing the total requests count for this client against API. | 1000 |
| X-Rate-Limit-Remaining | Number | A number representing the remaining allowed requests count for this client against API. | 900 |
| X-Rate-Limit-Reset | DateTime | Time in which the count will reset, assuming no further calls were attempted. The header will appear only when the limits have been exceeded. | 2020-05-04T12:23:41.6181792Z |


---
