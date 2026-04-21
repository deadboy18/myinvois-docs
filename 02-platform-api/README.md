<!-- source: https://sdk.myinvois.hasil.gov.my/api/ -->

# Application Programming Interface (API)

Platform API focuses on key platform functionalities and not dealing with the actual Invoice document.

[e-Invoice API](../03-einvoice-api/README.md)

## Login as Taxpayer System

This API is used to authenticate the ERP system associated with a specific taxpayer calling and issue access token which allows ERP system to access those protected APIs.

[Read more](01-login-taxpayer.md)

## Login as Intermediary System

This API is used to authenticate the ERP system associated with an intermediary that is representing a taxpayer (acting on behalf of a specific taxpayer) calling and issue access token which allows ERP system to access those protected APIs.

[Read more](02-login-intermediary.md)

## Get All Document Types

This API allows taxpayer's systems to retrieve list of document types published by the MyInvois System.

[Read more](03-get-document-types.md)

## Get Document Type

This API allows taxpayer's ERP system to retrieve the details of single document type that contains structure definitions of the document.

[Read more](04-get-document-type.md)

## Get Document Type Version

This API allows taxpayer's ERP system to retrieve the details of document type version that contains structure definitions of the documents.

[Read more](05-get-document-type-version.md)

## Get Notifications

This API allows ERP system to query for previously sent notifications.

[Read more](06-get-notifications.md)

[e-Invoice API](../03-einvoice-api/README.md)


---
