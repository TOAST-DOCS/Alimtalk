<!-- pre-align:aligned sig=473d4f693686 -->

## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

<a id="statistics"></a>

## Statistics

<a id="api-domain"></a>

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


<a id="statistics-search---event-based"></a>

### Statistics Search - Event Based
* Statistics collected based on the time when the event occurs.
* Statistics are collected based on the following times:
    * Request count(REQUESTED): Time when the scheduled delivery is registered
    * Delivery count(SENT): Time of delivery to the vendor(scheduled delivery time)
    * Success count(RECEIVED): Delivery result successful(receiving time)
    * Failure count(SENT_FAILED): When the delivery request fails or when the delivery result fails
    * Alternative delivery request count(RESENT): Time when the alternative delivery is requested
    * Alternative delivery failure count(RESENT_FAILED): Time when the alternate delivery request fails

<a id="get-statistics-information"></a>

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

<a id="get-count-per-event"></a>

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
<a id="kakao-statistics"></a>

## Kakao Statistics

<!-- TODO: translate body -->

<a id="send-statistics"></a>

### Send Statistics

<!-- TODO: translate body -->

<a id="template-statistics"></a>

### Template Statistics

<!-- TODO: translate body -->

<a id="get-alimtalk-send-statistics"></a>

### Get AlimTalk Send Statistics

<!-- TODO: translate body -->

<a id="request"></a>

#### Request

<!-- TODO: translate body -->

<a id="response"></a>

#### Response

<!-- TODO: translate body -->

<a id="get-alimtalk-template-statistics"></a>

### Get AlimTalk Template Statistics

<!-- TODO: translate body -->

<a id="request-2"></a>

#### Request

<!-- TODO: translate body -->

<a id="response-2"></a>

#### Response

<!-- TODO: translate body -->

<a id="get-brand-message-send-statistics"></a>

### Get Brand Message Send Statistics

<!-- TODO: translate body -->

<a id="request-3"></a>

#### Request

<!-- TODO: translate body -->

<a id="response-3"></a>

#### Response

<!-- TODO: translate body -->

<a id="get-brand-message-template-statistics"></a>

### Get Brand Message Template Statistics

<!-- TODO: translate body -->

<a id="request-4"></a>

#### Request

<!-- TODO: translate body -->

<a id="response-4"></a>

#### Response

<!-- TODO: translate body -->

