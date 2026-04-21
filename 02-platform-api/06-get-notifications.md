<!-- source: https://sdk.myinvois.hasil.gov.my/api/06-get-notifications/ -->

# Get Notifications

This API allows ERP system to query for previously sent notifications.

[Get Document Type Version](05-get-document-type-version.md)
[Full Platform API list](README.md)

## Overview

MyInvois System supports (i) sending out notification via email and (ii) storing the notification in notification history. The ERP system are able to utilise the Get Notification API to query the notification history to obtain the notifications. Note that reliance on notifications for implementing entire e-Invoice workflow is optional and it is provided as an additional functionality that can be leveraged.

System limits the number of the notifications that can be received through a single request by implementing paging mechanism for this API.

List of notifications is ordered based on descending notification date.

## Signature

This is REST based API that takes optional URL parameters to do data filtering and also enable paging.

Signature:
`GET /api/v1.0/notifications/taxpayer?dateFrom={dateFrom}&dateTo={dateTo}&type={type}&language={language}&status={status}&pageNo={pageNo}&pageSize={pageSize}`

## Inputs

This API accepts [standard e-Invoice API header parameters](../01-getting-started/standard-headers.md) for authenticated call.

URL parameters accepted:

| URL parameter | Type | Description | Value example | Requirement |
| --- | --- | --- | --- | --- |
| dateFrom | DateTime | Optional: start date and time for notifications to retrieve based on the date sent | 2015-02-13T14:20:10Z | Optional |
| dateTo | DateTime | Optional: end date and time for notifications to retrieve based on the date sent | 2015-02-14T14:20:10Z | Optional |
| type | String | Optional: type of notifications to retrieve specified as ID of the type. See [Notification types](#notification-types) | 2 | Optional |
| language | String | Optional: used to get notifications only if they were sent out in a specific language. Values:`ms`and `en` | en | Optional |
| status | String | Optional: used to get notifications of certain status only, e.g., only those that were not delivered. See [Notification Status](#notification-status) | 2 | Optional |
| pageNo | Number | Optional: number of the page to retrieve. Typically this parameter value is derived from initial parameter less call when caller learns total amount of page of certain size | 3 | Optional |
| pageSize | Number | Optional: number of the packages to retrieve per page. Page size cannot exceed system configured maximum page size for this API which is 100 | 20 | Optional |

### Notification Status

| Status Id | Status Name |
| --- | --- |
| 1 | New |
| 2 | Pending |
| 3 | Batched |
| 4 | Delivered |
| 5 | Error |

### Notification Types

| Notification Type Id | Notification Type Name |
| --- | --- |
| 3 | Profile data validation |
| 6 | Document received |
| 7 | Document validated |
| 8 | Document cancelled |
| 10 | User profile changed |
| 11 | Taxpayer profile changed |
| 15 | Document rejection initiated |
| 26 | ERP data validation |
| 33 | Documents processing summary |
| 34 | Document Template Published |
| 35 | Document Template Deletion |

## Outputs

### Successful Response

This API returns HTTP status code `200`.

The resulting structure is part of a single object containing `result` structure and `metadata` structure.

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| result | [Notification[]](#notification) | Array of notification objects | See structure |
| metadata | [Metadata](#metadata) | Information about the results retrieved or results matching the query | See structure |

#### Notification

| Output parameter | Type | Description | Value example | Â |
| --- | --- | --- | --- | --- |
| notificationId | String | Unique ID of the notification. | 73DKLJHH78NJUHQ | Â |
| receiverName | String | Receiver Name of the notification. | KXXX\_XXXXPS BHD | Â |
| notificationDeliveryId | String | Unique ID of the notification Delivery. | 73DKLJHH78NJUHQ | Â |
| creationDateTime | DateTime | The date and time when notification was created | 2015-02-13T14:20:10Z | Â |
| receivedDateTime | DateTime | The date and time when notification was sent out | 2015-02-13T14:20:10Z | Â |
| notificationSubject | String | Subject of the notification. | Documents Submitted | 73DKLJHH78NJUHQ |
| deliveredDateTime | DateTime | Optional date time when notification was delivered | 2015-02-13T14:23:10Z | Â |
| typeId | String | Id of the type of the message | 34 | Â |
| typeName | String | Type name of the message | Invoice received | Â |
| finalMessage | String | Optional: final message that was sent out - depends on the channel | Taxpayer 893838273 has received new documents | Â |
| address | String | Channel address that was used to deliver the message | test@test.com | Â |
| language | String | Language used for delivery. Values:`ms`,`en` | en | Â |
| status | String | Status of the notification delivery. Values - [Notification Status](#notification-status) | delivered | Â |
| deliveryAttempts | [Delivery Attempt[]](#delivery-attempt) | Structure containing information about delivery attempts of the notification or its batch (if batched with others and delivered together) | See structure. | Â |

#### Delivery Attempt

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| attemptDateTime | DateTime | Date time when delivery was attempted. | 2015-02-13T14:20:10Z |
| status | String | Status of the notification delivery. Values - `delivered`, `error` | delivered |
| statusDetails | String | Error message in case of error in delivery | Cannot connect to system URL supplied |

#### Metadata

| Output parameter | Type | Description | Value example |
| --- | --- | --- | --- |
| hasNext | Boolean | Returns whether next pagination exists or not | false |

### Error Response

Error situations are reported back by this API through the [standard error response](../01-getting-started/error-responses.md).

No custom error codes are provided by this API.

## Additional considerations

Maximum page size allowed is defined by e-Invoice system administrators.

[Get Document Type Version](05-get-document-type-version.md)
[Full Platform API list](README.md)


---
