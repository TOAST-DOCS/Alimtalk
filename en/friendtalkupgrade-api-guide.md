## Notification > KakaoTalk Bizmessage > Brand Message > API v1.0 Guide

<a id="brand-message"></a>

## Brand Message

<a id="api-domain"></a>

#### [API Domain]

| Domain                                                                          |
|------------------------------------------------------------------------------|
| [https://kakaotalk-bizmessage.api.nhncloudservice.com](https://kakaotalk-bizmessage.api.nhncloudservice.com) |

<a id="introduce-v10-api"></a>

## v1.0 API Introduction

<a id="manage-non-friend-message-sending-targeting-m-n"></a>

## Non-friend message sending (targeting M, N) management

Non-friend message sending (targeting M, N) can be sent when all of the following conditions are met:

- Business authenticated channel
- Business registration number registered
- Channel customer service phone number registered
- More than 50,000 channel friends
- History of successful AlimTalk message sending within 3 months

<a id="upload-marketing-consent-evidence"></a>

### Upload marketing consent records

<a id="requested"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | Unique app key |
| senderKey | String | Sender key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Request parameter]

| Name | Type | Required | Description |
|------|------|----------|-------------|
| file | File | O        | Marketing consent records |

<a id="response"></a>

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

| Name            | Type    | Not Null | Description |
|:----------------|:--------|:---------|:------------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |

<a id="apply-for-using-non-friend-message-sending-targeting-m-n"></a>

### Apply for using non-friend message sending (targeting M, N)

<a id="requested-2"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | Unique app key |
| senderKey | String | Sender key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="response-2"></a>

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

| Name            | Type    | Not Null | Description |
|:----------------|:--------|:---------|:------------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |

<a id="request-to-send-a-free-form-message"></a>

## Request Free-form Message Sending

* Marketing consent-based sending can be used.
    * You can specify the targeting field to designate the type of message target.
        * M: Users who consented to receive advertising information from the customer (KakaoTalk reception consent)
        * N: Users who consented to receive advertising information from the customer (KakaoTalk reception consent) - Channel friends
        * I: Customer's sending request targets ∩ Channel friends
* You can use all 8 message types of the existing FriendTalk.
* You can use BT and AC button types.
* When using the AC (Add Channel) button, the following restrictions apply:
    * It is displayed as an emphasis button (yellow).
    * When there are multiple buttons, they must be used at specified positions.
        * TEXT, IMAGE: First button (top)
        * Others: Second button (right)
    * The button name is fixed as "Add Channel".
    * For carousel types, only one can be used throughout the entire carousel.
    * Only targeting M and N can be used.
* When using the BF button, you can use it by entering the business form ID issued by Kakao.
* Alternative sending can be set with resendParameter for each recipient.
    * When using alternative sending, SMS AppKey registration and sending settings are required through the alternative sending management API.
* **Delivery restricted during night (20:50~08:00 on the following day)**

<a id="requested-3"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="request-text-type-sending"></a>

#### Request Text Type Send

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "TEXT",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String, 
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push alarm (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether adult message (default: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - For TEXT type: maximum 1,300 characters (line breaks: maximum 99, URL format can be entered)<br>- For IMAGE type: maximum 1,300 characters (line breaks: maximum 99, URL format can be entered)<br>- For WIDE type: maximum 76 characters (line breaks: maximum 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally. Maximum 76 characters (line breaks: maximum 5)<br>- For other types: this field is not used                           |
| buttons                | List    | X  | Button list<br>- For TEXT, IMAGE types: maximum 4 when coupon is applied, otherwise maximum 5<br>- For WIDE, WIDE_ITEM_LIST types: maximum 2<br>- For PREMIUM_VIDEO type: maximum 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT, IMAGE types: maximum 14 characters<br>- For other types: maximum 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Chatbot Transfer, BF: Business Form)<br>- BT type can only be used by channels that use Kakao Open Builder chatbots<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Reserve in Talk<br>  - Survey in Talk<br>  - Apply in Talk |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to pass for BT type buttons                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type buttons                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: maximum 18 characters. Line breaks: not allowed<br>- For other types: maximum 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (maximum 1,000 people)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when delivery fails<br>When fallback is set in the console, fallback is sent by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback type (SMS, LMS)<br>If there is no value, the type is determined by the template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback title<br>(If there is no value, fallback is sent with the PlusFriend ID.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback content<br>(If there is no value, fallback is sent with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback 080 unsubscribe number<br><span style="color:red">(Fallback may fail if the 080 unsubscribe number is not registered on the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who consented to marketing reception, N: only non-friend users who consented to marketing reception, I: friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Maximum 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Maximum 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sent from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-image-type-sending"></a>

#### Request Image Type Sending

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "IMAGE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys are not supported                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push alarm (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 free unsubscribe phone number (if all fields are left empty, sent with the free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 free unsubscribe authentication number (if all fields are left empty, sent with the free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally. Up to 76 characters (line breaks: up to 5)<br>- For other types: this field is not used                           |
| image                  | Object  | O  | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | Image URL. Use image URL uploaded as a regular image                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | URL to navigate to when image is clicked. Limited to 1,000 characters<br>Uses KakaoTalk's internal image viewer if not set                                                                                                                                                                                                                            |
| buttons                | List    | X  | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- BT type can only be used by channels using Kakao Open Builder chatbots<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Make a reservation in Talk<br>  - Take a survey in Talk<br>  - Apply in Talk |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), limited to 1,000 characters                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to deliver for BT type button                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type button                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type button                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed<br>- For other types: up to 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (up to 1,000 people)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback sending information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when delivery fails<br>When fallback sending is set in the console, it is sent as fallback by default.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback sending type (SMS, LMS)<br>If no value is provided, the type is determined based on the template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback sending title<br>(If no value is provided, it is sent as fallback with the PlusFriend ID.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback sending content<br>(If no value is provided, it is sent as fallback with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback sending sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback sending 080 unsubscribe number<br><span style="color:red">(Fallback sending may fail if the 080 unsubscribe number is not registered in the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who consented to marketing, N: only non-friend users who consented to marketing, I: friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 free unsubscribe phone number (if all fields are left empty, sent with the free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free unsubscribe authentication number (if all fields are left empty, sent with the free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-wide-image-type-sending"></a>

#### Request wide image type sending

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender key cannot be used                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push notification (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(To be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (sent with unsubscribe information registered in sender profile if all fields are not entered)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (sent with unsubscribe information registered in sender profile if all fields are not entered)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether adult message (default: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally. Up to 76 characters (line breaks: up to 5)<br>- For other types: this field is not used                           |
| image                  | Object  | O  | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | Image URL, use image URL uploaded as wide image                                                                                                                                                                                                                                             |
| - imageLink            | String  | X  | URL to navigate when image is clicked. 1,000 character limit<br>Uses KakaoTalk image viewer if not set                                                                                                                                                                                                                            |
| buttons                | List    | X  | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- BT type can only be used by channels that use Kakao Open Builder's chatbot<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Make a reservation in Talk<br>  - Take a survey in Talk<br>  - Enter in Talk |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to deliver for BT type button                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type button                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type button                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed<br>- For other types: up to 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (up to 1,000 people)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback delivery information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when delivery fails<br>When fallback delivery is set in the console, fallback is sent by default.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback delivery type (SMS, LMS)<br>If no value, type is determined by template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback delivery title<br>(If no value, sent as fallback with PlusFriend ID.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback delivery content<br>(If no value, sent as fallback with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback delivery 080 unsubscribe number<br><span style="color:red">(Fallback may fail if the 080 unsubscribe number is not registered in the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: Marketing consent users, N: Non-friend marketing consent users only, I: Friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (sent with unsubscribe information registered in sender profile if all fields are not entered)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (sent with unsubscribe information registered in sender profile if all fields are not entered)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (stored as user UUID when sent from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                            |

<a id="request-to-send-wide-item-list-type"></a>

#### Request Wide Item List Type Send

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push notifications (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if not entered, sent with unsubscribe information registered in sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if not entered, sent with unsubscribe information registered in sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                                                                                                       |
| header                 | String  | O  | Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (line breaks: not allowed)<br>- Optional field for PREMIUM_VIDEO type, up to 20 characters (line breaks: not allowed)                                                                                                                                                                     |
| item                   | Object  | O  | Wide list element (can only be used with WIDE_ITEM_LIST type)                                                                                                                                                                                                                                       |
| - list                 | List    | O  | Wide list (minimum: 3, maximum: 4)                                                                                                                                                                                                                                                         |
| -- title               | String  | O  | Item title<br>- First item is limited to up to 25 characters (line breaks: up to 1, title is not a required value for the first item)<br>- Items 2-4 are limited to up to 30 characters (line breaks: up to 1)                                                                                                                                                                |
| -- imageUrl            | String  | O  | Item image URL<br>- For the first item, use the image URL uploaded as the first wide item list image<br>- For items 2-4, use the image URL uploaded as a general wide item list image                                                                                                                                                             |
| -- linkMo              | String  | O  | Mobile web link, limited to 1,000 characters                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC web link, limited to 1,000 characters                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | Android app link, limited to 1,000 characters                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | iOS app link, limited to 1,000 characters                                                                                                                                                                                                                                                           |
| buttons                | List    | X  | Button list<br>- For TEXT and IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE and WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT and IMAGE types: up to 14 characters<br>- For other types: up to 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- BT type can only be used by channels using KakaoTalk Open Builder chatbots<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), limited to 1,000 characters                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to be delivered for BT type buttons                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type buttons                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed<br>- For other types: up to 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional field for WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), limited to 1,000 characters<br>If you enter the linkMo field in the coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (up to 1,000 recipients)                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when delivery fails<br>When fallback is set in the console, it will be sent as fallback by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback type (SMS, LMS)<br>If no value, the type is determined by template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback title<br>(If no value, sent as fallback with Plus Friend ID.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback content<br>(If no value, sent as fallback with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback 080 unsubscribe number<br><span style="color:red">(Fallback may fail if the 080 unsubscribe number is not registered on the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: Users who agreed to receive marketing, N: Only non-friend users who agreed to receive marketing, I: Friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if not entered, sent with unsubscribe information registered in sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if not entered, sent with unsubscribe information registered in sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when resellers send)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Creator (stored as user UUID when sent from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-premium-video-type"></a>

#### Request to Send Premium Video Type

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys are not available                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push alarms (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days later                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                                                                                                       |
| content                | String  | X  | - For TEXT type, up to 1,300 characters (line breaks: up to 99, URL format input available)<br>- For IMAGE type, up to 1,300 characters (line breaks: up to 99, URL format input available)<br>- For WIDE type, up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type, this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For other types, this field is not used                           |
| header                 | String  | X  | Header<br>- For WIDE_ITEM_LIST type, this is a required field with up to 20 characters (line breaks: not allowed)<br>- For PREMIUM_VIDEO type, this is an optional field with up to 20 characters (line breaks: not allowed)                                                                                                                                                                     |
| video                  | Object  | O  | Video element (only available for PREMIUM_VIDEO type)                                                                                                                                                                                                                                              |
| - videoUrl             | String  | O  | KakaoTV video URL (only KakaoTV uploaded video addresses can be used), up to 500 characters                                                                                                                                                                                                                         |
| - thumbnailUrl         | String  | X  | Image URL for video thumbnail, only URLs uploaded as regular images can be used (if not available, KakaoTV video default thumbnail is used), up to 500 characters                                                                                                                                                                            |
| buttons                | List    | X  | Button list<br>- For TEXT, IMAGE types, up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types, up to 2<br>- For PREMIUM_VIDEO type, up to 1<br>- For COMMERCE type, minimum 1, up to 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT, IMAGE types, up to 14 characters<br>- For other types, up to 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- BT type can only be used by channels using Kakao Open Builder chatbots<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Book in Talk<br>  - Survey in Talk<br>  - Apply in Talk |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional for the WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), 1,000 character limit                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), 1,000 character limit                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to be passed for BT type buttons                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type buttons                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five formats<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types, up to 18 characters. Line breaks: not allowed<br>- For other types, up to 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), 1,000 character limit<br>When the linkMo field is entered in the coupon, the remaining fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional for the WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), 1,000 character limit<br>When the linkMo field is entered in the coupon, the remaining fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), 1,000 character limit<br>When the linkMo field is entered in the coupon, the remaining fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (up to 1,000 people)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback sending information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when sending fails<br>When fallback sending is set in the console, it is sent as fallback by default.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback sending type (SMS, LMS)<br>If there is no value, the type is determined by the template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback sending title<br>(If there is no value, it is sent as fallback with the PlusFriend ID.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback sending content<br>(If there is no value, it is sent as fallback with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback sending sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback sending 080 unsubscribe number<br><span style="color:red">(Fallback may fail, if the 080 unsubscribe number is not registered on the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who consented to marketing, N: non-friend users who consented to marketing only, I: friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | X | Statistics ID (not included in sender search conditions. Up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-commerce"></a>

#### Request to Send Commerce Type

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "chatExtra": String,
      "chatEvent": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send message push notification (default: true)                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(If not entered, sent immediately)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all fields are not entered, sent with unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all fields are not entered, sent with unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                       |
| additionalContent      | String  | X  | Additional information (up to 34 characters, line breaks: maximum 1), can only be used in commerce type                                                                                                                                                      |
| image                  | Object  | O  | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | Image URL. Use image URL uploaded as general image                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | URL to navigate to when image is clicked. 1,000 character limit<br>If not set, KakaoTalk image viewer is used                                                                                                                                                                                                                            |
| commerce               | Object  | O  | Commerce (can only be used in COMMERCE type)                                                                                                                                                                                                                                                    |
| title                  | String  | O  | Product title (up to 30 characters, line breaks: not allowed)                                                                                                                                                                                                                                                       |
| regularPrice           | Integer | O  | Regular price (0~99,999,999)                                                                                                                                                                                                                                                        |
| discountPrice          | Integer | X  | Discount price (0~99,999,999)                                                                                                                                                                                                                                                          |
| discountRate           | Integer | X  | Discount rate (0~100), discount rate when discount price exists. One of discount rate or fixed discount price is required                                                                                                                                                                                                                                   |
| discountFixed          | Integer | X  | Fixed discount price (0~999,999), when discount price exists, one of discount rate or fixed discount price is required                                                                                                                                                                                                                            |
| buttons                | List    | O  | Button list<br>- For TEXT, IMAGE types: maximum 4 when coupon is applied, otherwise maximum 5<br>- For WIDE, WIDE_ITEM_LIST types: maximum 2<br>- For PREMIUM_VIDEO type: maximum 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For TEXT, IMAGE types: maximum 14 characters<br>- For other types: maximum 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- BT type can only be used by channels using KakaoTalk Open Builder chatbots<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to deliver for BT type button                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for BT type button                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for BF type button                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: maximum 18 characters. Line breaks: not allowed<br>- For other types: maximum 12 characters. Line breaks: not allowed                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required field for WL type), 1,000 character limit<br>If linkMo field is entered in coupon, other fields become optional,<br>and if channel coupon URL (format: alimtalk=coupon://) is entered in scheme_android or scheme_ios field, other fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required field for AL type), 1,000 character limit<br>If linkMo field is entered in coupon, other fields become optional,<br>and if channel coupon URL (format: alimtalk=coupon://) is entered in scheme_android or scheme_ios field, other fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required field for AL type), 1,000 character limit<br>If linkMo field is entered in coupon, other fields become optional,<br>and if channel coupon URL (format: alimtalk=coupon://) is entered in scheme_android or scheme_ios field, other fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (maximum 1,000 people)                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Fallback information                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send SMS fallback when delivery fails<br>When fallback is set in console, fallback is sent by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Fallback type (SMS, LMS)<br>If no value, type is classified according to template body length.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS fallback title<br>(If no value, sent as PlusFriend ID fallback.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Fallback content<br>(If no value, sent as [message body] fallback.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Fallback sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Fallback 080 unsubscribe number<br><span style="color:red">(Fallback may fail if the 080 unsubscribe number is not registered in the SMS service.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who agreed to receive marketing, N: only to non-friend users who agreed to receive marketing, I: friend users)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all fields are not entered, sent with unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all fields are not entered, sent with unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify grouping key for each recipient. Maximum 100 characters)                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify grouping key for each sender. Maximum 100 characters)                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                       |
| createUser             | String  | X  | Creator (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-carousel-feed-type"></a>

#### Request to Send Carousel Feed Type

##### Carousel Fixed Replacements (Path-Based Template Parameters)

You can apply different replacement values to each item in carousel types.

* **Usage format**: `key@$.carousel.list[index]` (e.g., `productName@$.carousel.list[0]`)
* If a head (carousel intro) exists, the head occupies `list[0]`, and actual items start from `list[1]`.
* If a value cannot be found with a path-based key, it falls back to a general key.

| Category | Replaceable Fields | Path Format |
|------|--------------|----------|
| Carousel intro (head) | header, content, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[0]` (when head exists) |
| Carousel item | header, message, additionalContent | `key@$.carousel.list[index]` |
| Carousel item button | linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item coupon | title, description, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item commerce | title | `key@$.carousel.list[index]` |

* **Tail**: Cannot use replacements
* Button index is not included in the path. Buttons within the same item are replaced with the same path value.

> **Caution** You cannot use the `@` character in replacement keys (`#{key}`). The system uses `@` as a path separator.

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                   | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                          |
|------------------------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O        | Sender key (40 characters). Group sender keys cannot be used                                                                                                                                                                                                                                                                                                                        |
| chatBubbleType         | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                                                                                                                        |
| pushAlarm              | boolean | X        | Whether to send message push alarm (default: true)                                                                                                                                                                                                                                                                                                                                  |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(To be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                                 |
| unsubscribeNo          | String  | X        | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                                                                        |
| unsubscribeAuthNo      | String  | X        | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234                                                                                                                                           |
| adult                  | boolean | X        | Whether it is an adult message (default: false)                                                                                                                                                                                                                                                                                                                                     |
| carousel               | Object  | O        | Carousel                                                                                                                                                                                                                                                                                                                                                                             |
| - list                 | List    | O        | Carousel list (minimum 2, maximum 6)                                                                                                                                                                                                                                                                                                                                                |
| -- header              | String  | O        | Carousel item title (up to 20 characters). Available only for carousel feed type                                                                                                                                                                                                                                                                                                    |
| -- message             | String  | O        | Carousel item title (up to 20 characters), carousel item message (up to 180 characters). Available only for carousel feed type                                                                                                                                                                                                                                                      |
| -- imageUrl            | String  | O        | Image URL (only images uploaded as carousel feed type images can be used)                                                                                                                                                                                                                                                                                                           |
| -- imageLink           | String  | X        | Image link, limited to 1,000 characters                                                                                                                                                                                                                                                                                                                                             |
| -- buttons             | List    | O        | Carousel list button list minimum 1, maximum 2                                                                                                                                                                                                                                                                                                                                      |
| --- name               | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters                                                                                                                                                                                                                                                                               |
| --- type               | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- BT type is only available for channels using Kakao Open Builder chatbot<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Make a reservation on Talk<br>  - Take a survey on Talk<br>  - Apply on Talk |
| --- linkMo             | String  | X        | Mobile web link (required for WL type), limited to 1,000 characters                                                                                                                                                                                                                                                                                                                 |
| --- linkPc             | String  | X        | PC web link (optional for WL type), limited to 1,000 characters                                                                                                                                                                                                                                                                                                                     |
| --- schemeAndroid      | String  | X        | Android app link (required for AL type), limited to 1,000 characters                                                                                                                                                                                                                                                                                                                |
| --- schemeIos          | String  | X        | iOS app link (required for AL type), limited to 1,000 characters                                                                                                                                                                                                                                                                                                                    |
| --- chatExtra          | String  | X        | Meta information to be delivered for BT type buttons                                                                                                                                                                                                                                                                                                                                 |
| --- chatEvent          | String  | X        | Bot event name to be connected for BT type buttons                                                                                                                                                                                                                                                                                                                                   |
| --- bizFormKey         | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                                                                                                                                                               |
| -- coupon              | Object  | X        | Coupon element                                                                                                                                                                                                                                                                                                                                                                       |
| --- title              | String  | O        | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                        |
| --- description        | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks: not allowed<br>- For other types: up to 12 characters, line breaks: not allowed                                                                                                                                                                                  |
| --- linkMo             | String  | X        | Mobile web link (required for WL type), limited to 1,000 characters<br>If you enter the linkMo field for coupons, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                       |
| --- linkPc             | String  | X        | PC web link (optional for WL type), limited to 1,000 characters                                                                                                                                                                                                                                                                                                                     |
| --- schemeAndroid      | String  | X        | Android app link (required for AL type), limited to 1,000 characters<br>If you enter the linkMo field for coupons, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                     |
| --- schemeIos          | String  | X        | iOS app link (required for AL type), limited to 1,000 characters<br>If you enter the linkMo field for coupons, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                       |
| - tail                 | Object  | X        | More button information                                                                                                                                                                                                                                                                                                                                                              |
| -- linkMo              | String  | O        | Mobile web link, limited to 1,000 characters                                                                                                                                                                                                                                                                                                                                        |
| -- linkPc              | String  | X        | PC web link, limited to 1,000 characters                                                                                                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X        | Android app link, limited to 1,000 characters                                                                                                                                                                                                                                                                                                                                       |
| -- schemeIos           | String  | X        | iOS app link, limited to 1,000 characters                                                                                                                                                                                                                                                                                                                                           |
| recipientList          | List    | O        | Recipient list (up to 1,000 people)                                                                                                                                                                                                                                                                                                                                                 |
| - recipientNo          | String  | O        | Recipient number                                                                                                                                                                                                                                                                                                                                                                     |
| - resendParameter      | Object  | X        | Alternative delivery information                                                                                                                                                                                                                                                                                                                                                     |
| -- isResend            | boolean | X        | Whether to send SMS alternative delivery when delivery fails<br>When alternative delivery is set in the console, it is sent as alternative delivery by default.                                                                                                                                                                                                                     |
| -- resendType          | String  | X        | Alternative delivery type (SMS, LMS)<br>If there is no value, the type is distinguished according to the length of the template body.                                                                                                                                                                                                                                               |
| -- resendTitle         | String  | X        | LMS alternative delivery title<br>(If there is no value, it is sent as alternative delivery with the Plus Friend ID.)                                                                                                                                                                                                                                                               |
| -- resendContent       | String  | X        | Alternative delivery content<br>(If there is no value, it is sent as alternative delivery with [message body].)                                                                                                                                                                                                                                                                     |
| -- resendSendNo        | String  | X        | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                                                               |
| -- resendUnsubscribeNo | String  | X        | Alternative delivery 080 unsubscribe number<br><span style="color:red">(Fallback may fail, if the 080 unsubscribe number is not registered on the SMS service.)</span>                                                                                                                                                                                                            |
| - targeting            | String  | X        | Type of message target (M: Users who consented to marketing reception, N: Users who consented to marketing reception but are not friends only, I: Users who are friends)                                                                                                                                                                                                            |
| - unsubscribeNo        | String  | X        | 080 toll-free unsubscribe phone number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                                                                        |
| - unsubscribeAuthNo    | String  | X        | 080 toll-free unsubscribe authentication number (if all are not entered, sent with the toll-free unsubscribe information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234                                                                                                                                           |
| - recipientGroupingKey | String  | X        | Recipient grouping key (you can specify a grouping key for each recipient. Up to 100 characters)                                                                                                                                                                                                                                                                                    |
| senderGroupingKey      | String  | X        | Sender grouping key (you can specify a grouping key for each sender. Up to 100 characters)                                                                                                                                                                                                                                                                                          |
| resellerCode           | String  | X        | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                                                                                                            |
| createUser             | String  | X        | Registrant (stored as user UUID when sent from console)                                                                                                                                                                                                                                                                                                                             |
| statsId                | String  | X        | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                                                                                  |

<a id="request-to-send-carousel-commerce-type"></a>

#### Request to Send Carousel Commerce Type

[Request body]

```
{
  "senderKey": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "adult": boolean,
  "carousel": {
    "head": {
      "header": String,
      "content": String,
      "imageUrl": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    },
    "list": [
      {
        "additionalContent": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  },
  "recipientList": [
    {
      "recipientNo": String,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "targeting": String,
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                   | Type    | Required | Description                                                                                                                                                                                                                                                                            |
|----------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey            | String  | O  | Sender key (40 characters), group sender key not available                                                                                                                                                                                                                                                       |
| chatBubbleType       | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm            | boolean | X  | Whether to send message push alarm (default: true)                                                                                                                                                                                                                                                   |
| requestDate          | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(To be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with unsubscribe information registered in sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with unsubscribe information registered in sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                | boolean | X  | Whether adult message (default: false)                                                                                                                                                                                                                                                       |
| carousel             | Object  | O  | Carousel                                                                                                                                                                                                                                                                           |
| - head               | Object  | X  | Carousel intro                                                                                                                                                                                                                                                                       |
| -- header            | String  | O  | Carousel intro header (maximum 20 characters)                                                                                                                                                                                                                                                            |
| -- content           | String  | O  | Carousel intro content (maximum 50 characters)                                                                                                                                                                                                                                                            |
| -- imageUrl          | String  | O  | Carousel intro image URL (use image uploaded as carousel commerce type image, the image used must have the same ratio as the carousel image)                                                                                                                                                                                                    |
| -- linkMo            | String  | X  | Mobile web link (linkMo is required if any of linkMo, linkPc, schemeAndroid, schemeIos are to be used), 1,000 character limit                                                                                                                                                                                    |
| -- linkPc            | String  | X  | PC web link, 1,000 character limit                                                                                                                                                                                                                                                           |
| -- schemeAndroid     | String  | X  | Android app link, 1,000 character limit                                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | iOS app link, 1,000 character limit                                                                                                                                                                                                                                                           |
| - list               | List    | O  | Carousel list (minimum 1, maximum 5 if head exists / minimum 2, maximum 6 otherwise)                                                                                                                                                                                                                      |
| -- additionalContent | String  | X  | Additional information (maximum 34 characters), can only be used with carousel commerce type                                                                                                                                                                                                                              |
| -- imageUrl          | String  | O  | Image URL (use image uploaded as carousel commerce type image)                                                                                                                                                                                                                                           |
| -- imageLink         | String  | X  | Image link, 1,000 character limit                                                                                                                                                                                                                                                              |
| -- commerce          | Object  | O  | Commerce (can only be used with CAROUSEL_COMMERCE type)                                                                                                                                                                                                                                           |
| --- title            | String  | O  | Product title (maximum 30 characters, line breaks: not allowed)                                                                                                                                                                                                                                                       |
| --- regularPrice     | Integer | O  | Regular price (0~99,999,999)                                                                                                                                                                                                                                                        |
| --- discountPrice    | Integer | X  | Discount price (0~99,999,999)                                                                                                                                                                                                                                                          |
| --- discountRate     | Integer | X  | Discount rate (0~100), when discount price exists, one of discount rate or fixed discount amount is required                                                                                                                                                                                                                                   |
| --- discountFixed    | Integer | X  | Fixed discount amount (0~999,999), when discount price exists, one of discount rate or fixed discount amount is required                                                                                                                                                                                                                            |
| -- buttons           | List    | O  | Carousel list button list minimum 1, maximum 2                                                                                                                                                                                                                                                    |
| --- name             | String  | O  | Button title<br>- Maximum 14 characters for TEXT, IMAGE types<br>- Maximum 8 characters for other types                                                                                                                                                                                                                    |
| --- type             | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- BT type can only be used by channels using KakaoTalk Open Builder chatbot<br>- BF type can only be used as the first button, and only the following 3 phrases can be used for name<br>  - Make a reservation on Talk<br>  - Take a survey on Talk<br>  - Enter a contest on Talk |
| --- linkMo           | String  | X  | Mobile web link (required field for WL type), 1,000 character limit                                                                                                                                                                                                                                         |
| --- linkPc           | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | Android app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                       |
| --- schemeIos        | String  | X  | iOS app link (required field for AL type), 1,000 character limit                                                                                                                                                                                                                                         |
| --- chatExtra        | String  | X  | Meta information to be delivered for BT type button                                                                                                                                                                                                                                                   |
| --- chatEvent        | String  | X  | Bot event name to connect for BT type button                                                                                                                                                                                                                                                       |
| --- bizFormKey       | String  | X  | Business form key for BF type button                                                                                                                                                                                                                                                            |
| -- coupon            | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| --- title            | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| --- description      | String  | O  | Coupon detailed description<br>- Maximum 18 characters for WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types, line breaks: not allowed<br>- Maximum 12 characters for other types, line breaks: not allowed                                                                                                      |
| --- linkMo           | String  | X  | Mobile web link (required field for WL type), 1,000 character limit<br>When entering linkMo field in coupon, other fields become optional,<br>When entering channel coupon URL (format: alimtalk=coupon://) in scheme_android or scheme_ios field, other fields become optional.                                                                                   |
| --- linkPc           | String  | X  | PC web link (optional field for WL type), 1,000 character limit                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | Android app link (required field for AL type), 1,000 character limit<br>When entering linkMo field in coupon, other fields become optional,<br>When entering channel coupon URL (format: alimtalk=coupon://) in scheme_android or scheme_ios field, other fields become optional.                                                                                 |
| --- schemeIos        | String  | X  | iOS app link (required field for AL type), 1,000 character limit<br>When entering linkMo field in coupon, other fields become optional,<br>When entering channel coupon URL (format: alimtalk=coupon://) in scheme_android or scheme_ios field, other fields become optional.                                                                                   |
| - tail               | Object  | X  | More button information                                                                                                                                                                                                                                                                     |
| -- linkMo            | String  | O  | Mobile web link, 1,000 character limit                                                                                                                                                                                                                                                           |
| -- linkPc            | String  | X  | PC web link, 1,000 character limit                                                                                                                                                                                                                                                            |
| -- schemeAndroid     | String  | X  | Android app link, 1,000 character limit                                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | iOS app link, 1,000 character limit                                                                                                                                                                                                                                                           |
| recipientList        | List    | O  | Recipient list (maximum 1,000 people)                                                                                                                                                                                                                                                             |
| - recipientNo        | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter    | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                      |
| -- isResend          | boolean | X  | Whether to send SMS alternative delivery when delivery fails<br>When alternative delivery is set in console, alternative delivery is sent by default.                                                                                                                                                                                                                       |
| -- resendType        | String  | X  | Alternative delivery type (SMS,LMS)<br>If no value, type is classified according to template body length.                                                                                                                                                                                                                       |
| -- resendTitle       | String  | X  | LMS alternative delivery title<br>(If no value, alternative delivery is sent with Plus Friend ID.)                                                                                                                                                                                                                               |
| -- resendContent     | String  | X  | Alternative delivery content<br>(If no value, alternative delivery is sent with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo      | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| - targeting         | String  | X  | Type of message target (M: Users who consented to marketing, N: Only to users who are not friends and consented to marketing, I: Users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free unsubscribe phone number (if all are not entered, sent with unsubscribe information registered in sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free unsubscribe authentication number (if all are not entered, sent with unsubscribe information registered in sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key for each recipient. Maximum 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key for each sender. Maximum 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when reseller sends)                                                                                                                                                                                                                                                       |
| createUser           | String  | X  | Creator (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId              | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="response-3"></a>

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
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| Name             | Type    | Not Null | Description                                                                                     |
|:-----------------|:--------|:---------|:------------------------------------------------------------------------------------------------|
| header           | Object  | O        | Response header information                                                                     |
| - isSuccessful   | boolean | O        | API request success                                                                             |
| - resultCode     | Integer | O        | Result code of API call (success: 0, error code on failure)                                   |
| - resultMessage  | String  | O        | Result message of API call ("success" or related success message on success, detailed failure message on failure) |
| message          | Object  | X        | Message sending result information (exists only when there is a sending request)               |
| - requestId      | String  | X        | Sending request ID (ID that uniquely identifies each sending request)                         |
| - sendResults    | Array   | O        | List of sending results by recipient                                                           |
| -- recipientSeq  | Integer | O        | Sequence number in the recipient list                                                          |
| -- recipientNo   | String  | X        | Recipient phone number                                                                         |
| -- resultCode    | Integer | O        | Sending result code by recipient (success and various failure codes may exist)                |
| -- resultMessage | String  | O        | Sending result message by recipient ("success" or related message on success, detailed failure message on failure) |

<a id="request-to-send-basic-message"></a>

## Send Basic Form Message Request

* Sending using templates.
* Marketing consent-based sending can be used.
    * You can specify the targeting field to designate the type of message recipients.
        * M: Users who consented to receive advertising information from the client (KakaoTalk reception consent)
        * N: Users who consented to receive advertising information from the client (KakaoTalk reception consent) - Channel friends
        * I: Client's delivery request targets ∩ Channel friends
* You can use all 8 message types from the existing FriendTalk.
* BT button type cannot be used.
* AC (Add Channel) button can be used.
* When using BF buttons, you can upload a business form ID issued by Kakao to get a business form key.
* Alternative delivery can be set with resendParameter for each recipient.
    * When using alternative delivery, SMS AppKey registration and delivery settings are required through the alternative delivery management API.
* **Delivery restricted during night (20:50~08:00 on the following day)**

<a id="cautions-for-use"></a>

### Notes on usage

- unsubscribeNo and unsubscribeAuthNo are the 080 free opt-out phone number and authentication number. If either is not entered, the message will be sent using the free opt-out information registered in the sender profile.
- When you enter unsubscribeNo and unsubscribeAuthNo during sending, the message will be sent with the entered values instead of the free opt-out information registered in the sender profile.
- When you do not enter unsubscribeNo and unsubscribeAuthNo during sending, the message will be sent with the free opt-out information registered in the sender profile.
- unsubscribeNo and unsubscribeAuthNo can be entered for each recipient. When both common fields and recipient-specific fields are entered, the common fields take precedence.

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                      |
|--------------|--------|----------|----------------------------------|
| X-Secret-Key | String | O        | Can be created from the console. |

<a id="requested-4"></a>

#### Delivery Request

```
{
  "senderKey": String,
  "templateCode": String,
  "pushAlarm": boolean,
  "requestDate": String,
  "unsubscribeNo": String,
  "unsubscribeAuthNo": String,
  "recipientList": [
    {
      "recipientNo": String,
      "targeting": String,
      "templateParameter": Object,
      "imageParameters": List,
      "videoParameter": Object,
      "resendParameter": {
          "isResend": boolean,
          "resendType": String,
          "resendTitle": String,
          "resendContent": String,
          "resendSendNo": String,
          "resendUnsubscribeNo": String
      },
      "unsubscribeNo": String,
      "unsubscribeAuthNo": String,
      "recipientGroupingKey": String
    }
  ],
  "senderGroupingKey": String,
  "resellerCode": String,
  "createUser": String,
  "statsId": String
}
```

| Name                   | Type    | Required | Description                                                                                                                                                                                                                                      |
|------------------------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O        | Sender key (40 characters), group sender key cannot be used                                                                                                                                                                                          |
| templateCode           | String  | O        | Template code to use                                                                                                                                                                                                                                 |
| pushAlarm              | boolean | X        | Whether to send message push alarm (default: true)                                                                                                                                                                                                   |
| requestDate            | String  | X        | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                |
| unsubscribeNo          | String  | X        | 080 free opt-out phone number (sent with opt-out information registered in the sender profile when all fields are not entered)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                        |
| unsubscribeAuthNo      | String  | X        | 080 free opt-out authentication number (sent with opt-out information registered in the sender profile when all fields are not entered)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234                         |
| recipientList          | List    | O        | Recipient list (up to 1,000 recipients)                                                                                                                                                                                                             |
| - recipientNo          | String  | O        | Recipient number                                                                                                                                                                                                                                     |
| - targeting            | String  | O        | Type of message target (M: users who agreed to receive marketing, N: only non-friend users who agreed to receive marketing, I: friend users)                                                                                                        |
| - templateParameter    | Object  | X        | Template parameter (required when template includes variables to be replaced)<br>- Carousel type: Use `key@$.carousel.list[index]` format when applying different replacement values for each item (example: `productName@$.carousel.list[0]`)<br>- If head exists, head occupies list[0], and actual items start from list[1]<br>- Cannot use `@` character in replacement key<br>- Maximum 1,300 characters for each key/value |
| - imageParameters      | List    | X        | Dynamic parameters that can change template image field values (only JSON lists of the same size as the number of images in the template can be used; when using, empty JSON objects must be entered for images that will not be changed)      |
| -- imageUrl            | String  | O        | Image URL                                                                                                                                                                                                                                            |
| -- imageLink           | String  | X        | Image link                                                                                                                                                                                                                                           |
| - videoParameter       | Object  | X        | Dynamic parameter that can change template video field values                                                                                                                                                                                        |
| -- videoUrl            | String  | O        | KakaoTV video URL                                                                                                                                                                                                                                    |
| -- thumbnailUrl        | String  | X        | Image URL for video thumbnail                                                                                                                                                                                                                        |
| - resendParameter      | Object  | X        | Fallback information                                                                                                                                                                                                                                 |
| -- isResend            | boolean | X        | Whether to send SMS fallback when sending fails<br>When fallback is configured in the console, fallback is sent by default.                                                                                                                        |
| -- resendType          | String  | X        | Fallback type (SMS,LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                     |
| -- resendTitle         | String  | X        | LMS fallback title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                          |
| -- resendContent       | String  | X        | Fallback content<br>(resent with [message body] if value is unavailable.)                                                                                                                                                                           |
| -- resendSendNo        | String  | X        | Fallback sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                           |
| - unsubscribeNo        | String  | X        | 080 free opt-out phone number (sent with opt-out information registered in the sender profile when all fields are not entered)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                        |
| - unsubscribeAuthNo    | String  | X        | 080 free opt-out authentication number (sent with opt-out information registered in the sender profile when all fields are not entered)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234                         |
| - recipientGroupingKey | String  | X        | Recipient grouping key (you can specify a grouping key for each recipient. Maximum 100 characters)                                                                                                                                                   |
| senderGroupingKey      | String  | X        | Sender grouping key (you can specify a grouping key for each sender. Maximum 100 characters)                                                                                                                                                        |
| resellerCode           | String  | X        | Reseller code (used when reseller sends)                                                                                                                                                                                                            |
| createUser             | String  | X        | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                           |
| statsId                | String  | X        | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                  |

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
    "sendResults": [
      {
        "recipientSeq": Integer,
        "recipientNo": String,
        "resultCode": Integer,
        "resultMessage": String
      }
    ]
  }
}
```

| Name             | Type    | Not Null | Description                                                                                    |
|:-----------------|:--------|:---------|:-----------------------------------------------------------------------------------------------|
| header           | Object  | O        | Response header information                                                                    |
| - isSuccessful   | boolean | O        | API call success                                                                               |
| - resultCode     | Integer | O        | API call result code (success: 0, error code on failure)                                      |
| - resultMessage  | String  | O        | API call result message (success message on success, detailed failure message on failure)     |
| message          | Object  | X        | Message sending result information (exists only when there is a sending request)               |
| - requestId      | String  | X        | Sending request ID (ID that uniquely identifies each sending request)                         |
| - sendResults    | Array   | O        | List of sending results by recipient                                                           |
| -- recipientSeq  | Integer | O        | Sequence number in the recipient list                                                          |
| -- recipientNo   | String  | X        | Recipient phone number                                                                         |
| -- resultCode    | Integer | O        | Sending result code by recipient (success and various failure codes may exist)                |
| -- resultMessage | String  | O        | Sending result message by recipient (success message on success, detailed failure message on failure) |

<a id="view-sending-list"></a>

## Query delivery list

<a id="requested-5"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description |
|--------|--------|-------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter] No.1 or No.2 is conditionally required

| Name                 | Type   | Required           | Description |
|----------------------|--------|--------------------|-------------|
| requestId            | String | Conditionally required (No.1) | Request ID |
| startRequestDate     | String | Conditionally required (No.2) | Start date of delivery request (yyyy-MM-dd HH:mm) |
| endRequestDate       | String | Conditionally required (No.2) | End date of delivery request (yyyy-MM-dd HH:mm) |
| senderKey            | String | X                  | Sender key |
| templateCode         | String | X                  | Template code |
| recipientNo          | String | X                  | Recipient number |
| messageStatus        | String | X                  | Request status (COMPLETED: Success, FAILED: Failed) |
| resultCode           | String | X                  | Delivery result (MRC01: Success, MRC02: Failed) |
| senderGroupingKey    | String | X                  | Sender grouping key |
| recipientGroupingKey | String | X                  | Recipient grouping key |
| pageNum              | String | X                  | Page number (Default: 1) |
| pageSize             | String | X                  | Number of queries (Default: 15, Max: 1000) |

<a id="response-5"></a>

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
        "targeting": String,
        "requestDate": String,
        "createDate": String,
        "receiveDate": String,
        "chatBubbleType": String,
        "pushAlarm": boolean,
        "messageStatus": String,
        "resendStatusCode": String,
        "resendStatusName": String,
        "resendResultCode": String,
        "resendRequestId": String,
        "isAddedChannel": boolean,
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

| Name                        | Type    | Not Null | Description |
|:----------------------------|:--------|:---------|:------------|
| header                      | Object  | O        | Header area |
| - resultCode                | Integer | O        | Result code |
| - resultMessage             | String  | O        | Result message |
| - isSuccessful              | boolean | O        | Success |
| messageSearchResultResponse | Object  | X        | Body area |
| - messages                  | Array   | O        | Message list |
| -- requestId                | String  | O        | Request ID |
| -- recipientSeq             | Integer | O        | Recipient sequence number |
| -- plusFriendId             | String  | O        | Sender Profile ID |
| -- senderKey                | String  | O        | Sender key |
| -- templateCode             | String  | X        | Template code |
| -- recipientNo              | String  | O        | Recipient number |
| -- targeting                | String  | O        | Type of message target (M: Users who agreed to receive marketing, N: Only to users who agreed to receive marketing but are not friends, I: Users who are friends) |
| -- requestDate              | String  | O        | Date and time of request (yyyy-MM-dd HH:mm)<br>(To be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance |
| -- createDate               | String  | O        | Registration date and time |
| -- receiveDate              | String  | X        | Reception date and time |
| -- chatBubbleType           | String  | O        | Message type |
| -- pushAlarm                | boolean | O        | Push alarm status |
| -- messageStatus            | String  | O        | Request status (COMPLETED: Success, FAILED: Failed) |
| -- resendStatusCode         | String  | X        | Alternative delivery status code |
| -- resendStatusName         | String  | X        | Alternative delivery status name |
| -- resendResultCode         | String  | X        | Alternative delivery result code |
| -- resendRequestId          | String  | X        | Alternative delivery request ID |
| -- isAddedChannel           | boolean | O        | Channel friend status |
| -- resultCode               | String  | X        | Reception result code |
| -- resultCodeName           | String  | X        | Reception result code name |
| -- createUser               | String  | X        | Registrant (saved as user UUID when sending from console) |
| -- senderGroupingKey        | String  | X        | Sender grouping key |
| -- recipientGroupingKey     | String  | X        | Recipient grouping key |
| - totalCount                | Integer | O        | Total count |

<a id="view-single-sending"></a>

## Query single delivery

<a id="requested-6"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type    | Description        |
|--------------|---------|-------------------|
| appkey       | String  | Unique app key    |
| requestId    | String  | Request ID        |
| recipientSeq | Integer | Recipient sequence number |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="response-6"></a>

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
    "targeting": String,
    "requestDate": String,
    "createDate": String,
    "receiveDate": String,
    "chatBubbleType": String,
    "content": String,
    "adult": boolean,
    "header": String,
    "additionalContent": String,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "chatExtra": String,
        "chatEvent": String,
        "bizFormKey": String
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeIos": String,
          "schemeAndroid": String
        }
      ]
    },
    "coupon": {
      "title": String,
      "description": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    },
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
    "carousel": {
      "head": {
        "header": String,
        "content": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String
      },
      "list": [
        {
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String,
              "chatExtra": String,
              "chatEvent": String,
              "bizFormKey": String
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          }
        }
      ],
      "tail": {
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    },
    "templateParameter": String,
    "pushAlarm": boolean,
    "messageStatus": String,
    "isAddedChannel": boolean,
    "resultCode": String,
    "resultCodeName": String,
    "resendStatusCode": String,
    "resendStatusName": String,
    "resendResultCode": String,
    "resendRequestId": String,
    "createUser": String,
    "senderGroupingKey": String,
    "recipientGroupingKey": String
  }
}
```

| Name                    | Type      | Not Null | Description                                                                                               | 
 |:----------------------|:--------|:---------|:-------------------------------------------------------------------------------------------------| 
| header                | Object  | O        | Header area                                                                                            | 
| - resultCode          | Integer | O        | Result code                                                                                            | 
| - resultMessage       | String  | O        | Result message                                                                                           | 
| - isSuccessful        | boolean | O        | Success                                                                                            | 
| message               | Object  | X        | Message body area (may not exist if message fails)                                                                     | 
| - requestId           | String  | O        | Request ID (Not Null when message object exists)                                                                 | 
| - recipientSeq        | Integer | O        | Recipient sequence number (Not Null when message object exists)                                                            | 
| - plusFriendId        | String  | O        | Sender Profile ID (Not Null when message object exists)                                                             | 
| - senderKey           | String  | O        | Sender Key (Not Null when message object exists)                                                                  | 
| - templateCode        | String  | X        | Template Code                                                                                           | 
| - recipientNo         | String  | O        | Recipient number (Not Null when message object exists)                                                                 | 
| - targeting           | String  | O        | Message target type (M: users who agreed to marketing reception, N: only non-friend users who agreed to marketing reception, I: friend users) (Not Null when message object exists) | 
| - requestDate         | String  | O        | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if field is not sent)<br>Can be scheduled up to 60 days in advance (Not Null when message object exists)                                                                 | 
| - createDate          | String  | O        | Registration date and time (Not Null when message object exists)                                                                 | 
| - receiveDate         | String  | X        | Reception date and time                                                                                            | 
| - chatBubbleType      | String  | O        | Message type (Not Null when message object exists)                                                                | 
| - content             | String  | X        | Message content                                                                                           | 
| - adult               | boolean | O        | Adult message status (Not Null when message object exists)                                                            | 
| - header              | String  | X        | Header (in message)                                                                                       | 
| - additionalContent   | String  | X        | Additional information (in message)                                                                                    |
| - image               | Object  | X        | Image element                                                                                           | 
| -- imageUrl           | String  | O        | Image URL (Not Null when image object exists)                                                                 | 
| -- imageLink          | String  | X        | Image link                                                                                           | 
| - buttons             | Array   | X        | Button list                                                                                            | 
| -- name               | String  | O        | Button title (Not Null when buttons array item exists)                                                              | 
| -- type               | String  | O        | Button type (Not Null when buttons array item exists)                                                              | 
| -- linkMo             | String  | X        | Mobile web link                                                                                         | 
| -- linkPc             | String  | X        | PC web link                                                                                          | 
| -- schemeIos          | String  | X        | iOS app link                                                                                         | 
| -- schemeAndroid      | String  | X        | Android app link                                                                                       | 
| -- chatExtra          | String  | X        | Meta information to deliver for BT type button                                                                      | 
| -- chatEvent          | String  | X        | Bot event name to connect for BT (Bot Transfer) type button                                                                          | 
| -- bizFormKey         | String  | X        | Business form key for BF type button                                                                               | 
| - item                | Object  | X        | Wide list element                                                                                       | 
| -- list               | Array   | X        | Wide list (Nullable when item object exists)                                                                  | 
| --- title             | String  | X        | Item title                                                                                           | 
| --- imageUrl          | String  | O        | Item image URL (Not Null when item.list item exists)                                                         | 
| --- linkMo            | String  | O        | Mobile web link (Not Null when item.list item exists)                                                            | 
| --- linkPc            | String  | X        | PC web link                                                                                          | 
| --- schemeIos         | String  | X        | iOS app link                                                                                         | 
| --- schemeAndroid     | String  | X        | Android app link                                                                                       | 
| - coupon              | Object  | X        | Coupon element                                                                                            | 
| -- title              | String  | O        | Coupon title (Not Null when coupon object exists)                                                                  | 
| -- description        | String  | O        | Coupon detailed description (Not Null when coupon object exists)                                                               | 
| -- linkMo             | String  | X        | Mobile web link                                                                                         | 
| -- linkPc             | String  | X        | PC web link                                                                                          | 
| -- schemeAndroid      | String  | X        | Android app link                                                                                       | 
| -- schemeIos          | String  | X        | iOS app link                                                                                         | 
| - commerce            | Object  | X        | Commerce element                                                                                           | 
| -- title              | String  | O        | Product title (Not Null when commerce object exists)                                                                | 
| -- regularPrice       | Integer | X        | Regular price                                                                                            | 
| -- discountPrice      | Integer | X        | Discount price                                                                                             | 
| -- discountRate       | Integer | X        | Discount rate                                                                                              | 
| -- discountFixed      | Integer | X        | Fixed discount price                                                                                           | 
| - video               | Object  | X        | Video element                                                                                           | 
| -- videoUrl           | String  | O        | KakaoTV video URL (Not Null when video object exists)                                                           | 
| -- thumbnailUrl       | String  | X        | Video thumbnail image URL                                                                                 | 
| - carousel            | Object  | X        | Carousel                                                                                              | 
| -- head               | Object  | X        | Carousel intro (Nullable when carousel object exists)                                                              | 
| --- header            | String  | O        | Carousel intro header (Not Null when head object exists)                                                               | 
| --- content           | String  | O        | Carousel intro content (Not Null when head object exists)                                                               | 
| --- imageUrl          | String  | O        | Carousel intro image URL (Not Null when head object exists)                                                           | 
| --- linkMo            | String  | X        | Mobile web link                                                                                         | 
| --- linkPc            | String  | X        | PC web link                                                                                          | 
| --- schemeIos         | String  | X        | iOS app link                                                                                         | 
| --- schemeAndroid     | String  | X        | Android app link                                                                                       | 
| -- list               | Array   | O        | Carousel list (Not Null when carousel object exists)                                                              | 
| --- header            | String  | X        | Carousel item header                                                                                       | 
| --- message           | String  | O        | Carousel item message (Not Null when list item exists)                                                              | 
| --- additionalContent | String  | X        | Additional information                                                                                            | 
| --- imageUrl          | String  | X        | Image URL                                                                                          | 
| --- imageLink         | String  | X        | Image link                                                                                           | 
| --- commerce          | Object  | X        | Commerce (in carousel)                                                                                      | 
| ---- title            | String  | O        | Product title (Not Null when carousel.list.commerce exists)                                                     | 
| ---- regularPrice     | Integer | X        | Regular price                                                                                            | 
| ---- discountPrice    | Integer | X        | Discount price                                                                                             | 
| ---- discountRate     | Integer | X        | Discount rate                                                                                              | 
| ---- discountFixed    | Integer | X        | Fixed discount price                                                                                           | 
| --- buttons           | Array   | X        | Button list (in carousel)                                                                                    | 
| ---- name             | String  | O        | Button title (Not Null when carousel.list.buttons item exists)                                                   | 
| ---- type             | String  | O        | Button type (Not Null when carousel.list.buttons item exists)                                                   | 
| ---- linkMo           | String  | X        | Mobile web link                                                                                         | 
| ---- linkPc           | String  | X        | PC web link                                                                                          | 
| ---- schemeAndroid    | String  | X        | Android app link                                                                                       | 
| ---- schemeIos        | String  | X        | iOS app link                                                                                         | 
| ---- chatExtra        | String  | X        | Meta information to deliver for BT type button                                                                      | 
| ---- chatEvent        | String  | X        | Bot event name to connect for BT (Bot Transfer) type button                                                                          | 
| ---- bizFormKey       | String  | X        | Business form key for BF type button                                                                               | 
| --- coupon            | Object  | X        | Coupon (in carousel)                                                                                       | 
| ---- title            | String  | O        | Coupon title (Not Null when carousel.list.coupon exists)                                                       | 
| ---- description      | String  | O        | Coupon detailed description (Not Null when carousel.list.coupon exists)                                                    | 
| ---- linkMo           | String  | X        | Mobile web link                                                                                         | 
| ---- linkPc           | String  | X        | PC web link                                                                                          | 
| ---- schemeAndroid    | String  | X        | Android app link                                                                                       | 
| ---- schemeIos        | String  | X        | iOS app link                                                                                         | 
| -- tail               | Object  | X        | More button information (Nullable when carousel object exists)                                                            | 
| --- linkMo            | String  | O        | Mobile web link (Not Null when tail object exists)                                                                 | 
| --- linkPc            | String  | X        | PC web link                                                                                          | 
| --- schemeAndroid     | String  | X        | Android app link                                                                                       | 
| --- schemeIos         | String  | X        | iOS app link                                                                                         | 
| - templateParameter   | String  | X        | Template parameter                                                                                         | 
| - pushAlarm           | boolean | O        | Push alarm status (Not Null when message object exists)                                                              | 
| - messageStatus       | String  | O        | Request status (COMPLETED: successful, FAILED: failed) (Not Null when message object exists)                                     | 
| - isAddedChannel      | boolean | O        | Channel friend status (Not Null when message object exists)                                                              | 
| - resultCode          | String  | X        | Reception result code (in message)                                                                                 | 
| - resultCodeName      | String  | X        | Reception result code name (in message)                                                                                | 
| - resendStatusCode    | String  | X        | Alternative delivery status code                                                                                      | 
| - resendStatusName    | String  | X        | Alternative delivery status code name                                                                                     | 
| - resendResultCode    | String  | X        | Alternative delivery result code                                                                                      | 
| - resendRequestId     | String  | X        | Alternative delivery request ID                                                                                      | 
| - createUser          | String  | X        | Registrant (saved as user UUID when sending from console)                                                                      |
| - senderGroupingKey   | String  | X        | Sender grouping key                                                 |
| - recipientGroupingKey | String  | X        | Recipient grouping key                                           |

<a id="cancel-message-sending"></a>

## Cancel Message Delivery

<a id="requested-7"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type    | Description    |
|-----------|---------|----------------|
| appkey    | String  | Unique app key |
| requestId | String  | Request ID     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter]

| Name         | Type   | Required | Description                                                                         |
|--------------|--------|-----------|-------------------------------------------------------------------------------------|
| recipientSeq | String | X        | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

<a id="response-7"></a>

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
| header          | Object  |    X     | Header area    |
| - resultCode    | Integer |    X     | Result code    |
| - resultMessage | String  |    X     | Result message |
| - isSuccessful  | Boolean |    X     | Success        |

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

<a id="manage-templates"></a>

## Template Management

<a id="view-template-list"></a>

### View Template List

<a id="requested-8"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description    |
|-----------|--------|----------------|
| appkey    | String | Unique app key |
| senderKey | String | Sender key     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.     |

[Query parameter]

| Name         | Type    | Required | Description                              |
|--------------|---------|----------|------------------------------------------|
| templateCode | String  | X        | Template code                            |
| templateName | String  | X        | Template name                            |
| status       | String  | X        | Template status code                     |
| pageNum      | Integer | X        | Page number (Default: 1)                |
| pageSize     | Integer | X        | Number of queries (Default: 15, Max: 1000) |

<a id="response-8"></a>

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
        "plusFriendType": String,
        "templateCode": String,
        "templateName": String,
        "chatBubbleType": String,
        "content": String,
        "header": String,
        "additionalContent": String,
        "adult": boolean,
        "image": {
          "imageUrl": String,
          "imageLink": String
        },
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeIos": String,
            "schemeAndroid": String,
            "bizFormId": Integer,
          }
        ],
        "item": {
          "list": [
            {
              "title": String,
              "imageUrl": String,
              "linkMo": String,
              "linkPc": String,
              "schemeAndroid": String,
              "schemeIos": String
            }
          ]
        },
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
        "video": {
          "videoUrl": String,
          "thumbnailUrl": String
        },
        "carousel": {
          "head": {
            "header": String,
            "content": String,
            "imageUrl": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          },
          "list": [
            {
              "header": String,
              "message": String,
              "additionalContent": String,
              "imageUrl": String,
              "imageLink": String,
              "commerce": {
                "title": String,
                "regularPrice": Integer,
                "discountPrice": Integer,
                "discountRate": Integer,
                "discountFixed": Integer
              },
              "buttons": [
                {
                  "name": String,
                  "type": String,
                  "linkMo": String,
                  "linkPc": String,
                  "schemeIos": String,
                  "schemeAndroid": String,
                  "bizFormId": Integer,
                }
              ],
              "coupon": {
                "title": String,
                "description": String,
                "linkMo": String,
                "linkPc": String,
                "schemeAndroid": String,
                "schemeIos": String
              }
            }
          ],
          "tail": {
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          }
        },
        "status": String,
        "createDate": String,
        "updateDate": String
      }
    ],
    "totalCount": Integer
  }
}
```

| Name                     | Type    | Not Null | Description                                           |
|:-------------------------|:--------|:---------|:------------------------------------------------------|
| header                   | Object  | O        | Header                                                |
| - resultCode             | Integer | O        | Result code                                           |
| - resultMessage          | String  | O        | Result message                                        |
| - isSuccessful           | boolean | O        | Success                                               |
| templateListResponse     | Object  | O        | Body                                                  |
| - templates              | Array   | O        | Template list                                         |
| -- plusFriendId          | String  | O        | Sender Profile ID                                     |
| -- plusFriendType        | String  | O        | Sender Profile type                                   |
| -- templateCode          | String  | O        | Template Code                                         |
| -- templateName          | String  | O        | Template name                                         |
| -- chatBubbleType        | String  | O        | Message type                                          |
| -- content               | String  | X        | Message content                                       |
| -- header                | String  | X        | Header                                                |
| -- additionalContent     | String  | X        | (See description table)                               |
| -- adult                 | boolean | O        | Whether adult message                                 |
| -- image                 | Object  | X        | Image information                                     |
| --- imageUrl             | String  | O        | Image URL                                             |
| --- imageLink            | String  | X        | Image link                                            |
| -- buttons               | Array   | X        | Button list                                           |
| --- name                 | String  | O        | Button title                                          |
| --- type                 | String  | O        | Button type                                           |
| --- linkMo               | String  | X        | Mobile web link                                       |
| --- linkPc               | String  | X        | PC web link                                           |
| --- schemeIos            | String  | X        | iOS app link                                          |
| --- schemeAndroid        | String  | X        | Android app link                                      |
| --- bizFormId            | Integer | X        | Biz form ID (JSON standard)                           |
| -- item                  | Object  | X        | Wide list element                                     |
| --- list                 | Array   | X        | Wide list                                             |
| ---- title               | String  | X        | Item title                                            |
| ---- imageUrl            | String  | O        | Item image URL                                        |
| ---- linkMo              | String  | O        | Mobile web link                                       |
| ---- linkPc              | String  | X        | PC web link                                           |
| ---- schemeAndroid       | String  | X        | Android app link                                      |
| ---- schemeIos           | String  | X        | iOS app link                                          |
| -- coupon                | Object  | X        | Coupon element                                        |
| --- title                | String  | O        | Coupon title                                          |
| --- description          | String  | O        | Coupon detailed description                           |
| --- linkMo               | String  | X        | Mobile web link                                       |
| --- linkPc               | String  | X        | PC web link                                           |
| --- schemeAndroid        | String  | X        | Android app link                                      |
| --- schemeIos            | String  | X        | iOS app link                                          |
| -- commerce              | Object  | X        | Commerce element                                      |
| --- title                | String  | O        | Product title                                         |
| --- regularPrice         | Integer | X        | Regular price                                         |
| --- discountPrice        | Integer | X        | Discount price                                        |
| --- discountRate         | Integer | X        | Discount rate                                         |
| --- discountFixed        | Integer | X        | Fixed discount amount                                 |
| -- video                 | Object  | X        | Video element                                         |
| --- videoUrl             | String  | O        | Kakao TV video URL                                    |
| --- thumbnailUrl         | String  | X        | Video thumbnail image URL                             |
| -- carousel              | Object  | X        | Carousel                                              |
| --- head                 | Object  | X        | Carousel intro                                        |
| ---- header              | String  | O        | Carousel intro header                                 |
| ---- content             | String  | O        | Carousel intro content                                |
| ---- imageUrl            | String  | O        | Carousel intro image address                          |
| ---- linkMo              | String  | X        | Mobile web link                                       |
| ---- linkPc              | String  | X        | PC web link                                           |
| ----- schemeAndroid      | String  | X        | Android app link                                      |
| ----- schemeIos          | String  | X        | iOS app link                                          |
| ---- list                | Array   | O        | Carousel list                                         |
| ----- header             | String  | O        | Carousel item header                                  |
| ----- message            | String  | O        | Carousel item message (content mapping for Not Null list) |
| ----- additionalContent  | String  | X        | Additional information                                |
| ----- imageUrl           | String  | O        | Image URL (within carousel item)                      |
| ----- imageLink          | String  | X        | Image link (within carousel item)                     |
| ----- commerce           | Object  | O        | Commerce (within carousel item)                       |
| ------ title             | String  | O        | Product title                                         |
| ------ regularPrice      | Integer | X        | Regular price                                         |
| ------ discountPrice     | Integer | X        | Discount price                                        |
| ------ discountRate      | Integer | X        | Discount rate                                         |
| ------ discountFixed     | Integer | X        | Fixed discount amount                                 |
| ----- buttons            | Array   | O        | Carousel list button list                             |
| ------ name              | String  | O        | Button title                                          |
| ------ type              | String  | O        | Button type                                           |
| ------ linkMo            | String  | X        | Mobile web link                                       |
| ------ linkPc            | String  | X        | PC web link                                           |
| ------ schemeIos         | String  | X        | iOS app link                                          |
| ------ schemeAndroid     | String  | X        | Android app link                                      |
| ------ bizFormId         | Integer | X        | Biz form ID (JSON standard)                           |
| ----- coupon             | Object  | X        | Coupon element (within carousel item)                 |
| ------ title             | String  | X        | Coupon title                                          |
| ------ description       | String  | X        | Coupon detailed description                           |
| ------ linkMo            | String  | X        | Mobile web link                                       |
| ------ linkPc            | String  | X        | PC web link                                           |
| ------ schemeAndroid     | String  | X        | Android app link                                      |
| ------ schemeIos         | String  | X        | iOS app link                                          |
| ---- tail                | Object  | X        | View more button information                          |
| ----- linkMo             | String  | O        | Mobile web link                                       |
| ----- linkPc             | String  | X        | PC web link                                           |
| ----- schemeAndroid      | String  | X        | Android app link                                      |
| ----- schemeIos          | String  | X        | iOS app link                                          |
| --- status               | String  | O        | Template status (A: registered, S: blocked)          |
| --- createDate           | String  | O        | Registration date and time                            |
| --- updateDate           | String  | X        | Modification date and time                            |
| - totalCount             | Integer | O        | Total count                                           |

<a id="view-single-template"></a>

### Get a template

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description  |
|--------------|--------|--------------|
| appkey       | String | Unique app key |
| senderKey    | String | Sender Key   |
| templateCode | String | Template Code |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description              |
|--------------|--------|----------|--------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | Key for duplicate message sending requests<br>If a request is made with the same key for 10 minutes, the request will be failed. |

<a id="response-9"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "plusFriendId": String,
    "plusFriendType": String,
    "templateCode": String,
    "templateName": String,
    "chatBubbleType": String,
    "content": String,
    "header": String,
    "additionalContent": String,
    "adult": boolean,
    "image": {
      "imageUrl": String,
      "imageLink": String
    },
    "buttons": [
      {
        "name": String,
        "type": String,
        "linkMo": String,
        "linkPc": String,
        "schemeIos": String,
        "schemeAndroid": String,
        "bizFormId": Integer
      }
    ],
    "item": {
      "list": [
        {
          "title": String,
          "imageUrl": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      ]
    },
    "coupon": {
      "title": String,
      "description": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    },
    "commerce": {
      "title": String,
      "regularPrice": Integer,
      "discountPrice": Integer,
      "discountRate": Integer,
      "discountFixed": Integer
    },
    "video": {
      "videoUrl": String,
      "thumbnailUrl": String
    },
    "carousel": {
      "head": {
        "header": String,
        "content": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      "list": [
        {
          "header": String,
          "message": String,
          "additionalContent": String,
          "imageUrl": String,
          "imageLink": String,
          "commerce": {
            "title": String,
            "regularPrice": Integer,
            "discountPrice": Integer,
            "discountRate": Integer,
            "discountFixed": Integer
          },
          "buttons": [
            {
              "name": String,
              "type": String,
              "linkMo": String,
              "linkPc": String,
              "schemeIos": String,
              "schemeAndroid": String,
              "bizFormId": Integer
            }
          ],
          "coupon": {
            "title": String,
            "description": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String
          }
        }
      ],
      "tail": {
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    },
    "status": String,
    "createDate": String,
    "updateDate": String
  }
}
```

| Name                    | Type      | Not Null | Description                   |
|:----------------------|:--------|:---------|:---------------------|
| header                | Object  | O        | Header area                |
| - resultCode          | Integer | O        | Result code                |
| - resultMessage       | String  | O        | Result message               |
| - isSuccessful        | boolean | O        | Success            |
| template              | Object  | O        | Template body area            |
| - plusFriendId        | String  | O        | Sender profile ID            |
| - plusFriendType      | String  | O        | Sender profile type            |
| - templateCode        | String  | O        | Template code               |
| - templateName        | String  | O        | Template name                |
| - chatBubbleType      | String  | O        | Message type               |
| - pushAlarm           | boolean | X        | Push alarm availability             |
| - content             | String  | X        | Message content               |
| - adult               | boolean | O        | Adult message availability           |
| - header              | String  | X        | Header (within template)           |
| - additionalContent   | String  | X        | Additional information (within template)        |
| - image               | Object  | X        | Image information               |
| -- imageUrl           | String  | O        | Image URL              |
| -- imageLink          | String  | X        | Image link               |
| - buttons             | Array   | X        | Button list                |
| -- name               | String  | O        | Button title                |
| -- type               | String  | O        | Button type                |
| -- linkMo             | String  | X        | Mobile web link             |
| -- linkPc             | String  | X        | PC web link              |
| -- schemeIos          | String  | X        | iOS app link             |
| -- schemeAndroid      | String  | X        | Android app link           |
| -- bizFormId          | Integer | X        | Biz form ID (JSON standard)     |
| - item                | Object  | X        | Wide list element           |
| -- list               | Array   | X        | Wide list              |
| --- title             | String  | X        | Item title               |
| --- imageUrl          | String  | O        | Item image URL          |
| --- linkMo            | String  | O        | Mobile web link             |
| --- linkPc            | String  | X        | PC web link              |
| --- schemeIos         | String  | X        | iOS app link             |
| --- schemeAndroid     | String  | X        | Android app link           |
| - coupon              | Object  | X        | Coupon element                |
| -- title              | String  | O        | Coupon title                |
| -- description        | String  | O        | Coupon detailed description             |
| -- linkMo             | String  | X        | Mobile web link             |
| -- linkPc             | String  | X        | PC web link              |
| -- schemeIos          | String  | X        | iOS app link             |
| -- schemeAndroid      | String  | X        | Android app link           |
| - commerce            | Object  | X        | Commerce element               |
| -- title              | String  | O        | Product title                |
| -- regularPrice       | Integer | X        | Regular price                |
| -- discountPrice      | Integer | X        | Discount price                 |
| -- discountRate       | Integer | X        | Discount rate                  |
| -- discountFixed      | Integer | X        | Fixed discount price               |
| - video               | Object  | X        | Video element               |
| -- videoUrl           | String  | O        | KakaoTV video URL        |
| -- thumbnailUrl       | String  | X        | Video thumbnail image URL     |
| - carousel            | Object  | X        | Carousel                  |
| -- head               | Object  | X        | Carousel intro              |
| --- header            | String  | O        | Carousel intro header           |
| --- content           | String  | O        | Carousel intro content           |
| --- imageUrl          | String  | O        | Carousel intro image address       |
| --- linkMo            | String  | X        | Mobile web link             |
| --- linkPc            | String  | X        | PC web link              |
| --- schemeIos         | String  | X        | iOS app link             |
| --- schemeAndroid     | String  | X        | Android app link           |
| -- list               | Array   | O        | Carousel list              |
| --- header            | String  | O        | Carousel item header           |
| --- message           | String  | O        | Carousel item message          |
| --- additionalContent | String  | X        | Additional information (within carousel item)    |
| --- imageUrl          | String  | O        | Image URL (within carousel item)  |
| --- imageLink         | String  | X        | Image link (within carousel item)   |
| --- commerce          | Object  | O        | Commerce (within carousel item)      |
| ---- title            | String  | O        | Product title                |
| ---- regularPrice     | Integer | X        | Regular price                |
| ---- discountPrice    | Integer | X        | Discount price                 |
| ---- discountRate     | Integer | X        | Discount rate                  |
| ---- discountFixed    | Integer | X        | Fixed discount price               |
| --- buttons           | Array   | O        | Carousel list button list        |
| ---- name             | String  | O        | Button title                |
| ---- type             | String  | O        | Button type                |
| ---- linkMo           | String  | X        | Mobile web link             |
| ---- linkPc           | String  | X        | PC web link              |
| ---- schemeIos        | String  | X        | iOS app link             |
| ---- schemeAndroid    | String  | X        | Android app link           |
| ---- bizFormId        | Integer | X        | Biz form ID (JSON standard)     |
| --- coupon            | Object  | X        | Coupon element (within carousel item)    |
| ---- title            | String  | X        | Coupon title                |
| ---- description      | String  | X        | Coupon detailed description             |
| ---- linkMo           | String  | X        | Mobile web link             |
| ---- linkPc           | String  | X        | PC web link              |
| ---- schemeIos        | String  | X        | iOS app link             |
| ---- schemeAndroid    | String  | X        | Android app link           |
| -- tail               | Object  | X        | More button information            |
| --- linkMo            | String  | O        | Mobile web link             |
| --- linkPc            | String  | X        | PC web link              |
| --- schemeIos         | String  | X        | iOS app link             |
| --- schemeAndroid     | String  | X        | Android app link           |
| - status              | String  | O        | Template status (A: registered, S: blocked) |
| - createDate          | String  | O        | Registration date                |
| - updateDate          | String  | X        | Modification date                |

<a id="register-template"></a>

### Template Registration

<a id="requested-9"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | Unique appkey |
| senderKey | String | Sender key  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                      |
|--------------|--------|----------|----------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="note"></a>

#### Notes

* When applying replacement variables to coupon titles, you must use the following fixed replacement variables:

```
- #{할인금액}KRW off coupon (#{할인금액} range is 1 to 99,999,999)
- #{할인율}% off coupon (#{할인율} range is 1 to 100)
- Shipping discount coupon
- #{상품명} Free coupon (#{상품명} is up to 7 characters)
- #{상품명} UP coupon (#{상품명} is up to 7 characters)
```

* In commerce, users cannot specify replacement variables for the regularPrice, discountPrice, discountRate, and discountFixed fields, except for the product title.
    * If you leave all fields empty except for the product title, fixed replacement variables are automatically filled and saved.
    * regularPrice -> #{정상가격}
    * discountPrice -> #{할인가격}
    * discountRate -> #{할인율}
    * discountFixed -> #{정액할인가격}

<a id="request-to-register-text-type-template"></a>

#### Request to Register Text Type Template

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "TEXT",
  "adult": boolean,
  "content": String,
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name            | Type    | Required | Description                                                                                                                                                                                                                                      |
|-----------------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                            |
| chatBubbleType  | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                     |
| adult           | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                              |
| content         | String  | O        | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For other types: this field is not used |
| buttons         | List    | X        | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                    |
| - name          | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables cannot be used                                                                                                   |
| - type          | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| - linkMo        | String  | X        | Mobile web link (required for WL type), limited to 500 characters                                                                                                                                                                               |
| - linkPc        | String  | X        | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                                   |
| - schemeAndroid | String  | X        | Android app link (required for AL type), limited to 500 characters                                                                                                                                                                              |
| - schemeIos     | String  | X        | iOS app link (required for AL type), limited to 500 characters                                                                                                                                                                                  |
| - bizFormKey    | String  | X        | Business form key for BF type button<br>Replacement variables cannot be used                                                                                                                                                                     |
| coupon          | Object  | X        | Coupon element                                                                                                                                                                                                                                   |
| - title         | String  | O        | For title, you are limited to five formats:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description   | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks not allowed<br>- For other types: up to 12 characters, line breaks not allowed                                             |
| - linkMo        | String  | X        | Mobile web link (required for WL type), limited to 500 characters<br>If you enter the linkMo field in the coupon, the other fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the other fields become optional. |
| - linkPc        | String  | X        | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                                   |
| - schemeAndroid | String  | X        | Android app link (required for AL type), limited to 500 characters<br>If you enter the linkMo field in the coupon, the other fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the other fields become optional. |
| - schemeIos     | String  | X        | iOS app link (required for AL type), limited to 500 characters<br>If you enter the linkMo field in the coupon, the other fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the other fields become optional. |

<a id="request-to-register-image-type-template"></a>

#### Request to Register Image Type Template

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "IMAGE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name            | Type    | Required | Description                                                                                                                                                                                                                                           |
|-----------------|---------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                                 |
| chatBubbleType  | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                         |
| adult           | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                                   |
| content         | String  | O        | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input available)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input available)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For other types: this field is not used |
| image           | Object  | O        | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                                    |
| - imageUrl      | String  | O        | Image URL, use the image URL uploaded as a regular image<br>Replacement variables cannot be used                                                                                                                                                     |
| - imageLink     | String  | X        | URL to move to when the image is clicked, limited to 500 characters<br>If not set, the KakaoTalk image viewer is used<br>Replacement variables cannot be used                                                                                      |
| buttons         | List    | X        | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                         |
| - name          | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables cannot be used                                                                                                       |
| - type          | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                                |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                    |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                               |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                                   |
| - bizFormKey    | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                                |
| coupon          | Object  | X        | Coupon element                                                                                                                                                                                                                                        |
| - title         | String  | O        | For title, you are limited to five formats:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description   | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks: not allowed<br>- For other types: up to 12 characters, line breaks: not allowed                                                 |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                    |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-wide-image-type-template"></a>

#### Wide image type template registration request

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE",
  "adult": boolean,
  "content": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name              | Type      | Required | Description                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | Template name (up to 200 characters)                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                               |
| adult           | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                                                                             |
| content         | String  | O  | - For TEXT type, up to 1,300 characters (line breaks: up to 99, URL format can be entered)<br>- For IMAGE type, up to 1,300 characters (line breaks: up to 99, URL format can be entered)<br>- For WIDE type, up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type, this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For other types, this field is not used |
| image           | Object  | O  | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                                                      |
| - imageUrl      | String  | O  | Image URL, use the image URL uploaded as a wide image<br>Replacement variables cannot be used                                                                                                                                                                                     |
| - imageLink     | String  | X  | URL to navigate to when the image is clicked, limited to 500 characters<br>If not set, KakaoTalk's internal image viewer is used<br>Replacement variables cannot be used                                                                                                                                                                                    |
| buttons         | List    | X  | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                                                                                       |
| - name          | String  | O  | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables cannot be used                                                                                                                                                                            |
| - type          | String  | O  | Button type (WL: Web link, AL: App link, BK: Bot keyword, MD: Message delivery, AC: Add channel, BT: Bot transfer, BF: Business form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types                   |
| - linkMo        | String  | X  | Mobile web link (required for WL type), limited to 500 characters                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android app link (required for AL type), limited to 500 characters                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | iOS app link (required for AL type), limited to 500 characters                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | Coupon element                                                                                                                                                                                                                                               |
| - title         | String  | O  | For title, you are limited to 5 types:<br>- "${number}KRW discount coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% discount coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters or less} Free coupon"<br>- "${7 characters or less} UP coupon"                                                                                  |
| - description   | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks: not allowed<br>- For other types: up to 12 characters, line breaks: not allowed                                                                                                                                                            |
| - linkMo        | String  | X  | Mobile web link (required for WL type), limited to 500 characters<br>If you enter the linkMo field in coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                         |
| - linkPc        | String  | X  | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android app link (required for AL type), limited to 500 characters<br>If you enter the linkMo field in coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                       |
| - schemeIos     | String  | X  | iOS app link (required for AL type), limited to 500 characters<br>If you enter the linkMo field in coupon, the remaining fields become optional,<br>and if you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                         |

<a id="request-to-register-wide-item-list-type-template"></a>

#### Wide Item List Type Template Registration Request

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "WIDE_ITEM_LIST",
  "adult": boolean,
  "header": String,
  "item": {
    "list": [
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      },
      {
        "title": String,
        "imageUrl": String,
        "linkMo": String,
        "linkPc": String,
        "schemeAndroid": String,
        "schemeIos": String
      }
    ]
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name             | Type    | Required | Description                                                                                                                                                                                                                                |
|------------------|---------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName     | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                     |
| chatBubbleType   | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                              |
| adult            | boolean | X        | Whether adult message (default: false)                                                                                                                                                                                                   |
| header           | String  | O        | Header<br>- Required field for WIDE_ITEM_LIST type, up to 20 characters (line breaks: not allowed)<br>- Optional field for PREMIUM_VIDEO type, up to 20 characters (line breaks: not allowed)                                       |
| item             | Object  | O        | Wide list element (can only be used for WIDE_ITEM_LIST type)                                                                                                                                                                             |
| - list           | List    | O        | Wide list (minimum: 3, maximum: 4)                                                                                                                                                                                                       |
| -- title         | String  | O        | Item title<br>- First item limited to up to 25 characters (line breaks: up to 1, title is not a required value for the first item)<br>- First item does not have title as a required field<br>- Items 2-4 limited to up to 30 characters (line breaks: up to 1) |
| -- imageUrl      | String  | O        | Item image URL<br>- Use image URL uploaded as first wide item list image for the first item<br>- Use image URL uploaded as general wide item list image for items 2-4<br>Replacement variables cannot be used                        |
| -- linkMo        | String  | O        | Mobile web link, 500 character limit                                                                                                                                                                                                     |
| -- linkPc        | String  | X        | PC web link, 500 character limit                                                                                                                                                                                                         |
| -- schemeAndroid | String  | X        | Android app link, 500 character limit                                                                                                                                                                                                    |
| -- schemeIos     | String  | X        | iOS app link, 500 character limit                                                                                                                                                                                                        |
| buttons          | List    | X        | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2          |
| - name           | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables cannot be used                                                                                          |
| - type           | String  | O        | Button type (WL: Web link, AL: App link, BK: Bot keyword, MD: Message delivery, AC: Add channel, BT: Chatbot transfer, BF: Business form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| - linkMo         | String  | X        | Mobile web link (required for the WL type), 500 character limit                                                                                                                                                                          |
| - linkPc         | String  | X        | PC web link (optional for the WL type), 500 character limit                                                                                                                                                                              |
| - schemeAndroid  | String  | X        | Android app link (required for the AL type), 500 character limit                                                                                                                                                                         |
| - schemeIos      | String  | X        | iOS app link (required for the AL type), 500 character limit                                                                                                                                                                             |
| - bizFormKey     | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                    |
| coupon           | Object  | X        | Coupon element                                                                                                                                                                                                                            |
| - title          | String  | O        | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description    | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks: not allowed<br>- For other types: up to 12 characters, line breaks: not allowed                                    |
| - linkMo         | String  | X        | Mobile web link (required for the WL type), 500 character limit<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc         | String  | X        | PC web link (optional for the WL type), 500 character limit                                                                                                                                                                              |
| - schemeAndroid  | String  | X        | Android app link (required for the AL type), 500 character limit<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos      | String  | X        | iOS app link (required for the AL type), 500 character limit<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-premium-video-type-template"></a>

#### Request to Register Premium Video Type Template

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "PREMIUM_VIDEO",
  "adult": boolean,
  "content": String,
  "header": String,
  "video": {
    "videoUrl": String,
    "thumbnailUrl": String
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name            | Type    | Required | Description                                                                                                                                                                                                                                                  |
|-----------------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                                        |
| chatBubbleType  | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                |
| adult           | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                                          |
| content         | String  | X        | - For TEXT type, up to 1,300 characters (line breaks: up to 99, URL format can be entered)<br>- For IMAGE type, up to 1,300 characters (line breaks: up to 99, URL format can be entered)<br>- For WIDE type, up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type, this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For other types, this field is not used |
| header          | String  | X        | Header<br>- For WIDE_ITEM_LIST type, required field with up to 20 characters (line breaks: not allowed)<br>- For PREMIUM_VIDEO type, optional field with up to 20 characters (line breaks: not allowed)                                                  |
| video           | Object  | O        | Video element (only available for PREMIUM_VIDEO type)                                                                                                                                                                                                       |
| - videoUrl      | String  | O        | Kakao TV video URL (only URLs of videos uploaded to Kakao TV can be used), up to 500 characters<br>Replacement variables not allowed                                                                                                                       |
| - thumbnailUrl  | String  | X        | Image URL for video thumbnail. Only URLs uploaded as general images can be used (if not provided, default Kakao TV video thumbnail is used), up to 500 characters<br>Replacement variables not allowed                                                   |
| buttons         | List    | X        | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2                             |
| - name          | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables not allowed                                                                                                                 |
| - type          | String  | O        | Button type (WL: Web link, AL: App link, BK: Bot keyword, MD: Message delivery, AC: Add channel, BT: Bot transfer, BF: Business form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), up to 500 characters                                                                                                                                                                                           |
| - linkPc        | String  | X        | PC web link (optional for the WL type), up to 500 characters                                                                                                                                                                                               |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), up to 1,000 characters                                                                                                                                                                                        |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), up to 500 characters                                                                                                                                                                                              |
| - bizFormKey    | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                                       |
| coupon          | Object  | X        | Coupon element                                                                                                                                                                                                                                              |
| - title         | String  | O        | For title, you are limited to five formats:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description   | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks not allowed<br>- For other types: up to 12 characters, line breaks not allowed                                                          |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), up to 500 characters<br>When the linkMo field is entered in the coupon, the other fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the other fields become optional. |
| - linkPc        | String  | X        | PC web link (optional for the WL type), up to 500 characters                                                                                                                                                                                               |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), up to 500 characters<br>When the linkMo field is entered in the coupon, the other fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the other fields become optional. |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), up to 500 characters<br>When the linkMo field is entered in the coupon, the other fields become optional,<br>and when a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the other fields become optional. |

<a id="request-to-register-commerce-type-template"></a>

#### Commerce Type Template Registration Request

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "COMMERCE",
  "pushAlarm": boolean,
  "adult": boolean,
  "additionalContent": String,
  "image": {
    "imageUrl": String,
    "imageLink": String
  },
  "commerce": {
    "title": String,
    "regularPrice": Integer,
    "discountPrice": Integer,
    "discountRate": Integer,
    "discountFixed": Integer
  },
  "buttons": [
    {
      "name": String,
      "type": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String,
      "bizFormKey": String
    }
  ],
  "coupon": {
    "title": String,
    "description": String,
    "linkMo": String,
    "linkPc": String,
    "schemeAndroid": String,
    "schemeIos": String
  }
}
```

| Name              | Type    | Required | Description                                                                                                                                                                                                                                |
|-------------------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                      |
| chatBubbleType    | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                               |
| adult             | boolean | X        | Whether it is an adult message (default: false)                                                                                                                                                                                           |
| additionalContent | String  | X        | Additional information (up to 34 characters, line breaks: up to 1), can only be used in Commerce type                                                                                                                                     |
| image             | Object  | O        | Image element<br>- Required field for IMAGE, WIDE, COMMERCE types                                                                                                                                                                          |
| - imageUrl        | String  | O        | Image URL, use image URL uploaded as a regular image<br>Replacement variables cannot be used                                                                                                                                               |
| - imageLink       | String  | X        | URL to navigate to when image is clicked, limited to 500 characters<br>If not set, KakaoTalk's internal image viewer is used<br>Replacement variables cannot be used                                                                     |
| commerce          | Object  | O        | Commerce (can only be used in COMMERCE type)                                                                                                                                                                                              |
| title             | String  | O        | Product title (up to 30 characters, line breaks: not allowed)                                                                                                                                                                             |
| regularPrice      | Integer | O        | Regular price (0~99,999,999)<br>User-defined replacement variables cannot be used, if value is empty, it is saved as fixed replacement variable `#{정상가격}`                                                                              |
| discountPrice     | Integer | X        | Discount price (0~99,999,999)<br>User-defined replacement variables cannot be used, if value is empty, it is saved as fixed replacement variable `#{할인가격}`                                                                            |
| discountRate      | Integer | X        | Discount rate (0~100), when discount price exists, one of discount rate or fixed discount amount is required<br>User-defined replacement variables cannot be used, if value is empty, it is saved as fixed replacement variable `#{할인율}` |
| discountFixed     | Integer | X        | Fixed discount amount (0~999,999), when discount price exists, one of discount rate or fixed discount amount is required<br>User-defined replacement variables cannot be used, if value is empty, it is saved as fixed replacement variable `#{정액할인가격}` |
| buttons           | List    | O        | Button list<br>- For TEXT, IMAGE types: up to 4 when coupon is applied, otherwise up to 5<br>- For WIDE, WIDE_ITEM_LIST types: up to 2<br>- For PREMIUM_VIDEO type: up to 1<br>- For COMMERCE type: minimum 1, maximum 2            |
| - name            | String  | O        | Button title<br>- For TEXT, IMAGE types: up to 14 characters<br>- For other types: up to 8 characters<br>Replacement variables cannot be used                                                                                            |
| - type            | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- AC type must be registered as the first button for TEXT, IMAGE, and as the last button for other message types |
| - linkMo          | String  | X        | Mobile web link (required for WL type), limited to 500 characters                                                                                                                                                                         |
| - linkPc          | String  | X        | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                             |
| - schemeAndroid   | String  | X        | Android app link (required for AL type), limited to 500 characters                                                                                                                                                                        |
| - schemeIos       | String  | X        | iOS app link (required for AL type), limited to 500 characters                                                                                                                                                                            |
| - bizFormKey      | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                     |
| coupon            | Object  | X        | Coupon element                                                                                                                                                                                                                             |
| - title           | String  | O        | For title, you are limited to five formats:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description     | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: up to 18 characters, line breaks: not allowed<br>- For other types: up to 12 characters, line breaks: not allowed                                     |
| - linkMo          | String  | X        | Mobile web link (required for WL type), limited to 500 characters<br>If the linkMo field is entered in the coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc          | String  | X        | PC web link (optional for WL type), limited to 500 characters                                                                                                                                                                             |
| - schemeAndroid   | String  | X        | Android app link (required for AL type), limited to 500 characters<br>If the linkMo field is entered in the coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos       | String  | X        | iOS app link (required for AL type), limited to 500 characters<br>If the linkMo field is entered in the coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-carousel-feed-type-template"></a>

#### Register Carousel Feed Template Request

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_FEED",
  "adult": boolean,
  "carousel": {
    "list": [
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "chatExtra": String,
            "chatEvent": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      },
      {
        "header": String,
        "message": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        }
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| Name              | Type    | Required | Description                                                                                                                                                                                                                                |
|-------------------|---------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                      |
| chatBubbleType    | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                              |
| pushAlarm         | boolean | X        | Whether to send message push alarm (default: true)                                                                                                                                                                                        |
| adult             | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                        |
| carousel          | Object  | O        | Carousel                                                                                                                                                                                                                                   |
| - list            | List    | O        | Carousel list (minimum 2, maximum 6)                                                                                                                                                                                                      |
| -- header         | String  | O        | Carousel item title (up to 20 characters), only available in carousel feeds                                                                                                                                                               |
| -- message        | String  | O        | Carousel item title (up to 20 characters), Carousel item message (up to 180 characters), only available in carousel feeds                                                                                                                |
| -- imageUrl       | String  | O        | Image URL (only images uploaded as carousel feed images can be used)<br>Replacement variables cannot be used                                                                                                                              |
| -- imageLink      | String  | X        | Image link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                             |
| -- buttons        | List    | O        | Carousel list button list minimum 1, maximum 2                                                                                                                                                                                            |
| --- name          | String  | O        | Button title<br>- For TEXT, IMAGE types, up to 14 characters<br>- For other types, up to 8 characters<br>Replacement variables cannot be used                                                                                           |
| --- type          | String  | O        | Button type (WL: Web link, AL: App link, BK: Bot keyword, MD: Message delivery, AC: Add channel, BT: Chatbot transfer, BF: Business form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| --- linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                     |
| --- linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                         |
| --- schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                    |
| --- schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                        |
| --- bizFormKey    | String  | X        | Business form key for BF type buttons                                                                                                                                                                                                     |
| -- coupon         | Object  | X        | Coupon element                                                                                                                                                                                                                             |
| --- title         | String  | O        | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| --- description   | String  | O        | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types, up to 18 characters, line breaks: not allowed<br>- For other types, up to 12 characters, line breaks: not allowed                                     |
| --- linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| --- linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                         |
| --- schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| --- schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional,<br>and if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - tail            | Object  | X        | More button information                                                                                                                                                                                                                    |
| -- linkMo         | String  | O        | Mobile web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                        |
| -- linkPc         | String  | X        | PC web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                            |
| -- schemeAndroid  | String  | X        | Android app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                       |
| -- schemeIos      | String  | X        | iOS app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                           |
| recipientList     | List    | O        | Recipient list (up to 1,000)                                                                                                                                                                                                              |
| - recipientNo     | String  | O        | Recipient number                                                                                                                                                                                                                           |
| createUser        | String  | X        | Creator (saved as user UUID when sent from console)                                                                                                                                                                                       |

<a id="request-to-register-carousel-commerce-type-template"></a>

#### Register Carousel Commerce Template Request

[Request body]

```
{
  "templateName": String,
  "chatBubbleType": "CAROUSEL_COMMERCE",
  "adult": boolean,
  "carousel": {
    "head": {
      "header": String,
      "content": String,
      "imageUrl": String,
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    },
    "list": [
      {
        "additionalContent": String,
        "imageUrl": String,
        "imageLink": String,
        "buttons": [
          {
            "name": String,
            "type": String,
            "linkMo": String,
            "linkPc": String,
            "schemeAndroid": String,
            "schemeIos": String,
            "bizFormKey": String
          }
        ],
        "coupon": {
          "title": String,
          "description": String,
          "linkMo": String,
          "linkPc": String,
          "schemeAndroid": String,
          "schemeIos": String
        },
        "commerce": {
          "title": String,
          "regularPrice": Integer,
          "discountPrice": Integer,
          "discountRate": Integer,
          "discountFixed": Integer
        },
      }
    ],
    "tail": {
      "linkMo": String,
      "linkPc": String,
      "schemeAndroid": String,
      "schemeIos": String
    }
  }
}
```

| Name                   | Type      | Required | Description                                                                                                                                                                                                                                |
|----------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName         | String  | O  | Template name (maximum 200 characters)                                                                                                                                                                                                                   |
| chatBubbleType       | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                             |
| pushAlarm            | boolean | X  | Whether to send message push alarms (default: true)                                                                                                                                       |
| adult                | boolean | X  | Whether it is an adult message (default: false)                                                                                                                                                                                                           |
| carousel             | Object  | O  | Carousel                                                                                                                                                                                                                               |
| - head               | Object  | X  | Carousel intro                                                                                                                                                                                                                           |
| -- header            | String  | O  | Carousel intro header (maximum 20 characters)                                                                                                                                                                                                                |
| -- content           | String  | O  | Carousel intro content (maximum 50 characters)                                                                                                                                                                                                                |
| -- imageUrl          | String  | O  | Carousel intro image URL (use images uploaded as carousel commerce type images, the image used must have the same ratio as the carousel image)<br>Replacement variables cannot be used                                                                                                                                          |
| -- linkMo            | String  | X  | Mobile web link (linkMo is required if you want to use any of linkMo, linkPc, schemeAndroid, schemeIos), limited to 500 characters                                                                                                                                        |
| -- linkPc            | String  | X  | PC web link, limited to 500 characters                                                                                                                                                                                               |
| -- schemeAndroid     | String  | X  | Android app link, limited to 500 characters                                                                                                                                                                                             |
| -- schemeIos         | String  | X  | iOS app link, limited to 500 characters                                                                                                                                                                                               |
| - list               | List    | O  | Carousel list (minimum 1, maximum 5 if head exists / minimum 2, maximum 6 otherwise)                                                                                                                                                                          |
| -- additionalContent | String  | X  | Additional information (maximum 34 characters), can only be used in carousel commerce type                                                                                                                                                                                                  |
| -- imageUrl          | String  | O  | Image URL (use images uploaded as carousel commerce type images)<br>Replacement variables cannot be used                                                                                                                                                                                 |
| -- imageLink         | String  | X  | Image link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                    |
| -- commerce          | Object  | O  | Commerce (can only be used in CAROUSEL_COMMERCE type)                                                                                                                                                                                               |
| --- title            | String  | O  | Product title (maximum 30 characters, line breaks: not allowed)                                                                                                                                                                                                           |
| --- regularPrice     | Integer | O  | Regular price (0~99,999,999)<br>Replacement variables cannot be customized, if value is empty, it is saved as fixed replacement variable `#{정상가격}`                                                                                                                                                          |
| --- discountPrice    | Integer | X  | Discount price (0~99,999,999)<br>Replacement variables cannot be customized, if value is empty, it is saved as fixed replacement variable `#{할인가격}`                                                                                                                                                            |
| --- discountRate     | Integer | X  | Discount rate (0~100), when discount price exists, either discount rate or fixed discount price is required<br>Replacement variables cannot be customized, if value is empty, it is saved as fixed replacement variable `#{할인율}`                                                                                                                                                      |
| --- discountFixed    | Integer | X  | Fixed discount price (0~999,999), when discount price exists, either discount rate or fixed discount price is required<br>Replacement variables cannot be customized, if value is empty, it is saved as fixed replacement variable `#{정액할인가격}`                                                                                                                                            |
| -- buttons           | List    | O  | Carousel list button list minimum 1, maximum 2                                                                                                                                                                                        |
| --- name             | String  | O  | Button title<br>- For TEXT, IMAGE types: maximum 14 characters<br>- For other types: maximum 8 characters<br>Replacement variables cannot be used                                                                                                                                                          |
| --- type             | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- AC type must be registered as the first button for TEXT, IMAGE types, and as the last button for other message types |
| --- linkMo           | String  | X  | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                             |
| --- linkPc           | String  | X  | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                           |
| --- schemeIos        | String  | X  | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                             |
| --- bizFormKey       | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                |
| -- coupon            | Object  | X  | Coupon element                                                                                                                                                                                                                             |
| --- title            | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                |
| --- description      | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO types: maximum 18 characters, line breaks: not allowed<br>- For other types: maximum 12 characters, line breaks: not allowed                                                                                                                                          |
| --- linkMo           | String  | X  | Mobile web link (required for the WL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                       |
| --- linkPc           | String  | X  | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | Android app link (required for the AL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                     |
| --- schemeIos        | String  | X  | iOS app link (required for the AL type), limited to 500 characters<br>When entering the linkMo field for coupons, the remaining fields become optional,<br>and when entering a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                       |
| - tail               | Object  | X  | More button information                                                                                                                                                                                                                         |
| -- linkMo            | String  | O  | Mobile web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                 |
| -- linkPc            | String  | X  | PC web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                  |
| -- schemeAndroid     | String  | X  | Android app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                               |
| -- schemeIos         | String  | X  | iOS app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                 |

<a id="response-10"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "template": {
    "templateCode": String
  }
}
```

| Name            | Type    | Not Null | Description    |
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |
| template        | Object  | X        | Template information |
| - templateCode  | String  | O        | Template code  |

<a id="modify-template"></a>

### Modify template

<a id="requested-10"></a>

#### Request

[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description    |
|--------------|--------|----------------|
| appkey       | String | Unique app key |
| senderKey    | String | Sender key     |
| templateCode | String | Template code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                         |
|--------------|--------|----------|-------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.      |

[Request Body]

* Same specifications as template registration

<a id="response-11"></a>

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
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |

<a id="delete-template"></a>

### Delete template

<a id="requested-11"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description    |
|--------------|--------|----------------|
| appkey       | String | Unique app key |
| senderKey    | String | Sender key     |
| templateCode | String | Template code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----|-------------------------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

<a id="response-12"></a>

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |

<a id="manage-image"></a>

## Image Management

<a id="upload-image"></a>

### Upload Image

<a id="requested-12"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Request parameter]

| Name      | Type   | Required | Description                                                                                                                      |
|-----------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------|
| image     | File   | O        | Image                                                                                                                            |
| imageType | String | O        | Image type <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |

<a id="response-13"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }, 
  "image": {
      "imageSeq": Integer,
      "imageUrl": String,
      "imageName": String,
  }
}
```

| Name            | Type    | Not Null | Description      |
|:----------------|:--------|:---------|:-----------------|
| header          | Object  | O        | Header area      |
| - resultCode    | Integer | O        | Result code      |
| - resultMessage | String  | O        | Result message   |
| - isSuccessful  | boolean | O        | Success          |
| image           | Object  | X        | Image area       |
| - imageSeq      | Integer | O        | Image sequence   |
| - imageUrl      | String  | O        | Image URL        |
| - imageName     | String  | X        | Image name       |

<a id="upload-image-specifications"></a>

#### Upload Image Specifications
| Image Type                 | Usage                                                   | Upload Image Specifications                                                                                                             |
|:---------------------------|:--------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------|
| IMAGE                      | Image type image, commerce type image, premium video type thumbnail | Recommended size: 800 X 400px (width 500px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and capacity limit: jpg, png / maximum 5MB |
| WIDE_IMAGE                 | Wide image type image                                   | Recommended size: 800 X 600px (width 500px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1<br>File format and capacity limit: jpg, png / maximum 5MB |
| MAIN_WIDE_ITEMLIST_IMAGE   | Wide item list type first item image                   | Size limit: width 500px or more<br/>Image ratio: height ÷ width = 0.5<br/>File format and capacity limit: jpg, png / maximum 5MB |
| NORMAL_WIDE_ITEMLIST_IMAGE | Wide item list type 2nd-4th item image                 | Size limit: width 500px or more<br/>Image ratio: height ÷ width = 1<br/>File format and size: jpg, png / maximum 5MB per file |
| CAROUSEL_FEED_IMAGE        | Carousel feed type cell image                          | Recommended size: 800 X 600px or 800 X 400px (width 500px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and capacity limit: jpg, png / maximum 5MB |
| CAROUSEL_COMMERCE_IMAGE    | Carousel commerce type intro image, carousel commerce type cell image | Recommended size: 800 X 600px or 800 X 400px (width 500px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and capacity limit: jpg, png / maximum 5MB |

* When updating a template, if the image is changed to a different one, the existing image is deleted from the Kakao CDN and the URL becomes invalid. Other templates using the same image are also affected, so caution is required. Although image information is retained in the image retrieval API, the actual image cannot be accessed, so it is recommended to keep the original file separately on your own server.

<a id="view-image"></a>

### View Image

<a id="requested-13"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Request parameter]

| Name       | Type   | Required | Description                                                                                                                      |
|------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------|
| imageTypes | List   | O        | Image type <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |
| pageNum    | String | X        | Page number (default: 1)                                                                                                        |
| pageSize   | String | X        | Number of queries (default: 15)                                                                                                 |

<a id="response-14"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "image": {
    "imageSeq": Integer,
    "imageUrl": String,
    "imageName": String
  }
}
```

| Name            | Type    | Not Null | Description      |
|:----------------|:--------|:---------|:-----------------|
| header          | Object  | O        | Header area      |
| - resultCode    | Integer | O        | Result code      |
| - resultMessage | String  | O        | Result message   |
| - isSuccessful  | boolean | O        | Success          |
| image           | Object  | X        | Image area       |
| - imageSeq      | Integer | O        | Image sequence   |
| - imageUrl      | String  | O        | Image URL        |
| - imageName     | String  | X        | Image name       |

<a id="delete-image"></a>

### Delete Image

<a id="requested-14"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                    |
|--------------|--------|----------|--------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter]

| Name     | Type   | Required | Description  |
|----------|--------|----------|--------------|
| imageSeq | String | O        | Image number |

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
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |

<a id="manage-video"></a>

## Video Management

This API registers, retrieves, and deletes videos to be used in Brand Message. Registered videos can be used for delivery after encoding processing in the Kakao Biz Center, and only videos with `PUBLIC` status can be used for template registration and delivery (`PRIVATE` can only be used for template registration).

<a id="video-upload-flow"></a>

### Video Upload Flow

Video uploading consists of two steps:

1. **Register Video Upload** — Send video metadata (filename and file size) as JSON to this API (`POST /brand-message/v1.0/appkeys/{appKey}/videos`). You receive `video` (registration information on the NHN Cloud side) and `uploadInfo` (Kakao upload URL and token) in the response.
2. **Upload Video File** — Directly upload the video file to `uploadInfo.uploadUrl` received in the response using `multipart/form-data`. For authentication, pass `uploadInfo.token` as the `x-kamp-upload-token` header.

The video file is sent directly to Kakao's upload server without going through NHN Cloud servers. Therefore, the request body of this API contains only metadata as JSON, and the actual file is sent separately in step 2.

> **Caution**
> * `uploadInfo.token` is valid for 5 minutes after issuance. After 5 minutes, you must call step 1 registration again to receive a new token.
> * The `fileSize` in the step 1 request must exactly match the size of the file to be uploaded in step 2 (if they don't match, Kakao will reject it with errCode 109).

<a id="register-video-upload"></a>

### Register Video Upload

<a id="requested-15"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique App Key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | Type   | Required | Description                     |
|----------------|--------|----------|---------------------------------|
| X-Secret-Key   | String | O        | Can be created from the console |

[Request body]

```
{
  "senderKey": String,
  "fileName": String,
  "fileSize": Long,
  "createUser": String
}
```

| Name       | Type   | Required | Description                                                                      |
|------------|--------|----------|----------------------------------------------------------------------------------|
| senderKey  | String | O        | Sender Key (40 characters)                                                       |
| fileName   | String | O        | Video file name (including extension, one of MP4·MOV·AVI, up to 250 characters) |
| fileSize   | Long   | O        | Video file size (bytes, up to 4 GB = 4,294,967,296)                             |
| createUser | String | X        | Upload user identifier (up to 100 characters)                                   |

<a id="response-16"></a>

#### Response

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  },
  "video": {
      "videoSeq": Long,
      "vid": String,
      "senderKey": String,
      "title": String,
      "fileName": String,
      "fileSize": Long,
      "status": String
  },
  "uploadInfo": {
      "uploadUrl": String,
      "token": String
  }
}
```

| Name            | Type    | Not Null | Description                                                                                                        |
|:----------------|:--------|:---------|:-------------------------------------------------------------------------------------------------------------------|
| header          | Object  | O        | Header area                                                                                                        |
| - resultCode    | Integer | O        | Result code                                                                                                        |
| - resultMessage | String  | O        | Result message                                                                                                     |
| - isSuccessful  | boolean | O        | Success                                                                                                            |
| video           | Object  | X        | NHN Cloud registered video information (status = `REGISTERED`)                                                     |
| - videoSeq      | Long    | O        | Video sequence                                                                                                     |
| - vid           | String  | O        | Kakao video ID                                                                                                     |
| - senderKey     | String  | O        | Sender Key                                                                                                         |
| - title         | String  | X        | Video title (file name immediately after upload, synchronized with the value modified in Kakao Biz Center after encoding completion) |
| - fileName      | String  | O        | Upload file name                                                                                                   |
| - fileSize      | Long    | O        | File size (bytes)                                                                                                  |
| - status        | String  | O        | Video status (see [Video Status](#동영상-상태)). Always `REGISTERED` in upload registration response                     |
| uploadInfo      | Object  | O        | Kakao upload information. Used in step 2                                                                          |
| - uploadUrl     | String  | O        | Kakao endpoint for direct video file upload                                                                        |
| - token         | String  | O        | Upload authentication token. Pass as `x-kamp-upload-token` header                                                 |

> The `thumbnailUrl`, `videoUrl`, `playUrl`, and `updateDate` fields filled after encoding completion can be obtained through the [Query Video](#동영상-조회) API.

<a id="upload-video-file"></a>

### Upload video file (Step 2)

Call the video file directly to `uploadInfo.uploadUrl` from the response above. This request is sent directly to Kakao's upload server, not to the NHN Cloud server.

<a id="requested-16"></a>

#### Request

[URL]

```
POST  {uploadInfo.uploadUrl}
Content-Type: multipart/form-data
```

[Header]

| Name                | Type   | Required | Description                                    |
|---------------------|--------|----------|------------------------------------------------|
| x-kamp-upload-token | String | O        | Pass the `uploadInfo.token` value from the Step 1 response as is |

[Request body (multipart)]

| Name | Type | Required | Description                                                  |
|------|------|----------|--------------------------------------------------------------|
| file | File | O        | Video file. Must match exactly with `fileSize` from the Step 1 request |

<a id="response-17"></a>

#### Response

```
{
  "vid": String,
  "playUrl": String,
  "errCode": Integer,
  "message": String
}
```

* On success, returns `vid` (same as Step 1 response) and `playUrl`, and `errCode`/`message` fields are not included.
* On failure, returns HTTP 4xx with `errCode` (100~110) and `message`. For detailed error codes, refer to the KakaoTalk Biz Message guide.

<a id="upload-video-specifications"></a>

#### Upload video specifications

| Item               | Limit                          |
|:-------------------|:-------------------------------|
| File format        | MP4, MOV, AVI                  |
| Maximum file size  | 4 GB                           |
| Maximum video length | 4 hours                      |
| Maximum resolution | 8K                             |
| Filename length    | Within 250 characters          |

* Uploaded videos can be used after encoding is completed in Kakao Biz Center. Encoding time varies depending on video length and usually takes 5 to 10 minutes.
* The video status immediately after upload starts as `REGISTERED`, goes through `ENCODING`, and then transitions to `PUBLIC` or `PRIVATE`. You can check the status in the console or through the [Query video](#view-video) API.
* Registered videos are permanently stored by Kakao, and videos in Kakao Biz Center are not automatically cleaned up even when templates are deleted. Kakao Channel administrators can delete them directly from the management screen of the channel business home.
* If Step 2 file upload fails or is delayed after Step 1 registration and the token (5 minutes) expires, you must call a new registration again. Videos that are only registered but not actually uploaded are automatically marked as `ERROR` status after a certain period of time.

<a id="view-video"></a>

### List videos

<a id="requested-17"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique App Key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.     |

[Request parameter]

| Name      | Type   | Required | Description                     |
|-----------|--------|----------|---------------------------------|
| senderKey | String | X        | Sender profile key (40 characters) |
| pageNum   | String | X        | Page number (default: 1)        |
| pageSize  | String | X        | Query count (default: 15)       |

<a id="response-18"></a>

#### Response

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  },
  "videosResponse": {
    "videos": [
      {
        "videoSeq": Long,
        "vid": String,
        "senderKey": String,
        "title": String,
        "fileName": String,
        "fileSize": Long,
        "status": String,
        "thumbnailUrl": String,
        "videoUrl": String,
        "playUrl": String,
        "createDate": String,
        "updateDate": String,
        "createUser": String
      }
    ],
    "totalCount": Integer
  }
}
```

| Name              | Type    | Not Null | Description        |
|:------------------|:--------|:---------|:-------------------|
| header            | Object  | O        | Header area        |
| - resultCode      | Integer | O        | Result code        |
| - resultMessage   | String  | O        | Result message     |
| - isSuccessful    | boolean | O        | Success            |
| videosResponse    | Object  | X        | Video list area    |
| - videos          | Array   | O        | Video array        |
| - totalCount      | Integer | O        | Total video count  |

> Each `videos` item field is the same as [Video upload response](#응답).

<a id="delete-video"></a>

### Delete video

<a id="requested-18"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                           |
|--------------|--------|----------|---------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter]

| Name     | Type   | Required | Description                                                  |
|----------|--------|----------|--------------------------------------------------------------|
| videoSeq | String | O        | Video sequence (multiple values can be passed separated by commas) |

<a id="response-19"></a>

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
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |

<a id="video-status"></a>

### Video status

Describes the `status` field values in video inquiry responses.

| status     | Description                                  |
|:-----------|:--------------------------------------------|
| REGISTERED | Upload registered                            |
| ENCODING   | Encoding in progress                         |
| PUBLIC     | Public status (sending and template registration available) |
| PRIVATE    | Private status (template registration available) |
| VIOLATED   | Violated video                               |
| ILLEGAL    | Illegal filming video                        |
| DELETED    | Deleted video                                |
| ERROR      | Error occurred during upload and encoding    |

<a id="upload"></a>

## Upload

<a id="upload-bizform-key"></a>

### Upload Biz Form Key

<a id="requested-19"></a>

#### Request

[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | Unique app key |
| senderKey | String | Sender key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="response-20"></a>

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

| Name            | Type    | Not Null | Description |
|:----------------|:--------|:---------|:------------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |

<a id="manage-outgoing-profiles"></a>

## Sender Profile Management

<a id="view-outgoing-profile"></a>

### Query sender profile

<a id="requested-20"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | App Key     |
| senderKey | String | Sender key  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.     |

<a id="response-21"></a>

#### Response

```
{
  "header":{
    "resultCode" :  Integer,
    "resultMessage" :  String,
    "isSuccessful" :  boolean
  },
  "sender":{
    "plusFriendId" : String,
    "senderKey" : String,
    "categoryCode" : String,
    "unsubscribePhoneNumber": String,
    "unsubscribeAuthNumber": String,
    "status" : String,
    "statusName" : String,
    "kakaoStatus" : String,
    "kakaoStatusName" : String,
    "kakaoProfileStatus" : String,
    "kakaoProfileStatusName" : String,
    "profileSpamLevel" : String,
    "profileMessageSpamLevel" : String,
    "brandMessage" : {
      "resendAppKey": String,
      "isResend": boolean,
      "resendSendNo": String,
      "resendUnsubscribeNo": String
    },
    "dormant" : boolean,
    "marketingAgreement" : boolean,
    "block" : boolean,
    "createDate" : String,
    "initialUserRestriction" : boolean
  }
}
```

| Name                         | Type    | Not Null | Description                                                                                                                                         |
|:-----------------------------|:--------|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------|
| header                       | Object  | O        | Header area                                                                                                                                         |
| - resultCode                 | Integer | O        | Result code                                                                                                                                         |
| - resultMessage              | String  | O        | Result message                                                                                                                                      |
| - isSuccessful               | boolean | O        | Success                                                                                                                                             |
| sender                       | Object  | X        | Sender profile                                                                                                                                      |
| - plusFriendId               | String  | O        | PlusFriend ID                                                                                                                                       |
| - senderKey                  | String  | O        | Sender key                                                                                                                                          |
| - categoryCode               | String  | X        | Category code                                                                                                                                       |
| - unsubscribePhoneNumber     | String  | X        | Toll-free opt-out phone number                                                                                                                      |
| - unsubscribeAuthNumber      | String  | X        | Toll-free opt-out authentication number                                                                                                             |
| - status                     | String  | X        | NHN Cloud PlusFriend status code <br>(YSC02: Waiting for registration, YSC03: Normal registration)                                                 |
| - statusName                 | String  | X        | NHN Cloud PlusFriend status name (Waiting for registration, Normal registration)                                                                   |
| - kakaoStatus                | String  | X        | Kakao PlusFriend status code<br>(A: Normal, S: Blocked)<br>If status is YSC02, kakaoStatus has a null value.                                      |
| - kakaoStatusName            | String  | X        | Kakao PlusFriend status name (Normal, Blocked)<br>If status is YSC02, kakaoStatusName has a null value.                                           |
| - kakaoProfileStatus         | String  | X        | Kakao PlusFriend profile status code<br>(A: Activated, B: Blocked, C: Deactivated, D: Deleted, E: Being deleted)<br>If status is YSC02, kakaoProfileStatus has a null value. |
| - kakaoProfileStatusName     | String  | X        | Kakao PlusFriend profile status name (Activated, Deactivated, Blocked, Being deleted, Deleted)<br>If status is YSC02, kakaoProfileStatusName has a null value. |
| - profileSpamLevel           | String  | X        | KakaoTalk Channel spam status name (Permanently restricted, Warning restricted, Normal)<br>Can have a null value if the sender profile status is not normal. |
| - profileMessageSpamLevel    | String  | X        | KakaoTalk message spam status name (Activity restricted, Warning restricted, Normal)<br>Can have a null value if the sender profile status is not normal. |
| - block                      | boolean | O        | Sender profile block status                                                                                                                         |
| - brandMessage               | Object  | X        | Brand message settings                                                                                                                              |
| -- resendAppKey              | String  | X        | SMS service appkey to set for fallback                                                                                                             |
| -- isResend                  | boolean | O        | Fallback setting (resend) status                                                                                                                   |
| -- resendSendNo              | String  | X        | tc-sms sender number for resend                                                                                                                     |
| -- resendUnsubscribeNo       | String  | X        | tc-sms 080 opt-out number for resend                                                                                                               |
| - dormant                    | boolean | O        | Sender profile dormant status                                                                                                                       |
| - marketingAgreement         | boolean | O        | M/N type usage application status                                                                                                                   |
| - createDate                 | String  | X        | Registration date                                                                                                                                   |
| - initialUserRestriction     | boolean | O        | Initial user restriction status                                                                                                                     |

<a id="modify-outgoing-profile-080-opt-out-number"></a>

### Modify sender profile 080 opt-out number

<a id="requested-21"></a>

#### Request

[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description |
|-----------|--------|-------------|
| appkey    | String | App Key     |
| senderKey | String | Sender key  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.     |

[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| Name              | Type   | Required | Description                                                                                                                                                     |
|-------------------|--------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| unsubscribeNo     | String | O        | 080 toll-free opt-out phone number (if all fields are not entered, it will be sent with the toll-free opt-out information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X        | 080 toll-free opt-out authentication number (if all fields are not entered, it will be sent with the toll-free opt-out information registered in the sender profile)<br>Cannot enter only unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |

<a id="response-22"></a>

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |

<a id="manage-fallback"></a>

## Fallback Management

<a id="register-sms-appkey"></a>

### Register SMS AppKey

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type    | Description   |
|--------|---------|---------------|
| appkey | String  | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type    | Required | Description                    |
|--------------|---------|----------|--------------------------------|
| X-Secret-Key | String  | O        | Can be created in the console. |

[Request body]

```
{
    "resendAppKey": String
}
```

| Name         | Type   | Required | Description                      |
|--------------|--------|----------|----------------------------------|
| resendAppKey | String | O        | SMS service appkey to set for fallback |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

<a id="response-23"></a>

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

<a id="register-fallback-settings"></a>

### Register Fallback Settings

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type    | Description   |
|--------|---------|---------------|
| appkey | String  | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type    | Required | Description                    |
|--------------|---------|----------|--------------------------------|
| X-Secret-Key | String  | O        | Can be created in the console. |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| Name                | Type     | Required | Description                                                                                                                |
|---------------------|----------|----------|----------------------------------------------------------------------------------------------------------------------------|
| senderKey           | String   | O        | Sender key                                                                                                                 |
| isResend            | Boolean  | O        | Whether to send SMS fallback when delivery fails<br>Fallback is sent by default when fallback is configured in the console. |
| resendSendNo        | String   | X        | Fallback sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span> |
| resendUnsubscribeNo | String   | X        | Fallback 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, fallback may fail.)</span> |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |