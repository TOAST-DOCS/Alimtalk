## Notification > KakaoTalk Bizmessage > Brand Message > API v1.0 Guide

<a id="brand-message"></a>

## Brand Message

<a id="api-domain"></a>

#### [API Domain]

| Domain |
|------------------------------------------------------------------------------|
| [https://kakaotalk-bizmessage.api.nhncloudservice.com](https://kakaotalk-bizmessage.api.nhncloudservice.com) |

<a id="introduce-v10-api"></a>

## Introduction to v1.0 API

<a id="manage-non-friend-message-sending-targeting-m-n"></a>

## Manage Non-Friend Message Sending (Targeting M, N)

Non-friend message sending (Targeting M, N) can be used when all of the following conditions are met:

- Business authentication channel
- Business registration number registered
- Channel customer center phone number registered
- At least 50,000 channel friends
- History of successful AlimTalk deliveries within the last 3 months

<a id="upload-marketing-consent-records"></a>

### Upload Marketing Consent Records

<a id="requested"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

[Path parameter]

| Name      | Type   | Description        |
|-----------|--------|--------------------|
| appkey    | String | Unique app key     |
| senderKey | String | Sender Key         |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.       |

[Request parameter]

| Name | Type | Required | Description                      |
|------|------|----------|----------------------------------|
| file | File | O        | Marketing consent records        |

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |

<a id="apply-for-using-non-friend-message-sending-targeting-m-n"></a>

### Apply for Non-Friend Message Sending (Targeting M, N)

<a id="requested-2"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name      | Type   | Description        |
|-----------|--------|--------------------|
| appkey    | String | Unique app key     |
| senderKey | String | Sender Key         |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.       |

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |

<a id="request-to-send-a-free-form-message"></a>

## Send Freestyle Messages

* Marketing consent-based sending can be used.
    * You can specify the message target type by setting the targeting field.
        * M: Users who have consented to receive advertising information from the company (KakaoTalk consent)
        * N: Users who have consented to receive advertising information from the company (KakaoTalk consent) - Channel friends
        * I: Intersection of the company's intended delivery targets ∩ Channel friends
* All 8 message types from the existing FriendTalk can be used.
* BT and AC button types can be used.
* The following restrictions apply when using the AC (Add Channel) button:
    * It is displayed as an emphasis button (yellow).
    * If there are multiple buttons, it must be placed at the designated position.
        * TEXT, IMAGE: First button (topmost)
        * Others: Second button (right)
    * The button name is fixed as "채널 추가".
    * For carousel type, only one AC button can be used across the entire carousel.
    * Only targeting types M and N can be used.
* When using the BF button, you can enter the business form ID issued by Kakao.
* Alternative delivery can be configured using resendParameter per recipient.
    * If you use alternative delivery, you must register an SMS AppKey and configure delivery settings using the Alternative Delivery Management API.
* **Delivery restricted during night (20:50~08:00 on the following day)**

<a id="requested-3"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name | Type | Description |
|--------|--------|--------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O | Can be created in the console. |

<a id="request-text-type-sending"></a>

#### Send Text Type

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | Sender key (40 characters). Group sender keys cannot be used. |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X | Whether to send a push notification for the message (default: true) |
| requestDate | String | X | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if field is not sent)<br>Can be scheduled up to 60 days in advance |
| unsubscribeNo | String | X | 080 toll-free opt-out phone number (if neither field is entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080 toll-free opt-out authentication number (up to 10 characters; if neither field is entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| adult | boolean | X | Whether the message is for adults only (default: false) |
| content | String | O | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field is optional. Up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| buttons | List | X | List of buttons<br>- For TEXT and IMAGE types: up to 4 buttons when a coupon is applied, up to 5 otherwise<br>- For WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For PREMIUM_VIDEO type: up to 1 button<br>- For COMMERCE type: minimum 1, maximum 2 buttons |
| - name | String | O | Button title<br>- For TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Chatbot Transfer, BF: Business Form)<br>- The BT type is available only for channels that use a chatbot from Kakao OpenBuilder<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 1,000 characters |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 1,000 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 1,000 characters |
| - chatExtra | String | X | Meta information to pass for the BT type button |
| - chatEvent | String | X | Bot event name to connect for the BT type button |
| - bizFormKey | String | X | Business form key for the BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed<br>- For all other types: up to 12 characters. Line breaks: not allowed |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, all remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, all remaining fields become optional. |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, all remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, all remaining fields become optional. |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, all remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, all remaining fields become optional. |
| recipientList | List | O | List of recipients (up to 1,000) |
| - recipientNo | String | O | Recipient number |
| - resendParameter | Object | X | Alternative delivery information |
| -- isResend | boolean | X | Whether to send the message as an SMS alternative if delivery fails<br>If alternative delivery is configured in the console, it is sent as an alternative by default. |
| -- resendType | String | X | Alternative delivery type (SMS, LMS)<br>If no value is provided, the type is determined based on the length of the template body. |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If no value is provided, the message is resent with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Alternative delivery 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Target type of the message (M: users who agreed to receive marketing messages, N: users who agreed to receive marketing messages but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 free opt-out phone number (if not entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if not entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient's grouping key (a grouping key can be specified per recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender's grouping key (a grouping key can be specified per sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                            |

<a id="request-image-type-sending"></a>

#### Send Image Type

[Request Body]

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used.                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send a push notification for the message (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 opt-out phone number (if all fields are left blank, the message is sent using the opt-out information registered in the Sender Profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 opt-out authentication number (up to 10 characters; if all fields are left blank, the message is sent using the opt-out information registered in the Sender Profile)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether the message is for adults only (default: false)                                                                                                                                                                                                                                                       |
| content                | String  | O  | - For the TEXT type: up to 1,300 characters (line breaks: up to 99; URL format input allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99; URL format input allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional. Up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used                           |
| image                  | Object  | O  | Image element<br>- Required field for the IMAGE, WIDE, and COMMERCE types                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | Image URL. Use the URL of an image uploaded as a general image.                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | URL to navigate to when the image is clicked. Limited to 1,000 characters.<br>If not set, the in-app KakaoTalk image viewer is used.                                                                                                                                                                                                                            |
| buttons                | List    | X  | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, up to 5 otherwise<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons                                                                                                                 |
| - name                 | String  | O  | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters                                                                                                                                                                                                                    |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Bot Transfer, BF: Business Form)<br>- The BT type is available only for channels that use a chatbot from Kakao Open Builder.<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - Make a reservation on Talk<br>  - Complete a survey on Talk<br>  - Enter a contest on Talk |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC web link (optional for the WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), limited to 1,000 characters                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), limited to 1,000 characters                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | Meta information to pass for the BT type button                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | Bot event name to connect for the BT type button                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | Business form key for the BF type button                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| - description          | String  | O  | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed.<br>- For all other types: up to 12 characters. Line breaks: not allowed.                                                                                                                                                                      |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), limited to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional for the WL type), limited to 1,000 characters                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | Recipient list (up to 1,000 recipients)                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to resend as SMS if delivery fails<br>If alternative delivery is configured in the console, it is sent as alternative delivery by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, the [message body] is used for alternative delivery.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Alternative delivery 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Target type for the message (M: users who have consented to marketing, N: users who have consented to marketing but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free opt-out number (if not entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free opt-out authentication number (up to 10 characters; if not entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key per recipient; up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key per sender; up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-wide-image-type-sending"></a>

#### Wide Image Type Delivery Request

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used.                                                                                                                                                                                                                 |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                   |
| pushAlarm              | boolean | X  | Whether to send a push notification for the message (default: true)                                                                                                                                                                                                           |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                           |
| unsubscribeNo       | String  | X  | 080 free opt-out phone number (if none entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if none entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether the message is for adults (default: false)                                                                                                                                                                                                                            |
| content                | String  | O  | - For TEXT type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For WIDE type: up to 76 characters (line breaks: up to 5)<br>- For PREMIUM_VIDEO type: this field can be used optionally, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| image                  | Object  | O  | Image element<br>- Required field for IMAGE, WIDE, and COMMERCE types                                                                                                                                                                                                         |
| - imageUrl             | String  | O  | Image URL; use the URL of an image uploaded as a wide image                                                                                                                                                                                                                   |
| - imageLink            | String  | X  | URL to navigate to when the image is clicked. Limited to 1,000 characters.<br>If not set, the KakaoTalk in-app image viewer is used.                                                                                                                                          |
| buttons                | List    | X  | Button list<br>- For TEXT and IMAGE types: up to 4 buttons when a coupon is applied, up to 5 otherwise<br>- For WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For PREMIUM_VIDEO type: up to 1 button<br>- For COMMERCE type: at least 1 button, up to 2 buttons         |
| - name                 | String  | O  | Button title<br>- For TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters                                                                                                                                                                  |
| - type                 | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The BT type is available only for channels that use a chatbot from Kakao Open Builder.<br>- The BF type can only be used as the first button, and the name field can only use one of the following three phrases:<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), limited to 1,000 characters                                                                                                                                                                                                       |
| - linkPc               | String  | X  | PC web link (optional for the WL type), limited to 1,000 characters                                                                                                                                                                                                           |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), limited to 1,000 characters                                                                                                                                                                                                      |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), limited to 1,000 characters                                                                                                                                                                                                          |
| - chatExtra            | String  | X  | Meta information to pass when using a BT type button                                                                                                                                                                                                                          |
| - chatEvent            | String  | X  | Bot event name to connect when using a BT type button                                                                                                                                                                                                                         |
| - bizFormKey           | String  | X  | Business form key for a BF type button                                                                                                                                                                                                                                        |
| coupon                 | Object  | X  | Coupon element                                                                                                                                                                                                                                                                |
| - title                | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description          | String  | O  | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed.<br>- For all other types: up to 12 characters. Line breaks: not allowed.                                                                   |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), limited to 1,000 characters.<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc               | String  | X  | PC web link (optional for the WL type), limited to 1,000 characters                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), limited to 1,000 characters.<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), limited to 1,000 characters.<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| recipientList          | List    | O  | Recipient list (up to 1,000 recipients)                                                                                                                                                                                                                                       |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                              |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send an alternative message if delivery fails<br>If alternative delivery is set in the console, it is sent as the default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, the [message body] is used.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Alternative delivery 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who have consented to marketing, N: users who have consented to marketing but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 free opt-out phone number (if none is entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if none is entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (a grouping key can be specified for each recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (a grouping key can be specified for each sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends a message)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-wide-item-list-type"></a>

#### Wide Item List Type Delivery Request

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | Sender Key (40 characters). Group sender keys are not supported. |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X | Whether to send a push notification for the message (default: true) |
| requestDate | String | X | Date and time of request (yyyy-MM-dd HH:mm)<br>(Sent immediately if not entered)<br>Scheduling is available up to 60 days in advance. |
| unsubscribeNo | String | X | 080 toll-free opt-out phone number (if neither field is entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080 toll-free opt-out authentication number (up to 10 characters; if neither field is entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| adult | boolean | X | Whether the message is for adults (default: false) |
| header | String | O | Header<br>- Required for the WIDE_ITEM_LIST type; up to 20 characters (line breaks not allowed)<br>- Optional for the PREMIUM_VIDEO type; up to 20 characters (line breaks not allowed) |
| item | Object | O | Wide list element (available only for the WIDE_ITEM_LIST type) |
| - list | List | O | Wide list (minimum: 3, maximum: 4) |
| -- title | String | O | Item title<br>- First item: up to 25 characters (maximum 1 line break; title is not required for the first item)<br>- Items 2–4: up to 30 characters (maximum 1 line break) |
| -- imageUrl | String | O | Item image URL<br>- For the first item, use the image URL uploaded as the first wide item list image<br>- For items 2–4, use the image URL uploaded as a regular wide item list image |
| -- linkMo | String | O | Mobile web link, up to 1,000 characters |
| -- linkPc | String | X | PC web link, up to 1,000 characters |
| -- schemeAndroid | String | X | Android app link, up to 1,000 characters |
| -- schemeIos | String | X | iOS app link, up to 1,000 characters |
| buttons | List | X | Button list<br>- For TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 button, up to 2 buttons |
| - name | String | O | Button title<br>- For TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Bot Transfer, BF: Business Form)<br>- The BT type is available only for channels using a chatbot built with Kakao Open Builder<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - Make a reservation in Talk<br>  - Take a survey in Talk<br>  - Enter a contest in Talk |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 1,000 characters |
| - linkPc | String | X | PC web link (optional for the WL type), up to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), up to 1,000 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), up to 1,000 characters |
| - chatExtra | String | X | Meta information to pass when using the BT type button |
| - chatEvent | String | X | Bot event name to connect when using the BT type button |
| - bizFormKey | String | X | Business form key for the BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters (line breaks not allowed)<br>- For all other types: up to 12 characters (line breaks not allowed) |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc               | String  | X  | PC web link (optional for the WL type), limited to 1,000 characters                                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | List of recipients (up to 1,000)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to use SMS as an alternative delivery method if the delivery fails<br>If alternative delivery is configured in the console, it is used as the default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | Title for LMS alternative delivery<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, the [message body] is used for alternative delivery.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 080 opt-out number for alternative delivery<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Target type for the message (M: users who agreed to receive marketing messages, N: users who agreed to receive marketing messages but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 free opt-out phone number (if not entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if not entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (a grouping key can be assigned per recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (a grouping key can be assigned per sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends a message)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as the user UUID when sending from the console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-premium-video-type"></a>

#### Send Premium Video Type

[Request Body]

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | Sender key (40 characters). Group sender keys cannot be used. |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X | Whether to send a push alarm for the message (default: true) |
| requestDate | String | X | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance |
| unsubscribeNo | String | X | 080 free opt-out phone number (if none is entered, the opt-out information registered in the Sender Profile is used for sending)<br>- 080-xxx-xxxx<br>- 080-xxxx-xxxx<br>- 080xxxxxxx<br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080 free opt-out authentication number (up to 10 characters; if none is entered, the opt-out information registered in the Sender Profile is used for sending)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| adult | boolean | X | Whether the message is for adults (default: false) |
| content | String | X | - For the TEXT type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| header | String | X | Header<br>- For the WIDE_ITEM_LIST type: required field, up to 20 characters (line breaks: not allowed)<br>- For the PREMIUM_VIDEO type: optional field, up to 20 characters (line breaks: not allowed) |
| video | Object | O | Video element (available only for the PREMIUM_VIDEO type) |
| - videoUrl | String | O | KakaoTV video URL (only URLs of videos uploaded to KakaoTV can be used), limited to 500 characters |
| - thumbnailUrl | String | X | Image URL for the video thumbnail (only URLs uploaded as regular images can be used; if not provided, the default KakaoTV video thumbnail is used), limited to 500 characters |
| buttons | List | X | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons |
| - name | String | O | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- The BT type is only available for channels that use a chatbot from Kakao Open Builder<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 1,000 characters |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 1,000 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 1,000 characters |
| - chatExtra | String | X | Meta information to pass when the button type is BT |
| - chatEvent | String | X | Bot event name to connect when the button type is BT |
| - bizFormKey | String | X | Business form key when the button type is BF |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed<br>- For all other types: up to 12 characters. Line breaks: not allowed |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 1,000 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| recipientList | List | O | Recipient list (up to 1,000 recipients) |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send as an alternative text message if delivery fails<br>If alternative delivery is set in the console, it is sent as an alternative by default.                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, sent with [message body] as alternative.)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Alternative delivery 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Type of message target (M: users who agreed to receive marketing messages, N: users who agreed to receive marketing messages but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 free opt-out phone number (if all are left blank, sent with the opt-out information registered in the Sender Profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if all are left blank, sent with the opt-out information registered in the Sender Profile)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key per recipient; up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key per sender; up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                            |

<a id="request-to-send-commerce"></a>

#### Commerce Type Delivery Request

[Request Body]

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

| Name | Type | Required | Description |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey | String | O | Sender key (40 characters). Group sender keys are not available. |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X | Whether to send a push notification for the message (default: true) |
| requestDate | String | X | Request date and time (yyyy-MM-dd HH:mm)<br>(If not entered, the message is sent immediately.)<br>Can be scheduled up to 60 days in advance. |
| unsubscribeNo | String | X | 080 toll-free opt-out number (if neither field is entered, the opt-out information registered in the Sender Profile is used for sending)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X | 080 toll-free opt-out authentication number (up to 10 characters; if neither field is entered, the opt-out information registered in the Sender Profile is used for sending)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| adult | boolean | X | Whether the message is for adults only (default: false) |
| additionalContent | String | X | Additional information (up to 34 characters, line breaks: up to 1). Available only for the Commerce type. |
| image | Object | O | Image element<br>- Required for the IMAGE, WIDE, and COMMERCE types |
| - imageUrl | String | O | Image URL. Use the URL of an image uploaded as a general image. |
| - imageLink | String | X | URL to navigate to when the image is clicked. Limited to 1,000 characters.<br>If not set, the KakaoTalk in-app image viewer is used. |
| commerce | Object | O | Commerce (available only for the COMMERCE type) |
| title | String | O | Product title (up to 30 characters, line breaks: not allowed) |
| regularPrice | Integer | O | Regular price (0–99,999,999) |
| discountPrice | Integer | X | Discounted price (0–99,999,999) |
| discountRate | Integer | X | Discount rate (0–100). When a discounted price exists, either the discount rate or the fixed discount price is required. |
| discountFixed | Integer | X | Fixed discount price (0–999,999). When a discounted price exists, either the discount rate or the fixed discount price is required. |
| buttons | List | O | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons |
| - name | String | O | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The BT type is available only for channels that use a chatbot from Kakao Open Builder.<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 1,000 characters |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 1,000 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 1,000 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 1,000 characters |
| - chatExtra | String | X | Meta information to pass when using a BT type button |
| - chatEvent | String | X | Bot event name to connect when using a BT type button |
| - bizFormKey | String | X | Biz form key for a BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters. Line breaks: not allowed.<br>- For all other types: up to 12 characters. Line breaks: not allowed. |
| - linkMo               | String  | X  | Mobile web link (required for the WL type), up to 1,000 characters<br>If you enter a value in the linkMo field for a coupon, the remaining fields become optional.<br>If you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - linkPc               | String  | X  | PC web link (optional for the WL type), up to 1,000 characters                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android app link (required for the AL type), up to 1,000 characters<br>If you enter a value in the linkMo field for a coupon, the remaining fields become optional.<br>If you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| - schemeIos            | String  | X  | iOS app link (required for the AL type), up to 1,000 characters<br>If you enter a value in the linkMo field for a coupon, the remaining fields become optional.<br>If you enter a channel coupon URL (format: alimtalk=coupon://) in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| recipientList          | List    | O  | List of recipients (up to 1,000)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send as an alternative text message if delivery fails<br>If alternative delivery is configured in the console, it is sent as an alternative by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | Title for LMS alternative delivery<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, it is sent with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 080 opt-out number for alternative delivery<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Target type for the message (M: users who have consented to marketing, N: users who have consented to marketing but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 toll-free opt-out phone number (if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 toll-free opt-out authentication number (up to 10 characters; if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>Cannot enter unsubscribeAuthNo alone without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key per recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key per sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as the user UUID when sent from the console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                                            |

<a id="request-to-send-carousel-feed-type"></a>

#### Send Carousel Feed

##### Carousel Fixed Replacement Variables (Path-Based Template Parameters)

In the carousel type, you can apply different replacement variable values to each item.

* **Format**: `key@$.carousel.list[index]` (e.g., `productName@$.carousel.list[0]`)
* If a head (carousel intro) is present, it occupies `list[0]`, and the actual items start from `list[1]`.
* If a value cannot be found using a path-based key, it falls back to the regular key.

| Category | Replaceable Fields | Path Format |
|------|--------------|----------|
| Carousel intro (head) | header, content, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[0]` (when head is present) |
| Carousel item | header, message, additionalContent | `key@$.carousel.list[index]` |
| Carousel item button | linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item coupon | title, description, linkMo, linkPc, schemeAndroid, schemeIos | `key@$.carousel.list[index]` |
| Carousel item commerce | title | `key@$.carousel.list[index]` |

* **Tail**: Replacement variables cannot be used.
* Button indexes are not included in the path. Buttons within the same item are replaced with the same path value.

> **Caution** The `@` character cannot be used in replacement variable keys (`#{key}`). The `@` character is used by the system as a path delimiter.

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

| Name                     | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters). Group sender keys cannot be used.                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm              | boolean | X  | Whether to send a push notification for the message (default: true)                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 toll-free opt-out number (if neither field is entered, the message is sent using the opt-out information registered in the Sender Profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 toll-free opt-out authentication number (up to 10 characters; if neither field is entered, the message is sent using the opt-out information registered in the Sender Profile)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                  | boolean | X  | Whether the message is for adults (default: false)                                                                                                                                                                                                                                                                       |
| carousel               | Object  | O  | Carousel                                                                                                                                                                                                                                                                           |
| - list                 | List    | O  | Carousel list (minimum 2, maximum 6)                                                                                                                                                                                                                                                        |
| -- header              | String  | O  | Carousel item title (up to 20 characters). Only available in carousel feeds.                                                                                                                                                                                                                                          |
| -- message             | String  | O  | Carousel item title (up to 20 characters), carousel item message (up to 180 characters). Only available in carousel feeds.                                                                                                                                                                                                                    |
| -- imageUrl            | String  | O  | Image URL (only images uploaded as carousel feed images can be used)                                                                                                                                                                                                                                        |
| -- imageLink           | String  | X  | Image link, up to 1,000 characters                                                                                                                                                                                                                                              |
| -- buttons             | List    | O  | Carousel list button list: minimum 1, maximum 2                                                                                                                                                                                                                                                                    |
| --- name               | String  | O  | Button title<br>- Up to 14 characters for TEXT and IMAGE types<br>- Up to 8 characters for all other types                                                                                                                                                                                                                    |
| --- type               | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Chatbot Transfer, BF: Business Form)<br>- The BT type is only available for channels that use a chatbot from Kakao Open Builder.<br>- The BF type can only be used as the first button, and the name field must use one of the following three phrases only:<br>  - Reserve via Talk<br>  - Survey via Talk<br>  - Enter via Talk |
| --- linkMo             | String  | X  | Mobile web link (required for the WL type), up to 1,000 characters                                                                                                                                                                                                                                         |
| --- linkPc             | String  | X  | PC web link (optional for the WL type), up to 1,000 characters                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | Android app link (required for the AL type), up to 1,000 characters                                                                                                                                                                                                                                       |
| --- schemeIos          | String  | X  | iOS app link (required for the AL type), up to 1,000 characters                                                                                                                                                                                                                         |
| --- chatExtra          | String  | X  | Meta information to pass for BT type buttons                                                                                                                                                                                                                                                   |
| --- chatEvent          | String  | X  | Bot event name to connect for BT type buttons                                                                                                                                                                                                                                                       |
| --- bizFormKey         | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                                            |
| -- coupon              | Object  | X  | Coupon element                                                                                                                                                                                                                                                                         |
| --- title              | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| --- description        | String  | O  | Coupon detailed description<br>- Up to 18 characters for WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types; line breaks not allowed<br>- Up to 12 characters for all other types; line breaks not allowed                                                                                                                                                                      |
| --- linkMo             | String  | X  | Mobile web link (required for the WL type), up to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| --- linkPc             | String  | X  | PC web link (optional for the WL type), up to 1,000 characters                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | Android app link (required for the AL type), up to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| --- schemeIos          | String  | X  | iOS app link (required for the AL type), up to 1,000 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - tail                 | Object  | X  | More button information                                                                                                                                                                                                                                                                     |
| -- linkMo              | String  | O  | Mobile web link, up to 1,000 characters                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC web link, up to 1,000 characters                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | Android app link, up to 1,000 characters                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | iOS app link, up to 1,000 characters                                                                                                                                                                                                                                                           |
| recipientList          | List    | O  | List of recipients (up to 1,000)                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | Recipient number                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | Whether to send via SMS if delivery fails<br>If alternative delivery is configured in the console, it is sent as alternative delivery by default.                                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, it is sent with [message body].)                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | Alternative delivery sender number<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | Alternative delivery 080 opt-out number<br><span style="color:red">(If it is not the 080 opt-out number registered in the SMS service, alternative delivery may fail.)</span>                                                                                                                                                                       |
| - targeting         | String  | X  | Target type of the message (M: users who agreed to receive marketing messages, N: users who are not friends but agreed to receive marketing messages, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 free opt-out phone number (if all fields are left blank, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 free opt-out authentication number (up to 10 characters; if all fields are left blank, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key per recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key per sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console)                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | Statistics ID (not included in the delivery search conditions, up to 8 characters)                                                                                                                                                                                                                            |

<a id="request-to-send-carousel-commerce-type"></a>

#### Carousel Commerce Delivery Request

[Request Body]

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

| Name                   | Type      | Required | Description                                                                                                                                                                                                                                                                            |
|----------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey            | String  | O  | Sender key (40 characters), group sender key not available                                                                                                                                                                                                                                                       |
| chatBubbleType       | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                         |
| pushAlarm            | boolean | X  | Whether to send a message push alarm (default: true)                                                                                                                                                                                                                                                   |
| requestDate          | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 opt-out toll-free number (if none is entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 opt-out authentication number (up to 10 characters; if none is entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| adult                | boolean | X  | Whether the message is for adults (default: false)                                                                                                                                                                                                                                                       |
| carousel             | Object  | O  | Carousel                                                                                                                                                                                                                                                                           |
| - head               | Object  | X  | Carousel intro                                                                                                                                                                                                                                                                       |
| -- header            | String  | O  | Carousel intro header (up to 20 characters)                                                                                                                                                                                                                                            |
| -- content           | String  | O  | Carousel intro content (up to 50 characters)                                                                                                                                                                                                                                            |
| -- imageUrl          | String  | O  | Carousel intro image URL (must use an image uploaded as a Carousel Commerce type image; the image ratio must match that of the carousel images)                                                                                                                                                                                                                    |
| -- linkMo            | String  | X  | Mobile web link (required if any of linkMo, linkPc, schemeAndroid, or schemeIos is used), up to 1,000 characters                                                                                                                                                                                    |
| -- linkPc            | String  | X  | PC web link, up to 1,000 characters                                                                                                                                                                                                                                           |
| -- schemeAndroid     | String  | X  | Android app link, up to 1,000 characters                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | iOS app link, up to 1,000 characters                                                                                                                                                                                                                                           |
| - list               | List    | O  | Carousel list (minimum 1, maximum 5 if head exists; otherwise minimum 2, maximum 6)                                                                                                                                                                                                                      |
| -- additionalContent | String  | X  | Additional information (up to 34 characters), available only for Carousel Commerce type                                                                                                                                                                                                                                              |
| -- imageUrl          | String  | O  | Image URL (must use an image uploaded as a Carousel Commerce type image)                                                                                                                                                                                                                                           |
| -- imageLink         | String  | X  | Image link, up to 1,000 characters                                                                                                                                                                                                                                              |
| -- commerce          | Object  | O  | Commerce (available only for CAROUSEL_COMMERCE type)                                                                                                                                                                                                                                           |
| --- title            | String  | O  | Product title (up to 30 characters, line breaks not allowed)                                                                                                                                                                                                                                                       |
| --- regularPrice     | Integer | O  | Regular price (0–99,999,999)                                                                                                                                                                                                                                                        |
| --- discountPrice    | Integer | X  | Discounted price (0–99,999,999)                                                                                                                                                                                                                                                          |
| --- discountRate     | Integer | X  | Discount rate (0–100); if a discounted price exists, either the discount rate or the fixed discount price is required                                                                                                                                                                                                                                   |
| --- discountFixed    | Integer | X  | Fixed discount price (0–999,999); if a discounted price exists, either the discount rate or the fixed discount price is required                                                                                                                                                                                                                            |
| -- buttons           | List    | O  | Carousel list button list: minimum 1, maximum 2                                                                                                                                                                                                                                                    |
| --- name             | String  | O  | Button title<br>- Up to 14 characters for TEXT and IMAGE types<br>- Up to 8 characters for all other types                                                                                                                                                                                                                    |
| --- type             | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Chatbot Transfer, BF: Business Form)<br>- The BT type is available only for channels that use a chatbot from Kakao Open Builder<br>- The BF type can only be used as the first button, and only the following three phrases are allowed for name:<br>  - 톡에서 예약하기<br>  - 톡에서 설문하기<br>  - 톡에서 응모하기 |
| --- linkMo           | String  | X  | Mobile web link (required for the WL type), up to 1,000 characters                                                                                                                                                                                                                                         |
| --- linkPc           | String  | X  | PC web link (optional for the WL type), 1,000-character limit                                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | Android app link (required for the AL type), 1,000-character limit                                                                                                                                                                                                                                                       |
| --- schemeIos        | String  | X  | iOS app link (required for the AL type), 1,000-character limit                                                                                                                                                                                                                                                         |
| --- chatExtra        | String  | X  | Metadata to pass for BT type buttons                                                                                                                                                                                                                                                                         |
| --- chatEvent        | String  | X  | Bot event name to connect for BT type buttons                                                                                                                                                                                                                                                                       |
| --- bizFormKey       | String  | X  | Biz form key for BF type buttons                                                                                                                                                                                                                                                                                            |
| -- coupon            | Object  | X  | Coupon element                                                                                                                                                                                                                                                                                         |
| --- title            | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                                                                            |
| --- description      | String  | O  | Coupon detail description<br>- For WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters, line breaks not allowed<br>- For all other types: up to 12 characters, line breaks not allowed                                                                                                                                                                      |
| --- linkMo           | String  | X  | Mobile web link (required for the WL type), 1,000-character limit<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| --- linkPc           | String  | X  | PC web link (optional for the WL type), 1,000-character limit                                                                                                                                                                                                                                                          |
| --- schemeAndroid    | String  | X  | Android app link (required for the AL type), 1,000-character limit<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                 |
| --- schemeIos        | String  | X  | iOS app link (required for the AL type), 1,000-character limit<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                   |
| - tail               | Object  | X  | View More button information                                                                                                                                                                                                                                                                                     |
| -- linkMo            | String  | O  | Mobile web link, 1,000-character limit                                                                                                                                                                                                                                                                           |
| -- linkPc            | String  | X  | PC web link, 1,000-character limit                                                                                                                                                                                                                                                                            |
| -- schemeAndroid     | String  | X  | Android app link, 1,000-character limit                                                                                                                                                                                                                                                                         |
| -- schemeIos         | String  | X  | iOS app link, 1,000-character limit                                                                                                                                                                                                                                                                           |
| recipientList        | List    | O  | List of recipients (up to 1,000)                                                                                                                                                                                                                                                                             |
| - recipientNo        | String  | O  | Recipient number                                                                                                                                                                                                                                                                                         |
| - resendParameter    | Object  | X  | Alternative delivery information                                                                                                                                                                                                                                                                                      |
| -- isResend          | boolean | X  | Whether to send as an SMS alternative if delivery fails<br>If alternative delivery is configured in the console, it is sent as an alternative by default.                                                                                                                                                                                                                       |
| -- resendType        | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable.                                                                                                                                                                                                                       |
| -- resendTitle       | String  | X  | Title for LMS alternative delivery<br>(resent with PlusFriend ID if value is unavailable.)                                                                                                                                                                                                                               |
| -- resendContent     | String  | X  | Alternative delivery content<br>(If value is unavailable, the [message body] is used for alternative delivery.)                                                                                                                                                                                                                                  |
| -- resendSendNo      | String  | X  | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span>                                                                                                                                                                                 |
| - targeting         | String  | X  | Type of message target (M: users who agreed to receive marketing messages, N: users who agreed to receive marketing messages but are not friends, I: users who are friends)                                                                |
| - unsubscribeNo       | String  | X  | 080 opt-out phone number (if none is entered, the opt-out information registered in the Sender Profile is used)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 opt-out authentication number (up to 10 characters; if none is entered, the opt-out information registered in the Sender Profile is used)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234        |
| - recipientGroupingKey | String  | X  | Recipient grouping key (you can specify a grouping key per recipient, up to 100 characters)                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | Sender grouping key (you can specify a grouping key per sender, up to 100 characters)                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | Reseller code (used when a reseller sends messages)                                                                                                                                                                                                                                                                       |
| createUser           | String  | X  | Registrant (saved as the user UUID when sending from the console)                                                                                                                                                                                                                                                   |
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

| Name               | Type      | Not Null | Description                                                           |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | Response header information                                                     |
| - isSuccessful   | boolean | O        | Whether the API call was successful                                                 |
| - resultCode     | Integer | O        | API call result code (0 on success, error code on failure)                           |
| - resultMessage  | String  | O        | API call result message ("success" or a related success message on success, detailed failure message on failure)  |
| message          | Object  | X        | Message delivery result information (exists only when a delivery request is made)                             |
| - requestId      | String  | X        | Delivery request ID (a unique ID that identifies each delivery request)                             |
| - sendResults    | Array   | O        | List of delivery results per recipient                                                |
| -- recipientSeq  | Integer | O        | Sequence number in the recipient list                                                   |
| -- recipientNo   | String  | X        | Recipient phone number                                                     |
| -- resultCode    | Integer | O        | Delivery result code per recipient (may include success and various failure codes)                     |
| -- resultMessage | String  | O        | Delivery result message per recipient ("success" or a related message on success, detailed failure message on failure) |

<a id="request-to-send-basic-message"></a>

## Send Basic Message Request

* This delivery uses a template.
* Marketing consent-based sending can be used.
    * You can specify the message target type by setting the targeting field.
        * M: Users who have consented to receive advertising information from the customer company (KakaoTalk consent)
        * N: Users who have consented to receive advertising information from the customer company (KakaoTalk consent) — channel friends
        * I: Intersection of the customer company's requested delivery targets ∩ channel friends
* All 8 message types from the existing FriendTalk can be used.
* The BT button type cannot be used.
* The AC (Add Channel) button can be used.
* When using the BF button, you can upload the business form ID issued by Kakao to receive a BizForm key and use it.
* Alternative delivery can be configured per recipient using resendParameter.
    * To use alternative delivery, you must register an SMS AppKey and configure delivery settings via the Alternative Delivery Management API.
* **Delivery restricted during night (20:50~08:00 on the following day)**

<a id="cautions-for-use"></a>

### Notes on Usage

- `unsubscribeNo` and `unsubscribeAuthNo` are the 080 toll-free opt-out phone number and authentication number. If either one is not entered, the message will be sent using the opt-out information registered in the Sender Profile.
- If you enter `unsubscribeNo` and `unsubscribeAuthNo` when sending, the message will be sent using the entered values instead of the opt-out information registered in the Sender Profile.
- If you do not enter `unsubscribeNo` and `unsubscribeAuthNo` when sending, the message will be sent using the opt-out information registered in the Sender Profile.
- `unsubscribeNo` and `unsubscribeAuthNo` can be entered per recipient. If both the common field and the per-recipient field are entered, the common field takes priority.

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

| Name | Type | Description |
|--------|--------|--------|
| appkey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O | Can be created in the console. |

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

| Name | Type | Required | Description |
|------------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | Sender key (40 characters); group sender keys are not allowed |
| templateCode           | String  | O  | Template code to use |
| pushAlarm              | boolean | X  | Whether to send a message push notification (default: true) |
| requestDate            | String  | X  | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance |
| unsubscribeNo          | String  | X  | 080 free opt-out phone number (if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo      | String  | X  | 080 free opt-out authentication number (up to 10 characters; if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| recipientList          | List    | O  | List of recipients (up to 1,000) |
| - recipientNo          | String  | O  | Recipient number |
| - targeting            | String  | O  | Type of message target (M: users who agreed to receive marketing messages, N: users who are not friends but agreed to receive marketing messages only, I: users who are friends) |
| - templateParameter    | Object  | X  | Template parameter (required if the template contains variables to replace)<br>- Carousel type: use the format `key@$.carousel.list[index]` to apply different replacement values per item (e.g., `productName@$.carousel.list[0]`)<br>- If a head exists, head occupies list[0] and actual items start from list[1]<br>- The `@` character cannot be used in replacement variable keys<br>- Up to 1,300 characters each for key and value |
| - imageParameters      | List    | X  | Dynamic parameters for changing template image field values (only a JSON list of the same size as the number of images in the template can be used; if used, an empty JSON object must be entered for images that are not to be changed) |
| -- imageUrl            | String  | O  | Image URL |
| -- imageLink           | String  | X  | Image link |
| - videoParameter       | Object  | X  | Dynamic parameters for changing template video field values |
| -- videoUrl            | String  | O  | KakaoTV video URL |
| -- thumbnailUrl        | String  | X  | Image URL for video thumbnail |
| - resendParameter      | Object  | X  | Alternative delivery information |
| -- isResend            | boolean | X  | Whether to send the message as an alternative delivery if sending fails<br>If alternative delivery is configured in the console, it is sent as alternative delivery by default. |
| -- resendType          | String  | X  | Alternative delivery type (SMS, LMS)<br>Categorized by the length of template body, if value is unavailable. |
| -- resendTitle         | String  | X  | LMS alternative delivery title<br>(resent with PlusFriend ID if value is unavailable.) |
| -- resendContent       | String  | X  | Alternative delivery content<br>(If value is unavailable, the [message body] is used for alternative delivery.) |
| -- resendSendNo        | String  | X  | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail, if the sender number is not registered on the SMS service.)</span> |
| - unsubscribeNo        | String  | X  | 080 free opt-out phone number (if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| - unsubscribeAuthNo    | String  | X  | 080 free opt-out authentication number (up to 10 characters; if all fields are left blank, the opt-out information registered in the Sender Profile is used for delivery)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |
| - recipientGroupingKey | String  | X  | Recipient grouping key (a grouping key can be specified per recipient; up to 100 characters) |
| senderGroupingKey      | String  | X  | Sender grouping key (a grouping key can be specified per sender; up to 100 characters) |
| resellerCode           | String  | X  | Reseller code (used when a reseller sends a message) |
| createUser             | String  | X  | Registrant (saved as user UUID when sending from console) |
| statsId                | String  | X  | Statistics ID (not included in the delivery search conditions, up to 8 characters) |

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

| Name               | Type      | Not Null | Description                                                                                                             |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | Response header information                                                                                             |
| - isSuccessful   | boolean | O        | Whether the API call was successful                                                                                     |
| - resultCode     | Integer | O        | API call result code (success: 0, error code on failure)                                                                |
| - resultMessage  | String  | O        | API call result message ("success" or a related success message on success, detailed failure reason on failure)         |
| message          | Object  | X        | Message delivery result information (present only if a delivery request was made)                                       |
| - requestId      | String  | X        | Delivery request ID (a unique identifier for each delivery request)                                                     |
| - sendResults    | Array   | O        | List of delivery results per recipient                                                                                  |
| -- recipientSeq  | Integer | O        | Sequence number in the recipient list                                                                                   |
| -- recipientNo   | String  | X        | Recipient phone number                                                                                                  |
| -- resultCode    | Integer | O        | Delivery result code per recipient (may include success and various failure codes)                                      |
| -- resultMessage | String  | O        | Delivery result message per recipient ("success" or a related message on success, detailed failure reason on failure)   |

<a id="view-sending-list"></a>

## List Deliveries

<a id="requested-5"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
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

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter] No.1 or No.2 is conditionally required

| Name                 | Type   | Required              | Description                                                  |
|----------------------|--------|-----------------------|--------------------------------------------------------------|
| requestId            | String | Conditionally required (No.1) | Request ID                                                   |
| startRequestDate     | String | Conditionally required (No.2) | Start date of delivery request (yyyy-MM-dd HH:mm)            |
| endRequestDate       | String | Conditionally required (No.2) | End date of delivery request (yyyy-MM-dd HH:mm)              |
| senderKey            | String | X                     | Sender Key                                                   |
| templateCode         | String | X                     | Template Code                                                |
| recipientNo          | String | X                     | Recipient number                                             |
| messageStatus        | String | X                     | Request status (COMPLETED: success, FAILED: failure)         |
| resultCode           | String | X                     | Delivery result (MRC01: success, MRC02: failure)             |
| senderGroupingKey    | String | X                     | Sender grouping key                                          |
| recipientGroupingKey | String | X                     | Recipient grouping key                                       |
| pageNum              | String | X                     | Page number (Default: 1)                                     |
| pageSize             | String | X                     | Number of results (Default: 15, Max: 1000)                   |

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

| Name                        | Type    | Not Null | Description                                                                                                                  |
|:----------------------------|:--------|:---------|:-----------------------------------------------------------------------------------------------------------------------------|
| header                      | Object  | O        | Header area                                                                                                                  |
| - resultCode                | Integer | O        | Result code                                                                                                                  |
| - resultMessage             | String  | O        | Result message                                                                                                               |
| - isSuccessful              | boolean | O        | Success                                                                                                                      |
| messageSearchResultResponse | Object  | X        | Body area                                                                                                                    |
| - messages                  | Array   | O        | Message list                                                                                                                 |
| -- requestId                | String  | O        | Request ID                                                                                                                   |
| -- recipientSeq             | Integer | O        | Recipient sequence number                                                                                                    |
| -- plusFriendId             | String  | O        | Sender Profile ID                                                                                                            |
| -- senderKey                | String  | O        | Sender Key                                                                                                                   |
| -- templateCode             | String  | X        | Template Code                                                                                                                |
| -- recipientNo              | String  | O        | Recipient number                                                                                                             |
| -- targeting                | String  | O        | Target type of message (M: users who agreed to receive marketing messages, N: users who are not friends but agreed to receive marketing messages, I: users who are friends) |
| -- requestDate              | String  | O        | Date and time of request (yyyy-MM-dd HH:mm)<br>(to be sent immediately if not entered)<br>Can be scheduled up to 60 days in advance |
| -- createDate               | String  | O        | Date and time of registration                                                                                                |
| -- receiveDate              | String  | X        | Date and time of receipt                                                                                                     |
| -- chatBubbleType           | String  | O        | Message type                                                                                                                 |
| -- pushAlarm                | boolean | O        | Whether push alarm is used                                                                                                   |
| -- messageStatus            | String  | O        | Request status (COMPLETED: success, FAILED: failure)                                                                         |
| -- resendStatusCode         | String  | X        | Alternative delivery status code                                                                                             |
| -- resendStatusName         | String  | X        | Alternative delivery status name                                                                                             |
| -- resendResultCode         | String  | X        | Alternative delivery result code                                                                                             |
| -- resendRequestId          | String  | X        | Alternative delivery request ID                                                                                              |
| -- isAddedChannel           | boolean | O        | Whether the user is a channel friend                                                                                         |
| -- resultCode               | String  | X        | Receipt result code                                                                                                          |
| -- resultCodeName           | String  | X        | Receipt result code name                                                                                                     |
| -- createUser               | String  | X        | Registrant (saved as user UUID when sending from console)                                                                    |
| -- senderGroupingKey        | String  | X        | Sender grouping key                                                                                                          |
| -- recipientGroupingKey     | String  | X        | Recipient grouping key                                                                                                       |
| - totalCount                | Integer | O        | Total count                                                                                                                  |

<a id="view-single-sending"></a>

## Get Delivery

<a id="requested-6"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name           | Type      | Description              |
|--------------|---------|------------|
| appkey       | String  | Unique app key     |
| requestId    | String  | Request ID      |
| recipientSeq | Integer | Recipient sequence number |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | Type     | Required | Description               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

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
| message               | Object  | X        | Message body area (may not exist if the message fails)                                                                     |
| - requestId           | String  | O        | Request ID (Not Null if the message object exists)                                                                 |
| - recipientSeq        | Integer | O        | Recipient sequence number (Not Null if the message object exists)                                                            |
| - plusFriendId        | String  | O        | Sender Profile ID (Not Null if the message object exists)                                                             |
| - senderKey           | String  | O        | Sender Key (Not Null if the message object exists)                                                                  |
| - templateCode        | String  | X        | Template Code                                                                                           |
| - recipientNo         | String  | O        | Recipient number (Not Null if the message object exists)                                                                 |
| - targeting           | String  | O        | Type of message target (M: users who have agreed to receive marketing messages, N: only users who are not friends but have agreed to receive marketing messages, I: users who are friends) (Not Null if the message object exists) |
| - requestDate         | String  | O        | Date and time of request (yyyy-MM-dd HH:mm)<br>(send immediately, if it is left blank)<br>Can be scheduled up to 60 days in advance (Not Null if the message object exists)                                                                 |
| - createDate          | String  | O        | Registration date and time (Not Null if the message object exists)                                                                 |
| - receiveDate         | String  | X        | Date and time of receipt                                                                                            |
| - chatBubbleType      | String  | O        | Message type (Not Null if the message object exists)                                                                |
| - content             | String  | X        | Message content                                                                                           |
| - adult               | boolean | O        | Whether the message is for adults (Not Null if the message object exists)                                                            |
| - header              | String  | X        | Header (in message)                                                                                       |
| - additionalContent   | String  | X        | Additional information (in message)                                                                                    |
| - image               | Object  | X        | Image element                                                                                           |
| -- imageUrl           | String  | O        | Image URL (Not Null if the image object exists)                                                                 |
| -- imageLink          | String  | X        | Image link                                                                                           |
| - buttons             | Array   | X        | Button list                                                                                            |
| -- name               | String  | O        | Button title (Not Null if the buttons array item exists)                                                              |
| -- type               | String  | O        | Button type (Not Null if the buttons array item exists)                                                              |
| -- linkMo             | String  | X        | Mobile web link                                                                                         |
| -- linkPc             | String  | X        | PC web link                                                                                          |
| -- schemeIos          | String  | X        | iOS app link                                                                                         |
| -- schemeAndroid      | String  | X        | Android app link                                                                                       |
| -- chatExtra          | String  | X        | Meta information to be delivered for BT type button                                                                      |
| -- chatEvent          | String  | X        | Bot event name to connect for BT type button                                                                          |
| -- bizFormKey         | String  | X        | Biz form key for BF type button                                                                               |
| - item                | Object  | X        | Wide list element                                                                                       |
| -- list               | Array   | X        | Wide list (Nullable if the item object exists)                                                                  |
| --- title             | String  | X        | Item title                                                                                           |
| --- imageUrl          | String  | O        | Item image URL (Not Null if the item.list item exists)                                                         |
| --- linkMo            | String  | O        | Mobile web link (Not Null if the item.list item exists)                                                            |
| --- linkPc            | String  | X        | PC web link                                                                                          |
| --- schemeIos         | String  | X        | iOS app link                                                                                         |
| --- schemeAndroid     | String  | X        | Android app link                                                                                       |
| - coupon              | Object  | X        | Coupon element                                                                                            |
| -- title              | String  | O        | Coupon title (Not Null if the coupon object exists)                                                                  |
| -- description        | String  | O        | Coupon detailed description (Not Null if the coupon object exists)                                                               |
| -- linkMo             | String  | X        | Mobile web link                                                                                         |
| -- linkPc             | String  | X        | PC web link                                                                                          |
| -- schemeAndroid      | String  | X        | Android app link                                                                                       |
| -- schemeIos          | String  | X        | iOS app link                                                                                         |
| - commerce            | Object  | X        | Commerce element                                                                                           |
| -- title              | String  | O        | Product title (Not Null if the commerce object exists)                                                                |
| -- regularPrice       | Integer | X        | Regular price                                                                                            |
| -- discountPrice      | Integer | X        | Discounted price                                                                                             |
| -- discountRate       | Integer | X        | Discount rate                                                                                              |
| -- discountFixed      | Integer | X        | Fixed discount price                                                                                           |
| - video               | Object  | X        | Video element                                                                                           |
| -- videoUrl           | String  | O        | KakaoTV video URL (Not Null if the video object exists)                                                           |
| -- thumbnailUrl       | String  | X        | Image URL for video thumbnail                                                                                 |
| - carousel            | Object  | X        | Carousel                                                                                              |
| -- head               | Object  | X        | Carousel intro (Nullable if the carousel object exists)                                                              |
| --- header            | String  | O        | Carousel intro header (Not Null if the head object exists)                                                               |
| --- content           | String  | O        | Carousel intro content (Not Null if the head object exists)                                                               |
| --- imageUrl          | String  | O        | Carousel intro image URL (Not Null if the head object exists)                                                           |
| --- linkMo            | String  | X        | Mobile web link                                                                                         |
| --- linkPc            | String  | X        | PC web link                                                                                          |
| --- schemeIos         | String  | X        | iOS app link                                                                                         |
| --- schemeAndroid     | String  | X        | Android app link                                                                                       |
| -- list               | Array   | O        | Carousel list (Not Null if carousel object exists)                                                              |
| --- header            | String  | X        | Carousel item header                                                                                       |
| --- message           | String  | O        | Carousel item message (Not Null if list item exists)                                                              |
| --- additionalContent | String  | X        | Additional information                                                                                            |
| --- imageUrl          | String  | X        | Image URL                                                                                          |
| --- imageLink         | String  | X        | Image link                                                                                           |
| --- commerce          | Object  | X        | Commerce (in carousel)                                                                                      |
| ---- title            | String  | O        | Product title (Not Null if carousel.list.commerce exists)                                                     |
| ---- regularPrice     | Integer | X        | Regular price                                                                                            |
| ---- discountPrice    | Integer | X        | Discounted price                                                                                             |
| ---- discountRate     | Integer | X        | Discount rate                                                                                              |
| ---- discountFixed    | Integer | X        | Fixed discount price                                                                                           |
| --- buttons           | Array   | X        | Button list (in carousel)                                                                                    |
| ---- name             | String  | O        | Button title (Not Null if carousel.list.buttons item exists)                                                   |
| ---- type             | String  | O        | Button type (Not Null if carousel.list.buttons item exists)                                                   |
| ---- linkMo           | String  | X        | Mobile web link                                                                                         |
| ---- linkPc           | String  | X        | PC web link                                                                                          |
| ---- schemeAndroid    | String  | X        | Android app link                                                                                       |
| ---- schemeIos        | String  | X        | iOS app link                                                                                         |
| ---- chatExtra        | String  | X        | Meta information to be delivered for BT type button                                                                      |
| ---- chatEvent        | String  | X        | Bot event name to connect for BT type button                                                                          |
| ---- bizFormKey       | String  | X        | Biz form key for BF type button                                                                               |
| --- coupon            | Object  | X        | Coupon (in carousel)                                                                                       |
| ---- title            | String  | O        | Coupon title (Not Null if carousel.list.coupon exists)                                                       |
| ---- description      | String  | O        | Coupon detailed description (Not Null if carousel.list.coupon exists)                                                    |
| ---- linkMo           | String  | X        | Mobile web link                                                                                         |
| ---- linkPc           | String  | X        | PC web link                                                                                          |
| ---- schemeAndroid    | String  | X        | Android app link                                                                                       |
| ---- schemeIos        | String  | X        | iOS app link                                                                                         |
| -- tail               | Object  | X        | More button information (Nullable if carousel object exists)                                                            |
| --- linkMo            | String  | O        | Mobile web link (Not Null if tail object exists)                                                                 |
| --- linkPc            | String  | X        | PC web link                                                                                          |
| --- schemeAndroid     | String  | X        | Android app link                                                                                       |
| --- schemeIos         | String  | X        | iOS app link                                                                                         |
| - templateParameter   | String  | X        | Template parameter                                                                                         |
| - pushAlarm           | boolean | O        | Whether push alarm is enabled (Not Null if message object exists)                                                              |
| - messageStatus       | String  | O        | Request status (COMPLETED: successful, FAILED: failed) (Not Null if message object exists)                                     |
| - isAddedChannel      | boolean | O        | Whether channel friend is added (Not Null if message object exists)                                                              |
| - resultCode          | String  | X        | Received result code (in message)                                                                                 |
| - resultCodeName      | String  | X        | Received result code name (in message)                                                                                |
| - resendStatusCode    | String  | X        | Alternative delivery status code                                                                                      |
| - resendStatusName    | String  | X        | Alternative delivery status code name                                                                                     |
| - resendResultCode    | String  | X        | Alternative delivery result code                                                                                      |
| - resendRequestId     | String  | X        | Alternative delivery request ID                                                                                      |
| - createUser          | String  | X        | Registrant (saved as user UUID when sending from console)                                                                      |
| - senderGroupingKey   | String  | X        | Sender grouping key                                                 |
| - recipientGroupingKey | String  | X        | Recipient grouping key                                           |

<a id="message-results"></a>

## Query Message Result Updates

<a id="requested-25"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name   | Type   | Description    |
|--------|--------|----------------|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter]

| Name            | Type    | Required | Description                                                                                                                              |
|-----------------|---------|----------|------------------------------------------------------------------------------------------------------------------------------------------|
| startUpdateDate | String  | O        | Start time of querying result updates (yyyy-MM-dd HH:mm)                                                                                 |
| endUpdateDate   | String  | O        | End time of querying result updates (yyyy-MM-dd HH:mm)                                                                                   |
| targeting       | String  | X        | Type of message target (M: users who agreed to receive marketing, N: users who agreed to receive marketing but are not friends, I: friend users) |
| pageNum         | Integer | X        | Page number (default: 1)                                                                                                                 |
| pageSize        | Integer | X        | Number of results to retrieve (default: 15)                                                                                              |

!!! tip "Note"
    The queryable period is within the last 90 days, and the maximum range per query is 31 days.

<a id="response-25"></a>

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
        "recipientNo": String,
        "targeting": String,
        "requestDate": String,
        "messageStatus": String,
        "isAddedChannel": boolean,
        "resendStatus": String,
        "resendStatusName": String,
        "resultCode": String,
        "resultCodeName": String,
        "senderGroupingKey": String,
        "recipientGroupingKey": String
      }
    ],
    "totalCount": Integer
  }
}
```

| Name                        | Type    | Not Null | Description                                                                                                                              |
|:----------------------------|:--------|:---------|:-----------------------------------------------------------------------------------------------------------------------------------------|
| header                      | Object  | O        | Header area                                                                                                                              |
| - resultCode                | Integer | O        | Result code                                                                                                                              |
| - resultMessage             | String  | O        | Result message                                                                                                                           |
| - isSuccessful              | Boolean | O        | Success                                                                                                                                  |
| messageSearchResultResponse | Object  | X        | Body area                                                                                                                                |
| - messages                  | Array   | X        | Message list                                                                                                                             |
| -- requestId                | String  | O        | Request ID                                                                                                                               |
| -- recipientSeq             | Integer | O        | Recipient sequence number                                                                                                                |
| -- plusFriendId             | String  | O        | Sender Profile ID                                                                                                                        |
| -- recipientNo              | String  | O        | Recipient number                                                                                                                         |
| -- targeting                | String  | O        | Type of message target (M: users who agreed to receive marketing, N: users who agreed to receive marketing but are not friends, I: friend users) |
| -- requestDate              | String  | O        | Request date and time                                                                                                                    |
| -- messageStatus            | String  | O        | Request status (COMPLETED: successful, FAILED: failed, CANCEL: canceled)                                                                 |
| -- isAddedChannel           | Boolean | O        | Whether the channel is a friend                                                                                                          |
| -- resendStatus             | String  | X        | Alternative delivery status code                                                                                                         |
| -- resendStatusName         | String  | X        | Alternative delivery status code name                                                                                                    |
| -- resultCode               | String  | X        | Receipt result code                                                                                                                      |
| -- resultCodeName           | String  | X        | Receipt result code name                                                                                                                 |
| -- senderGroupingKey        | String  | X        | Sender grouping key                                                                                                                      |
| -- recipientGroupingKey     | String  | X        | Recipient grouping key                                                                                                                   |
| - totalCount                | Integer | X        | Total count                                                                                                                              |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appKey}/message-results?startUpdateDate=2026-06-01%2000:00&endUpdateDate=2026-06-30%2023:59"
```

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

| Name        | 	Type     | 	Description     |
|-----------|---------|---------|
| appkey    | 	String | 	Unique app key |
| requestId | String  | Request ID   |

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

| Name           | 	Type     | 	Required | 	Description                                         |
|--------------|---------|-----|---------------------------------------------|
| recipientSeq | 	String | 	X  | Recipient sequence number<br>(to cancel all deliveries of request ID, if the value is left blank) |

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

| Name              | Type      | Not Null | Description     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    X     | Header area  |
| - resultCode    | Integer |    X     | Result code  |
| - resultMessage | String  |    X     | Result message |
| - isSuccessful  | Boolean |    X     | Success |

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

<a id="manage-templates"></a>

## Template Management

<a id="view-template-list"></a>

### List Templates

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
| senderKey | String | Sender Key     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

[Query parameter]

| Name         | Type    | Required | Description                                  |
|--------------|---------|----------|----------------------------------------------|
| templateCode | String  | X        | Template Code                                |
| templateName | String  | X        | Template name                                |
| status       | String  | X        | Template Status code                         |
| pageNum      | Integer | X        | Page number (Default: 1)                     |
| pageSize     | Integer | X        | Number of results (Default: 15, Max: 1,000)  |

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

| Name                    | Type    | Not Null | Description                                          |
|:------------------------|:--------|:---------|:-----------------------------------------------------|
| header                  | Object  | O        | Header area                                          |
| - resultCode            | Integer | O        | Result code                                          |
| - resultMessage         | String  | O        | Result message                                       |
| - isSuccessful          | boolean | O        | Success                                              |
| templateListResponse    | Object  | O        | Body area                                            |
| - templates             | Array   | O        | Template list                                        |
| -- plusFriendId         | String  | O        | Sender Profile ID                                    |
| -- plusFriendType       | String  | O        | Sender Profile type                                  |
| -- templateCode         | String  | O        | Template Code                                        |
| -- templateName         | String  | O        | Template name                                        |
| -- chatBubbleType       | String  | O        | Message type                                         |
| -- content              | String  | X        | Message content                                      |
| -- header               | String  | X        | Header                                               |
| -- additionalContent    | String  | X        | (See description table)                              |
| -- adult                | boolean | O        | Whether the message is for adults only               |
| -- image                | Object  | X        | Image information                                    |
| --- imageUrl            | String  | O        | Image URL                                            |
| --- imageLink           | String  | X        | Image link                                           |
| -- buttons              | Array   | X        | Button list                                          |
| --- name                | String  | O        | Button title                                         |
| --- type                | String  | O        | Button type                                          |
| --- linkMo              | String  | X        | Mobile web link                                      |
| --- linkPc              | String  | X        | PC web link                                          |
| --- schemeIos           | String  | X        | iOS app link                                         |
| --- schemeAndroid       | String  | X        | Android app link                                     |
| --- bizFormId           | Integer | X        | Biz form ID (based on JSON)                          |
| -- item                 | Object  | X        | Wide list element                                    |
| --- list                | Array   | X        | Wide list                                            |
| ---- title              | String  | X        | Item title                                           |
| ---- imageUrl           | String  | O        | Item image URL                                       |
| ---- linkMo             | String  | O        | Mobile web link                                      |
| ---- linkPc             | String  | X        | PC web link                                          |
| ---- schemeAndroid      | String  | X        | Android app link                                     |
| ---- schemeIos          | String  | X        | iOS app link                                         |
| -- coupon               | Object  | X        | Coupon element                                       |
| --- title               | String  | O        | Coupon title                                         |
| --- description         | String  | O        | Coupon detailed description                          |
| --- linkMo              | String  | X        | Mobile web link                                      |
| --- linkPc              | String  | X        | PC web link                                          |
| --- schemeAndroid       | String  | X        | Android app link                                     |
| --- schemeIos           | String  | X        | iOS app link                                         |
| -- commerce             | Object  | X        | Commerce element                                     |
| --- title               | String  | O        | Product title                                        |
| --- regularPrice        | Integer | X        | Regular price                                        |
| --- discountPrice       | Integer | X        | Discounted price                                     |
| --- discountRate        | Integer | X        | Discount rate                                        |
| --- discountFixed       | Integer | X        | Fixed discount price                                 |
| -- video                | Object  | X        | Video element                                        |
| --- videoUrl            | String  | O        | KakaoTV video URL                                    |
| --- thumbnailUrl        | String  | X        | Image URL for video thumbnail                        |
| -- carousel             | Object  | X        | Carousel                                             |
| --- head                | Object  | X        | Carousel intro                                       |
| ---- header             | String  | O        | Carousel intro header                                |
| ---- content            | String  | O        | Carousel intro content                               |
| ---- imageUrl           | String  | O        | Carousel intro image URL                             |
| ---- linkMo             | String  | X        | Mobile web link                                      |
| ---- linkPc             | String  | X        | PC web link                                          |
| ----- schemeAndroid     | String  | X        | Android app link                                     |
| ----- schemeIos         | String  | X        | iOS app link                                         |
| ---- list               | Array   | O        | Carousel list                                        |
| ----- header            | String  | O        | Carousel item header                                 |
| ----- message           | String  | O        | Carousel item message (maps to content in Not Null list) |
| ----- additionalContent | String  | X        | Additional information                               |
| ----- imageUrl          | String  | O        | Image URL (within carousel item)                     |
| ----- imageLink         | String  | X        | Image link (within carousel item)                    |
| ----- commerce          | Object  | O        | Commerce (within carousel item)                      |
| ------ title            | String  | O        | Product title                                        |
| ------ regularPrice     | Integer | X        | Regular price                                        |
| ------ discountPrice    | Integer | X        | Discounted price                                     |
| ------ discountRate     | Integer | X        | Discount rate                                        |
| ------ discountFixed    | Integer | X        | Fixed discount price                                 |
| ----- buttons           | Array   | O        | Carousel list button list                            |
| ------ name             | String  | O        | Button title                                         |
| ------ type             | String  | O        | Button type                                          |
| ------ linkMo           | String  | X        | Mobile web link                                      |
| ------ linkPc           | String  | X        | PC web link                                          |
| ------ schemeIos        | String  | X        | iOS app link                                         |
| ------ schemeAndroid    | String  | X        | Android app link                                     |
| ------ bizFormId        | Integer | X        | Biz form ID (based on JSON)                          |
| ----- coupon            | Object  | X        | Coupon element (within carousel item)                |
| ------ title            | String  | X        | Coupon title                                         |
| ------ description      | String  | X        | Coupon detailed description                          |
| ------ linkMo           | String  | X        | Mobile web link                                      |
| ------ linkPc           | String  | X        | PC web link                                          |
| ------ schemeAndroid    | String  | X        | Android app link                                     |
| ------ schemeIos        | String  | X        | iOS app link                                         |
| ---- tail               | Object  | X        | View More button information                         |
| ----- linkMo            | String  | O        | Mobile web link                                      |
| ----- linkPc            | String  | X        | PC web link                                          |
| ----- schemeAndroid     | String  | X        | Android app link                                     |
| ----- schemeIos         | String  | X        | iOS app link                                         |
| --- status              | String  | O        | Template status (A: registered, S: blocked)          |
| --- createDate          | String  | O        | Registration date and time                           |
| --- updateDate          | String  | X        | Modification date and time                           |
| - totalCount            | Integer | O        | Total count                                          |

<a id="view-single-template"></a>

### Get a Template

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description    |
|--------------|--------|----------------|
| appkey       | String | Unique app key |
| senderKey    | String | Sender Key     |
| templateCode | String | Template Code  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name                        | Type   | Required | Description                                                                                                                              |
|-----------------------------|--------|----------|------------------------------------------------------------------------------------------------------------------------------------------|
| X-Secret-Key                | String | O        | Can be created in the console.                                                                                                           |
| X-NC-API-IDEMPOTENCY-KEY    | String | X        | Key used as the reference for duplicate message sending requests.<br>If a request is made with the same key for 10 minutes, the request will be failed. |

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

| Name                  | Type    | Not Null | Description                              |
|:----------------------|:--------|:---------|:-----------------------------------------|
| header                | Object  | O        | Header area                              |
| - resultCode          | Integer | O        | Result code                              |
| - resultMessage       | String  | O        | Result message                           |
| - isSuccessful        | boolean | O        | Success                                  |
| template              | Object  | O        | Template body area                       |
| - plusFriendId        | String  | O        | Sender Profile ID                        |
| - plusFriendType      | String  | O        | Sender Profile type                      |
| - templateCode        | String  | O        | Template Code                            |
| - templateName        | String  | O        | Template name                            |
| - chatBubbleType      | String  | O        | Message Type                             |
| - pushAlarm           | boolean | X        | Whether push alarm is enabled            |
| - content             | String  | X        | Message content                          |
| - adult               | boolean | O        | Whether the message is for adults        |
| - header              | String  | X        | Header (within template)                 |
| - additionalContent   | String  | X        | Additional information (within template) |
| - image               | Object  | X        | Image information                        |
| -- imageUrl           | String  | O        | Image URL                                |
| -- imageLink          | String  | X        | Image link                               |
| - buttons             | Array   | X        | Button list                              |
| -- name               | String  | O        | Button title                             |
| -- type               | String  | O        | Button type                              |
| -- linkMo             | String  | X        | Mobile web link                          |
| -- linkPc             | String  | X        | PC web link                              |
| -- schemeIos          | String  | X        | iOS app link                             |
| -- schemeAndroid      | String  | X        | Android app link                         |
| -- bizFormId          | Integer | X        | Biz form ID (based on JSON)              |
| - item                | Object  | X        | Wide list element                        |
| -- list               | Array   | X        | Wide list                                |
| --- title             | String  | X        | Item title                               |
| --- imageUrl          | String  | O        | Item image URL                           |
| --- linkMo            | String  | O        | Mobile web link                          |
| --- linkPc            | String  | X        | PC web link                              |
| --- schemeIos         | String  | X        | iOS app link                             |
| --- schemeAndroid     | String  | X        | Android app link                         |
| - coupon              | Object  | X        | Coupon element                           |
| -- title              | String  | O        | Coupon title                             |
| -- description        | String  | O        | Coupon detail description                |
| -- linkMo             | String  | X        | Mobile web link                          |
| -- linkPc             | String  | X        | PC web link                              |
| -- schemeIos          | String  | X        | iOS app link                             |
| -- schemeAndroid      | String  | X        | Android app link                         |
| - commerce            | Object  | X        | Commerce element                         |
| -- title              | String  | O        | Product title                            |
| -- regularPrice       | Integer | X        | Regular price                            |
| -- discountPrice      | Integer | X        | Discount price                           |
| -- discountRate       | Integer | X        | Discount rate                            |
| -- discountFixed      | Integer | X        | Fixed discount price                     |
| - video               | Object  | X        | Video element                            |
| -- videoUrl           | String  | O        | KakaoTV video URL                        |
| -- thumbnailUrl       | String  | X        | Image URL for video thumbnail            |
| - carousel            | Object  | X        | Carousel                                 |
| -- head               | Object  | X        | Carousel intro                           |
| --- header            | String  | O        | Carousel intro header                    |
| --- content           | String  | O        | Carousel intro content                   |
| --- imageUrl          | String  | O        | Carousel intro image URL                 |
| --- linkMo            | String  | X        | Mobile web link                          |
| --- linkPc            | String  | X        | PC web link                              |
| --- schemeIos         | String  | X        | iOS app link                             |
| --- schemeAndroid     | String  | X        | Android app link                         |
| -- list               | Array   | O        | Carousel list                            |
| --- header            | String  | O        | Carousel item header                     |
| --- message           | String  | O        | Carousel item message                    |
| --- additionalContent | String  | X        | Additional information (within carousel item) |
| --- imageUrl          | String  | O        | Image URL (within carousel item)         |
| --- imageLink         | String  | X        | Image link (within carousel item)        |
| --- commerce          | Object  | O        | Commerce (within carousel item)          |
| ---- title            | String  | O        | Product title                            |
| ---- regularPrice     | Integer | X        | Regular price                            |
| ---- discountPrice    | Integer | X        | Discount price                           |
| ---- discountRate     | Integer | X        | Discount rate                            |
| ---- discountFixed    | Integer | X        | Fixed discount price                     |
| --- buttons           | Array   | O        | Carousel list button list                |
| ---- name             | String  | O        | Button title                             |
| ---- type             | String  | O        | Button type                              |
| ---- linkMo           | String  | X        | Mobile web link                          |
| ---- linkPc           | String  | X        | PC web link                              |
| ---- schemeIos        | String  | X        | iOS app link                             |
| ---- schemeAndroid    | String  | X        | Android app link                         |
| ---- bizFormId        | Integer | X        | Biz form ID (based on JSON)              |
| --- coupon            | Object  | X        | Coupon element (within carousel item)    |
| ---- title            | String  | X        | Coupon title                             |
| ---- description      | String  | X        | Coupon detail description                |
| ---- linkMo           | String  | X        | Mobile web link                          |
| ---- linkPc           | String  | X        | PC web link                              |
| ---- schemeIos        | String  | X        | iOS app link                             |
| ---- schemeAndroid    | String  | X        | Android app link                         |
| -- tail               | Object  | X        | View More button information             |
| --- linkMo            | String  | O        | Mobile web link                          |
| --- linkPc            | String  | X        | PC web link                              |
| --- schemeIos         | String  | X        | iOS app link                             |
| --- schemeAndroid     | String  | X        | Android app link                         |
| - status              | String  | O        | Template Status (A: registered, S: blocked) |
| - createDate          | String  | O        | Registration date and time               |
| - updateDate          | String  | X        | Modification date and time               |

<a id="register-template"></a>

### Template Registration

<a id="requested-10"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path Parameter]

| Name      | Type   | Description        |
|-----------|--------|--------------------|
| appkey    | String | Unique app key     |
| senderKey | String | Sender key         |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console. |

<a id="note"></a>

#### Notes

* When applying replacement variables to a coupon title, you must use the following fixed replacement variables.

```
- #{Discount amount} KRW off coupon (#{Discount amount} range: 1 to 99,999,999)
- #{Discount rate}% off coupon (#{Discount rate} range: 1 to 100)
- Shipping discount coupon
- #{Product name} Free coupon (#{Product name} is up to 7 characters)
- #{Product name} UP coupon (#{Product name} is up to 7 characters)
```

* In Commerce, users cannot specify replacement variables for the regularPrice, discountPrice, discountRate, and discountFixed fields, excluding the product title.
    * If you leave all fields other than the product title empty, they are automatically populated with fixed replacement variables and saved.
    * regularPrice -> #{Regular price}
    * discountPrice -> #{Discounted price}
    * discountRate -> #{Discount rate}
    * discountFixed -> #{Discount fixed amount}

<a id="request-to-register-text-type-template"></a>

#### Register Text Type Template Request

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

| Name            | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | Template name (up to 200 characters)                                                                                                                                                                                                                                                                                                                        |
| chatBubbleType  | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                                                                                                 |
| adult           | boolean | X  | Whether the message is for adults (default: false)                                                                                                                                                                                                                                                                                                          |
| content         | String  | O  | - For the TEXT type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| buttons         | List    | X  | List of buttons<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, up to 5 buttons otherwise<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons                                                               |
| - name          | String  | O  | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters<br>Replacement variables cannot be used                                                                                                                                                                                                    |
| - type          | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE types, and as the last button for all other message types                                                                                   |
| - linkMo        | String  | X  | Mobile web link (required for the WL type), for up to 500 characters                                                                                                                                                                                                                                                                                       |
| - linkPc        | String  | X  | PC web link (optional for the WL type), for up to 500 characters                                                                                                                                                                                                                                                                                           |
| - schemeAndroid | String  | X  | Android app link (required for the AL type), for up to 500 characters                                                                                                                                                                                                                                                                                      |
| - schemeIos     | String  | X  | iOS app link (required for the AL type), for up to 500 characters                                                                                                                                                                                                                                                                                          |
| - bizFormKey    | String  | X  | Business form key for the BF type button<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                            |
| coupon          | Object  | X  | Coupon element                                                                                                                                                                                                                                                                                                                                              |
| - title         | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description   | String  | O  | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters, no line breaks allowed<br>- For all other types: up to 12 characters, no line breaks allowed                                                                                                                                                   |
| - linkMo        | String  | X  | Mobile web link (required for the WL type), for up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                               |
| - linkPc        | String  | X  | PC web link (optional for the WL type), for up to 500 characters                                                                                                                                                                                                                                                                                           |
| - schemeAndroid | String  | X  | Android app link (required for the AL type), for up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                              |
| - schemeIos     | String  | X  | iOS app link (required for the AL type), for up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                  |

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

| Name            | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                    |
|-----------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                                                                                                                                                                                           |
| chatBubbleType  | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                                                                                                                                                    |
| adult           | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                                                                                                                                                                                             |
| content         | String  | O        | - For the TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| image           | Object  | O        | Image element<br>- Required for the IMAGE, WIDE, and COMMERCE types                                                                                                                                                                                                                                                                                                                                            |
| - imageUrl      | String  | O        | Image URL; use the URL of an image uploaded as a general image<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                                                         |
| - imageLink     | String  | X        | URL to navigate to when the image is clicked; limited to 500 characters<br>If not set, the KakaoTalk in-app image viewer is used<br>Replacement variables cannot be used                                                                                                                                                                                                                                       |
| buttons         | List    | X        | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons                                                                                                                               |
| - name          | String  | O        | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                       |
| - type          | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for the TEXT and IMAGE types, and as the last button for all other message types                                                                                                                                  |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                         |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                             |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                        |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                            |
| - bizFormKey    | String  | X        | Business form key for the BF type button                                                                                                                                                                                                                                                                                                                                                                       |
| coupon          | Object  | X        | Coupon element                                                                                                                                                                                                                                                                                                                                                                                                 |
| - title         | String  | O        | For title, you are limited to five formats:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                                |
| - description   | String  | O        | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters, no line breaks allowed<br>- For all other types: up to 12 characters, no line breaks allowed                                                                                                                                                                                                      |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                                |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                             |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                               |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                                   |

<a id="request-to-register-wide-image-type-template"></a>

#### Request to Register Wide Image Type Template

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

| Name            | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------------|---------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O        | Template name (up to 200 characters)                                                                                                                                                                                                                                                                                                                                                                 |
| chatBubbleType  | String  | O        | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                                                                                                                                          |
| adult           | boolean | X        | Whether the message is for adults (default: false)                                                                                                                                                                                                                                                                                                                                                   |
| content         | String  | O        | - For the TEXT type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format input allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| image           | Object  | O        | Image element<br>- Required field for the IMAGE, WIDE, and COMMERCE types                                                                                                                                                                                                                                                                                                                            |
| - imageUrl      | String  | O        | Image URL; use the image URL uploaded as a wide image<br>Replacement variables are not allowed                                                                                                                                                                                                                                                                                                       |
| - imageLink     | String  | X        | URL to navigate to when the image is clicked; limited to 500 characters<br>If not set, the KakaoTalk in-app image viewer is used<br>Replacement variables are not allowed                                                                                                                                                                                                                            |
| buttons         | List    | X        | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: at least 1 and up to 2 buttons                                                                                                                    |
| - name          | String  | O        | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters<br>Replacement variables are not allowed                                                                                                                                                                                                                                            |
| - type          | String  | O        | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE types, and as the last button for all other message types                                                                                                                           |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                               |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                   |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                               |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                   |
| - bizFormKey    | String  | X        | Business form key for the BF type button                                                                                                                                                                                                                                                                                                                                                             |
| coupon          | Object  | X        | Coupon element                                                                                                                                                                                                                                                                                                                                                                                       |
| - title         | String  | O        | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon"                                        |
| - description   | String  | O        | Detailed coupon description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters, line breaks not allowed<br>- For all other types: up to 12 characters, line breaks not allowed                                                                                                                                                                                          |
| - linkMo        | String  | X        | Mobile web link (required for the WL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                     |
| - linkPc        | String  | X        | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                                                                   |
| - schemeAndroid | String  | X        | Android app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                    |
| - schemeIos     | String  | X        | iOS app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                                                        |

<a id="request-to-register-wide-item-list-type-template"></a>

#### Register Wide Item List Type Template Request

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

| Name | Type | Required | Description |
|------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName | String | O | Template name (up to 200 characters) |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| adult | boolean | X | Whether the message is for adults (default: false) |
| header | String | O | Header<br>- Required for the WIDE_ITEM_LIST type, up to 20 characters (line breaks: not allowed)<br>- Optional for the PREMIUM_VIDEO type, up to 20 characters (line breaks: not allowed) |
| item | Object | O | Wide list element (available only for the WIDE_ITEM_LIST type) |
| - list | List | O | Wide list (minimum: 3, maximum: 4) |
| -- title | String | O | Item title<br>- Up to 25 characters for the first item (line breaks: up to 1; title is not required for the first item)<br>- title is not a required field for the first item<br>- Up to 30 characters for items 2–4 (line breaks: up to 1) |
| -- imageUrl | String | O | Item image URL<br>- For the first item, use the image URL uploaded as the first wide item list image<br>- For items 2–4, use the image URL uploaded as a general wide item list image<br>Replacement variables cannot be used |
| -- linkMo | String | O | Mobile web link, up to 500 characters |
| -- linkPc | String | X | PC web link, up to 500 characters |
| -- schemeAndroid | String | X | Android app link, up to 500 characters |
| -- schemeIos | String | X | iOS app link, up to 500 characters |
| buttons | List | X | Button list<br>- Up to 4 buttons when a coupon is applied for the TEXT or IMAGE type; otherwise up to 5<br>- Up to 2 buttons for the WIDE or WIDE_ITEM_LIST type<br>- Up to 1 button for the PREMIUM_VIDEO type<br>- At least 1 and up to 2 buttons for the COMMERCE type |
| - name | String | O | Button title<br>- Up to 14 characters for the TEXT or IMAGE type<br>- Up to 8 characters for all other types<br>Replacement variables cannot be used |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for the TEXT and IMAGE types, and as the last button for all other message types |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters |
| - linkPc | String | X | PC web link (optional for the WL type), up to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), up to 500 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), up to 500 characters |
| - bizFormKey | String | X | Business form key for the BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- Up to 18 characters for the WIDE, WIDE_ITEM_LIST, or PREMIUM_VIDEO type (line breaks: not allowed)<br>- Up to 12 characters for all other types (line breaks: not allowed) |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc | String | X | PC web link (optional for the WL type), up to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos | String | X | iOS app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-premium-video-type-template"></a>

#### Register Premium Video Type Template

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

| Name | Type | Required | Description |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName | String | O | Template name (up to 200 characters) |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| adult | boolean | X | Whether the message is for adults (default: false) |
| content | String | X | - For the TEXT type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the IMAGE type: up to 1,300 characters (line breaks: up to 99, URL format allowed)<br>- For the WIDE type: up to 76 characters (line breaks: up to 5)<br>- For the PREMIUM_VIDEO type: this field is optional, up to 76 characters (line breaks: up to 5)<br>- For all other types: this field is not used |
| header | String | X | Header<br>- For the WIDE_ITEM_LIST type: required field, up to 20 characters (line breaks: not allowed)<br>- For the PREMIUM_VIDEO type: optional field, up to 20 characters (line breaks: not allowed) |
| video | Object | O | Video element (available for the PREMIUM_VIDEO type only) |
| - videoUrl | String | O | KakaoTV video URL (only URLs of videos uploaded to KakaoTV are allowed), up to 500 characters<br>Replacement variables cannot be used |
| - thumbnailUrl | String | X | Image URL for the video thumbnail. Only URLs uploaded as general images are allowed (if not provided, the default KakaoTV video thumbnail is used), up to 500 characters<br>Replacement variables cannot be used |
| buttons | List | X | Button list<br>- For the TEXT and IMAGE types: up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE and WIDE_ITEM_LIST types: up to 2 buttons<br>- For the PREMIUM_VIDEO type: up to 1 button<br>- For the COMMERCE type: minimum 1 button, maximum 2 buttons |
| - name | String | O | Button title<br>- For the TEXT and IMAGE types: up to 14 characters<br>- For all other types: up to 8 characters<br>Replacement variables cannot be used |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE types, and as the last button for all other message types |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters |
| - linkPc | String | X | PC web link (optional for the WL type), up to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), up to 1,000 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), up to 500 characters |
| - bizFormKey | String | X | Biz form key for the BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types: up to 18 characters, line breaks not allowed<br>- For all other types: up to 12 characters, line breaks not allowed |
| - linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc | String | X | PC web link (optional for the WL type), up to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos | String | X | iOS app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-commerce-type-template"></a>

#### Register Commerce Type Template Request

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

| Name | Type | Required | Description |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName | String | O | Template name (up to 200 characters) |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| adult | boolean | X | Whether the message is for adults (default: false) |
| additionalContent | String | X | Additional information (up to 34 characters, line breaks: up to 1), available for the Commerce type only |
| image | Object | O | Image element<br>- Required for the IMAGE, WIDE, or COMMERCE type |
| - imageUrl | String | O | Image URL; use the URL of an image uploaded as a general image<br>Replacement variables cannot be used |
| - imageLink | String | X | URL to navigate to when the image is clicked, limited to 500 characters<br>If not set, the KakaoTalk in-app image viewer is used<br>Replacement variables cannot be used |
| commerce | Object | O | Commerce (available for the COMMERCE type only) |
| title | String | O | Product title (up to 30 characters, line breaks: not allowed) |
| regularPrice | Integer | O | Regular price (0–99,999,999)<br>Replacement variable customization is not available; if left empty, it is saved as the fixed replacement variable `#{정상가격}` |
| discountPrice | Integer | X | Discount price (0–99,999,999)<br>Replacement variable customization is not available; if left empty, it is saved as the fixed replacement variable `#{할인가격}` |
| discountRate | Integer | X | Discount rate (0–100); when a discount price exists, either the discount rate or the fixed discount price is required<br>Replacement variable customization is not available; if left empty, it is saved as the fixed replacement variable `#{할인율}` |
| discountFixed | Integer | X | Fixed discount price (0–999,999); when a discount price exists, either the discount rate or the fixed discount price is required<br>Replacement variable customization is not available; if left empty, it is saved as the fixed replacement variable `#{정액할인가격}` |
| buttons | List | O | Button list<br>- For the TEXT or IMAGE type, up to 4 buttons when a coupon is applied, otherwise up to 5<br>- For the WIDE or WIDE_ITEM_LIST type, up to 2 buttons<br>- For the PREMIUM_VIDEO type, up to 1 button<br>- For the COMMERCE type, at least 1 and up to 2 buttons |
| - name | String | O | Button title<br>- For the TEXT or IMAGE type, up to 14 characters<br>- For other types, up to 8 characters<br>Replacement variables cannot be used |
| - type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE, and as the last button for all other message types |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 500 characters |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 500 characters |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 500 characters |
| - bizFormKey | String | X | Business form key for the BF type button |
| coupon | Object | X | Coupon element |
| - title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| - description | String | O | Coupon detailed description<br>- For the WIDE, WIDE_ITEM_LIST, or PREMIUM_VIDEO type, up to 18 characters, line breaks: not allowed<br>- For other types, up to 12 characters, line breaks: not allowed |
| - linkMo | String | X | Mobile web link (required for the WL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional;<br>if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - linkPc | String | X | PC web link (optional for the WL type), limited to 500 characters |
| - schemeAndroid | String | X | Android app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional;<br>if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| - schemeIos | String | X | iOS app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional;<br>if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |

<a id="request-to-register-carousel-feed-type-template"></a>

#### Register Carousel Feed Template

[Request Body]

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

| Name                | Type      | Required | Description                                                                                                                                                                                                                                                                                                                                    |
|-------------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | Template name (up to 200 characters)                                                                                                                                                                                                                                                                                                                           |
| chatBubbleType    | String  | O  | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE)                                                                                                                                                                                                                                                    |
| pushAlarm         | boolean | X  | Whether to send push alarm for message (default: true)                                                                                                                                                                                                                                                                                                         |
| adult             | boolean | X  | Whether the message is for adults only (default: false)                                                                                                                                                                                                                                                                                                        |
| carousel          | Object  | O  | Carousel                                                                                                                                                                                                                                                                                                                                                       |
| - list            | List    | O  | Carousel list (minimum 2, maximum 6)                                                                                                                                                                                                                                                                                                                           |
| -- header         | String  | O  | Carousel item title (up to 20 characters), only available in carousel feeds                                                                                                                                                                                                                                                                                    |
| -- message        | String  | O  | Carousel item title (up to 20 characters), carousel item message (up to 180 characters), only available in carousel feeds                                                                                                                                                                                                                                      |
| -- imageUrl       | String  | O  | Image URL (only images uploaded as carousel feed images can be used)<br>Replacement variables cannot be used                                                                                                                                                                                                                                                   |
| -- imageLink      | String  | X  | Image link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                                  |
| -- buttons        | List    | O  | Carousel list button list, minimum 1, maximum 2                                                                                                                                                                                                                                                                                                                |
| --- name          | String  | O  | Button title<br>- Up to 14 characters for TEXT and IMAGE types<br>- Up to 8 characters for other types<br>Replacement variables cannot be used                                                                                                                                                                                                                 |
| --- type          | String  | O  | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Add Channel, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE types, and as the last button for other message types |
| --- linkMo        | String  | X  | Mobile web link (required for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                         |
| --- linkPc        | String  | X  | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                             |
| --- schemeAndroid | String  | X  | Android app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                        |
| --- schemeIos     | String  | X  | iOS app link (required for the AL type), limited to 500 characters                                                                                                                                                                                                                                                                                            |
| --- bizFormKey    | String  | X  | Business form key for BF type buttons                                                                                                                                                                                                                                                                                                                          |
| -- coupon         | Object  | X  | Coupon element                                                                                                                                                                                                                                                                                                                                                 |
| --- title         | String  | O  | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| --- description   | String  | O  | Coupon detailed description<br>- Up to 18 characters for WIDE, WIDE_ITEM_LIST, and PREMIUM_VIDEO types; line breaks not allowed<br>- Up to 12 characters for other types; line breaks not allowed                                                                                                                                                              |
| --- linkMo        | String  | X  | Mobile web link (required for the WL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                               |
| --- linkPc        | String  | X  | PC web link (optional for the WL type), limited to 500 characters                                                                                                                                                                                                                                                                                             |
| --- schemeAndroid | String  | X  | Android app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                              |
| --- schemeIos     | String  | X  | iOS app link (required for the AL type), limited to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                                  |
| - tail            | Object  | X  | View More button information                                                                                                                                                                                                                                                                                                                                   |
| -- linkMo         | String  | O  | Mobile web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                            |
| -- linkPc         | String  | X  | PC web link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                                |
| -- schemeAndroid  | String  | X  | Android app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                           |
| -- schemeIos      | String  | X  | iOS app link, limited to 500 characters<br>Replacement variables cannot be used                                                                                                                                                                                                                                                                               |
| recipientList     | List    | O  | Recipient list (up to 1,000)                                                                                                                                                                                                                                                                                                                                   |
| - recipientNo     | String  | O  | Recipient number                                                                                                                                                                                                                                                                                                                                               |
| createUser        | String  | X  | Registrant (saved as user UUID when sending from the console)                                                                                                                                                                                                                                                                                                  |

<a id="request-to-register-carousel-commerce-type-template"></a>

#### Register Template Request for Carousel Commerce Type

[Request Body]

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

| Name | Type | Required | Description |
|----------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName | String | O | Template name (maximum 200 characters) |
| chatBubbleType | String | O | Message type (TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE) |
| pushAlarm | boolean | X | Whether to send a push notification for the message (default: true) |
| adult | boolean | X | Whether the message is for adults only (default: false) |
| carousel | Object | O | Carousel |
| - head | Object | X | Carousel intro |
| -- header | String | O | Carousel intro header (maximum 20 characters) |
| -- content | String | O | Carousel intro content (maximum 50 characters) |
| -- imageUrl | String | O | Carousel intro image URL (use an image uploaded as a Carousel Commerce type image; the image must have the same aspect ratio as the carousel images)<br>Replacement variables cannot be used |
| -- linkMo | String | X | Mobile web link (required if you intend to use any of linkMo, linkPc, schemeAndroid, or schemeIos), up to 500 characters |
| -- linkPc | String | X | PC web link, up to 500 characters |
| -- schemeAndroid | String | X | Android app link, up to 500 characters |
| -- schemeIos | String | X | iOS app link, up to 500 characters |
| - list | List | O | Carousel list (minimum 1, maximum 5 items if head exists; otherwise minimum 2, maximum 6 items) |
| -- additionalContent | String | X | Additional information (maximum 34 characters); available only for the Carousel Commerce type |
| -- imageUrl | String | O | Image URL (use an image uploaded as a Carousel Commerce type image)<br>Replacement variables cannot be used |
| -- imageLink | String | X | Image link, up to 500 characters<br>Replacement variables cannot be used |
| -- commerce | Object | O | Commerce (available only for the CAROUSEL_COMMERCE type) |
| --- title | String | O | Product title (maximum 30 characters; line breaks not allowed) |
| --- regularPrice | Integer | O | Regular price (0–99,999,999)<br>Custom replacement variables cannot be used; if left empty, the fixed replacement variable `#{정상가격}` is used |
| --- discountPrice | Integer | X | Discount price (0–99,999,999)<br>Custom replacement variables cannot be used; if left empty, the fixed replacement variable `#{할인가격}` is used |
| --- discountRate | Integer | X | Discount rate (0–100); when a discount price exists, either discount rate or fixed discount price is required<br>Custom replacement variables cannot be used; if left empty, the fixed replacement variable `#{할인율}` is used |
| --- discountFixed | Integer | X | Fixed discount price (0–999,999); when a discount price exists, either discount rate or fixed discount price is required<br>Custom replacement variables cannot be used; if left empty, the fixed replacement variable `#{정액할인가격}` is used |
| -- buttons | List | O | List of carousel buttons (minimum 1, maximum 2) |
| --- name | String | O | Button title<br>- Maximum 14 characters for the TEXT or IMAGE type<br>- Maximum 8 characters for all other types<br>Replacement variables cannot be used |
| --- type | String | O | Button type (WL: Web Link, AL: App Link, BK: Bot Keyword, MD: Message Delivery, AC: Channel Added, BT: Bot Transfer, BF: Business Form)<br>- The AC type must be registered as the first button for TEXT and IMAGE types, and as the last button for all other message types |
| --- linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters |
| --- linkPc | String | X | PC web link (optional for the WL type), up to 500 characters |
| --- schemeAndroid | String | X | Android app link (required for the AL type), up to 500 characters |
| --- schemeIos | String | X | iOS app link (required for the AL type), up to 500 characters |
| --- bizFormKey | String | X | Business form key for the BF type button |
| -- coupon | Object | X | Coupon element |
| --- title | String | O | For title, you are limited to five types:<br>- "${number}KRW off coupon" with a number greater than or equal to 1 and less than or equal to 99,999,999<br>- "${number}% off coupon" with a number greater than or equal to 1 and less than or equal to 100<br>- "Shipping discount coupon"<br>- "${7 characters} Free coupon"<br>- "${7 characters} UP coupon" |
| --- description | String | O | Coupon detailed description<br>- Maximum 18 characters for the WIDE, WIDE_ITEM_LIST, or PREMIUM_VIDEO type; line breaks not allowed<br>- Maximum 12 characters for all other types; line breaks not allowed |
| --- linkMo | String | X | Mobile web link (required for the WL type), up to 500 characters<br>If the linkMo field is entered for the coupon, the remaining fields become optional;<br>if a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional. |
| --- linkPc           | String  | X  | PC web link (optional for the WL type), up to 500 characters                                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | Android app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                     |
| --- schemeIos        | String  | X  | iOS app link (required for the AL type), up to 500 characters<br>If the linkMo field is entered for a coupon, the remaining fields become optional.<br>If a channel coupon URL (format: alimtalk=coupon://) is entered in the scheme_android or scheme_ios field, the remaining fields become optional.                                       |
| - tail               | Object  | X  | More button information                                                                                                                                                                                                                         |
| -- linkMo            | String  | O  | Mobile web link, up to 500 characters<br>Replacement variables cannot be used.                                                                                                                                                                                                 |
| -- linkPc            | String  | X  | PC web link, up to 500 characters<br>Replacement variables cannot be used.                                                                                                                                                                                                  |
| -- schemeAndroid     | String  | X  | Android app link, up to 500 characters<br>Replacement variables cannot be used.                                                                                                                                                                                               |
| -- schemeIos         | String  | X  | iOS app link, up to 500 characters<br>Replacement variables cannot be used.                                                                                                                                                                                                 |

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

| Name            | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:----------------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |
| template        | Object  | X        | Template information |
| - templateCode  | String  | O        | Template Code   |

<a id="modify-template"></a>

### Modify Template

<a id="requested-11"></a>

#### Request

[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description      |
|--------------|--------|------------------|
| appkey       | String | Unique app key   |
| senderKey    | String | Sender Key       |
| templateCode | String | Template Code    |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                          |
|--------------|--------|----------|--------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.       |

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

### Delete Template

<a id="requested-12"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name         | Type   | Description       |
|--------------|--------|-------------------|
| appkey       | String | Unique app key    |
| senderKey    | String | Sender Key        |
| templateCode | String | Template Code     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
| X-Secret-Key | String | O        | Can be created in the console.     |

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

| Name            | Type    | Not Null | Description    |
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |

<a id="manage-image"></a>

## Image Management

<a id="upload-image"></a>

### Upload Image

<a id="requested-13"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| Name | Type | Description |
|--------|--------|--------|
| appkey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Request parameter]

| Name | Type | Required | Description |
|-----------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| image     | File   | O  | Image |
| imageType | String | O  | Image type <br>(IMAGE, WIDE_IMAGE, MAIN_WIDE_ITEMLIST_IMAGE, NORMAL_WIDE_ITEMLIST_IMAGE, CAROUSEL_FEED_IMAGE, CAROUSEL_COMMERCE_IMAGE) |

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

| Name | Type | Not Null | Description |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |
| image           | Object  | X        | Image area |
| - imageSeq      | Integer | O        | Image sequence |
| - imageUrl      | String  | O        | Image URL |
| - imageName     | String  | X        | Image name |

<a id="upload-image-specifications"></a>

#### Upload Image Specifications

| Image Type | Usage | Upload Image Specifications |
|:---------------------------|:--------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------|
| IMAGE                      | Image type image, Commerce type image, Premium video type thumbnail | Recommended size: 800 x 400 px (width 500 px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and size limit: jpg, png / up to 5 MB |
| WIDE_IMAGE                 | Wide Image type image | Recommended size: 800 x 600 px (width 500 px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1<br>File format and size limit: jpg, png / up to 5 MB |
| MAIN_WIDE_ITEMLIST_IMAGE   | First item image of Wide Item List type | Size limit: width 500 px or more<br/>Image ratio: height ÷ width = 0.5<br/>File format and size limit: jpg, png / up to 5 MB |
| NORMAL_WIDE_ITEMLIST_IMAGE | 2nd–4th item images of Wide Item List type | Size limit: width 500 px or more<br/>Image ratio: height ÷ width = 1<br/>File format and size: jpg, png / up to 5 MB per file |
| CAROUSEL_FEED_IMAGE        | Per-cell image of Carousel Feed type | Recommended size: 800 x 600 px or 800 x 400 px (width 500 px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and size limit: jpg, png / up to 5 MB |
| CAROUSEL_COMMERCE_IMAGE    | Intro image of Carousel Commerce type, per-cell image of Carousel Commerce type | Recommended size: 800 x 600 px or 800 x 400 px (width 500 px or more)<br/>Image ratio: 0.5 ≤ height ÷ width ≤ 1.333<br/>File format and size limit: jpg, png / up to 5 MB |

* If all templates that reference an uploaded image are deleted or changed to a different image, the image is deleted from the Kakao CDN and the URL becomes invalid. Although image information is retained in the image retrieval API, the actual image cannot be accessed, so it is recommended to keep the original file separately on your own server.

<a id="view-image"></a>

### View Image

<a id="requested-14"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name | Type | Description |
|--------|--------|--------|
| appkey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Request parameter]

| Name | Type | Required | Description |
|------------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| imageTypes | List   | O  | Image type <br>(IMAGE, WIDE_IMAGE, MAIN_WIDE_ITEMLIST_IMAGE, NORMAL_WIDE_ITEMLIST_IMAGE, CAROUSEL_FEED_IMAGE, CAROUSEL_COMMERCE_IMAGE) |
| pageNum    | String | X  | Page number (default: 1) |
| pageSize   | String | X  | Number of results (default: 15) |

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

| Name | Type | Not Null | Description |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |
| image           | Object  | X        | Image area |
| - imageSeq      | Integer | O        | Image sequence |
| - imageUrl      | String  | O        | Image URL |
| - imageName     | String  | X        | Image name |

<a id="delete-image"></a>

### Delete Image

<a id="requested-15"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name | Type | Description |
|--------|--------|--------|
| appkey | String | Unique appkey |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Query parameter]

| Name | Type | Required | Description |
|----------|--------|----|--------|
| imageSeq | String | O  | Image number |

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

| Name | Type | Not Null | Description |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | Header area |
| - resultCode    | Integer | O        | Result code |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success |

<a id="manage-video"></a>

## Video Management

This API is used to register, view, and delete videos for use in Brand Message. Registered videos can be used for sending after encoding by the Kakao Biz Center, and only videos with a status of `PUBLIC` can be used for template registration and sending (`PRIVATE` allows template registration only).

<a id="video-upload-flow"></a>

### Video Upload Flow

Video upload is a two-step process.

1. **Register video upload** — Send video metadata (file name and file size) as JSON to this API (`POST /brand-message/v1.0/appkeys/{appKey}/videos`). The response returns `video` (registration information on the NHN Cloud side) and `uploadInfo` (Kakao upload URL and token).
2. **Upload the video file** — Upload the video file directly to `uploadInfo.uploadUrl` using `multipart/form-data`. For authentication, pass `uploadInfo.token` as the `x-kamp-upload-token` header.

The video file is sent directly to the Kakao upload server without passing through NHN Cloud servers. Therefore, the request body of this API contains only metadata in JSON format, and the actual file is sent separately in step 2.

> **Caution**
> * `uploadInfo.token` is valid for 5 minutes after it is issued. If 5 minutes have elapsed, you must call the step 1 registration again to obtain a new token.
> * The `fileSize` in the step 1 request must exactly match the size of the file that will be uploaded in step 2. If the sizes do not match, Kakao will reject the request with errCode 109.

<a id="register-video-upload"></a>

### Register Video Upload

<a id="requested-16"></a>

#### Request

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | Type     | Description     |
|--------|--------|--------|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | Type     | Required | Description               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Request body]

```
{
  "senderKey": String,
  "fileName": String,
  "fileSize": Long,
  "createUser": String
}
```

| Name         | Type     | Required | Description                                                                  |
|------------|--------|----|---------------------------------------------------------------------|
| senderKey  | String | O  | Sender Profile key (40 characters)                                                       |
| fileName   | String | O  | Video file name (including extension; must be MP4, MOV, or AVI; up to 250 characters)                          |
| fileSize   | Long   | O  | Video file size (bytes; up to 4 GB)                                              |
| createUser | String | X  | Upload user identifier (up to 100 characters)                                                |

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

| Name              | Type      | Not Null | Description                                                                              |
|:----------------|:--------|:---------|:--------------------------------------------------------------------------------|
| header          | Object  | O        | Header area                                                                           |
| - resultCode    | Integer | O        | Result code                                                                           |
| - resultMessage | String  | O        | Result message                                                                          |
| - isSuccessful  | boolean | O        | Success                                                                           |
| video           | Object  | X        | Video information registered on the NHN Cloud side (status = `REGISTERED`)                                   |
| - videoSeq      | Long    | O        | Video sequence                                                                         |
| - vid           | String  | O        | Kakao video ID                                                                      |
| - senderKey     | String  | O        | Sender Profile key                                                                         |
| - title         | String  | X        | Video title (set to the file name immediately after upload; synchronized with the value edited in Kakao Biz Center after encoding is complete)                        |
| - fileName      | String  | O        | Uploaded file name                                                                         |
| - fileSize      | Long    | O        | File size (bytes)                                                                    |
| - status        | String  | O        | Video status (see [Video Status](#동영상-상태)). Always `REGISTERED` in the upload registration response.                     |
| uploadInfo      | Object  | O        | Kakao upload information. Used in step 2.                                                            |
| - uploadUrl     | String  | O        | Kakao endpoint to which the video file is uploaded directly                                                     |
| - token         | String  | O        | Upload authentication token. Passed via the `x-kamp-upload-token` header.                                          |

> The `thumbnailUrl`, `videoUrl`, `playUrl`, and `updateDate` fields, which are populated after encoding is complete, can be retrieved using the [Get Video](#동영상-조회) API.

<a id="video-file-upload-step-2"></a>

### Upload Video File (Step 2)

Call the video file directly to `uploadInfo.uploadUrl` from the response above. This request is sent directly to the Kakao upload server, not to the NHN Cloud server.

<a id="requested-17"></a>

#### Request

[URL]

```
POST  {uploadInfo.uploadUrl}
Content-Type: multipart/form-data
```

[Header]

| Name                | Type   | Required | Description                                                        |
|---------------------|--------|----------|--------------------------------------------------------------------|
| x-kamp-upload-token | String | O        | Pass the `uploadInfo.token` value from the Step 1 response as-is  |

[Request body (multipart)]

| Name | Type | Required | Description                                                                    |
|------|------|----------|--------------------------------------------------------------------------------|
| file | File | O        | Video file. Must exactly match the `fileSize` specified in the Step 1 request  |

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

* On success, `vid` (same as in the Step 1 response) and `playUrl` are returned, and the `errCode`/`message` fields are not included.
* On failure, HTTP 4xx is returned along with `errCode` (100–110) and `message`. For detailed error codes, refer to the Kakao Biz Message guide.

<a id="upload-video-specifications"></a>

#### Uploaded Video Specifications

| Item               | Limit                    |
|:-------------------|:-------------------------|
| File format        | MP4, MOV, AVI            |
| Maximum file size  | 4 GB                     |
| Maximum duration   | 4 hours                  |
| Maximum resolution | 8K                       |
| File name length   | Up to 250 characters     |

* Uploaded videos are available after encoding is complete in the Kakao Biz Center. Encoding time varies depending on the video length and typically takes 5 to 10 minutes.
* Immediately after upload, the video status starts as `REGISTERED`, transitions through `ENCODING`, and then changes to either `PUBLIC` or `PRIVATE`. You can check the status in the console or via the [View Video](#동영상-조회) API.
* Registered videos are permanently retained by Kakao. Even if a template is deleted, the videos in the Kakao Biz Center are not automatically removed. The KakaoTalk Channel administrator can manually delete them from the management screen on the channel's business home.
* If the Step 2 file upload fails or is delayed after Step 1 registration and the token (valid for 5 minutes) expires, you must call the registration again. Videos that were registered but never actually uploaded will be automatically marked with a status of `ERROR` after a certain period of time.

<a id="view-video"></a>

### Query Video

<a id="requested-18"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name | Type | Description |
|--------|--------|--------|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name | Type | Required | Description |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Request parameter]

| Name | Type | Required | Description |
|------------|--------|----|-------------------|
| senderKey  | String | X  | Sender profile key (40 characters) |
| pageNum    | String | X  | Page number (default: 1) |
| pageSize   | String | X  | Number of records to query (default: 15) |

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

| Name | Type | Not Null | Description |
|:--------------------|:--------|:---------|:------------------------------------------------------------|
| header              | Object  | O        | Header area |
| - resultCode        | Integer | O        | Result code |
| - resultMessage     | String  | O        | Result message |
| - isSuccessful      | boolean | O        | Success |
| videosResponse      | Object  | X        | Video list area |
| - videos            | Array   | O        | Video array |
| - - videoSeq        | Long    | O        | Video sequence |
| - - vid             | String  | O        | Kakao video ID |
| - - senderKey       | String  | O        | Sender profile key |
| - - title           | String  | X        | Video title (immediately after upload, set to the file name; synced with the value modified in Kakao Biz Center after encoding is complete) |
| - - fileName        | String  | O        | Uploaded file name |
| - - fileSize        | Long    | O        | File size (bytes) |
| - - status          | String  | O        | Video status (see [Video Status](#동영상-상태)) |
| - - thumbnailUrl    | String  | X        | Thumbnail URL (provided after encoding is complete) |
| - - videoUrl        | String  | X        | URL for sending and management (provided in `PUBLIC` status) |
| - - playUrl         | String  | X        | Playback URL |
| - - createDate      | String  | O        | Registration time |
| - - updateDate      | String  | X        | Status sync time (recorded when updated via webhook or batch) |
| - - createUser      | String  | X        | Upload user identifier |
| - totalCount        | Integer | O        | Total number of videos |

> The `video` in the upload registration response represents the state immediately after registration, so `status` is always `REGISTERED`, and the `thumbnailUrl`, `videoUrl`, `playUrl`, `createDate`, `updateDate`, and `createUser` fields are not included. These fields can be checked in the Query Video API after encoding is complete.

<a id="delete-video"></a>

### Delete a Video

<a id="requested-19"></a>

#### Request

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | Type   | Description        |
|--------|--------|--------|
| appKey | String | Unique app key |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Name           | Type   | Required | Description                        |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | Can be created in the console. |

[Query parameter]

| Name       | Type   | Required | Description                                                          |
|----------|--------|----|-----------------------------------|
| videoSeq | String | O  | Video sequence (multiple values can be passed separated by commas) |

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

| Name              | Type    | Not Null | Description     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | Header area     |
| - resultCode    | Integer | O        | Result code     |
| - resultMessage | String  | O        | Result message  |
| - isSuccessful  | boolean | O        | Success         |

<a id="video-status"></a>

### Video Status

Describes the `status` field values in the video query response.

| status     | Description                                                                 |
|:-----------|:----------------------------------------------------------------------------|
| REGISTERED | Upload registered                                                           |
| ENCODING   | Encoding in progress                                                        |
| PUBLIC     | Public status (delivery and template registration available)                |
| PRIVATE    | Private status (template registration only; delivery fails)                 |
| VIOLATED   | Policy-violating video                                                      |
| ILLEGAL    | Illegally filmed video                                                      |
| DELETED    | Deleted video                                                               |
| ERROR      | Error occurred during upload or encoding                                    |

<a id="upload"></a>

## Upload

<a id="upload-bizform-key"></a>

### Upload Biz Form Key

<a id="requested-20"></a>

#### Request

[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
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

| Name         | Type   | Required | Description                        |
|--------------|--------|----------|------------------------------------|
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

| Name            | Type    | Not Null | Description    |
|:----------------|:--------|:---------|:---------------|
| header          | Object  | O        | Header area    |
| - resultCode    | Integer | O        | Result code    |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success        |

<a id="manage-outgoing-profiles"></a>

## Sender Profile Management

<a id="view-outgoing-profile"></a>

### View Sender Profile

<a id="requested-21"></a>

#### Request

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
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

| Name                      | Type    | Not Null | Description                                                                                                                                                           |
|:--------------------------|:--------|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                    | Object  | O        | Header area                                                                                                                                                           |
| - resultCode              | Integer | O        | Result code                                                                                                                                                           |
| - resultMessage           | String  | O        | Result message                                                                                                                                                        |
| - isSuccessful            | boolean | O        | Success                                                                                                                                                               |
| sender                    | Object  | X        | Sender Profile                                                                                                                                                        |
| - plusFriendId            | String  | O        | PlusFriend ID                                                                                                                                                         |
| - senderKey               | String  | O        | Sender Key                                                                                                                                                            |
| - categoryCode            | String  | X        | Category code                                                                                                                                                         |
| - unsubscribePhoneNumber  | String  | X        | Toll-free opt-out phone number                                                                                                                                        |
| - unsubscribeAuthNumber   | String  | X        | Toll-free opt-out authentication number                                                                                                                               |
| - status                  | String  | X        | NHN Cloud PlusFriend status code <br>(YSC02: Registration pending, YSC03: Normally registered)                                                                        |
| - statusName              | String  | X        | NHN Cloud PlusFriend status name (Registration pending, Normally registered)                                                                                          |
| - kakaoStatus             | String  | X        | Kakao PlusFriend status code<br>(A: Normal, S: Blocked)<br>If status is YSC02, kakaoStatus has a null value.                                                          |
| - kakaoStatusName         | String  | X        | Kakao PlusFriend status name (Normal, Blocked)<br>If status is YSC02, kakaoStatusName has a null value.                                                               |
| - kakaoProfileStatus      | String  | X        | Kakao PlusFriend profile status code<br>(A: Active, B: Blocked, C: Inactive, D: Deleted, E: Deletion in progress)<br>If status is YSC02, kakaoProfileStatus has a null value. |
| - kakaoProfileStatusName  | String  | X        | Kakao PlusFriend profile status name (Active, Inactive, Blocked, Deletion in progress, Deleted)<br>If status is YSC02, kakaoProfileStatusName has a null value.       |
| - profileSpamLevel        | String  | X        | KakaoTalk Channel spam status name (Permanently restricted, Warning restricted, Normal)<br>May have a null value if the sender profile status is not normal.           |
| - profileMessageSpamLevel | String  | X        | KakaoTalk message spam status name (Activity restricted, Warning restricted, Normal)<br>May have a null value if the sender profile status is not normal.              |
| - block                   | boolean | O        | Whether the sender profile is blocked                                                                                                                                 |
| - brandMessage            | Object  | X        | Brand Message configuration information                                                                                                                               |
| -- resendAppKey           | String  | X        | SMS service appkey to set for fallback                                                                                                                                |
| -- isResend               | boolean | O        | Whether fallback (resend) is configured                                                                                                                               |
| -- resendSendNo           | String  | X        | tc-sms sender number used for resending                                                                                                                               |
| -- resendUnsubscribeNo    | String  | X        | tc-sms 080 opt-out number used for resending                                                                                                                          |
| - dormant                 | boolean | O        | Whether the sender profile is dormant                                                                                                                                 |
| - marketingAgreement      | boolean | O        | Whether M/N type usage is requested                                                                                                                                   |
| - createDate              | String  | X        | Registration date                                                                                                                                                     |
| - initialUserRestriction  | boolean | O        | Whether the initial user restriction is applied                                                                                                                       |

<a id="modify-outgoing-profile-080-opt-out-number"></a>

### Modify Sender Profile 080 Opt-Out Number

<a id="requested-22"></a>

#### Request

[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
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

[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| Name              | Type   | Required | Description                                                                                                                                                                                                          |
|-------------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| unsubscribeNo     | String | O        | 080 toll-free opt-out phone number (if neither field is entered, the message is sent using the opt-out information registered in the sender profile)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx |
| unsubscribeAuthNo | String | X        | 080 toll-free opt-out authentication number (up to 10 characters; if neither field is entered, the message is sent using the opt-out information registered in the sender profile)<br>Cannot enter unsubscribeAuthNo without unsubscribeNo<br>Example: 1234 |

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

## Alternative Delivery Management

<a id="register-sms-appkey"></a>

### Register SMS AppKey

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique appkey |

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
| resendAppKey | 	String | 	O  | SMS service appkey to set for fallback |

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

### Register Alternative Delivery Settings

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| Name     | 	Type     | 	Description     |
|--------|---------|---------|
| appkey | 	String | 	Unique appkey |

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
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| Name                  | 	Type      | 	Required | 	Description                                                                                                        |
|---------------------|----------|-----|------------------------------------------------------------------------------------------------------------|
| senderKey           | 	String  | 	O  | Sender Key                                                                                                       |
| isResend            | 	Boolean | 	O  | Whether to send an SMS as a fallback if delivery fails<br>If alternative delivery is configured in the console, it is sent as a fallback by default.                                           |
| resendSendNo        | 	String  | 	X  | Sender number for alternative delivery<br><span style="color:red">(Fallback may fail if the sender number is not registered on the SMS service.)</span>                  |
| resendUnsubscribeNo | 	String  | 	X  | 080 opt-out number for alternative delivery<br><span style="color:red">(Fallback may fail if the 080 opt-out number is not registered on the SMS service.)</span> |

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

| Name              | Type      | Not Null | Description     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | Header area  |
| - resultCode    | Integer | O        | Result code  |
| - resultMessage | String  | O        | Result message |
| - isSuccessful  | boolean | O        | Success  |