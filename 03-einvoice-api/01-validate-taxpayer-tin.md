<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/01-validate-taxpayer-tin/ -->

# Validate Taxpayer's TIN

This API allows taxpayer's ERP system to validate specific Tax Identification Number (TIN) before adding this number to an invoice and issuing the invoice.

[Full e-Invoice API list](README.md)
[Submit Documents](02-submit-documents.md)

## Overview

Validate Taxpayerâs TIN API is used to validate specific Tax Identification Number (TIN) before adding this number to an invoice and issuing the invoice.

**Note!** When you are logged in as intermediary, permissions that are granted by the taxpayer will be applied on your profile and this will control the functionalities that you'll be able to executed on behalf of the taxpayer you are representing.

## Signature

This is REST based API that validates TIN.

Signature:
`GET /api/v1.0/taxpayer/validate/{tin}?idType={idType}&idValue={idValue}`

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call except for the one defined below:

| Input parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| tin | Number | The Tax Identification Number to get the validity of the tin. | C25845632020 | Mandatory |
| idType | Query | NRIC, Passport number, Business registration number, army number | NRIC ID type example: NRIC  Passport ID type example: PASSPORT  BRN ID type example: BRN  Army ID type example: ARMY | Mandatory |
| idValue | Query | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 | Mandatory |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

### Error Response

This API returns HTTP status code `400` (BadArgument) if the TIN or any of the input parameters does not match the argument structure.
  
This API returns HTTP status code `404` (Not Found) if that TIN and ID combination cannot be found or are considered invalid.

## Notes

**Important !**

â¢ This API should be used to validate buyer's TIN before this is used to submit the documents. This should be cached from the ERP system side so that to reduce the calls occurring on this API. Excessive requests for this API may result in throttling and would be treated as malicious activity in this case. System may impose limits on the usage policy of this API. These limits will be based on each client ID and hence would be linked to the specific ERP system using the APIs.

â¢ It is advised to perform validation of buyer's TINs when the buyer entity is being defined in your ERP system and once validated reflect this in your ERP system to make sure there is no need to recall the same API repeatedly.

â¢ It is not advised to call this API before every document submission as in this case this may result in these calls being flagged and throttled as well.

â¢ Always review the integration recommended practices here [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to make sure your ERP integration is implemented following them and following healthy integration patterns.

[Full e-Invoice API list](README.md)
[Submit Documents](02-submit-documents.md)


---
