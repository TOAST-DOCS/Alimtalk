## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## Statistics

### [API Domain]

<table>
<thead>
<tr>
<th>Domain</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://kakaotalk-bizmessage.api.nhncloudservice.com</td>
</tr>
</tbody>
</table>


### Statistics Search - Event Based
* Statistics collected based on the time when the event occurs.
* Statistics are collected based on the following times:
    * Request count (REQUESTED): Time when the scheduled delivery is registered
    * Delivery count (SENT): Time of delivery to the vendor (scheduled delivery time)
    * Success count (RECEIVED): Delivery result successful (receiving time)
    * Failure count (SENT_FAILED): When the delivery request fails or when the delivery result fails
    * Alternative delivery request count (RESENT): Time when the alternative delivery is requested
    * Alternative delivery failure count (RESENT_FAILED): Time when the alternate delivery request fails

### Get Statistics Information

[URL]

|Http method|   URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey | String | Unique appkey |

[Header]
```
{
  "X-Secret-Key": String
}
```
| Name |	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | It can be created in the console.  |

[Query parameter]

| Name | Type | Max. Length | Required | Description |
|---|---|---|---|---|
| statsType | String | - | Required | Statistics type<br/>NORMAL: basic, MINUTELY: by minute, HOURLY: by hour, DAILY: daily, BY_DAY: by day |
| from | String | - | Required | Start date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | Required | End date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | Optional | When retrieving statistics data, set whether to provide the data in tree form |
| extra1s | List<String> | - | Optional | Sub-product type<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | Optional | senderKey |
| eventTypes | List<String> | - | Optional | Event type<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | Optional | Event list(Currently only `MESSAGE` is supported)<br/> MESSAGE |
| templateCodes | List<String> | - | Optional | Template code list (FriendTalk unsupported) |
| requestIds | List<String> | 5 | Optional | Request ID list |
| statsIds | List<String> | - | Optional | Statistics ID list |
| statsCriteria | List<String> | - | Optional | Statistics criteria<br/>- EVENT: event (default value)<br/>- EXTRA_1,EVENT: sub product type, event<br/>- EXTRA_2,EVENT: senderKey, event |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "stats": [
         {
            "eventDateTime": "2021-10-13T00:00:00.000Z",
            "events": {
                "RECEIVED": 5,
                "RESENT_FAILED": 0,
                "SENT_FAILED": 0,
                "REQUESTED": 1,
                "RESENT": 0,
                "SENT": 5
            }
        },
        {
            "eventDateTime": "2021-10-14T00:00:00.000Z",
            "events": {
                "RECEIVED": 25,
                "RESENT_FAILED": 1,
                "SENT_FAILED": 19,
                "REQUESTED": 55,
                "RESENT": 0,
                "SENT": 27
            }
        }
    ]
}
```

### Get Count per Event

[URL]

|Http method|   URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

| Name |  Type| Description|
|---|---|---|
| appKey | String | Unique appkey |

[Query parameter]

| Name | Type | Max. Length | Required | Description |
|---|---|---|---|---|
| statsType | String | - | Required | Statistics type<br/>NORMAL: basic, MINUTELY: by minute, HOURLY: by hour, DAILY: daily, BY_DAY: by day |
| from | String | - | Required | Start date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | Required | End date of statistics search<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | Optional | When retrieving statistics data, set whether to provide the data in tree form |
| extra1s | List<String> | - | Optional | Sub-product type<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | Optional | senderKey |
| eventTypes | List<String> | - | Optional | Event type<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | Optional | Event list(Currently only `MESSAGE` is supported)<br/> MESSAGE |
| templateCodes | List<String> | - | Optional | Template code list(FriendTalk unsupported) |
| requestIds | List<String> | 5 | Optional | Request ID list |
| statsIds | List<String> | - | Optional | Statistics ID list |
| statsCriteria | List<String> | - | Optional | Statistics criteria<br/>- EVENT: event(default value)<br/>- EXTRA_1,EVENT: sub product type, event<br/>- EXTRA_2,EVENT: senderKey, event |

[Response body]
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "total": {
        "RECEIVED": 45,
        "REQUESTED": 56,
        "RESENT": 0,
        "RESENT_FAILED": 1,
        "SENT": 47,
        "SENT_FAILED": 19
    }
}
```

## Kakao Statistics

* Retrieves statistics data provided by KakaoBizCenter.
* Statistics data can be retrieved on a daily (DAILY) or monthly (MONTHLY) basis by sender key.
* DAILY: Only data within the last 90 days can be retrieved, with a maximum retrieval range of 90 days.
* MONTHLY: Only data within the last 3 months can be retrieved, with a maximum retrieval range of 3 months.

Clicking **Go to Kakao Statistics** in Sender Profile Management opens Kakao Statistics in a new window. Statistics criteria include delivery statistics and template statistics, and the retrieval conditions vary by message channel. Results can be viewed as charts and tables.

* Real-time statistics are not provided. Data collected the previous day is provided daily at around 7 AM.
* AlimTalk statistics are initially provided on D+1 and finalized on D+2.
* Valid read counts are not duplicated for the same message.
* Click counts are duplicated for the same message.
* If the number of successful sends is 10 or fewer, valid read counts and click counts are not provided.

### Delivery Statistics

Retrieves the daily send count, valid read count, and click count by sender profile. You can filter by period, send identifier, message type, and more.

### Template Statistics

Retrieves the daily send count, valid read count, and click count by template and group tag. You can filter by period, message type, and more.

* Brand message freestyle is only provided when a group tag is used.

### Retrieve AlimTalk Delivery Statistics

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/ALIMTALK |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | Can be created in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender key |
| periodType | String | O | Statistics unit (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Retrieval start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Retrieval end date<br/>DAILY: yyyy-MM-dd (maximum range: 90 days), MONTHLY: yyyy-MM (maximum range: 3 months) |
| messageType | String | X | Message type (AT: standard AlimTalk, AI: image AlimTalk) |
| receiveUserType | String | X | Recipient type (PhoneNumber: phone number, None: no recipient identifier) |
| limit | Integer | X | Number of results to retrieve (Default: 500, Max: 1,000) |
| offset | Integer | X | Start position (Default: 0) |

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "totalCount": 1,
    "alimtalkDeliveryStatistics": [
        {
            "date": "2026-04-01",
            "messageType": "NORMAL",
            "receiveUserType": "ALL",
            "totalSendRequestCount": 100,
            "validSendRequestCount": 95,
            "validReadCount": 80
        }
    ]
}
```

| Name | Type | Not Null | Description |
|---|---|:---:|---|
| header | Object | O |  |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Whether the request was successful |
| totalCount | Integer | O | Total count |
| alimtalkDeliveryStatistics | List | O | AlimTalk delivery statistics list |
| - date | String | O | Date |
| - messageType | String | O | Message type (AT: standard AlimTalk, AI: image AlimTalk) |
| - receiveUserType | String | O | Recipient type (PhoneNumber: phone number, None: no recipient identifier) |
| - totalSendRequestCount | Integer | O | Total send request count |
| - validSendRequestCount | Integer | O | Valid send request count |
| - validReadCount | Integer | O | Valid read count |

### Retrieve AlimTalk Template Statistics

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/ALIMTALK |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | Can be created in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender key |
| periodType | String | O | Statistics unit (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Retrieval start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Retrieval end date<br/>DAILY: yyyy-MM-dd (maximum range: 90 days), MONTHLY: yyyy-MM (maximum range: 3 months) |
| kakaoTemplateCode | String | X | Kakao template code |
| messageType | String | X | Message type (AT: standard AlimTalk, AI: image AlimTalk) |
| limit | Integer | X | Number of results to retrieve (Default: 500, Max: 1,000) |
| offset | Integer | X | Start position (Default: 0) |

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "totalCount": 1,
    "alimtalkTemplateStatistics": [
        {
            "date": "2026-04-01",
            "messageType": "NORMAL",
            "templateCode": "template01",
            "totalSendSuccessCount": 90,
            "validReadCount": 75,
            "totalClickCount": 30
        }
    ]
}
```

| Name | Type | Not Null | Description |
|---|---|:---:|---|
| header | Object | O |  |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Whether the request was successful |
| totalCount | Integer | O | Total count |
| alimtalkTemplateStatistics | List | O | AlimTalk template statistics list |
| - date | String | O | Date |
| - messageType | String | O | Message type (AT: standard AlimTalk, AI: image AlimTalk) |
| - templateCode | String | O | Template code |
| - totalSendSuccessCount | Integer | O | Total send success count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |

### Retrieve Brand Message Delivery Statistics

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/BRANDMESSAGE |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | Can be created in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender key |
| periodType | String | O | Statistics unit (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Retrieval start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Retrieval end date<br/>DAILY: yyyy-MM-dd (maximum range: 90 days), MONTHLY: yyyy-MM (maximum range: 3 months) |
| messageSpec | String | X | Message spec (BASIC: basic, FREESTYLE: freestyle) |
| chatBubbleType | String | X | Chat bubble type (TEXT: text, IMAGE: image, WIDE: wide image, WIDE_ITEM_LIST: wide item list, CAROUSEL_FEED: carousel feed, PREMIUM_VIDEO: premium video, COMMERCE: commerce, CAROUSEL_COMMERCE: carousel commerce) |
| targeting | String | X | Targeting (M: all users who agreed to receive marketing, N: excluding channel friends, I: channel friends only, F: all channel friends) |
| friendType | String | X | Friend type (F: friend, N: non-friend) |
| receiveUserType | String | X | Recipient type (PhoneNumber: phone number, None: no recipient identifier) |
| limit | Integer | X | Number of results to retrieve (Default: 500, Max: 1,000) |
| offset | Integer | X | Start position (Default: 0) |

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "totalCount": 1,
    "brandmessageDeliveryStatistics": [
        {
            "date": "2026-04-01",
            "messageSpec": "TEMPLATE",
            "chatBubbleType": "TEXT",
            "targeting": "ALL",
            "friendType": "ALL",
            "receiveUserType": "ALL",
            "totalSendRequestCount": 200,
            "validSendRequestCount": 190,
            "validReadCount": 150,
            "totalClickCount": 60
        }
    ]
}
```

| Name | Type | Not Null | Description |
|---|---|:---:|---|
| header | Object | O |  |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Whether the request was successful |
| totalCount | Integer | O | Total count |
| brandmessageDeliveryStatistics | List | O | Brand message delivery statistics list |
| - date | String | O | Date |
| - messageSpec | String | O | Message spec (BASIC: basic, FREESTYLE: freestyle) |
| - chatBubbleType | String | O | Chat bubble type (TEXT: text, IMAGE: image, WIDE: wide image, WIDE_ITEM_LIST: wide item list, CAROUSEL_FEED: carousel feed, PREMIUM_VIDEO: premium video, COMMERCE: commerce, CAROUSEL_COMMERCE: carousel commerce) |
| - targeting | String | O | Targeting (M: all users who agreed to receive marketing, N: excluding channel friends, I: channel friends only, F: all channel friends) |
| - friendType | String | O | Friend type (F: friend, N: non-friend) |
| - receiveUserType | String | O | Recipient type (PhoneNumber: phone number, None: no recipient identifier) |
| - totalSendRequestCount | Integer | O | Total send request count |
| - validSendRequestCount | Integer | O | Valid send request count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |

### Retrieve Brand Message Template Statistics

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/BRANDMESSAGE |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | Can be created in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender key |
| periodType | String | O | Statistics unit (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Retrieval start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Retrieval end date<br/>DAILY: yyyy-MM-dd (maximum range: 90 days), MONTHLY: yyyy-MM (maximum range: 3 months) |
| kakaoTemplateCode | String | X | Kakao template code |
| groupTagKey | String | X | Group tag key |
| messageSpec | String | X | Message spec (BASIC: basic, FREESTYLE: freestyle) |
| chatBubbleType | String | X | Chat bubble type (TEXT: text, IMAGE: image, WIDE: wide image, WIDE_ITEM_LIST: wide item list, CAROUSEL_FEED: carousel feed, PREMIUM_VIDEO: premium video, COMMERCE: commerce, CAROUSEL_COMMERCE: carousel commerce) |
| targeting | String | X | Targeting (M: all users who agreed to receive marketing, N: excluding channel friends, I: channel friends only, F: all channel friends) |
| friendType | String | X | Friend type (F: friend, N: non-friend) |
| limit | Integer | X | Number of results to retrieve (Default: 500, Max: 1,000) |
| offset | Integer | X | Start position (Default: 0) |

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "totalCount": 1,
    "brandmessageTemplateStatistics": [
        {
            "date": "2026-04-01",
            "templateCode": "brandtemplate01",
            "groupTagKey": "group01",
            "messageSpec": "TEMPLATE",
            "chatBubbleType": "TEXT",
            "targeting": "ALL",
            "friendType": "ALL",
            "totalSendSuccessCount": 180,
            "validReadCount": 140,
            "totalClickCount": 55
        }
    ]
}
```

| Name | Type | Not Null | Description |
|---|---|:---:|---|
| header | Object | O |  |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Whether the request was successful |
| totalCount | Integer | O | Total count |
| brandmessageTemplateStatistics | List | O | Brand message template statistics list |
| - date | String | O | Date |
| - templateCode | String | O | Template code |
| - groupTagKey | String | X | Group tag key |
| - messageSpec | String | O | Message spec (BASIC: basic, FREESTYLE: freestyle) |
| - chatBubbleType | String | O | Chat bubble type (TEXT: text, IMAGE: image, WIDE: wide image, WIDE_ITEM_LIST: wide item list, CAROUSEL_FEED: carousel feed, PREMIUM_VIDEO: premium video, COMMERCE: commerce, CAROUSEL_COMMERCE: carousel commerce) |
| - targeting | String | O | Targeting (M: all users who agreed to receive marketing, N: excluding channel friends, I: channel friends only, F: all channel friends) |
| - friendType | String | O | Friend type (F: friend, N: non-friend) |
| - totalSendSuccessCount | Integer | O | Total send success count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |