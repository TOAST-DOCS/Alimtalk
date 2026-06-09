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
    * Request count(REQUESTED): Time when the scheduled delivery is registered
    * Delivery count(SENT): Time of delivery to the vendor(scheduled delivery time)
    * Success count(RECEIVED): Delivery result successful(receiving time)
    * Failure count(SENT_FAILED): When the delivery request fails or when the delivery result fails
    * Alternative delivery request count(RESENT): Time when the alternative delivery is requested
    * Alternative delivery failure count(RESENT_FAILED): Time when the alternate delivery request fails

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

* You can retrieve statistics data provided by Kakao Biz Center.
* You can retrieve statistics data by sender key on a daily (DAILY) or monthly (MONTHLY) basis.
* DAILY: You can retrieve only data within the last 90 days, and the query range is up to 90 days.
* MONTHLY: You can retrieve only data within the last 3 months, and the query range is up to 3 months.

In Sender Profile Management, if you click **Go to Kakao Statistics**, you can view Kakao statistics in a new window. Statistics criteria include delivery statistics and template statistics, and query conditions vary depending on the message channel. You can view the query results in charts and tables.

* Real-time statistics are not provided. Data collected from the previous day is provided daily around 7 AM.
* AlimTalk statistics are first provided on D+1 and finalized on D+2.
* Valid read counts are not duplicated for the same message.
* Click counts are duplicated for the same message.
* If the number of successful deliveries is 10 or less, valid read counts and click counts are not provided.

### Send Statistics

Queries daily delivery count, valid read count, and click count based on Sender Profile. You can query by setting the period, delivery identifier, Message Type, and other parameters.

### Template Statistics

You can query daily delivery counts, valid read counts, and click counts based on templates and group tags. You can query by setting the period, message type, and other parameters.

* Brand Message free-form is provided only when group tags are used.

### AlimTalk Send Statistics Query

<!-- TODO: translate body -->

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/ALIMTALK |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | You can create this in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender Key |
| periodType | String | O | Statistics category (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Query start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Query end date<br/>DAILY: yyyy-MM-dd (maximum range 90 days), MONTHLY: yyyy-MM (maximum range 3 months) |
| messageType | String | X | Message Type (AT: general AlimTalk, AI: image AlimTalk) |
| receiveUserType | String | X | Recipient type (PhoneNumber: phone number, None: no recipient identifier) |
| limit | Integer | X | Query count (Default: 500, Max: 1000) |
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
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success status |
| totalCount | Integer | O | Total count |
| alimtalkDeliveryStatistics | List | O | AlimTalk delivery statistics list |
| - date | String | O | Date |
| - messageType | String | O | Message Type (AT: General AlimTalk, AI: Image AlimTalk) |
| - receiveUserType | String | O | Recipient type (PhoneNumber: Phone number, None: No recipient identifier) |
| - totalSendRequestCount | Integer | O | Total send request count |
| - validSendRequestCount | Integer | O | Valid send request count |
| - validReadCount | Integer | O | Valid read count |

### AlimTalk Template Statistics Query

<!-- TODO: translate body -->

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/ALIMTALK |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | You can create it in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender Key |
| periodType | String | O | Statistics classification (DAILY: daily, MONTHLY: monthly) |
| startDate | String | O | Query start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Query end date<br/>DAILY: yyyy-MM-dd (maximum range of 90 days), MONTHLY: yyyy-MM (maximum range of 3 months) |
| kakaoTemplateCode | String | X | Kakao Template Code |
| messageType | String | X | Message Type (AT: general AlimTalk, AI: image AlimTalk) |
| limit | Integer | X | Number of queries (Default: 500, Max: 1000) |
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
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success status |
| totalCount | Integer | O | Total count |
| alimtalkTemplateStatistics | List | O | AlimTalk template statistics list |
| - date | String | O | Date |
| - messageType | String | O | Message type (AT: general AlimTalk, AI: image AlimTalk) |
| - templateCode | String | O | Template Code |
| - totalSendSuccessCount | Integer | O | Total delivery success count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |

### Brand Message Send Statistics Query

<!-- TODO: translate body -->

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/BRANDMESSAGE |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | You can create it in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender Key |
| periodType | String | O | Statistics classification (DAILY: Daily, MONTHLY: Monthly) |
| startDate | String | O | Query start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Query end date<br/>DAILY: yyyy-MM-dd (maximum range 90 days), MONTHLY: yyyy-MM (maximum range 3 months) |
| messageSpec | String | X | Message specification (BASIC: Basic type, FREESTYLE: Freestyle type) |
| chatBubbleType | String | X | Chat bubble type (TEXT: Text Type, IMAGE: Image Type, WIDE: Wide Image, WIDE_ITEM_LIST: Wide Item List, CAROUSEL_FEED: Carousel Feed, PREMIUM_VIDEO: Premium Video, COMMERCE: Commerce, CAROUSEL_COMMERCE: Carousel Commerce) |
| targeting | String | X | Targeting (M: All marketing consent users, N: Exclude Channel Friends, I: Channel Friends only, F: All Channel Friends) |
| friendType | String | X | Friend type (F: Friend, N: Non-friend) |
| receiveUserType | String | X | Recipient type (PhoneNumber: Phone number, None: No recipient identifier) |
| limit | Integer | X | Number of queries (Default: 500, Max: 1000) |
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
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success status |
| totalCount | Integer | O | Total count |
| brandmessageDeliveryStatistics | List | O | Brand Message delivery statistics list |
| - date | String | O | Date |
| - messageSpec | String | O | Message specification (BASIC: Basic type, FREESTYLE: Freestyle type) |
| - chatBubbleType | String | O | Chat bubble type (TEXT: Text Type, IMAGE: Image Type, WIDE: Wide Image Type, WIDE_ITEM_LIST: Wide Item List Type, CAROUSEL_FEED: Carousel Feed Type, PREMIUM_VIDEO: Premium Video Type, COMMERCE: Commerce Type, CAROUSEL_COMMERCE: Carousel Commerce Type) |
| - targeting | String | O | Targeting (M: All users who agreed to marketing, N: Excluding channel friends, I: Channel friends only, F: All channel friends) |
| - friendType | String | O | Friend type (F: Friend, N: Non-friend) |
| - receiveUserType | String | O | Recipient type (PhoneNumber: Phone number, None: No recipient identifier) |
| - totalSendRequestCount | Integer | O | Total send request count |
| - validSendRequestCount | Integer | O | Valid send request count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |

### Brand Message Template Statistics Query

<!-- TODO: translate body -->

#### Request

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/BRANDMESSAGE |

[Path parameter]

| Name | Type | Description |
|---|---|---|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|---|---|---|---|
| X-Secret-Key | String | O | You can create this in the console. |

[Query parameter]

| Name | Type | Required | Description |
|---|---|---|---|
| senderKey | String | O | Sender Key |
| periodType | String | O | Statistics type (DAILY: Daily, MONTHLY: Monthly) |
| startDate | String | O | Query start date<br/>DAILY: yyyy-MM-dd (within the last 90 days), MONTHLY: yyyy-MM (within the last 3 months) |
| endDate | String | O | Query end date<br/>DAILY: yyyy-MM-dd (maximum range of 90 days), MONTHLY: yyyy-MM (maximum range of 3 months) |
| kakaoTemplateCode | String | X | Kakao Template Code |
| groupTagKey | String | X | Group tag key |
| messageSpec | String | X | Message specification (BASIC: Basic type, FREESTYLE: Freestyle type) |
| chatBubbleType | String | X | Chat bubble type (TEXT: Text Type, IMAGE: Image Type, WIDE: Wide Image, WIDE_ITEM_LIST: Wide Item List, CAROUSEL_FEED: Carousel Feed, PREMIUM_VIDEO: Premium Video, COMMERCE: Commerce, CAROUSEL_COMMERCE: Carousel Commerce) |
| targeting | String | X | Targeting (M: All users who consented to marketing, N: Excluding channel friends, I: Channel friends only, F: All channel friends) |
| friendType | String | X | Friend type (F: Friends, N: Non-friends) |
| limit | Integer | X | Number of queries (Default: 500, Max: 1000) |
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
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success status |
| totalCount | Integer | O | Total count |
| brandmessageTemplateStatistics | List | O | Brand Message template statistics list |
| - date | String | O | Date |
| - templateCode | String | O | Template Code |
| - groupTagKey | String | X | Group tag key |
| - messageSpec | String | O | Message spec (BASIC: Basic type, FREESTYLE: Freestyle type) |
| - chatBubbleType | String | O | Chat bubble type (TEXT: Text Type, IMAGE: Image Type, WIDE: Wide Image, WIDE_ITEM_LIST: Wide Item List, CAROUSEL_FEED: Carousel Feed, PREMIUM_VIDEO: Premium Video, COMMERCE: Commerce, CAROUSEL_COMMERCE: Carousel Commerce) |
| - targeting | String | O | Targeting (M: All users who agreed to marketing, N: Excluding channel friends, I: Channel friends only, F: All channel friends) |
| - friendType | String | O | Friend type (F: Friend, N: Non-friend) |
| - totalSendSuccessCount | Integer | O | Total delivery success count |
| - validReadCount | Integer | O | Valid read count |
| - totalClickCount | Integer | O | Total click count |

