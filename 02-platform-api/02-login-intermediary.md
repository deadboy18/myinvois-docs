<!-- source: https://sdk.myinvois.hasil.gov.my/api/08-login-as-intermediary-system/ -->

# Login as Intermediary System

This API is used to authenticate the ERP system associated with an intermediary that is representing a taxpayer (acting on behalf of a specific taxpayer) calling and issue access token which allows ERP system to access those protected APIs.

[Login as Taxpayer System](01-login-taxpayer.md)
[Get All Document Types](03-get-document-types.md)

## Overview

MyInvois APIs are protected except the Login APIs (Login as taxpayer and Login as Intermediary System) and made available only to relevant taxpayerâs representatives and their ERP systems.

This API is used to authenticate the ERP system calling and issue access token which allows ERP system to access those protected APIs. Note that each token is issued for a certain time period configured as part of MyInvois System, example can be found in [expires\_in](#successful-response). This means that compliant ERP systems integrating with the solution should expect that some calls will return unauthorised error codes (see more on [standard error responses](../01-getting-started/error-responses.md)) which means that most likely the token issued has expired and needs to be renewed (by another login).

**Note!** Authentication of the systems is done on identity service, not on the service hosting actual integration APIs. Use the Identity Service base address when creating the full URL to be called.

## Signature

As described in the [Getting started overview](../01-getting-started/overview.md), solution is leveraging externalised standards based identity system to manage users, systems that are accessing solution user interfaces and APIs.

Therefore solution leverages [OAuth 2.0 client credentials flow](https://tools.ietf.org/html/rfc6749#section-4.4) for authenticating systems and granting them access token that allows them to call other APIs described in this SDK which requires authorisation.

Signature:
`POST /connect/token`

## Rate Limit

To optimize the use of our APIs, a rate limit of 12 Requests Per Minute (RPM) / Client ID is recommended.

## Inputs

When trying to login as intermediary, add an extra header parameter to the request. This header parameter will be mapped to the taxpayer that the intermediary is trying to login on his behalf.

**Note!** When trying to login as intermediary, permissions that are granted by the taxpayer will be applied on the intermediary's profile and this will control the functionalities that they will be able to executed on behalf of the taxpayer they are representing.

| Header parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| onbehalfof | String | This should be either the Tax Identification Number (TIN) of the taxpayer the intermediary is representing, or it can follow the format : if the intermediary is representing a taxpayer identified by an ROB number. | C25845632020 (in case of taxpayer identified by TIN only), or IG12345678912:201901234567 (in case of taxpayer identified by TIN and has an ROB number) | Mandatory |

| Body parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| client\_id | String | Client ID for the ERP system. | Â | Mandatory |
| client\_secret | String | Client secret for the ERP system. | Â | Mandatory |
| grant\_type | String | Must be âclient\_credentialsâ | client\_credentials | Mandatory |
| scope | String | Optional parameter asking for a specific access scope. In case of external access to e-Invoice APIs, this parameter can be omitted | InvoicingAPI | Optional |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| access\_token | JWT token | Encoded JWT token structure that contains the fields of the issued token, token protection attributes. | Encoded token value |
| token\_type | String | Solution in this case returns only Bearer authentication tokens | Bearer |
| expires\_in | Number | The lifetime of the access token defined in seconds | 3600 (means it is valid for one hour) |
| scope | String | Optional if matches the requested scope. Otherwise contains information on scope granted to token. This defines the APIs that client will have access to use this token. | InvoicingAPI |

### Error Responses: 400 Bad Request

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| error | String | Possible values: invalid\_request, invalid\_client, invalid\_grant, unauthorised\_client, unsupported\_grant\_type, invalid\_scope | invalid\_request |
| error\_description | String | Optional human readable error message containing more details about error encountered. | User blocked |
| error\_uri | URI | Optional URI containing more information about the error. Not used in MyInvois System | Â |

## Additional Considerations

Each token already includes information about the taxpayer that the system is going to represent, therefore taxpayer information is available to the solution APIs when API calls are being done at a later point.

System authentication can be rejected if (i) invalid client ID and secret is used to authenticate; or (ii) the system user registered against the taxpayer is blocked or expired.

Tokens issued as a result of this login operation are valid only for a pre-configured limited time, example can be found in [expires\_in](#successful-response).

**Note:** Your system should be responsible to obtain a new access token using this endpoint before the expiry of the current valid token to continue calling the APIs.

## Notes

**Important !**

â¢ It is advisable to use separate credentials for intermediary systems to avoid conflicts with taxpayer system credentials. To optimise performance, avoid making frequent login attempts. Instead, use session tokens to maintain active sessions. In the event of failed login attempts, implement retry mechanisms with exponential backoff to prevent overloading the system. Always ensure that credentials are securely stored, and use HTTPS for all API calls to safeguard sensitive data.

â¢ Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Login as Taxpayer System](01-login-taxpayer.md)
[Get All Document Types](03-get-document-types.md)


---
