## Notification > KakaoTalk Bizmessage > 알림톡 > API v2.3 Guide

<a id="alimtalk"></a>

## AlimTalk

<a id="api-domain"></a>

#### [API Domain]

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

<a id="overview-of-v23-api"></a>

## Overview of v2.3 API

1. Added Quick Reply, Item List, Talk Biz plugin, Main Link, and Business Form button.
2. Added the AlimTalk item highlight image registration API.
3. Added APIs to register, modify, delete, and retrieve AlimTalk plugins.
4. Removed the buttons field from the API to retrieve message list.

<a id="general-messages"></a>

## General Messages

<a id="request-of-sending-replaced-messages"></a>

### Send Replacement Message Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name                       | 	Type     | 	Required | 	Description                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | Can be created in the console.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | Key used as the basis for duplicate message send requests<br>If a request is made with the same key for 10 minutes, the request will be failed. |

[Request body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [{
        "recipientNo": String,
        "templateParameter": {
            String: String
        },
        "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String
        },
        "buttons": [
          {
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String,
            "telNumber": String
          }
        ],
        "quickReplies": [
            "ordering": Integer,
            "chatExtra": String,
            "chatEvent": String,
            "target": String
        ],
        "recipientGroupingKey": String
    }],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| Name                     | 	Type      | 	Required | 	Description                                                                                           |
|------------------------|----------|-----|-----------------------------------------------------------------------------------------------|
| senderKey              | 	String  | 	O  | Sender key (40 characters)                                                                                     |
| templateCode           | 	String  | 	O  | Registered template code (up to 20 characters)                                                                         |
| requestDate            | String   | X   | Request date and time (yyyy-MM-dd HH:mm)<br>(If not specified, sent immediately)<br>Schedulable up to 60 days in advance                            |
| senderGroupingKey      | String   | X   | Sender grouping key (up to 100 characters)                                                                             |
| createUser             | String   | X   | Registrant (saved as user UUID when sent from the console)                                                                   |
| recipientList          | 	List    | 	O  | 	Recipient list (up to 1,000)                                                                            |
| - recipientNo          | 	String  | 	O  | 	Recipient number (up to 15 characters)                                                                                 |
| - templateParameter    | 	Object  | 	X  | 	Template parameter<br>(Required if the template contains replacement variables)                                                           |
| -- key                 | 	String  | 	X  | 	Replacement key (#{key})                                                                                 |
| -- value               | String   | 	X  | 	Value mapped to the replacement key                                                                            |
| - resendParameter      | 	Object  | 	X  | 	Alternative delivery information                                                                                      |
| -- isResend            | 	boolean | 	X  | 	Whether to send by SMS as alternative delivery upon send failure<br>If alternative delivery is configured in the console, it is sent as alternative delivery by default.                                      |
| -- resendType          | 	String  | 	X  | 	Alternative delivery type (SMS, LMS)<br>If no value is specified, the type is determined based on the length of the template body.                                      |
| -- resendTitle         | 	String  | 	X  | 	LMS alternative delivery title<br>(resent with the Plus Friend ID if value is unavailable.)                                              |
| -- resendContent       | 	String  | 	X  | 	Alternative delivery content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                        |
| -- resendSendNo        | String   | X   | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span> |
| - buttons              | 	List    | 	X  | Additional button information                                                                                      |
| -- ordering            | Integer  | X   | 	Button order (required if buttons exist)                                                                          |
| -- chatExtra           | 	String  | 	X  | 	Meta information to pass for BC (consultation talk switch) / BT (bot switch) type buttons                                                       |
| -- chatEvent           | 	String  | 	X  | 	Bot event name to connect for BT (bot switch) type buttons                                                                  |
| -- relayId             | 	String  | 	X  | Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed                                               |
| -- oneClickId          | 	String  | 	X  | Payment information used in the one-click payment plugin                                                                      |
| -- productId           | 	String  | 	X  | Payment information used in the one-click payment plugin                                                                      |
| -- target              | 	String  | 	X  | 	For web link buttons, adding the `"target":"out"` property opens an out-link<br>Sent as an in-app link by default                                    |
| -- telNumber           | 	String  | 	X  | Phone number to pass for TN (call) type buttons                                                                    |
| - quickReplies         | 	List    | 	X  | Quick Reply information                                                                                       |
| -- ordering            | Integer  | X   | 	Quick Reply order (required if Quick Replies exist)                                                                      |
| -- chatExtra           | 	String  | 	X  | 	Meta information to pass for BC (consultation talk switch) / BT (bot switch) types                                                          |
| -- chatEvent           | 	String  | 	X  | 	Bot event name to connect for BT (bot switch) types                                                                     |
| -- target              | 	String  | 	X  | 	For web link types, adding the `"target":"out"` property opens an out-link<br>Sent as an in-app link by default                                    |
| - recipientGroupingKey | 	String  | 	X  | 	Recipient grouping key (up to 100 characters)                                                                           |
| messageOption          | Object   | 	X  | Message option                                                                                        |
| - price                | Integer  | 	X  | Price/amount/payment amount included in the message to be delivered to the user (related to moment advertisement)                                                   |
| - currencyType         | String   | 	X  | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message (message to be delivered to the user) (related to moment advertisement)                |
| statsId                | String   | 	X  | Statistics ID (not included in send search conditions; up to 8 characters)                                                            |

* <b>The request date and time can be set from the time of the call to up to 60 days later.</b>
* <b>Since fallback is made in the SMS service, field values must follow the API specifications for SMS (e.g., Sender number registered at the SMS service, or restriction in the field length). </b>
* <b>Alternative delivery is available via SMS and LMS. The SMS Service supports international SMS only. For international receiver numbers, the resendType (fallback type) must be changed to SMS to allow sending without fail.</b>
* <b>Alternative delivery titles or content that exceed the byte limit of the specified alternative delivery type may be truncated. (Refer to [[SMS Precautions](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>
* <b>You can apply a strikethrough style by appending a `\s` character to the end of the `templateTitle` and `templateItemHighlight.title` fields using a replacement variable and `templateParameter`.</b>
    * <b>However, this does not apply if `\s` is added to the field in advance when registering the template.</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"{sender key}","templateCode":"{template code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","templateParameter":{"{replacement field}":"{replacement data}"}}]}'
```

<a id="response"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "senderGroupingKey": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String,
        "recipientGroupingKey": String
      }
    ]
  }
}
```

| Name                      | Type      | Not Null | Description           |
|-------------------------|---------|:--------:|--------------|
| header                  | Object  |    O     | Header area        |
| - resultCode            | Integer |    O     | Result code        |
| - resultMessage         | String  |    O     | Result message       |
| - isSuccessful          | Boolean |    O     | Success        |
| message                 | Object  |    X     | Body area        |
| - requestId             | String  |    X     | Request ID       |
| - senderGroupingKey     | String  |    X     | Sender grouping key     |
| - sendResults           | Object  |    O     | Send request result     |
| -- recipientSeq         | Integer |    O     | Recipient sequence number   |
| -- recipientNo          | String  |    X     | Recipient number        |
| -- resultCode           | Integer |    O     | Send request result code  |
| -- resultMessage        | String  |    O     | Send request result message |
| -- recipientGroupingKey | String  |    X     | Recipient grouping key    |

<a id="request-of-sending-full-text"></a>

### Send Full Message Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name                       | 	Type     | 	Required | 	Description                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | Can be created in the console.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | Key used as the basis for duplicate message sending requests<br>If a request is made with the same key for 10 minutes, the request will be failed. |

[Request Body]

```
{
    "senderKey": String,
    "templateCode": String,
    "requestDate": String,
    "senderGroupingKey": String,
    "createUser": String,
    "recipientList": [
        {
            "recipientNo": String,
            "content": String,
            "templateTitle": String,
            "templateHeader": String,
            "templateItem": {
              "list": [{
                "title": String,
                "description": String
              }],
              "summary": {
                "title": String,
                "description": String
              }
            },
            "templateItemHighlight": {
              "title": String,
              "description": String,
              "imageUrl": String
            },
            "templateRepresentLink": {
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
            },
            "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "chatExtra": String,
                    "chatEvent": String,
                    "bizFormId": Integer,
                    "pluginId": String,
                    "relayId": String,
                    "oneClickId": String,
                    "productId": String,
                    "target": String,
                    "telNumber": String
                }
            ],
            "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "chatExtra": String,
                    "chatEvent": String,
                    "bizFormId": Integer,
                    "target": String
                }
            ],
            "resendParameter": {
              "isResend": boolean,
              "resendType": String,
              "resendTitle": String,
              "resendContent": String,
              "resendSendNo": String
            },
            "recipientGroupingKey": String
        }
    ],
    "messageOption": {
      "price": Integer,
      "currencyType": String
    },
    "statsId": String
}
```

| Name                      | 	Type      | 	Required | 	Description                                                                                                                                                                        |
|-------------------------|----------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey               | 	String  | 	O  | Sender key (40 characters)                                                                                                                                                                  |
| templateCode            | 	String  | 	O  | Registered delivery template code (up to 20 characters)                                                                                                                                                      |
| requestDate             | String   | X   | Request date and time (yyyy-MM-dd HH:mm)<br>(If not entered, the message is sent immediately)<br>Scheduling is available up to 60 days in advance                                                                                                                                                                         |
| senderGroupingKey       | String   | X   | Sender grouping key (up to 100 characters)                                                                                                                                                          |
| createUser              | String   | X   | Registrant (saved as user UUID when sending from the console)                                                                                                                                                |
| recipientList           | 	List    | 	O  | 	List of recipients (up to 1,000)                                                                                                                                                        |
| - recipientNo           | 	String  | 	O  | 	Recipient number (up to 15 characters)                                                                                                                                              |
| - content               | 	String  | 	O  | 	Content (up to 1,300 characters)                                                                                                                                              |
| - templateTitle         | String   | X   | Title (up to 50 characters)                                                                                                                                                                 |
| - templateHeader        | String   | X   | Template header (up to 16 characters)                                                                                                                                             |
| - templateItem          | Object   | X   | Item                                                                                                                                                                        |
| -- list                 | List     | X   | Item list (minimum 2, maximum 10)                                                                                                                                                     |
| --- title               | String   | X   | Title (up to 6 characters)                                                                                                                                                                 |
| --- description         | String   | X   | Description (up to 23 characters)                                                                                                                                              |
| -- summary              | Object   | X   | Item summary information                                                                                                                                                                  |
| --- title               | String   | X   | Title (up to 6 characters)                                                                                                                                                                 |
| --- description         | String   | X   | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                              |
| - templateItemHighlight | Object   | X   | Item highlight                                                                                                                                                                  |
| --- title               | String   | X   | Title (up to 30 characters, up to 21 characters if a thumbnail image is present)                                                                                                                                            |
| --- description         | String   | X   | Description (up to 19 characters, up to 13 characters if a thumbnail image is present)                                                                                                                                          |
| --- imageUrl            | String   | X   | Thumbnail image URL                                                                                                                                                                 |
| - templateRepresentLink | Object   | X   | Representative link                                                                                                                                                                      |
| -- linkMo               | String   | 	X  | 	Mobile web link (up to 500 characters)                                                                                                                                                         |
| -- linkPc               | String   | 	X  | PC web link (up to 500 characters)                                                                                                                                           |
| -- schemeIos            | String   | X   | 	iOS app link (up to 500 characters)                                                                                                                                                         |
| -- schemeAndroid        | String   | X   | 	Android app link (up to 500 characters)                                                                                                                                       |
| - buttons               | 	List    | 	X  | Button list (up to 5)                                                                                                                                                              |
| -- ordering             | 	Integer | 	X  | 	Button order (required if buttons are present)                                                                                                                                                       |
| -- type                 | String   | 	X  | 	Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| -- name                 | String   | 	X  | 	Button name (required if buttons are present, up to 14 characters)                                                                                                                                               |
| -- linkMo               | String   | 	X  | 	Mobile web link (required for the WL type, up to 500 characters)                                                                                                                                        |
| -- linkPc               | String   | 	X  | PC web link (optional for the WL type, up to 500 characters)                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOS app link (required for the AL type, up to 500 characters)                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	Android app link (required for the AL type, up to 500 characters)                                                                                                                                      |
| -- chatExtra            | 	String  | 	X  | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                                                    |
| -- chatEvent            | 	String  | 	X  | Bot event name to connect for BT (Bot Transfer) type buttons                                                                                                                               |
| -- bizFormId            | 	Integer | 	X  | 	Business form ID (required for the BF type)                                                                                                                                                    |
| -- pluginId             | 	String  | 	X  | 	Plugin ID (up to 24 characters)                                                                                                                                           |
| -- relayId              | 	String  | 	X  | Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed                                                                                                                            |
| -- oneClickId           | 	String  | 	X  | Payment information used in the one-click payment plugin                                                                                                                                                   |
| -- productId            | 	String  | 	X  | Payment information used in the one-click payment plugin                                                                                                                                                   |
| -- target               | 	String  | 	X  | 	For web link buttons, adding the `"target":"out"` attribute sets an outbound link<br>Sent as an in-app link by default                                                                                                                 |
| -- telNumber           | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                                                                                                    |
| - quickReplies          | 	List    | 	X  | Quick reply list (up to 5)                                                                                                                                                            |
| -- ordering             | 	Integer | 	X  | 	Quick reply order (required if quick reply is included)                                                                                                                                                   |
| -- type                 | String   | 	X  | 	Quick reply type (WL: Web Link, AL: App Link, BK: Bot Keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business Form)                                                                                                   |
| -- name                 | String   | 	X  | 	Quick reply name (required if quick reply is included, up to 14 characters)                                                                                                                                           |
| -- linkMo               | String   | 	X  | 	Mobile web link (required for the WL type, for up to 500 characters)                                                                                                                                        |
| -- linkPc               | String   | 	X  | PC web link (required for the WL type, for up to 500 characters)                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOS app link (required for the AL type, for up to 500 characters)                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	Android app link (required for the AL type, for up to 500 characters)                                                                                                                                      |
| -- bizFormId            | 	Integer | 	X  | 	Business form ID (required for the BF type)                                                                                                                                                    |
| -- target               | 	String  | 	X  | 	For the web link type, add the `"target":"out"` attribute to use an outbound link.<br>Sent as an in-app link by default.                                                                                                                 |
| - resendParameter       | 	Object  | 	X  | Alternative sending information                                                                                                                                                                   |
| -- isResend             | 	boolean | 	X  | 	Whether to send a text message as an alternative if delivery fails.<br>If alternative sending is configured in the console, it is sent as an alternative by default.                                                                                                                   |
| -- resendType           | 	String  | 	X  | 	Alternative sending type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                   |
| -- resendTitle          | 	String  | 	X  | 	LMS alternative sending title<br>(resent with the Plus Friend ID if value is unavailable.)                                                                                                                           |
| -- resendContent        | 	String  | 	X  | 	Alternative sending content<br>(resent with [Message body and web link button name - web link mobile link] if value is unavailable.)                                                                                                     |
| -- resendSendNo         | String   | X   | Sender number for alternative sending<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                              |
| - recipientGroupingKey  | 	String  | 	X  | 	Recipient grouping key (up to 100 characters)                                                                                                                                        |
| messageOption           | Object   | 	X  | Message option                                                                                                                                                                     |
| - price                 | Integer  | 	X  | Price/amount/payment amount included in the message to be delivered to the user (related to moment advertisement)                                                                                                                                |
| - currencyType          | String   | 	X  | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message to be delivered to the user (related to moment advertisement)                                                                                                             |
| statsId                 | String   | 	X  | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                         |

* <b>Enter the substitution-completed data in the body and buttons.</b>
* <b>The request date and time can be set from the time of the call up to 60 days later.</b>
* <b>Since fallback is made in the SMS service, field values must follow the API specifications for SMS (e.g., Sender number registered at the SMS service, or restriction in the field length).</b>
* <b>Fallback delivery is available via SMS and LMS. For international fallback delivery, only SMS is supported. For international receiver numbers, the resendType (fallback type) must be changed to SMS to allow sending without fail.</b>
* <b>Fallback delivery subject or content that exceeds the byte limit of the specified fallback delivery type may be truncated. (See [[SMS Cautions](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)])</b>
* <b>At the time of delivery, you can apply a strikethrough style by adding a `\s` character at the end of the templateTitle and templateItemHighlight.title fields.</b>
    * <b>However, this does not apply if `\s` is added to the fields in advance when registering a template.</b>

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/raw-messages -d '{"senderKey":"{Sender Key}","templateCode":"{Template Code}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{recipient number}","content":"{content}","buttons":[{"ordering":"{button order}","type":"{button type}","name":"{button name}","linkMo":"{mobile web link}"}]}]}'
```

<a id="response-2"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "message": {
    "requestId": String,
    "senderGroupingKey": String,
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String,
        "recipientGroupingKey": String
      }
    ]
  }
}
```

| Name                    | Type    | Not Null | Description                        |
|-------------------------|---------|:--------:|------------------------------------|
| header                  | Object  |    O     | Header area                        |
| - resultCode            | Integer |    O     | Result code                        |
| - resultMessage         | String  |    O     | Result message                     |
| - isSuccessful          | Boolean |    O     | Success                            |
| message                 | Object  |    X     | Body area                          |
| - requestId             | String  |    X     | Request ID                         |
| - senderGroupingKey     | String  |    X     | Sender grouping key                |
| - sendResults           | Object  |    O     | Delivery request result            |
| -- recipientSeq         | Integer |    O     | Recipient sequence number          |
| -- recipientNo          | String  |    X     | Recipient number                   |
| -- resultCode           | Integer |    O     | Result code of delivery request    |
| -- resultMessage        | String  |    O     | Result message of delivery request |
| -- recipientGroupingKey | String  |    X     | Recipient grouping key             |

<a id="list-messages"></a>

### List Messages

<a id="request"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Query parameter] No.1 or (2, 3) is conditionally required

| Name                   | 	Type      | 	Required        | 	Description                                                   |
|----------------------|----------|------------|-------------------------------------------------------|
| requestId            | 	String  | 	Conditionally required (No.1) | Request ID                                                |
| startRequestDate     | 	String  | 	Conditionally required (No.2) | Start date of delivery request (yyyy-MM-dd HH:mm)                       |
| endRequestDate       | 	String  | Conditionally required (No.2)  | 	End date of delivery request (yyyy-MM-dd HH:mm)                       |
| startCreateDate      | String   | Conditionally required (No.3)  | Start date of registration (yyyy-MM-dd HH:mm)                           |
| endCreateDate        | String   | Conditionally required (No.3)  | End date of registration (yyyy-MM-dd HH:mm)                            |
| recipientNo          | 	String  | 	X         | 	Recipient number                                                 |
| senderKey            | 	String  | 	X         | 	Sender Key                                                 |
| templateCode         | 	String  | 	X         | 	Template Code                                               |
| senderGroupingKey    | String   | X          | Sender grouping key                                              |
| recipientGroupingKey | 	String  | 	X         | 	Recipient grouping key                                            |
| messageStatus        | String   | 	X         | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled)	 |
| resultCode           | String   | 	X         | Delivery result (MRC01 -> successful, MRC02 -> failed)	                     |
| createUser           | String   | X          | Registrant (saved as user UUID when sending from console)                           |
| pageNum              | 	Integer | 	X         | 	Page number (Default: 1)                                   |
| pageSize             | 	Integer | 	X         | 	Number of queries (Default: 15, Max: 1000)                        |

* Delivery request data before 90 days cannot be queried.
* The range of delivery request dates is up to 30 days.

<a id="response-3"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messageSearchResultResponse": {
    "messages": [
    {
      "requestId": String,
      "recipientSeq": Integer,
      "plusFriendId": String,
      "senderKey": String,
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
      "senderGroupingKey": String,
      "recipientGroupingKey": String
    }
    ],
    "totalCount": Integer
  }
}
```

| Name                          | Type      | Not Null | Description                                                                                                                                                                     |
|-----------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                      | Object  |    O     | Header area                                                                                                                                                                  |
| - resultCode                | Integer |    O     | Result code                                                                                                                                                                  |
| - resultMessage             | String  |    O     | Result message                                                                                                                                                                 |
| - isSuccessful              | Boolean |    O     | Success                                                                                                                                                                  |
| messageSearchResultResponse | Object  |    X     | Body area                                                                                                                                                                  |
| - messages                  | List    |    O     | Message list                                                                                                                                                                |
| -- requestId                | String  |    O     | Request ID                                                                                                                                                                 |
| -- recipientSeq             | Integer |    O     | Recipient sequence number                                                                                                                                             |
| -- plusFriendId             | String  |    O     | PlusFriend ID                                                                                                                                                               |
| -- senderKey                | String  |    O     | Sender Key                                                                                                                                                                   |
| -- templateCode             | String  |    O     | Template Code                                                                                                                                                                 |
| -- recipientNo              | String  |    O     | Recipient number                                                                                                                                                                  |
| -- content                  | String  |    X     | Body                                                                                                                                                                     |
| -- requestDate              | String  |    O     | Request date and time                                                                                                                                                  |
| -- createDate               | String  |    O     | Registration date and time                                                                                                                                                  |
| -- receiveDate              | String  |    X     | Received date and time                                                                                                                                                  |
| -- resendStatus             | String  |    O     | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([See [Alternative Delivery Status Table](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] below) |
| -- resendStatusName         | String  |    O     | Status code name of resending                                                                                                                                           |
| -- messageStatus            | String  |    O     | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled)                                                                                                                                |
| -- createUser               | String  |    X     | Registrant (saved as user UUID when sending from console)                                                                                                                                            |
| -- resultCode               | String  |    X     | Recipient result code                                                                                                                                               |
| -- resultCodeName           | String  |    X     | Recipient result code name                                                                                                                                              |
| -- senderGroupingKey        | String  |    X     | Sender grouping key                                                                                                                                               |
| -- recipientGroupingKey     | String  |    X     | Recipient grouping key                                                                                                                                              |
| - totalCount                | Integer |    X     | Total count                                                                                                                                                                    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

<a id="get-messages"></a>

### Get Message

<a id="request-2"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name           | 	Type      | 	Description         |
|--------------|----------|-------------|
| appkey       | 	String  | 	Unique app key     |
| requestId    | 	String  | 	Request ID     |
| recipientSeq | 	Integer | 	Recipient sequence number |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

<a id="response-4"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "message": {
      "requestId": String,
      "recipientSeq": Integer,
      "plusFriendId": String,
      "senderKey": String,
      "templateCode": String,
      "recipientNo": String,
      "content": String,
      "templateTitle": String,
      "templateSubtitle": String,
      "templateExtra": String,
      "templateAd": String,
      "templateHeader": String,
      "templateItem": {
        "list": [{
          "title": String,
          "description": String
        }],
        "summary": {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight": {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink": {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "requestDate": String,
      "receiveDate": String,
      "createDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String,
      "createUser": String,
      "buttons": [
        {
          "ordering": Integer,
          "type": String,
          "name": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "bizFormId": Integer,
          "pluginId": String,
          "relayId": String,
          "oneClickId": String,
          "productId": String,
          "target": String,
          "telNumber": String
        }
      ],
      "quickReplies": [
        {
          "ordering": Integer,
          "type": String,
          "name": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
          "chatExtra": String,
          "chatEvent": String,
          "bizFormId": Integer,
          "target": String
          }
      ],
      "messageOption": {
        "price": Integer,
        "currencyType": String
      },
      "senderGroupingKey": String,
      "recipientGroupingKey": String
  }
}
```

-----

| Name | Type | Not Null | Description |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success |
| message | Object | X | Message |
| - requestId | String | O | Request ID |
| - recipientSeq | Integer | O | Recipient sequence number |
| - plusFriendId | String | O | PlusFriend ID |
| - senderKey | String | O | Sender Key |
| - templateCode | String | O | Template Code |
| - recipientNo | String | X | Recipient number |
| - content | String | X | Body |
| - templateTitle | String | X | Template title |
| - templateSubtitle | String | X | Template subtitle |
| - templateExtra | String | X | Template additional content |
| - templateAd | String | X | Request for consent of receiving within template or simple ad phrases |
| - templateHeader | String | X | Template header (up to 16 characters) |
| - templateItem | Object | X | Item |
| -- list | List | X | Item list (at least 2, up to 10) |
| --- title | String | X | Title (up to 6 characters) |
| --- description | String | X | Description (up to 23 characters) |
| -- summary | Object | X | Item summary information |
| --- title | String | X | Title (up to 6 characters) |
| --- description | String | X | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
| - templateItemHighlight | Object | X | Item highlight |
| --- title | String | X | Title (up to 30 characters, 21 characters with a thumbnail image) |
| --- description | String | X | Description (up to 19 characters, 13 characters with a thumbnail image) |
| --- imageUrl | String | X | Thumbnail image URL |
| - templateRepresentLink | Object | X | Representative link |
| -- linkMo | String | X | Mobile web link (up to 500 characters) |
| -- linkPc | String | X | PC web link (up to 500 characters) |
| -- schemeIos | String | X | iOS app link (up to 500 characters) |
| -- schemeAndroid | String | X | Android app link (up to 500 characters) |
| - requestDate | String | O | Request date and time |
| - receiveDate | String | X | Received date and time |
| - createDate | String | O | Registration date and time |
| - resendStatus | String | O | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>(See [[alternative delivery status table below](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)]) |
| - resendStatusName | String | O | Status code name of resending |
| - resendResultCode | String | X | Result code of resending [SMS result code](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | X | Resending SMS request ID |
| - messageStatus | String | O | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled) |
| - resultCode | String | X | Received result code |
| - resultCodeName | String | X | Received result code name |
| - createUser            | String  |    X     | Registrant (saved as user UUID when sending from the console)                                                                                                                                            |
| - buttons               | List    |    X     | Button list                                                                                                                                                                 |
| -- ordering             | Integer |    X     | Button order                                                                                                                                                                  |
| -- type                 | String  |    X     | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| -- name                 | String  |    X     | Button name                                                                                                                                                                  |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type)                                                                                                                              |
| -- linkPc               | String  |    X     | PC web link (optional for the WL type)                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type)                                                                                                                              |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type)                                                                                                                            |
| -- chatExtra            | String  |    X     | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                                                |
| -- chatEvent            | String  |    X     | Bot event name to connect for BT (Bot Transfer) type buttons                                                                                                                                           |
| -- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                                 |
| -- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                                        |
| -- relayId              | String  |    X     | Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed                                                                                                                        |
| -- oneClickId           | String  |    X     | Payment information used by the one-click payment plugin                                                                                                                                               |
| -- productId            | String  |    X     | Payment information used by the one-click payment plugin                                                                                                                                               |
| -- target               | String  |    X     | For web link buttons, adding the `"target":"out"` attribute sends as an out-link\<br\>Sent as an in-app link by default                                                                                                            |
| - quickReplies          | List    |    X     | Quick reply list (up to 5)                                                                                                                                                        |
| -- ordering             | Integer |    X     | Quick reply order (required if quick replies exist)                                                                                                                                                |
| -- type                 | String  |    X     | Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                                |
| -- name                 | String  |    X     | Quick reply name (required if quick replies exist, up to 14 characters)                                                                                                                                        |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type, up to 500 characters)                                                                                                                                     |
| -- linkPc               | String  |    X     | PC web link (optional for the WL type, up to 500 characters)                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type, up to 500 characters)                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type, up to 500 characters)                                                                                                                                   |
| -- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                        |
| -- target               | String  |    X     | For web link types, adding the `"target":"out"` attribute sends as an out-link\<br\>Sent as an in-app link by default                                                                                                            |
| -- telNumber           | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                                                    |
| - messageOption         | Object  |    X     | Message option                                                                                                                                                                 |
| -- price                | Integer |    X     | Price/amount/payment amount included in the message to be delivered to the user (related to moment advertisement)                                                                                                                            |
| -- currencyType         | String  |    X     | Use of international currency codes such as KRW, USD, EUR, which is the currency unit of the price/amount/payment amount included in the message to be delivered to the user (related to moment advertisement)                                                                                         |
| - senderGroupingKey     | String  |    X     | Sender grouping key                                                                                                                                                               |
| - recipientGroupingKey  | String  |    X     | Recipient grouping key                                                                                                                                              |

<a id="authentication-messages"></a>

## Authentication Messages

<span id="precautions-authword"></span>

1. Guide for authentication words required to be included for Authentication Messages API

| Category | Authentication Words |
|--------|--------------------------------------------|
| Authentication Messages | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- Example 1-1) Delivery shall fail if the full text (including template replacement) does not include authentication words, in the request of Authentication Messages API (for emergency)
- Example 1-2) Validity for English words shall be checked regardless of small or capital letters

<a id="request-of-sending-replaced-messages-2"></a>

### Send Replaced Message Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name                       | 	Type     | 	Required | 	Description                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | Can be created in the console.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | Key used as the standard for duplicate message delivery requests<br>If a request is made with the same key for 10 minutes, the request will be failed. |

[Request body]
[Same as above](./alimtalk-api-guide/#_3)

<a id="request-of-sending-full-text-2"></a>

### Send Full Text Message Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name                       | 	Type     | 	Required | 	Description                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | Can be created in the console.                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | Key used as the standard for duplicate message delivery requests<br>If a request is made with the same key for 10 minutes, the request will be failed. |

[Request Body]
[Same as above](./alimtalk-api-guide/#_5)

<a id="list-messages-2"></a>

### List Messages

<a id="request-3"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Query parameter]
[Same as above](./alimtalk-api-guide/#_7)

<a id="get-messages-2"></a>

### Get Message

<a id="request-4"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name           | 	Type      | 	Description         |
|--------------|----------|-------------|
| appkey       | 	String  | 	Unique app key     |
| requestId    | 	String  | 	Request ID     |
| recipientSeq | 	Integer | 	Recipient sequence number |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
```

<a id="response-5"></a>

#### Response

[Same as above](./alimtalk-api-guide/#_9)

<a id="message"></a>

## Messages

<a id="cancel-sending-messages"></a>

### Cancel Sending Messages

<a id="request-5"></a>

#### Request

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | 	Type   | 	Description    |
|-----------|---------|-----------------|
| appkey    | 	String | 	Unique app key |
| requestId | String  | Request ID      |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                          |
|--------------|---------|-----------|---------------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Query parameter]

| Name         | 	Type   | 	Required | 	Description                                                                           |
|--------------|---------|-----------|----------------------------------------------------------------------------------------|
| recipientSeq | 	String | 	X        | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

* Both general and authentication messages can be canceled by the same API.

<a id="response-6"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

<a id="query-updates-of-message-result"></a>

### Query Updates of Message Result

<a id="request-6"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | 	Type   | 	Description    |
|--------|---------|-----------------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                   |
|--------------|---------|-----------|--------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Query parameter]

| Name                | 	Type    | 	Required | 	Description                                              |
|---------------------|----------|-----------|-----------------------------------------------------------|
| startUpdateDate     | 	String  | 	O        | Start time of querying result updates (yyyy-MM-dd HH:mm)  |
| endUpdateDate       | 	String  | O         | End time of querying result updates (yyyy-MM-dd HH:mm)    |
| alimtalkMessageType | 	String  | X         | AlimTalk message type (NORMAL, AUTH)                      |
| pageNum             | 	Integer | 	X        | Page number (default: 1)                                  |
| pageSize            | 	Integer | 	X        | Number of queries (default: 15, max: 1000)                |

<a id="response-7"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "messages": [
    {
      "requestId": String,
      "recipientSeq": Integer,
      "requestDate": String,
      "createDate": String,
      "receiveDate": String,
      "resendStatus": String,
      "resendStatusName": String,
      "resendResultCode": String,
      "resendRequestId": String,
      "messageStatus": String,
      "resultCode": String,
      "resultCodeName": String
    }
  ]
}
```

| Name               | Type    | Not Null | Description                                                                                                                                                                        |
|--------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header             | Object  |    O     | Header area                                                                                                                                                                        |
| - resultCode       | Integer |    O     | Result code                                                                                                                                                                        |
| - resultMessage    | String  |    O     | Result message                                                                                                                                                                     |
| - isSuccessful     | Boolean |    O     | Success                                                                                                                                                                            |
| messages           | List    |    X     | Message list                                                                                                                                                                       |
| - requestId        | String  |    O     | Request ID                                                                                                                                                                         |
| - recipientSeq     | Integer |    O     | Recipient sequence number                                                                                                                                                          |
| - requestDate      | String  |    O     | Request date and time                                                                                                                                                              |
| - createDate       | String  |    O     | Creation date and time                                                                                                                                                             |
| - receiveDate      | String  |    X     | Receipt date and time                                                                                                                                                              |
| - resendStatus     | String  |    O     | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>(See [[SMS/LMS resending status table](http://docs.toast.com/en/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] below) |
| - resendStatusName | String  |    O     | Status code name of resending                                                                                                                                                      |
| - resendResultCode | String  |    X     | Result code of resending [SMS result code](https://docs.toast.com/en/Notification/SMS/ko/error-code/#api)                                                                          |
| - resendRequestId  | String  |    X     | Request ID of resending SMS                                                                                                                                                        |
| - messageStatus    | String  |    O     | Request status (COMPLETED -> successful, FAILED -> failed, CANCEL -> canceled)                                                                                                     |
| - resultCode       | String  |    X     | Receipt result code                                                                                                                                                                |
| - resultCodeName   | String  |    X     | Receipt result code name                                                                                                                                                           |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

<a id="query-the-number-of-message-result-updates"></a>

### Query the Number of Message Result Updates

<a id="request-7"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results/count
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | 	Type   | 	Description    |
|--------|---------|-----------------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                   |
|--------------|---------|-----------|--------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Query parameter]

| Name                | 	Type   | 	Required | 	Description                                             |
|---------------------|---------|-----------|----------------------------------------------------------|
| startUpdateDate     | 	String | 	O        | Start time of querying result updates (yyyy-MM-dd HH:mm) |
| endUpdateDate       | 	String | O         | End time of querying result updates (yyyy-MM-dd HH:mm)   |
| alimtalkMessageType | 	String | X         | AlimTalk message type (NORMAL, AUTH)                     |

<a id="response-8"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "totalCount": Integer
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |
| totalCount      | Integer |    O     | Total count    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results/count?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

<a id="status-code-of-smslms-resending"></a>

### SMS/LMS Resending Status Code

| Name  | 	Description                                                             |
|-------|--------------------------------------------------------------------------|
| RSC01 | 	Not a target of resending                                               |
| RSC02 | 	Target of resending (If sending fails, resending is performed.)         |
| RSC03 | 	Resending in progress                                                   |
| RSC04 | 	Resending successful                                                    |
| RSC05 | 	Resending failed                                                        |

<a id="mass-delivery"></a>

## Mass Delivery

<a id="list-mass-delivery-requests"></a>

### List Mass Delivery Requests

<a id="request-8"></a>

#### Request

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appKey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Description       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	Unique secret key |

[Query parameter]

* One of the following is required: requestId, startRequestDate + endRequestDate, or startCreateDate + endCreateDate.

| Name               | 	Type               | Max Length | 	Required | 	Description       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | Request ID     |
| startRequestDate | String            | -     | O   | Delivery start date  |
| endRequestDate   | String            | -     | O   | Delivery end date  |
| startCreateDate  | 	String           | -     | 	O  | 	Registration start date |
| endCreateDate    | 	String           | -     | 	O  | 	Registration end date |
| pageNum          | optional, Integer | -     | X   | Page number    |
| pageSize         | optional, Integer | 1000  | X   | Number of results      |

<a id="curl"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-9"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "body": {
    "messages": [
      {
        "requestId": String,
        "requestDate": String,
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "masterStatusCode": String,
        "content": String,
        "fileId": String,
        "autoSendYn": String,
        "statsId": String,
        "createDate": String,
        "createUser": String
      }
    ],
    "totalCount": Integer
  }
}
```

| Name                  | Type      | Not Null | Description                                                                             |
|---------------------|---------|:--------:|--------------------------------------------------------------------------------|
| header              | Object  |    O     | Header area                                                                          |
| - resultCode        | Integer |    O     | Result code                                                                          |
| - resultMessage     | String  |    O     | Result message                                                                         |
| - isSuccessful      | Boolean |    O     | Success                                                                          |
| body                | Object  |    X     | Body area                                                                          |
| - messages          | Object  |    X     | Message list                                                                        |
| -- requestId        | String  |    O     | Request ID                                                                          |
| -- requestDate      | String  |    O     | Request date                                                                          |
| -- plusFriendId     | String  |    O     | PlusFriend ID                                                                      |
| -- senderKey        | String  |    O     | Sender key (40 characters)                                                                      |
| -- masterStatusCode | String  |    O     | Mass delivery status code (WAIT, READY, SENDREADY, SENDWAIT, SENDING, COMPLETE, CANCEL, FAIL) |
| -- content          | String  |    X     | Content                                                                             |
| -- fileId           | String  |    X     | Attachment file ID                                                                       |
| -- templateCode     | String  |    O     | Template Code (up to 20 characters)                                                                 |
| -- autoSendYn       | String  |    X     | Auto-send enabled                                                                       |
| -- statsId          | String  |    X     | Statistics ID                                                                          |
| -- createDate       | String  |    O     | Creation date                                                                          |
| -- createUser       | String  |    X     | User who created the request (saved as user UUID when sending from console)                                                 |
| - totalCount        | Integer |    X     | Total count                                                                            |

<a id="list-mass-delivery-recipients"></a>

### Retrieve Recipients of Mass Delivery Requests

<a id="request-9"></a>

#### Request

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | 	Type   | 	Description  |
|-----------|---------|---------------|
| appKey    | 	String | 	Unique app key |
| requestId | 	String | 	Request ID    |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Description        |
|--------------|---------|---------------------|
| X-Secret-Key | 	String | 	Unique secret key   |

| Name             | 	Type             | Max Length | 	Required | 	Description          |
|------------------|-------------------|------------|-----------|------------------------|
| requestId        | String            | -          | O         | Request ID             |
| startRequestDate | String            | -          | X         | Delivery start date    |
| endRequestDate   | String            | -          | X         | Delivery end date      |
| startCreateDate  | 	String           | -          | 	X        | 	Registration start date |
| endCreateDate    | 	String           | -          | 	X        | 	Registration end date |
| pageNum          | optional, Integer | -          | X         | Page number            |
| pageSize         | optional, Integer | 1000       | X         | Number of results      |

<a id="curl-2"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-10"></a>

#### Response

```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
    },
    "body": {
        "recipients": [
            {
                "requestId": String,
                "recipientSeq": Integer,
                "recipientNo": String,
                "requestDate": String,
                "receiveDate": String,
                "messageStatus": String,
                "resultCode": String,
                "resultCodeName": String
            }
        ],
        "totalCount": Integer
    }
}
```

| Name              | Type    | Not Null |Description             |
|-------------------|---------|:--------:|------------------------|
| header            | Object  |    O     | Header area            |
| - resultCode      | Integer |    O     | Result code            |
| - resultMessage   | String  |    O     | Result message         |
| - isSuccessful    | Boolean |    O     | Success                |
| body              | Object  |    X     | Body area              |
| - messages        | Object  |    X     | Message list           |
| -- requestId      | String  |    O     | Request ID             |
| -- recipientSeq   | String  |    O     | Recipient sequence number |
| -- recipientNo    | String  |    X     | Recipient number       |
| -- requestDate    | String  |    O     | Request date           |
| -- receiveDate    | String  |    X     | Received date          |
| -- messageStatus  | String  |    O     | Message status         |
| -- resultCode     | String  |    X     | Result code            |
| -- resultCodeName | String  |    X     | Result code description |
| - totalCount      | Integer |    X     | Total count            |

<a id="get-a-mass-delivery-recipient"></a>

### Retrieve a Mass Delivery Recipient

<a id="request-10"></a>

#### Request

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description       |
|--------------|---------|--------------------|
| appKey       | 	String | Unique app key     |
| requestId    | 	String | Request ID         |
| recipientSeq | String  | Recipient sequence |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Description        |
|--------------|---------|---------------------|
| X-Secret-Key | 	String | 	Unique secret key  |

| Name             | 	Type             | Max Length | 	Required | 	Description           |
|------------------|-------------------|------------|-----------|------------------------|
| requestId        | String            | -          | O         | Request ID             |
| startRequestDate | String            | -          | X         | Delivery start date    |
| endRequestDate   | String            | -          | X         | Delivery end date      |
| startCreateDate  | 	String           | -          | 	X        | 	Registration start date |
| endCreateDate    | 	String           | -          | 	X        | 	Registration end date |
| pageNum          | optional, Integer | -          | X         | Page number            |
| pageSize         | optional, Integer | 1000       | X         | Search count           |

<a id="curl-3"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/1" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-11"></a>

#### Response

```
{
    "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
    },
    "body": {
        "requestId": String,
        "recipientSeq": Integer,
        "plusFriendId": String,
        "senderKey": String,
        "templateCode": String,
        "recipientNo": String,
        "content": String,
        "templateTitle": String,
        "templateSubtitle": String,
        "templateExtra": String,
        "templateAd": String,
        "templateHeader": String,
        "templateItem": {
          "list": [{
            "title": String,
            "description": String
          }],
          "summary": {
            "title": String,
            "description": String
          }
        },
        "templateItemHighlight": {
          "title": String,
          "description": String,
          "imageUrl": String
        },
        "templateRepresentLink": {
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String,
        },
        "requestDate": String,
        "receiveDate": String,
        "createDate": String,
        "resendStatus": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "messageStatus": String,
        "resultCode": String,
        "resultCodeName": String,
        "createUser": String,
        "buttons": [
          {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormId": Integer,
            "pluginId": String,
            "relayId": String,
            "oneClickId": String,
            "productId": String,
            "target": String,
            "telNumber": String
          }
        ],
        "quickReplies": [
          {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormId": Integer,
            "target": String
            }
        ]
    }
}
```

| Name | Type | Not Null | Description |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success |
| body | Object | X | Body area |
| - requestId | String | O | Request ID |
| - recipientSeq | String | O | Recipient sequence number |
| - plusFriendId | String | O | PlusFriend ID |
| - senderKey | String | O | Sender ID |
| - templateCode | String | O | Template Code (up to 20 characters) |
| - recipientNo | String | X | Recipient number |
| - content | String | X | Content |
| - tempalteTitle | String | X | Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters) |
| - templateSubtitle | String | X | Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters) |
| - templateExtra | String | X | Additional template information (Required, if template message type is [Ad Included/Mixed Purposes]) |
| - templateAd | String | X | Request for consent of receiving within template or simple ad phrases |
| - templateHeader | String | X | Template header (up to 16 characters) |
| - templateItem | Object | X | Item |
| -- list | List | X | Item list (minimum 2, maximum 10) |
| --- title | String | X | Title (up to 6 characters) |
| --- description | String | X | Description (up to 23 characters) |
| -- summary | Object | X | Item summary information |
| --- title | String | X | Title (up to 6 characters) |
| --- description | String | X | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters) |
| - templateItemHighlight | Object | X | Item highlight |
| --- title | String | X | Title (up to 30 characters, 21 characters with a thumbnail image) |
| --- description | String | X | Description (up to 19 characters, 13 characters with a thumbnail image) |
| --- imageUrl | String | X | Thumbnail image URL |
| - templateRepresentLink | Object | X | Representative link |
| -- linkMo | String | X | Mobile web link (up to 500 characters) |
| -- linkPc | String | X | PC web link (up to 500 characters) |
| -- schemeIos | String | X | iOS app link (up to 500 characters) |
| -- schemeAndroid | String | X | Android app link (up to 500 characters) |
| - requestDate | String | O | Request date |
| - receiveDate | String | X | Received date |
| - createDate | String | O | Created date |
| - resendStatus | String | O | Status code of resending (RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[See the alternative delivery status table below](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)]) |
| - resendStatusName | String | O | Status name of resending |
| - resendResultCode | String | O | Result code of resending [SMS result code](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api) |
| - resendRequestId | String | X | Request ID of resending |
| - messageStatus | String | O | Mass recipient delivery status code (READY, COMPLETED, FAILED, CANCEL) |
| - resultCode | String | X | Result status code |
| - resultCodeName | String | X | Result status name |
| - createUser            | String  |    X     | Created user (saved as user UUID when sent from the console)                                                                                                                                         |
| - buttons               | List    |    X     | Button list                                                                                                                                                                 |
| -- ordering             | Integer |    X     | Button order                                                                                                                                                                  |
| -- type                 | String  |    X     | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| -- name                 | String  |    X     | Button name                                                                                                                                                                  |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type)                                                                                                                                              |
| -- linkPc               | String  |    X     | PC web link (optional for the WL type)                                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type)                                                                                                                                              |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type)                                                                                                                                            |
| -- chatExtra            | String  |    X     | Meta information to send for BC (Bot for Consultation) or BT (Bot Transfer) type buttons                                                                                                                                |
| -- chatEvent            | String  |    X     | Bot event name to connect for BT (Bot Transfer) type buttons                                                                                                                           |
| -- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                                 |
| -- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                                        |
| -- relayId              | String  |    X     | Value passed via the X-Kakao-Plugin-Relay-Id header when the plugin is executed                                                                                                                        |
| -- oneClickId           | String  |    X     | Payment information used by the one-click payment plugin                                                                                                                                               |
| -- productId            | String  |    X     | Payment information used by the one-click payment plugin                                                                                                                                               |
| -- target               | String  |    X     | For web link buttons, adding the `"target":"out"` attribute sends an out-link\<br\>Sent as an in-app link by default                                                                                                            |
| -- telNumber           | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                                                                                                    |
| - quickReplies          | List    |    X     | Quick reply list (up to 5)                                                                                                                                                        |
| -- ordering             | Integer |    X     | Quick reply order (required if quick replies are present)                                                                                                                                                |
| -- type                 | String  |    X     | Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                                |
| -- name                 | String  |    X     | Quick reply name (required if quick replies are present, up to 14 characters)                                                                                                                                        |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type, up to 500 characters)                                                                                                                                     |
| -- linkPc               | String  |    X     | PC web link (optional for the WL type, up to 500 characters)                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type, up to 500 characters)                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type, up to 500 characters)                                                                                                                                   |
| -- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                                        |
| -- target               | String  |    X     | For web link types, adding the `"target":"out"` attribute sends an out-link\<br\>Sent as an in-app link by default                                                                                                            |

<a id="templates"></a>

## Templates

<a id="list-template-categories"></a>

### Query Template Categories

<a id="request-11"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

<a id="response-12"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "categories": [
    {
      "name": String,
      "subCategories": [
        {
          "code": String,
          "name": String,
          "groupName": String,
          "inclusion": String,
          "exclusion": String
        }
      ]
    }
  ]
}
```

| Name              | Type      | Not Null | Description                       |
|-----------------|---------|:--------:|--------------------------|
| header          | Object  |    O     | Header area                    |
| - resultCode    | Integer |    O     | Result code                    |
| - resultMessage | String  |    O     | Result message                   |
| - isSuccessful  | Boolean |    O     | Success                    |
| categories      | List    |    X     | Category list                 |
| - name          | String  |    X     | Category name                  |
| - subCategories | List    |    X     | Sub-category list              |
| -- code         | String  |    X     | Category code (Used when registering/modifying templates) |
| -- name         | String  |    X     | Category name                  |
| -- groupName    | String  |    X     | Category group name                 |
| -- inclusion    | String  |    X     | Description of templates to which the category applies        |
| -- exclusion    | String  |    X     | Description of templates to which the category does not apply        |

<a id="register-templates"></a>

### Register Templates

<a id="request-12"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name        | 	Type     | 	Description     |
|-----------|---------|---------|
| appkey    | 	String | 	Unique app key |
| senderKey | 	String | 	Sender Key   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request Body]

```
{
  "templateCode": String,
  "templateName": String,
  "templateContent": String,
  "templateMessageType": String,
  "templateEmphasizeType": String,
  "templateExtra": String,
  "templateTitle": String,
  "templateSubtitle": String,
  "templateHeader": String,
  "templateItem": {
    "list": [{
      "title": String,
      "description": String
    }],
    "summary": {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight": {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink": {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName": String,
  "templateImageUrl": String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer,
      "pluginId": String,
      "telNumber": String
    }
  ],
  "quickReplies": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer
    }
  ]
}
```

| Name                  | Type     | Required | Description                                                                                                                                                                                                                                                                                               |
|-----------------------|----------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateCode          | String   | O        | Template code (up to 20 characters)                                                                                                                                                                                                                                                                       |
| templateName          | String   | O        | Template name (up to 150 characters)                                                                                                                                                                                                                                                                      |
| templateContent       | String   | O        | Template body (up to 1,300 characters)                                                                                                                                                                                                                                                                    |
| templateMessageType   | String   | X        | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: BA)                                                                                                                                                                                            |
| templateEmphasizeType | String   | X        | Template emphasis display type (NONE: Default, TEXT: Emphasis, IMAGE: Image type, ITEM_LIST: Item list type, default: NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required<br>- IMAGE: templateImageName and templateImageUrl fields are required<br>- ITEM_LIST: At least one of image, header, item highlight, or item list is required |
| templateExtra         | String   | X        | Template extra information (required if the template message type is [Extra Information/Mixed Purposes])                                                                                                                                                                                                  |
| tempalteTitle         | String   | X        | Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters)                                                                                                     |
| templateSubtitle      | String   | X        | Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters)                                                                                                                                        |
| templateHeader        | String   | X        | Template header (up to 16 characters)                                                                                                                                                                                                                                                                     |
| templateItem          | Object   | X        | Item                                                                                                                                                                                                                                                                                                      |
| - list                | List     | X        | Item list (minimum 2, maximum 10)                                                                                                                                                                                                                                                                         |
| -- title              | String   | X        | Title (up to 6 characters)                                                                                                                                                                                                                                                                                |
| -- description        | String   | X        | Description (up to 23 characters)                                                                                                                                                                                                                                                                         |
| - summary             | Object   | X        | Item summary information                                                                                                                                                                                                                                                                                  |
| -- title              | String   | X        | Title (up to 6 characters)                                                                                                                                                                                                                                                                                |
| -- description        | String   | X        | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                                                                                                        |
| templateItemHighlight | Object   | X        | Item highlight                                                                                                                                                                                                                                                                                            |
| - title               | String   | X        | Title (up to 30 characters; up to 21 characters if a thumbnail image is present)                                                                                                                                                                                                                          |
| - description         | String   | X        | Description (up to 19 characters; up to 13 characters if a thumbnail image is present)                                                                                                                                                                                                                    |
| - imageUrl            | String   | X        | Thumbnail image URL                                                                                                                                                                                                                                                                                       |
| templateRepresentLink | Object   | X        | Representative link                                                                                                                                                                                                                                                                                       |
| - linkMo              | String   | X        | Mobile web link (up to 500 characters)                                                                                                                                                                                                                                                                    |
| - linkPc              | String   | X        | PC web link (up to 500 characters)                                                                                                                                                                                                                                                                        |
| - schemeIos           | String   | X        | iOS app link (up to 500 characters)                                                                                                                                                                                                                                                                       |
| - schemeAndroid       | String   | X        | Android app link (up to 500 characters)                                                                                                                                                                                                                                                                   |
| templateImageName     | String   | X        | Image name (name of the uploaded file)                                                                                                                                                                                                                                                                    |
| templateImageUrl      | String   | X        | Image URL                                                                                                                                                                                                                                                                                                 |
| securityFlag          | Boolean  | X        | Whether it is a security template<br>Set this for security messages such as OTP<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false)                                                                                                    |
| categoryCode          | String   | X        | Template category code (refer to the Get Template Categories API, default: 999999)<br>If the category is "Other", the template is reviewed with the lowest priority                                                                                                                                       |
| buttons               | List     | X        | Button list (up to 5)                                                                                                                                                                                                                                                                                     |
| - ordering            | Integer  | X        | Button order (1–5)                                                                                                                                                                                                                                                                                        |
| - type                | String   | X        | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| - name                | String   | X        | Button name (required if a button is present; up to 14 characters)                                                                                                                                                                                                                                        |
| - linkMo              | String   | 	X  | 	Mobile web link (required for the WL type, for up to 500 characters)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC web link (optional for the WL type, for up to 500 characters)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS app link (required for the AL type, for up to 500 characters)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	Android app link (required for the AL type, for up to 500 characters)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	Business form ID (required for the BF type)                                                                                                                                                                                                                         |
| - pluginId            | 	String  | 	X  | 	Plugin ID (up to 24 characters)                                                                                                                                                                                                                                |
| -- telNumber           | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                    |
| quickReplies          | 	List    | 	X  | Quick reply list (up to 5)                                                                                                                                                                                                                                 |
| - ordering            | 	Integer | 	X  | 	Quick reply order (required when quick reply exists)                                                                                                                                                                                                                        |
| - type                | String   | 	X  | 	Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                                                                                                        |
| - name                | String   | 	X  | 	Quick reply name (required when quick reply exists, up to 14 characters)                                                                                                                                                                                                                |
| - linkMo              | String   | 	X  | 	Mobile web link (required for the WL type, for up to 500 characters)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC web link (optional for the WL type, for up to 500 characters)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS app link (required for the AL type, for up to 500 characters)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	Android app link (required for the AL type, for up to 500 characters)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	Business form ID (required for the BF type)                                                                                                                                                                                                                         |

* The templateAd value is fixed when registering the AD included (AD) or Mixed Purposes (MI) message type template.
* The Add Channel (AC) button must be in the first position when registering the AD included (AD) or Mixed Purposes (MI) message type template.
* The Add Channel (AC) button name must be registered as "Add channel".

Refer to the table below for whether replacement variables (#{variable}) can be used for each field.

| Category | Field | Replaceable |
|------|------|---------|
| Basic | templateContent | O |
| Basic | templateTitle | O |
| Basic | templateSubtitle | X |
| Basic | templateHeader | O |
| Basic | templateExtra | X |
| Basic | templateAd | X |
| Button | name | X |
| Button | linkMo, linkPc, schemeIos, schemeAndroid | O |
| Quick Reply | name | X |
| Quick Reply | linkMo, linkPc, schemeIos, schemeAndroid | O |
| Template item | title | X |
| Template item | description | O |
| Template item summary | title | X |
| Template item summary | description | O |
| Template item highlight | title | O |
| Template item highlight | description | O |
| Template item highlight | imageUrl | X |
| Representative link | linkMo, linkPc, schemeIos, schemeAndroid | O |

<a id="response-13"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name | Type | Not Null | Description |
|-----------------|---------|:--------:|--------|
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success |

<a id="modify-templates"></a>

### Modify Templates

<a id="request-13"></a>

#### Request

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

| Name           | 	Type     | 	Description     |
|--------------|---------|---------|
| appkey       | 	String | 	Unique app key |
| senderKey    | 	String | 	Sender key   |
| templateCode | 	String | 	Template code |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request Body]

```
{
  "templateName": String,
  "templateContent": String,
  "templateMessageType": String,
  "templateEmphasizeType": String,
  "templateExtra": String,
  "templateTitle": String,
  "templateSubtitle": String,
  "templateHeader": String,
  "templateItem": {
    "list": [{
      "title": String,
      "description": String
    }],
    "summary": {
      "title": String,
      "description": String
    }
  },
  "templateItemHighlight": {
    "title": String,
    "description": String,
    "imageUrl": String
  },
  "templateRepresentLink": {
    "linkMo": String,
    "linkPc": String,
    "schemeIos": String,
    "schemeAndroid": String,
  },
  "templateImageName": String,
  "templateImageUrl": String,
  "securityFlag": Boolean,
  "categoryCode": String,
  "buttons": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer,
      "pluginId": String,
      "telNumber": String
    }
  ],
  "quickReplies": [
    {
      "ordering": Integer,
      "type": String,
      "name": String,
      "linkMo": String,
      "linkPc": String,
      "schemeIos": String,
      "schemeAndroid": String
      "bizFormId": Integer
    }
  ]
}
```

| Name                  | 	Type     | 	Required | 	Description                                                                                                                                                                                                                                                        |
|-----------------------|----------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName          | 	String  | 	O        | Template name (up to 150 characters)                                                                                                                                                                                                                                |
| templateContent       | 	String  | 	O        | Template body (up to 1,300 characters)                                                                                                                                                                                                                              |
| templateMessageType   | String   | X         | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic)                                                                                                                                                   |
| templateEmphasizeType | String   | X         | Template emphasis display type (NONE: Default, TEXT: Emphasis, IMAGE: Image type, default: NONE)<br>- TEXT: templateTitle and templateSubtitle fields are required<br>- IMAGE: templateImageName and templateImageUrl fields are required                            |
| templateExtra         | String   | X         | Template extra information (required if the template message type is [Extra Information/Mixed Purposes])                                                                                                                                                            |
| tempalteTitle         | String   | X         | Template title (No more than 50 characters, Android: To be abbreviated if it exceeds 2 lines with more than 23 characters, iOS: To be abbreviated if it exceeds 2 lines with more than 27 characters)                                                               |
| templateSubtitle      | String   | X         | Auxiliary template phrase (No more than 50 characters, Android: To be abbreviated if it exceeds 18 characters, iOS: To be abbreviated if it exceeds 21 characters)                                                                                                  |
| templateHeader        | String   | X         | Template header (up to 16 characters)                                                                                                                                                                                                                               |
| templateItem          | Object   | X         | Item                                                                                                                                                                                                                                                                |
| - list                | List     | X         | Item list (minimum 2, maximum 10)                                                                                                                                                                                                                                   |
| -- title              | String   | X         | Title (up to 6 characters)                                                                                                                                                                                                                                          |
| -- description        | String   | X         | Description (up to 23 characters)                                                                                                                                                                                                                                   |
| - summary             | Object   | X         | Item summary information                                                                                                                                                                                                                                             |
| -- title              | String   | X         | Title (up to 6 characters)                                                                                                                                                                                                                                          |
| -- description        | String   | X         | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                                                                  |
| templateItemHighlight | Object   | X         | Item highlight                                                                                                                                                                                                                                                      |
| - title               | String   | X         | Title (up to 30 characters; up to 21 characters if a thumbnail image is present)                                                                                                                                                                                    |
| - description         | String   | X         | Description (up to 19 characters; up to 13 characters if a thumbnail image is present)                                                                                                                                                                              |
| - imageUrl            | String   | X         | Thumbnail image URL                                                                                                                                                                                                                                                 |
| templateRepresentLink | Object   | X         | Representative link                                                                                                                                                                                                                                                 |
| - linkMo              | String   | 	X        | 	Mobile web link (up to 500 characters)                                                                                                                                                                                                                             |
| - linkPc              | String   | 	X        | PC web link (up to 500 characters)                                                                                                                                                                                                                                  |
| - schemeIos           | String   | X         | 	iOS app link (up to 500 characters)                                                                                                                                                                                                                                |
| - schemeAndroid       | String   | X         | 	Android app link (up to 500 characters)                                                                                                                                                                                                                            |
| templateImageName     | String   | 	X        | Image name (name of the uploaded file)                                                                                                                                                                                                                              |
| templateImageUrl      | String   | 	X        | Image URL                                                                                                                                                                                                                                                           |
| securityFlag          | Boolean  | X         | Whether it is a security template<br>Set this if the message contains security content such as OTPs<br>If set, message text is unexposed to all devices except for the main device at the time of sending (default: false)                                          |
| categoryCode          | String   | X         | Template category code (refer to the Query Template Categories API, default: 999999)<br>If the category is "Other," the template is reviewed at the lowest priority                                                                                                 |
| buttons               | 	List    | 	X        | Button list (up to 5)                                                                                                                                                                                                                                               |
| - ordering            | 	Integer | 	X        | Button order (1–5)                                                                                                                                                                                                                                                  |
| - type                | String   | 	X        | 	Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| - name                | String   | 	X        | 	Button name (required if a button exists, up to 14 characters)                                                                                                                                                                                                     |
| - linkMo              | String   | 	X        | 	Mobile web link (required field for WL type, up to 500 characters)                                                                                                                                                                                                 |
| - linkPc              | String   | 	X        | PC web link (optional field for WL type, up to 500 characters)                                                                                                                                                                                                      |
| - schemeIos           | String   | X         | 	iOS app link (required field for AL type, up to 500 characters)                                                                                                                                                                                                    |
| - schemeAndroid       | String   | X         | 	Android app link (required field for AL type, up to 500 characters)                                                                                                                                                                                                |
| - bizFormId           | 	Integer | 	X        | 	Business form ID (required for BF type)                                                                                                                                                                                                                            |
| - pluginId            | 	String  | 	X        | 	Plugin ID (up to 24 characters)                                                                                                                                                                                                                                    |
| -- telNumber          | 	String  | 	X        | Phone number to be delivered for TN (Call) type buttons                                                                                                                                                                                                             |
| quickReplies          | 	List    | 	X        | Quick Reply list (up to 5)                                                                                                                                                                                                                                          |
| - ordering            | 	Integer | 	X        | 	Quick Reply order (required if a Quick Reply exists)                                                                                                                                                                                                               |
| - type                | String   | 	X        | 	Quick Reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                                                                      |
| - name                | String   | 	X        | 	Quick Reply name (required if a Quick Reply exists, up to 14 characters)                                                                                                                                                                                           |
| - linkMo              | String   | 	X        | 	Mobile web link (required field for WL type, up to 500 characters)                                                                                                                                                                                                 |
| - linkPc              | String   | 	X  | PC web link (optional for the WL type, for up to 500 characters)                                                                                                                          |
| - schemeIos           | String   | X   | 	iOS app link (required for the AL type, for up to 500 characters)                                                                                                                        |
| - schemeAndroid       | String   | X   | 	Android app link (required for the AL type, for up to 500 characters)                                                                                                                      |
| - bizFormId           | 	Integer | 	X  | 	Business form ID (required for the BF type)                                                                                                                                                    |

* The templateAd value is fixed when modifying the AD included (AD) or Mixed Purposes (MI) message type template.
* The Add Chanel button must be in the first when modifying the AD included (AD) or Mixed Purposes (MI) message type template.
* The button name of the Add Channel (AC) button must be fixed to "Add Channel" and modified accordingly.

<a id="response-14"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | Header area  |
| - resultCode    | Integer |    O     | Result code  |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success |

<a id="delete-templates"></a>

### Delete Template

<a id="request-14"></a>

#### Request

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description      |
|--------------|---------|-------------------|
| appkey       | 	String | 	App Key           |
| senderKey    | 	String | 	Sender Key        |
| templateCode | 	String | 	Template Code     |

[Header]

```
{
  "X-Secret-Key": String
}
```

* When an approved template is deleted, it is only deleted within NHN Cloud. (Only templates that have not been sent for 3 days can be deleted.)
* In the case of an approved template, Kakao's internal data cannot be deleted due to the restrictions of KakaoTalk BizMessage.
* A template remaining in Kakao becomes dormant if it is not used for 1 year, and gets deleted if it remains dormant for 1 year. (If a template becomes dormant or gets deleted on Kakao, the person in charge will be notified.)

<a id="response-15"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

<a id="inquire-of-templates"></a>

### Inquire About a Template

<a id="request-15"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description  |
|--------------|---------|---------------|
| appkey       | 	String | 	Appkey        |
| senderKey    | 	String | 	Sender Key    |
| templateCode | 	String | 	Template Code |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                    |
|--------------|---------|-----------|----------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Request Body]

```
{
  "comment": String
}
```

| Name    | 	Type   | 	Required | 	Description    |
|---------|---------|-----------|-----------------|
| comment | 	String | 	O        | Inquiry content |

* When commenting a template in the REJ status, it will be changed to the REQ status.

<a id="response-16"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

<a id="send-inquiry-on-templates-with-file-attachment"></a>

### Add a Comment with File Attachment to a Template

<a id="request-16"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description     |
|--------------|---------|-----------------|
| appkey       | 	String | 	App Key         |
| senderKey    | 	String | 	Sender Key      |
| templateCode | 	String | 	Template Code   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                          |
|--------------|---------|-----------|---------------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Request Body]

```
{
  "comment": String,
  "attachments": File
}
```

| Name        | 	Type      | 	Required | 	Description                                                                                                                                                                        |
|-------------|------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| comment     | 	String    | 	O        | Comment content                                                                                                                                                                     |
| attachments | List<File> | X         | List of attachments (up to 10)<br>- Supported extensions: .png, .jpg, .jpeg, .gif, .pdf, .hwp, .doc, .docx<br>- Maximum size per file: 50 MB<br>- Maximum total upload size: 100 MB |

* When commenting a template in the REJ status, it will be changed to the REQ status.

<a id="response-17"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

<a id="change-template-to-channel-add-type"></a>

### Change Template to Add Channel Type

<a id="request-17"></a>

#### Request

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appKey}/senders/{senderKey}/templates/{templateCode}/convert-add-channel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description    |
|--------------|---------|-----------------|
| appkey       | 	String | 	Unique app key |
| senderKey    | 	String | 	Sender key     |
| templateCode | 	String | 	Template code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                        |
|--------------|---------|-----------|-------------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

* Templates registered to a group Sender Profile cannot be converted to the Add Channel type.

<a id="response-18"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

<a id="single-query-for-template"></a>

### Get Template

<a id="request-18"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description  |
|--------------|---------|---------------|
| appkey       | 	String | 	Unique app key |
| senderKey    | 	String | 	Sender Key    |
| templateCode | String  | Template Code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                    |
|--------------|---------|-----------|----------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

| Template Status Code | Description |
|----------------------|-------------|
| TSC01                | Requested   |
| TSC02                | Reviewing   |
| TSC03                | Approved    |
| TSC04                | Rejected    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

<a id="response-19"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templates": {
      "plusFriendId": String,
      "senderKey": String,
      "plusFriendType": String,
      "templateCode": String,
      "kakaoTemplateCode": String,
      "templateName": String,
      "templateMessageType": String,
      "templateEmphasizeType": String,
      "templateContent": String,
      "templateExtra": String,
      "templateAd": String,
      "templateTitle": String,
      "templateSubtitle": String,
      "templateHeader": String,
      "templateItem": {
        "list": [{
          "title": String,
          "description": String
        }],
        "summary": {
          "title": String,
          "description": String
        }
      },
      "templateItemHighlight": {
        "title": String,
        "description": String,
        "imageUrl": String
      },
      "templateRepresentLink": {
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
      },
      "templateImageName": String,
      "templateImageUrl": String,
      "buttons": [
        {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer,
            "pluginId": String,
            "telNumber": String
        }
      ],
      "quickReplies": [
        {
            "ordering": Integer,
            "type": String,
            "name": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer
        }
      ],
      "comments": [
          {
              "id": Integer,
              "content": String,
              "userName": String,
              "createdAt": String,
              "attachment": [{
                "originalFileName": String,
                "filePath": String
              }],
              "status": String
          }  
      ],
      "status": String,
      "statusName": String,
      "securityFlag": Boolean,
      "categoryCode": String,
      "block": Boolean,
      "dormant": Boolean,
      "createDate": String,
      "updateDate": String
  }
}
```

| Name                    | Type    | Not Null | Description                                                                                                                                                                                                                  |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | Header area                                                                                                                                                                                                                  |
| - resultCode            | Integer |    O     | Result code                                                                                                                                                                                                                  |
| - resultMessage         | String  |    O     | Result message                                                                                                                                                                                                               |
| - isSuccessful          | Boolean |    O     | Success                                                                                                                                                                                                                      |
| templates               | Object  |    X     | Template list                                                                                                                                                                                                                |
| - plusFriendId          | String  |    O     | KakaoTalk Channel search ID or Sender Profile group name                                                                                                                                                                     |
| - senderKey             | String  |    O     | Sender Key                                                                                                                                                                                                                   |
| - plusFriendType        | String  |    O     | PlusFriend type (NORMAL, GROUP)                                                                                                                                                                                              |
| - templateCode          | String  |    O     | Template Code                                                                                                                                                                                                                |
| - kakaoTemplateCode     | String  |    O     | Original template code                                                                                                                                                                                                       |
| - templateName          | String  |    O     | Template name                                                                                                                                                                                                                |
| - templateMessageType   | String  |    X     | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes)                                                                                                                            |
| - templateEmphasizeType | String  |    X     | Types of emphasized template (NONE: Basic, TEXT: Emphasized, IMAGE: Image type, ITEM_LIST: Item List type)                                                                                                                   |
| - templateContent       | String  |    X     | Template body                                                                                                                                                                                                                |
| - templateExtra         | String  |    X     | Template extra information                                                                                                                                                                                                   |
| - templateAd            | String  |    X     | Request for consent of receiving within template or simple ad phrases                                                                                                                                                        |
| - tempalteTitle         | String  |    X     | Template title                                                                                                                                                                                                               |
| - templateSubtitle      | String  |    X     | Template subtitle                                                                                                                                                                                                            |
| - templateHeader        | String  |    X     | Template header (up to 16 characters)                                                                                                                                                                                        |
| - templateItem          | Object  |    X     | Item                                                                                                                                                                                                                         |
| -- list                 | List    |    X     | Item list (minimum 2, maximum 10)                                                                                                                                                                                            |
| --- title               | String  |    X     | Title (up to 6 characters)                                                                                                                                                                                                   |
| --- description         | String  |    X     | Description (up to 23 characters)                                                                                                                                                                                            |
| -- summary              | Object  |    X     | Item summary information                                                                                                                                                                                                     |
| --- title               | String  |    X     | Title (up to 6 characters)                                                                                                                                                                                                   |
| --- description         | String  |    X     | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                           |
| - templateItemHighlight | Object  |    X     | Item highlight                                                                                                                                                                                                               |
| -- title                | String  |    X     | Title (up to 30 characters, up to 21 characters if a thumbnail image is present)                                                                                                                                             |
| -- description          | String  |    X     | Description (up to 19 characters, up to 13 characters if a thumbnail image is present)                                                                                                                                       |
| -- imageUrl             | String  |    X     | Thumbnail image URL                                                                                                                                                                                                          |
| - templateRepresentLink | Object  |    X     | Representative link                                                                                                                                                                                                          |
| -- linkMo               | String  |    X     | Mobile web link (up to 500 characters)                                                                                                                                                                                       |
| -- linkPc               | String  |    X     | PC web link (up to 500 characters)                                                                                                                                                                                           |
| -- schemeIos            | String  |    X     | iOS app link (up to 500 characters)                                                                                                                                                                                          |
| -- schemeAndroid        | String  |    X     | Android app link (up to 500 characters)                                                                                                                                                                                      |
| - templateImageName     | String  |    X     | Image name (uploaded file name)                                                                                                                                                                                              |
| - templateImageUrl      | String  |    X     | Image URL                                                                                                                                                                                                                    |
| - buttons               | List    |    X     | Button list                                                                                                                                                                                                                  |
| -- ordering             | Integer |    X     | Button order (1–5)                                                                                                                                                                                                           |
| -- type                 | String  |    X     | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| -- name                 | String  |    X     | Button name                                                                                                                                                                                                                  |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type)                                                                                                                                      |
| -- linkPc               | String  |    X     | PC web link (optional for the WL type)                                                                                                                                          |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type)                                                                                                                                         |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type)                                                                                                                                     |
| -- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                     |
| -- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                                 |
| -- telNumber            | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                                                                                                 |
| - quickReplies          | List    |    X     | Quick reply list (up to 5)                                                                                                                                                      |
| -- ordering             | Integer |    X     | Quick reply order (required when quick reply exists)                                                                                                                            |
| -- type                 | String  |    X     | Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                   |
| -- name                 | String  |    X     | Quick reply name (required when quick reply exists, up to 14 characters)                                                                                                        |
| -- linkMo               | String  |    X     | Mobile web link (required for the WL type, for up to 500 characters)                                                                                                           |
| -- linkPc               | String  |    X     | PC web link (required for the WL type, for up to 500 characters)                                                                                                               |
| -- schemeIos            | String  |    X     | iOS app link (required for the AL type, for up to 500 characters)                                                                                                              |
| -- schemeAndroid        | String  |    X     | Android app link (required for the AL type, for up to 500 characters)                                                                                                          |
| -- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                     |
| - comments              | List    |    X     | Inspection results                                                                                                                                                              |
| -- id                   | Integer |    X     | Inquiry ID                                                                                                                                                                      |
| -- content              | String  |    X     | Inquiry content                                                                                                                                                                 |
| -- userName             | String  |    X     | Author                                                                                                                                                                          |
| -- createAt             | String  |    O     | Registration date                                                                                                                                                               |
| -- attachment           | List    |    X     | Attached files                                                                                                                                                                  |
| --- originalFileName    | String  |    X     | Attachment file name                                                                                                                                                            |
| --- filePath            | String  |    X     | Attachment file path                                                                                                                                                            |
| -- status               | String  |    X     | Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied, REQ: Under Review)                                                                                   |
| - status                | String  |    O     | Template status                                                                                                                                                                 |
| - statusName            | String  |    X     | Template status name                                                                                                                                                            |
| - securityFlag          | Boolean |    X     | Whether it is a security template                                                                                                                                               |
| - categoryCode          | String  |    X     | Template category code                                                                                                                                                          |
| - block                 | Boolean |    X     | Whether blocked                                                                                                                                                                 |
| - dormant               | Boolean |    X     | Whether dormant                                                                                                                                                                 |
| - createDate            | String  |    O     | Creation date                                                                                                                                                                   |
| - updateDate            | String  |    X     | Modification date                                                                                                                                                               |

<a id="list-templates"></a>

### List Templates

<a id="request-19"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | 	Type   | 	Description  |
|-----------|---------|---------------|
| appkey    | 	String | 	Unique app key |
| senderKey | 	String | 	Sender key    |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                       |
|--------------|---------|-----------|-------------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Query parameter]

| Name           | 	Type    | 	Required | 	Description                            |
|----------------|----------|-----------|------------------------------------------|
| templateCode   | 	String  | 	X        | 	Template Code                          |
| templateName   | 	String  | 	X        | 	Template name                          |
| templateStatus | String   | 	X        | Template status code                    |
| pageNum        | 	Integer | 	X        | 	Page number (Default: 1)               |
| pageSize       | 	Integer | 	X        | 	Number of records to retrieve (Default: 15, Max: 1,000) |

| Template Status Code | Description  |
|----------------------|--------------|
| TSC01                | Requested    |
| TSC02                | Reviewing    |
| TSC03                | Approved     |
| TSC04                | Rejected     |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates?templateStatus={template status code}"
```

<a id="response-20"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templateListResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "kakaoTemplateCode": String,
              "templateName": String,
              "templateMessageType": String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra": String,
              "templateAd": String,
              "templateTitle": String,
              "templateSubtitle": String,
              "templateHeader": String,
              "templateItem": {
                "list": [{
                  "title": String,
                  "description": String
                }],
                "summary": {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight": {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink": {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName": String,
              "templateImageUrl": String,
              "buttons": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer,
                    "pluginId": String,
                    "telNumber": String
                }
              ],
              "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer
                }
              ],
              "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                  }  
              ],
              "status": String,
              "statusName": String,
              "securityFlag": Boolean,
              "categoryCode": String,
              "createDate": String,
              "updateDate": String
          }
      ],
      "totalCount": Integer
  }
}
```

| Name                     | Type    | Not Null | Description                                                                                                                                                                     |
|--------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                   | Object  |    O     | Header area                                                                                                                                                                  |
| - resultCode             | Integer |    O     | Result code                                                                                                                                                                  |
| - resultMessage          | String  |    O     | Result message                                                                                                                                                                 |
| - isSuccessful           | Boolean |    O     | Success                                                                                                                                                                  |
| templateListResponse     | Object  |    X     | Body area                                                                                                                                                                  |
| - templates              | List    |    X     | Template list                                                                                                                                                                |
| -- plusFriendId          | String  |    O     | KakaoTalk Channel search ID or Sender Profile group name                                                                                                                                           |
| -- senderKey             | String  |    O     | Sender Key                                                                                                                                                                   |
| -- plusFriendType        | String  |    O     | Sender Profile type (NORMAL, GROUP)                                                                                                                                                |
| -- templateCode          | String  |    O     | Template Code                                                                                                                                                                 |
| -- kakaoTemplateCode     | String  |    O     | Original template code                                                                                                                                                              |
| -- templateName          | String  |    O     | Template name                                                                                                                                                                   |
| -- templateMessageType   | String  |    X     | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes)                                                                                                                   |
| -- templateEmphasizeType | String  |    X     | Template emphasis type (NONE: Default, TEXT: Emphasis, IMAGE: Image Type, ITEM_LIST: Item List Type)                                                                                                   |
| -- templateContent       | String  |    X     | Template body                                                                                                                                                                 |
| -- templateExtra         | String  |    X     | Template extra information                                                                                                                                                              |
| -- templateAd            | String  |    X     | Request for consent to receive messages or simple ad phrases within the template                                                                                                                                            |
| -- tempalteTitle         | String  |    X     | Template title                                                                                                                                                                 |
| -- templateSubtitle      | String  |    X     | Template subtitle                                                                                                                                                              |
| - templateHeader         | String  |    X     | Template header (up to 16 characters)                                                                                                                                                         |
| - templateItem           | Object  |    X     | Item                                                                                                                                                                    |
| -- list                  | List    |    X     | Item list (minimum 2, maximum 10)                                                                                                                                                 |
| --- title                | String  |    X     | Title (up to 6 characters)                                                                                                                                                             |
| --- description          | String  |    X     | Description (up to 23 characters)                                                                                                                                                          |
| -- summary               | Object  |    X     | Item summary information                                                                                                                                                              |
| --- title                | String  |    X     | Title (up to 6 characters)                                                                                                                                                             |
| --- description          | String  |    X     | Description (only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                          |
| - templateItemHighlight  | Object  |    X     | Item highlight                                                                                                                                                              |
| -- title                 | String  |    X     | Title (up to 30 characters, up to 21 characters if a thumbnail image is present)                                                                                                                                        |
| -- description           | String  |    X     | Description (up to 19 characters, up to 13 characters if a thumbnail image is present)                                                                                                                                      |
| -- imageUrl              | String  |    X     | Thumbnail image URL                                                                                                                                                             |
| - templateRepresentLink  | Object  |    X     | Representative link                                                                                                                                                                  |
| -- linkMo                | String  |    X     | Mobile web link (up to 500 characters)                                                                                                                                                      |
| -- linkPc                | String  |    X     | PC web link (up to 500 characters)                                                                                                                                                       |
| -- schemeIos             | String  |    X     | iOS app link (up to 500 characters)                                                                                                                                                      |
| -- schemeAndroid         | String  |    X     | Android app link (up to 500 characters)                                                                                                                                                    |
| -- templateImageName     | String  |    X     | Image name (uploaded file name)                                                                                                                                                         |
| -- templateImageUrl      | String  |    X     | Image URL                                                                                                                                                                |
| -- buttons               | List    |    X     | Button list                                                                                                                                                                 |
| --- ordering             | Integer |    X     | Button order (1–5)                                                                                                                                                             |
| --- type                 | String  |    X     | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| --- name                 | String  |    X     | Button name                                                                                                                                                                  |
| --- linkMo               | String  |    X     | Mobile web link (required for the WL type)                                                                                                                                              |
| --- linkPc               | String  |    X     | PC web link (optional field for the WL type)                                                                                                                               |
| --- schemeIos            | String  |    X     | iOS app link (required for the AL type)                                                                                                                                    |
| --- schemeAndroid        | String  |    X     | Android app link (required for the AL type)                                                                                                                                |
| --- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                |
| --- pluginId             | String  |    X     | Plugin ID (up to 24 characters)                                                                                                                                            |
| --- telNumber            | 	String  | 	X  | Phone number to send for TN (Call) type buttons                                                                                                                            |
| -- quickReplies          | List    |    X     | Quick reply list (up to 5)                                                                                                                                                 |
| --- ordering             | Integer |    X     | Quick reply order (required when quick reply exists)                                                                                                                       |
| --- type                 | String  |    X     | Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                              |
| --- name                 | String  |    X     | Quick reply name (required when quick reply exists, up to 14 characters)                                                                                                   |
| --- linkMo               | String  |    X     | Mobile web link (required for the WL type, for up to 500 characters)                                                                                                      |
| --- linkPc               | String  |    X     | PC web link (required for the WL type, for up to 500 characters)                                                                                                          |
| --- schemeIos            | String  |    X     | iOS app link (required for the AL type, for up to 500 characters)                                                                                                         |
| --- schemeAndroid        | String  |    X     | Android app link (required for the AL type, for up to 500 characters)                                                                                                     |
| --- bizFormId            | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                |
| -- comments              | List    |    X     | Review results                                                                                                                                                             |
| --- id                   | Integer |    X     | Inquiry ID                                                                                                                                                                 |
| --- content              | String  |    X     | Inquiry content                                                                                                                                                            |
| --- userName             | String  |    X     | Author                                                                                                                                                                     |
| --- createAt             | String  |    O     | Registration date                                                                                                                                                          |
| --- attachment           | List    |    X     | Attachments                                                                                                                                                                |
| ---- originalFileName    | String  |    X     | Attachment file name                                                                                                                                                       |
| ---- filePath            | String  |    X     | Attachment file path                                                                                                                                                       |
| --- status               | String  |    X     | Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied, REQ: Under Review)                                                                              |
| -- status                | String  |    O     | Template status                                                                                                                                                            |
| -- statusName            | String  |    X     | Template status name                                                                                                                                                       |
| -- securityFlag          | Boolean |    X     | Whether it is a security template                                                                                                                                          |
| -- categoryCode          | String  |    X     | Template category code                                                                                                                                                     |
| -- createDate            | String  |    O     | Created date                                                                                                                                                               |
| -- updateDate            | String  |    X     | Modified date                                                                                                                                                              |
| - totalCount             | Integer |    X     | Total count                                                                                                                                                                |

<a id="list-template-modifications"></a>

### List Template Modifications

<a id="request-20"></a>

#### Request

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | 	Type   | 	Description    |
|--------------|---------|-----------------|
| appkey       | 	String | 	Unique app key |
| senderKey    | 	String | 	Sender Key     |
| templateCode | 	String | 	Template Code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | 	Type   | 	Required | 	Description                         |
|--------------|---------|-----------|--------------------------------------|
| X-Secret-Key | 	String | O         | Can be created in the console. |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

<a id="response-21"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "templateModificationsResponse": {
      "templates": [
          {
              "plusFriendId": String,
              "senderKey": String,
              "plusFriendType": String,
              "templateCode": String,
              "templateName": String,
              "templateMessageType": String,
              "templateEmphasizeType": String,
              "templateContent": String,
              "templateExtra": String,
              "templateAd": String,
              "templateTitle": String,
              "templateSubtitle": String,
              "templateHeader": String,
              "templateItem": {
                "list": [{
                  "title": String,
                  "description": String
                }],
                "summary": {
                  "title": String,
                  "description": String
                }
              },
              "templateItemHighlight": {
                "title": String,
                "description": String,
                "imageUrl": String
              },
              "templateRepresentLink": {
                "linkMo": String,
                "linkPc": String,
                "schemeIos": String,
                "schemeAndroid": String,
              },
              "templateImageName": String,
              "templateImageUrl": String,
              "buttons": [
                {
                    "ordering":Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "telNumber": String
                }
              ],
              "quickReplies": [
                {
                    "ordering": Integer,
                    "type": String,
                    "name": String,
                    "linkMo": String,
                    "linkPc": String,
                    "schemeIos": String,
                    "schemeAndroid": String,
                    "bizFormId": Integer
                }
              ],
              "comments": [
                  {
                      "id": Integer,
                      "content": String,
                      "userName": String,
                      "createdAt": String,
                      "attachment": [{
                        "originalFileName": String,
                        "filePath": String
                      }],
                      "status": String
                  }  
              ],
              "status": String,
              "statusName": String,
              "securityFlag": Boolean,
              "categoryCode": String,
              "activated": boolean,
              "createDate": String,
              "updateDate": String
          }
      ],
      "totalCount": Integer
  }
}
```

| Name                          | Type    | Not Null | Description                                                                                                                                                                                                                                  |
|-------------------------------|---------|:--------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                        | Object  |    O     | Header area                                                                                                                                                                                                                                  |
| - resultCode                  | Integer |    O     | Result code                                                                                                                                                                                                                                  |
| - resultMessage               | String  |    O     | Result message                                                                                                                                                                                                                               |
| - isSuccessful                | Boolean |    O     | Success                                                                                                                                                                                                                                      |
| templateModificationsResponse | Object  |    X     | Body area                                                                                                                                                                                                                                    |
| - templates                   | List    |    X     | Template list                                                                                                                                                                                                                                |
| -- plusFriendId               | String  |    O     | KakaoTalk Channel search ID or Sender Profile group name                                                                                                                                                                                     |
| -- senderKey                  | String  |    O     | Sender Key                                                                                                                                                                                                                                   |
| -- plusFriendType             | String  |    O     | Sender Profile type (NORMAL, GROUP)                                                                                                                                                                                                          |
| -- templateCode               | String  |    O     | Template Code                                                                                                                                                                                                                                |
| -- templateName               | String  |    O     | Template name                                                                                                                                                                                                                                |
| -- templateMessageType        | String  |    X     | Types of template message (BA: Basic, EX: Extra Information, AD: Ad Included, MI: Mixed Purposes, default: Basic)                                                                                                                            |
| -- templateEmphasizeType      | String  |    X     | Template emphasis type (NONE: Default, TEXT: Emphasis Type, IMAGE: Image Type)                                                                                                                                                               |
| -- templateContent            | String  |    O     | Template body                                                                                                                                                                                                                                |
| -- templateExtra              | String  |    X     | Template additional information                                                                                                                                                                                                              |
| -- templateAd                 | String  |    X     | Request for consent to receive messages or simple ad phrases within the template                                                                                                                                                             |
| -- tempalteTitle              | String  |    X     | Template title                                                                                                                                                                                                                               |
| -- templateSubtitle           | String  |    X     | Template auxiliary phrase                                                                                                                                                                                                                    |
| - templateHeader              | String  |    X     | Template header (up to 16 characters)                                                                                                                                                                                                        |
| - templateItem                | Object  |    X     | Item                                                                                                                                                                                                                                         |
| -- list                       | List    |    X     | Item list (minimum 2, maximum 10)                                                                                                                                                                                                            |
| --- title                     | String  |    X     | Title (up to 6 characters)                                                                                                                                                                                                                   |
| --- description               | String  |    X     | Description (up to 23 characters)                                                                                                                                                                                                            |
| -- summary                    | Object  |    X     | Item summary information                                                                                                                                                                                                                     |
| --- title                     | String  |    X     | Title (up to 6 characters)                                                                                                                                                                                                                   |
| --- description               | String  |    X     | Description (Only variables and monetary units, numbers, commas, and periods, up to 14 characters)                                                                                                                                           |
| - templateItemHighlight       | Object  |    X     | Item highlight                                                                                                                                                                                                                               |
| -- title                      | String  |    X     | Title (up to 30 characters; up to 21 characters if a thumbnail image is present)                                                                                                                                                             |
| -- description                | String  |    X     | Description (up to 19 characters; up to 13 characters if a thumbnail image is present)                                                                                                                                                       |
| -- imageUrl                   | String  |    X     | Thumbnail image URL                                                                                                                                                                                                                          |
| - templateRepresentLink       | Object  |    X     | Representative link                                                                                                                                                                                                                          |
| -- linkMo                     | String  |    X     | Mobile web link (up to 500 characters)                                                                                                                                                                                                       |
| -- linkPc                     | String  |    X     | PC web link (up to 500 characters)                                                                                                                                                                                                           |
| -- schemeIos                  | String  |    X     | iOS app link (up to 500 characters)                                                                                                                                                                                                          |
| -- schemeAndroid              | String  |    X     | Android app link (up to 500 characters)                                                                                                                                                                                                      |
| -- templateImageName          | String  |    X     | Image name (uploaded file name)                                                                                                                                                                                                              |
| -- templateImageUrl           | String  |    X     | Image URL                                                                                                                                                                                                                                    |
| -- buttons                    | List    |    X     | Button list                                                                                                                                                                                                                                  |
| --- ordering                  | Integer |    X     | Button order (1–5)                                                                                                                                                                                                                           |
| --- type                      | String  |    X     | Button type (WL: Web link, AL: App link, DS: Delivery search, BK: Bot keyword, MD: Message delivery, BC: Bot for Consultation, BT: Bot Transfer, AC: Add channel, BF: Business form, P1: Image secure transmission plugin ID, P2: Personal information use plugin ID, P3: One-click payment plugin ID, TN: Call) |
| --- name                      | String  |    X     | Button name                                                                                                                                                                                                                                  |
| --- linkMo                    | String  |    X     | Mobile web link (required for the WL type)                                                                                                                                                                                                   |
| --- linkPc                    | String  |    X     | PC web link (optional for the WL type)                                                                                                                                               |
| --- schemeIos                 | String  |    X     | iOS app link (required for the AL type)                                                                                                                                              |
| --- schemeAndroid             | String  |    X     | Android app link (required for the AL type)                                                                                                                                            |
| --- telNumber                 | 	String  | 	X  | Phone number to send for TN (call) type buttons                                                                    |
| -- quickReplies               | List    |    X     | Quick reply list (up to 5)                                                                                                                                                        |
| --- ordering                  | Integer |    X     | Quick reply order (required when quick reply exists)                                                                                                                                                |
| --- type                      | String  |    X     | Quick reply type (WL: Web link, AL: App link, BK: Bot keyword, BC: Bot for Consultation, BT: Bot Transfer, BF: Business form)                                                                                                |
| --- name                      | String  |    X     | Quick reply name (required when quick reply exists, up to 14 characters)                                                                                                                                        |
| --- linkMo                    | String  |    X     | Mobile web link (required for the WL type, for up to 500 characters)                                                                                                                                     |
| --- linkPc                    | String  |    X     | PC web link (required for the WL type, for up to 500 characters)                                                                                                                                      |
| --- schemeIos                 | String  |    X     | iOS app link (required for the AL type, for up to 500 characters)                                                                                                                                     |
| --- schemeAndroid             | String  |    X     | Android app link (required for the AL type, for up to 500 characters)                                                                                                                                   |
| --- bizFormId                 | Integer |    X     | Business form ID (required for the BF type)                                                                                                                                                 |
| -- comments                   | List    |    X     | Review results                                                                                                                                                                  |
| --- id                        | Integer |    X     | Inquiry ID                                                                                                                                                                 |
| --- content                   | String  |    X     | Inquiry content                                                                                                                                                                  |
| --- userName                  | String  |    X     | Author                                                                                                                                                                    |
| --- createAt                  | String  |    O     | Registration date                                                                                                                                                                  |
| --- attachment                | List    |    X     | Attachments                                                                                                                                                                 |
| ---- originalFileName         | String  |    X     | Attachment file name                                                                                                                                                                 |
| ---- filePath                 | String  |    X     | Attachment file path                                                                                                                                                               |
| --- status                    | String  |    X     | Comment status (INQ: Inquired, APR: Approved, REJ: Rejected, REP: Replied, REQ: Under Review)                                                                                                                   |
| -- status                     | String  |    O     | Template status                                                                                                                                                                 |
| -- statusName                 | String  |    O     | Template status name                                                                                                                                                                |
| -- securityFlag               | Boolean |    X     | Whether it is a security template                                                                                                                                              |
| -- categoryCode               | String  |    X     | Template category code                                                                                                                                                            |
| -- activated                  | Boolean |    X     | Whether it is activated                                                                                                                                                 |
| -- createDate                 | String  |    O     | Creation date                                                                                                                                                                   |
| -- updateDate                 | String  |    X     | Modification date                                                                                                                                                                   |
| - totalCount                  | Integer |    X     | Total count                                                                                                                                                                    |

<a id="register-template-image"></a>

### Register Template Image

<a id="request-21"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request parameter]

| Name   | 	Type   | 	Required | 	Description     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	Image file |

[Example]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

<a id="response-22"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| Name                  | Type      | Not Null | Description             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | Header area          |
| - resultCode        | Integer |    O     | Result code          |
| - resultMessage     | String  |    O     | Result message         |
| - isSuccessful      | Boolean |    O     | Success          |
| templateImage       | Object  |    X     | Body area          |
| - templateImageName | String  |    X     | Image name (uploaded file name) |
| - templateImageUrl  | String  |    X     | Image URL        |

<a id="register-template-item-highlight-images"></a>

### Register Template Item Highlight Images

<a id="request-22"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight
Content-Type: multipart/form-data
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request parameter]

| Name   | 	Type   | 	Required | 	Description     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	Image file |

[Example]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight" -F "file=@alimtalk-template-image.jpeg"
```

<a id="response-23"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "templateImage" {
    "templateImageName": String,
    "templateImageUrl": String
  }
}
```

| Name                  | Type      | Not Null | Description             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | Header area          |
| - resultCode        | Integer |    O     | Result code          |
| - resultMessage     | String  |    O     | Result message         |
| - isSuccessful      | Boolean |    O     | Success          |
| templateImage       | Object  |    X     | Body area          |
| - templateImageName | String  |    X     | Image name (uploaded file name) |
| - templateImageUrl  | String  |    X     | Image URL        |

<a id="register-template-plugin"></a>

### Register Template Plugin

<a id="request-23"></a>

#### Request

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description    |
|-----------|--------|----------------|
| appkey    | String | Unique app key |
| senderKey | String | Sender Key     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Request Body]

```
{
  "pluginType": String,
  "pluginId": String,
  "callbackUrl": String
}
```

| Name        | Type   | Required | Description                                                                                         |
|-------------|--------|----------|-----------------------------------------------------------------------------------------------------|
| pluginType  | String | O        | Plugin type (SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
| pluginId    | String | O        | Plugin ID                                                                                           |
| callbackUrl | String | O        | The callback URL to receive when the plugin button is clicked                                       |

<a id="response-24"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name            | Type    | Not Null | Description    |
|-----------------|---------|:--------:|----------------|
| header          | Object  |    O     | Header area    |
| - resultCode    | Integer |    O     | Result code    |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success        |

<a id="modify-template-plugin"></a>

### Modify Template Plugin

<a id="request-24"></a>

#### Request

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name        | 	Type     | 	Description       |
|-----------|---------|-----------|
| appkey    | 	String | 	Unique app key   |
| senderKey | 	String | 	Sender Key     |
| pluginId  | 	String | 	Plugin ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request Body]

```
{
  "pluginType": String,
  "callbackUrl": String
}
```

| Name          | 	Type     | 	Required | 	Description                                                        |
|-------------|---------|-----|------------------------------------------------------------|
| pluginType  | 	String | 	O  | Plugin type (SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
| callbackUrl | 	String | 	O  | The callback URL to receive when the plugin button is clicked                                  |

<a id="response-25"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | Header area  |
| - resultCode    | Integer |    O     | Result code  |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success  |

<a id="modify-template-plugin-2"></a>

### Delete Template Plugin

<a id="request-25"></a>

#### Request

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name        | 	Type     | 	Description       |
|-----------|---------|-----------|
| appkey    | 	String | 	Unique app key   |
| senderKey | 	String | 	Sender Key     |
| pluginId  | 	String | 	Plugin ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

<a id="response-26"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | Header area  |
| - resultCode    | Integer |    O     | Result code  |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success |

<a id="retrieve-template-plugin"></a>

### Get Template Plugin

<a id="request-26"></a>

#### Request

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name | Type | Description |
|-----------|---------|---------|
| appkey | String | Unique app key |
| senderKey | String | Sender Key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|---------|-----|------------------|
| X-Secret-Key | String | O | Can be created in the console. |

<a id="response-27"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "plugins": [
    {
      "pluginId": String,
      "pluginType": String,
      "pluginTypeName": String,
      "callbackUrl": String,
      "modifiable": boolean,
      "deletable": boolean
    }
  
  ]
}
```

| Name | Type | Not Null | Description |
|------------------|---------|:--------:|------------------------------------------------------------|
| header | Object | O | Header area |
| - resultCode | Integer | O | Result code |
| - resultMessage | String | O | Result message |
| - isSuccessful | Boolean | O | Success |
| plugins | List | X | Plugin list |
| - pluginId | String | X | Plugin ID |
| - pluginType | String | X | Plugin type (SECURE_IMAGE: secure image transmission, ONE_TIME_PROFILE: personal information use) |
| - pluginTypeName | String | X | Plugin name |
| - callbackUrl | String | X | The callback URL to receive when the plugin button is clicked |
| - modifiable | Boolean | X | Whether modification is possible |
| - deletable | Boolean | X | Whether deletion is possible |

<a id="manage-alternative-delivery"></a>

## Alternative Delivery Management

<a id="register-an-sms-appkey"></a>

### Register an SMS AppKey

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request body]

```
{
    "resendAppKey": String
}
```

| Name           | 	Type     | 	Required | 	Description                    |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | App key of the SMS service to set for alternative delivery |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

<a id="response-28"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | Header area  |
| - resultCode    | Integer |    O     | Result code  |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success  |

<a id="register-alternative-delivery-settings"></a>

### Register Alternative Delivery Settings

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | 	Type     | 	Required | 	Description              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | Can be created in the console. |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| Name           | 	Type      | 	Required | 	Description                                                                                       |
|--------------|----------|-----|-------------------------------------------------------------------------------------------|
| senderKey    | 	String  | 	O  | Sender key                                                                                      |
| isResend     | 	Boolean | 	O  | Whether to send an SMS as an alternative if delivery fails<br>Resent by default, if fallback is set on console.                          |
| resendSendNo | 	String  | 	O  | Sender number for alternative delivery<br><span style="color:red">(Alternative delivery may fail, if the sender number is not registered on the SMS service.)</span> |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

<a id="response-29"></a>

#### Response

```

{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | Header area  |
| - resultCode    | Integer |    O     | Result code  |
| - resultMessage | String  |    O     | Result message |
| - isSuccessful  | Boolean |    O     | Success  |