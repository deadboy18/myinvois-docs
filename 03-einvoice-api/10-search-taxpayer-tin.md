<!-- source: https://sdk.myinvois.hasil.gov.my/einvoicingapi/10-search-taxpayer-tin/ -->

# Search Taxpayer's TIN

This API allows taxpayer's ERP system to search for a specific Tax Identification Number (TIN) using the supported search parameters. The supported search parameters are either:  
  
 1) Taxpayer Name or   
 2) taxpayer, ID Type, ID Value or   
 3) ID Type, ID Value or   
 4) ID Type, ID Value, File Type, or   
 5) If all parameters are provided then the search would use an AND operator to make sure the result found matches all search parameters provided.

[Search Documents](09-search-documents.md)
[Taxpayer's QR Code](11-qr-code.md)

## Overview

This API allows taxpayerâs ERP system to search for a specific Tax Identification Number (TIN) using the supported search parameters. The supported search parameters are either:

1) Taxpayer Name or  
2) taxpayer, ID Type, ID Value or  
3) ID Type, ID Value or  
4) ID Type, ID Value, File Type, or  
5) If all parameters are provided then the search would use an AND operator to make sure the result found matches all search parameters provided.

**Note!** When you are logged in as intermediary, permissions that are granted by the taxpayer will be applied on your profile and this will control the functionalities that you'll be able to executed on behalf of the taxpayer you are representing.

## Signature

This is REST based API that searches for TIN.

Signature:
`GET /api/v1.0/taxpayer/search/tin?idType={idType}&idValue={idValue}&taxpayerName={taxpayerName}&fileType={fileType}`

## Rate Limit

To optimize the use of our APIs, a rate limit of 60 Requests Per Minute (RPM) is recommended.

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call except for the one defined below:

| Input parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| taxpayerName | Query | The Taxpayer Name. | ABC XYZ | Mandatory if idType and idValue are not passed |
| idType | Query | NRIC, Passport number, Business registration number, army number | NRIC ID type example: NRIC  Passport ID type example: PASSPORT | Mandatory if idValue is passed |
| idValue | Query | The actual value of the ID Type selected. For example, if NRIC selected as ID Type, then pass the NRIC value here. | BRN example: 201901234567 NRIC example: 770625015324 Passport number example: A12345678 Army number example: 551587706543 | Mandatory if idType is passed |
| fileType | Query | Value to differentiate between individual and non-individual taxpayers | â¢ 1 for IG TIN (individual)   â¢ 2 for non-IG TIN (non-individual) | Optional |

## Output

### Successful Response

This API returns HTTP status code `200`.
The system would return one and only one Tax Identification Number (TIN) that is matching all the search criteria parameters. If more than one TIN was found matching the search criteria parameters, then an error would be returned to the caller.

| Parameter | Type | Description | Value example | Â |
| --- | --- | --- | --- | --- |
| tin | String | Matching TIN for given search criteria | C1234567890 | Â |

### Error Response

This API returns HTTP status code `400` (BadArgument) if the any of the input parameters does not match the given set of search parameters structure.
  
  
This API returns HTTP status code `400` (Bad Request) `Search criteria in not conclusive and more than one TIN can be found to match the search criteria provided, please revise the search criteria.` when there are more than 1 TIN found for the given set of search parameters.
  
  
This API returns HTTP status code `404` (Not Found) if no TIN is found for the given set of search parameters.

## Notes

**Important !**

â¢ This API should be used to Search for TIN before this is used to submit the documents. This should be cached from the ERP system side so that to reduce the calls occurring on this API. Excessive requests for this API may result in throttling and would be treated as malicious activity in this case. System may impose limits on the usage policy of this API. These limits will be based on each client ID and hence would be linked to the specific ERP system using the APIs.

â¢ API would return one and only one search result and if more than one result can be found matching the search criteria then the API would return an error as described above to the caller.

â¢ It is advised to perform validation of buyer's TINs when the buyer entity is being defined in your ERP system and once validated reflect this in your ERP system to make sure there is no need to recall the same API repeatedly.

â¢ It is not advised to call this API before every document submission as in this case this may result in these calls being flagged and throttled as well.

â¢ Always review the integration recommended practices at [sdk.myinvois.hasil.gov.my/integration-practices](../01-getting-started/integration-practices.md) to ensure your ERP integration is in accordance with these guidelines and follows healthy integration patterns.

[Search Documents](09-search-documents.md)
[Taxpayer's QR Code](11-qr-code.md)


---
