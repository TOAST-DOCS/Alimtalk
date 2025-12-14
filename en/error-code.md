## Notification > KakaoTalk Bizmessage > Error Codes

## API Response Code

| Category | Success | Result Code | Result Code Message | API Response Message |
|---|---|---|---|---|
| Common | true | 0 | Success | SUCCESS |
| Common | false | 4 | Parameter validation failed (See resultMessage) |  |
| Common | false | -1000 | Invalid appKey | Invalid appKey. |
| Common | false | -1001 | Invalid secretKey | Invalid secretKey. |
| Common | false | -1002 | Invalid SMS appKey | Invalid Sms appkey |
| Common | false | -1003 | Invalid SMS sending number | Invalid Sms Sendno |
| Common | false | -1005 | Requested message sending with the same X-NC-API-IDEMPOTENCY-KEY value within 10 minutes | The same 'X-NC-API-IDEMPOTENCY-KEY' was used within the last 10 minute. |
| Common | false | -1006 | Sender profile does not have sender key | Plus friend don't have senderKey. |
| Common | false | -1019 | Invalid alternative sending 080 unsubscribe number | Invalid Sms UnSubscribeno |
| Common | false | -1020 | Invalid UUID | Invalid uuid |
| Common | false | -1027 | Sender profile is blocked | The sender is blocked status. |
| Common | false | -1028 | Template variable replacement exceeds 14 characters (first-time user restriction) | Blacklist can't use more than 14 characters in template value. |
| Common | false | -2000 | Field value exceeds maximum length | The '{}' must be at less than or equal to {}. |
| Common | false | -2001 | Required field value is blank | The '{}' must not be blank. |
| Common | false | -2002 | Field value is NULL | The '{}' must not be null. |
| Common | false | -2003 | Field value is less than minimum length | The '{}' must be greater than or equal to {}. |
| Common | false | -2004 | Field value exceeds maximum length | The '{}' must be between {} and {}. |
| Common | false | -2005 | Field value is not within the allowed range | The '{}' must be less than or equal to {}. |
| Common | false | -2017 | Sender profile does not exist | Not exist plus friend. |
| Common | false | -2018 | Invalid button parameter | Button parameter is invalid. |
| Common | false | -2033 | Template item parameter is invalid | TemplateItem parameter is invalid. |
| Common | false | -2034 | Template item highlight parameter is invalid | TemplateItemHighlight parameter is invalid. |
| Common | false | -2036 | TemplateRepresentLink parameter is invalid | TemplateRepresentLink parameter is invalid. |
| Common | false | -2037 | Message body exceeds 700 characters when registering template with item list | The 'content' is too long. If you request the 'templateItem', It must be less than 1000 characters. |
| Common | false | -2502 | Alternative sending requested without sending failure settings configured | Please set up plus-friend resend setting. |
| Common | false | -2504 | Invalid quick reply | The quickReplies parameter is invalid. |
| Common | false | -3000 | linkMo is required for web link (WL) button type templates | If template have a WL(webLink), must input linkMo |
| Common | false | -3002 | Unable to read required request body content | Field is not valid. |
| Common | false | -3003 | Template does not exist | Template does not exist. |
| Common | false | -3004 | Template parameter error for sending | Please check template parameter. |
| Common | false | -3005 | Template status error (sending request before approval) | Please check template status. |
| Common | false | -3006 | Button URL must include http:// or https:// | The linkMo/linkPc must include http:// or https:// |
| Common | false | -3007 | Button name and URL can only be entered for free button type templates | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, LinkMo must be entered. |
| Common | false | -3008 | Delivery tracking button type cannot have button URL | The button name can't include the replacement parameter. |
| Common | false | -3009 | Button name does not exist | Not exist button name. |
| Common | false | -3010 | Does not match registered template body | The contents are not matched to template. This message can be resend by sms service if the resend setting is on. |
| Common | false | -3026 | AC type button name can only be "채널 추가" | AC type button name must have 채널 추가 |
| Common | false | -3038 | templateItem summary title cannot have replacement variables | The templateItem's summary title can't include the replacement parameter. |
| Common | false | -3040 | Item highlight title max 21 characters, description max 13 characters | The itemHighlight's title with thumbnail can not exceed 21 characters, and description can not excced 13 characters |
| Common | false | -3041 | imageUrl must include http:// or https:// protocol | The imageUrl must include http:// or https:// |
| Common | false | -3051 | TN button name must be one of: '전화 연결', '고객센터 연결', '상담원 연결' | TN button name must be one of: 전화 연결, 고객센터 연결, 상담원 연결 |
| Common | false | -3052 | TN button requires telNumber field | TN button must have telNumber field. |
| Common | false | -3053 | Invalid phone number format. Only digits and hyphens allowed, max 14 characters | Invalid telephone number format. Must be digits/hyphens only, max 14 characters. |
| Common | false | -3101 | Quick reply name does not exist | Not exist quickReply name. |
| Common | false | -3102 | Quick reply cannot include replacement variables | The quickReply name can't include the replacement parameter. |
| Common | false | -3303 | Required input value is missing | {} is empty. |
| Common | false | -3304 | Input value is out of valid range. Please enter a value within the specified range | The '{}' must be between {} and {}. |
| Common | false | -3305 | Selected chat bubble type cannot use specific field | The chatBubbleType {} is not allowed {} field. |
| Common | false | -3306 | Commerce type message must include button | The commerce type require buttons. |
| Common | false | -3307 | Required input field is missing based on button link type | linkType {} is required field {} |
| Common | false | -3308 | Multiple required input fields are missing based on button link type | linkType {} is required fields {} |
| Common | false | -3309 | When using app link (AL) in template, at least two of schemeAndroid, schemeIos, LinkMo must be entered | If there is an AL(appLink) in the template, at least two of schemeAndroid, schemeIos, LinkMo must be entered. |
| Common | false | -3310 | Web link URL (linkMo/linkPc) must include 'http://' or 'https://' | The linkMo/linkPc must include http:// or https:// |
| Common | false | -3311 | When message type is text or image, channel addition (AC) button must be first button | When the type is TEXT or IMAGE, AC  button type must be the first button. |
| Common | false | -3312 | When message type is not text or image, channel addition (AC) button must be last button | When the type is not TEXT or IMAGE, AC button type must be the last button. |
| Common | false | -3313 | Coupon URL information is incorrect. linkMo field required for basic coupon, schemeAndroid or schemeIos field required for channel coupon URL | If a basic coupon is used instead of a channel coupon URL, the linkMo field is required. If a channel coupon URL (format: alimtalk=coupon://) is used, either schemeAndroid or schemeIos must be provided. |
| Common | false | -3314 | Required field is missing when entering coupon information | The coupon field require {} |
| Common | false | -3315 | Coupon title is invalid | The title is not valid coupon title |
| Common | false | -3316 | Required field is missing when entering commerce information | commerce field require {} |
| Common | false | -3317 | Required field is missing when entering video information | video field require {} |
| Common | false | -3318 | Video URL format is invalid | The videoUrl is not valid video url |
| Common | false | -3319 | Required field is missing when entering image information | image field require {} |
| Common | false | -3320 | Carousel item count is out of allowed range (min/max) | The carousel list size must be between {} and {} |
| Common | false | -3321 | When entering link in carousel header, mobile link (linkMo) is required | If at least one link is entered in the Carousel Head, linkMo cannot be empty. |
| Common | false | -3322 | Required field is missing when entering carousel header information | The carousel head field require {} |
| Common | false | -3323 | Required field is missing when entering carousel footer information | The carousel tail field require {} |
| Common | false | -3324 | Template variable value is incorrect | The template parameter is invalid. |
| Common | false | -3325 | Required field is missing when entering item (product) information | item field require {} |
| Common | false | -3326 | Image URL must include 'http://' or 'https://' | The imageUrl must include http:// or https:// |
| Common | false | -3327 | Image link URL must include 'http://' or 'https://' | The imageLink must include http:// or https:// |
| Common | false | -3328 | Button type is not allowed | The button type {} is not allowed. |
| Common | false | -3330 | In vertical button layout without channel addition button, bot form (BF) button must be first | BF button must be the first button in vertical layout without channel-add button. |
| Common | false | -3331 | Button name is not allowed | The button is not allowed to have {} as its name. |
| Common | false | -3332 | Required field is missing when entering carousel commerce item information | The carousel commerce item field require {} |
| Common | false | -3333 | Carousel commerce item contains information that is not allowed | The carousel commerce field not allowed {} |
| Common | false | -3334 | Required field is missing when entering carousel general item information | The carousel item field require {} |
| Common | false | -3335 | Carousel general item contains information that is not allowed | The carousel item field not allowed {} |
| Common | false | -3336 | In vertical button layout with channel addition button, bot form (BF) button must be second | BF button must be the second button in vertical layout with channel-add button. |
| Common | false | -3337 | In horizontal button layout without channel addition button, bot form (BF) button must be on the right (second) | BF button must be the second (right side) button in horizontal layout without channel-add button. |
| Common | false | -3338 | In horizontal button layout with channel addition button, bot form (BF) button must be on the left (first) | BF button must be the first (left side) button in horizontal layout with channel-add button. |
| Common | false | -3340 | Invalid 080 unsubscribe number | Invalid unsubscribeNo format. Expected format: 080-xxx-xxxx or 080-xxxx-xxxx or 080xxxxxxx or 080xxxxxxxx. |
| Common | false | -3341 | Invalid 080 authentication number | Invalid format. Only digits are allowed, up to a maximum of 9. |
| Common | false | -4000 | Invalid parameter | Invalid parameter |
| Common | false | -4001 | AppKey is already activated | Already activated appkey. |
| Common | false | -4002 | AppKey is not activated | Not activated appkey. |
| Common | false | -4003 | Query range exceeds one month | Search is possible within 31 days. |
| Common | false | -4004 | AppKey does not exist | Not exist appkey. |
| Common | false | -4005 | AppKey is in terminated status | Appkey is disabled status. |
| Common | false | -4007 | File size exceeded | The file size is less than {}. |
| Common | false | -4009 | Invalid file extension | Check the file extension. |
| Common | false | -4010 | File not found | Not found file. |
| Common | false | -4011 | Recipient list not found | Not found recipientList. |
| Common | false | -4014 | File does not have recipient_no header | There is no recipient_no header in the file. |
| Common | false | -4016 | Data does not exist | Not exist data. |
| Common | false | -4018 | File upload error | Upload attach file error. |
| Common | false | -4020 | File read failed | Failed to reading the file. |
| Common | false | -4023 | To disable product, all senders must be deleted | For disabling the product, have to delete all senders |
| Common | false | -4101 | Invalid statistics query parameter | Invalid search parameter. |
| Common | false | -4103 | When querying, sending request start time/end time value is missing | RequestId or startRequestDate/endRequestDate is invalid. |
| Common | false | -5000 | Invalid recipient number | RecipientNo is invalid. |
| Common | false | -7000 | Vendor request API failed | Vender request API is failed. |
| Common | false | -8001 | Image file is not normal | The image you upload is invalid. |
| Common | false | -8002 | Image not found. Carousel feed type must use carousel type image, carousel commerce type must use carousel commerce type image, commerce type must use general image type | It is not found any images. If you want to send carousel-feed type messages, you must use carousel type images. If you want to send carousel-commerce type messages, you must use commerce type images. If you want to send commerce type messages, you must use IMAGE type images. |
| Common | false | -8007 | Storage settings are empty | The storage configs can't empty. |
| Common | false | -8009 | Project is already shared | This project has already been shared. |
| Common | false | -8010 | Image file upload failed due to network or other issues | Uploading image file has failed for an unexpected error. |
| Common | false | -9992 | fade-out된 친구톡 API를 호출했을 경우 | The API is no longer supported. Please migrate to the new brand-message endpoint/API. |
| Common | false | -9993 | Required request part is missing | Required request part is not present. |
| Common | false | -9994 | Method argument type is different from expected | A method argument has not the expected type. |
| Common | false | -9996 | Content-type is not application/json | Only application/json Content-type is supported. |
| Common | false | -9997 | Failed due to inappropriate request | Client Error. |
| Common | false | -9998 | API does not exist | Not exist API |
| Common | false | -9999 | System error | System error. Please inquire at support@toast.com. |
| Sender Profile | false | -3342 | Failed to upload marketing consent evidence | Failed to upload marketing agreement file. |
| Sender Profile | false | -3343 | Failed to apply for M/N type usage | {} |
| Sender Profile | false | -3345 | 080 unsubscribe information is invalid | Failed to update unsubscribe content. message: {} |
| Sender Profile Group | false | -1010 | Sender profile group does not exist | Not exist plus friend group. |
| Sender Profile Group | false | -1013 | Sender profile group already exists | Already exist plus friend group. |
| Sender Profile Group | false | -1018 | Already registered in sender profile group | This is a plusFriend that has already been added. |
| Sender Profile Group | false | -1022 | Sender profile is not registered in group | This is not a plusFriend added to the group. |
| Sender Profile Group | false | -1023 | Sender profile group count exceeded maximum of 10 | The max group size is 10. |
| Sender Profile Group | false | -1025 | Attempted to delete sender profile group with sender profile deletion API | The sender-group can't deleted. |
| Sender Profile Group | false | -1026 | Sender profile group member count exceeded 5000 | The maximum number of members in a group is 5000. |
| Sender Profile Group | false | -1029 | Cannot add as member to group profile (first-time user restriction) | Blacklist can't join the group. |
| Template | false | -1014 | Sender profile is not activated | Not active status plus friend. |
| Template | false | -2505 | Sending with quick reply exceeded maximum button count of 2 | The 'buttons' is too many. If you request the 'quickReplies', the buttons size muse be 2 or less. |
| Template | false | -3001 | Template code or template name already exists | Already exist templateCode or templateName. |
| Template | false | -3012 | Template status cannot be modified (only approved/rejected status allowed) | Only templates with TSC03(APPROVE)/TSC04(REJECT) can be modified. |
| Template | false | -3013 | Template is already being modified | There are templates already being modified. |
| Template | false | -3016 | Emphasized template requires templateTitle and templateSubtitle fields | The Template which emphasizeType is 'TEXT' must have templateTitle, templateSubtitle. |
| Template | false | -3017 | templateSubtitle cannot use replacement variables | The templateSubtitle can't include the replacement parameter. |
| Template | false | -3018 | Additional information template requires templateExtra field | The Template which messageType is 'EX' must have templateExtra. |
| Template | false | -3020 | Mixed template requires templateExtra and templateAd fields | The Template which messageType is 'MI' must have templateExtra. |
| Template | false | -3021 | templateExtra cannot use replacement variables | The templateExtra can't include the replacement parameter. |
| Template | false | -3024 | AC type button can only be registered for channel addition mixed templates | The button of AC type can using only templateMessageType (AD/MI). |
| Template | false | -3025 | AC type button must be used alone or positioned at the top | AC type buttons should be located alone or on top. |
| Template | false | -3027 | Cannot register templateTitle and templateSubtitle fields when template emphasis type is NONE | The Template which emphasizeType is 'NONE' can't have templateTitle, templateSubtitle. |
| Template | false | -3028 | Cannot register templateExtra field when template message type is BA | The Template which messageType is 'BA' can't have templateExtra. |
| Template | false | -3030 | Cannot register templateExtra field when template message type is AD | The Template which messageType is 'AD' can't have templateExtra. |
| Template | false | -3032 | templateImageName and templateImageUrl fields required when template emphasis type is IMAGE | The Template which emphasizeType is 'IMAGE' must have templateImageName, templateImageUrl. |
| Template | false | -3037 | templateItem list title cannot have replacement variables | The templateItem's title can't include the replacement parameter. |
| Template | false | -3047 | Attempted to apply font style when registering template (font styles can be applied at sending time) | The templateTitle and templateItemHighlight's title can't end with \s. |
| Template | false | -3050 | Template message type (AD/MI) must have AC type button | The templateMessageType (AD/MI) must have button of AC type. |
| Template | false | -3100 | Cannot add comment when template is in requested/approved status | Could not add comment on registered/completed template status. |
| Template | false | -3103 | Button or quick reply format is invalid (not JSON format or contains unescaped characters) | The button or quickReply has an invalid format. |
| Template | false | -4021 | Comment file size exceeded (10MB) | The file size is less than 10MB. |
| Template | false | -4024 | Failed to release dormant template | Failed to release the dormant template. |
| Template | false | -4025 | Template upload exceeded maximum of 20 | Only upload up to 20 templates at a time. |
| Template | false | -4026 | Template upload file header is invalid | Uploaded template's headers are invalid. |
| Template | false | -4027 | Attempting to convert to channel addition type when button length is at maximum | Conversion to AD/MI type failed. The 'buttons' length does not exceed its maximum length. |
| Template | false | -4028 | Attempting to convert to channel addition type with unapproved template | Conversion to AD/MI type failed. The template must be approved |
| Send/Query | false | -1016 | Message not found | It is not found any messages responding with that requestId or recipientSeq. |
| Send/Query | false | -1024 | When sender profile group sends message | The sender-group can't send the message. |
| Send/Query | false | -1031 | All requested recipients failed to send | All of receivers are failed to send. |
| Send/Query | false | -2019 | Failed due to template body exceeding 1,000 characters | The content that is replaced by the template parameters can not exceed 1,300 characters. |
| Send/Query | false | -2023 | Friendtalk message body exceeds 400 characters (with image) | The 'content' is too long. If you request the 'image', It can be less than 400 characters. |
| Send/Query | false | -2024 | Friendtalk message body exceeds 1,000 characters | The 'content' is too long. It can be less than 1,300 characters without any image. |
| Send/Query | false | -2025 | Scheduled time is in the past | You can not send messages on past dates. Please check `requestDate` again. |
| Send/Query | false | -2026 | Scheduled time is after 90 days (maximum 90 days allowed) | You can not send messages after 90 days. Please check `requestDate` again. |
| Send/Query | false | -2027 | Date format parsing error occurred | The 'requestDate' has invalid format. |
| Send/Query | false | -2028 | Invalid request ID | The `requestId` is invalid |
| Send/Query | false | -2029 | Requested message does not exist or no message can be canceled | All of your messages to cancel are not found or do not meet conditions to cancel. |
| Send/Query | false | -2030 | Wide image sending exceeded maximum body length of 76 characters | The 'content' is too long. If you request the 'wide-image', It must be less than 76 characters. |
| Send/Query | false | -2031 | Wide image sending exceeded maximum button count of 2 | The 'buttons' is too many. If you request the 'wide-image', It must be less than 2 buttons size. |
| Send/Query | false | -2032 | Template title exceeded 50 characters | The templateTitle that is replaced by the template parameters can not exceed 50 characters. |
| Send/Query | false | -2035 | Template header exceeded 16 characters | The templateHeader that is replaced by the template parameters can not exceed 16 characters. |
| Send/Query | false | -2500 | Mass sending request not found | Not found mass message request |
| Send/Query | false | -2501 | Requested message does not exist or no message can be canceled | Send request is failed. because the deadline is expired |
| Send/Query | false | -3011 | Does not match registered template buttons | The buttons or quickReplies are not matched to template. This message can be resend by sms service if the resend setting is on. |
| Send/Query | false | -3033 | Button or quick reply not registered in template | The button or quickReply is not exist in template. |
| Send/Query | false | -3034 | Template to be deleted has sending history within 3 days | The template could not be deleted because recently sent message. requestId: {} |
| Send/Query | false | -3035 | At least one of templateHeader, templateImageName, templateImageUrl, templateItem, templateItemHighlight fields required when template emphasis type is ITEM_LIST | The Template which emphasizeType is 'ITEM_LIST' must have at least one of templateImageInfo, templateHeader, templateItem and templateItemHighlight. |
| Send/Query | false | -3036 | Cannot register as security template when template emphasis type is ITEM_LIST | The Template which emphasizeType is 'ITEM_LIST' can't be a security template. |
| Send/Query | false | -3039 | templateItem summary cannot exist without templateItem list | The TemplateItem's summary can't exist without templateItem list |
| Send/Query | false | -3042 | templateHeader does not match template | The templateHeader are not matched to template. This message can be resend by sms service if the resend setting is on. |
| Send/Query | false | -3043 | templateItem or templateItemHighlight does not match template | The templateItem or templateItemHighlight are not matched to template. This message can be resend by sms service if the resend setting is on. |
| Send/Query | false | -3044 | Business form (BF) type button must be used alone or positioned at the top | BF type buttons should be located on top. |
| Send/Query | false | -3045 | Business form (BF) type button name must be "톡에서 예약하기", "톡에서 설문하기", or "톡에서 응모하기", or bizFormKey is not registered | The button linkType 'BF' must follow this constraint. The BF button must have a bizFormKey. The BF button name must be in "톡에서 예약하기", "톡에서 설문하기", "톡에서 응모하기". |
| Send/Query | false | -3046 | templateRepresentLink does not match template | The templateRepresentLink is not matched to template. This message can be resend by sms service if the resend setting is on. |
| Send/Query | false | -3048 | Template parameter cannot exceed 1000 characters | The template parameter's length can not exceed 1300 characters. |
| Send/Query | false | -3049 | Template parameter does not match template | The template parameter is not matched to template. |
| Send/Query | false | -3200 | Wide item list is missing name | Friendtalk wide item must have a title. |
| Send/Query | false | -3201 | Wide item list is missing image | Friendtalk wide item must have a image. |
| Send/Query | false | -3202 | Wide item list is missing linkMo | Friendtalk wide item must have a linkMo. |
| Send/Query | false | -3203 | Wide item list requires list of size 3~4 and header | Friendtalk wide item must have 3 ~ 4 list and a header. |
| Send/Query | false | -3204 | Carousel is missing header | Friendtalk carousel must have a header. |
| Send/Query | false | -3205 | Carousel is missing message | Friendtalk carousel must have a message. |
| Send/Query | false | -3206 | Carousel is missing attachment | Friendtalk carousel must have a attachment. |
| Send/Query | false | -3207 | Carousel is missing image | Friendtalk carousel must have a image. |
| Send/Query | false | -3208 | Carousel requires list of size 2~10, carousel commerce type with intro requires list of size 1~10 | Friendtalk carousel must have 2 ~ 10 list. If message type is friendtalk carousel-commerce and carousel intro exists, carousel must have 1 ~ 10 list. |
| Send/Query | false | -3209 | Carousel tail is missing linkMo | Friendtalk carousel tail must have linkMo. |
| Send/Query | false | -3210 | Coupon requires title and description | Friendtalk coupon must have a title and a description. |
| Send/Query | false | -3211 | Friendtalk text/image type message coupon description cannot exceed 12 characters, FW/FL type cannot exceed 18 characters | If the message type is friendTalk text/image type, the friendtalk coupon description's length cannot exceed 12 characters. If the message type is friendTalk wide-image/wide-item-list type, the friendtalk coupon description's length cannot exceed 18 characters |
| Send/Query | false | -3212 | Coupon title content is invalid | Friendtalk coupon title is invalid. |
| Send/Query | false | -3213 | Coupon requires mobile link or channel-type ios/android link | Friendtalk must have a mobile link or a channel formatted ios/android link |
| Send/Query | false | -3215 | Wide item list and carousel can only be used for ad type | Friendtalk wide item / carousel can only be sent in AD type. |
| Send/Query | false | -3216 | Wide item list first item title cannot exceed 25 characters, 2nd~4th item title cannot exceed 30 characters | Friendtalk first wide item's title length cannot exceed 25 characters, and 2nd ~ 4th wide item's title length cannot exceed 30 characters. |
| Send/Query | false | -3217 | Friendtalk text/image type cannot exceed 5 buttons, 4 buttons if coupon included, wide image/wide item list cannot exceed 2 buttons, premium video type cannot exceed 1 button, commerce type must have 1~2 buttons | Friendtalk button size is invalid. The button size must be 5 or less. If coupon is included, the button size must be 4 or less. If the message type is friendTalk wide-image/wide-item-list type, the button size must be 2 or less. If the message type is video type, the button size must be 1 or less. If the message type is commerce, the button size must be 1 or 2 |
| Send/Query | false | -3218 | Friendtalk videoUrl is invalid | Friendtalk video url is invalid. |
| Send/Query | false | -3219 | Friendtalk content exceeds maximum length. Premium video type content max length is 76 characters | The 'content' is too long. If you request the 'video', It must be less than 76 characters. |
| Send/Query | false | -3220 | Friendtalk header exceeds maximum length. Premium video type header max length is 20 characters | The 'header' is too long. If you request the 'video', It must be less than 20 characters. |
| Send/Query | false | -3221 | Carousel feed type cannot use carousel intro | Friendtalk carousel feed type cannot have a 'head' field. |
| Send/Query | false | -3222 | Carousel feed type cannot use additional information field | Friendtalk carousel feed type cannot have a 'additionalContent' field. |
| Send/Query | false | -3223 | Carousel feed type cannot use commerce | Friendtalk carousel feed type cannot have a 'commerce' field. |
| Send/Query | false | -3224 | Carousel commerce type cannot use header and message fields | Friendtalk carousel commerce type cannot have 'header' & 'message' fields. |
| Send/Query | false | -3225 | Carousel button is invalid. Carousel feed type cannot exceed 2 buttons, carousel commerce type must have 1~2 buttons | Friendtalk carousel button size is invalid. If the message type is friendtalk carousel-feed, the button size must be 2 or less. If the message type is friendtalk carousel-commerce, the button size must be 1 ~ 2. |
| Send/Query | false | -3226 | If commerce has discountPrice field, discountRate or discountFixed field is required | If commerce has 'discountPrice' field, commerce must have a 'discountRate' or 'discountFixed' field. |
| Send/Query | false | -3298 | Targeting information is not set | The targeting is invalid. |
| Send/Query | false | -3299 | Commerce variables must be used in specified combinations: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed'] | The commerce variable must be used in the following combinations: ['regularPrice'], ['regularPrice', 'discountPrice', 'discountRate'], ['regularPrice', 'discountPrice', 'discountFixed'] |
| Send/Query | false | -3300 | Cannot find unsubscribed number | The unsubscribe number could not be found. |
| Send/Query | false | -3301 | Includes users who have unsubscribed | The unsubscribed recipient are found. |
| Send/Query | false | -3344 | Number of image change parameters does not match number of template images | The image parameter is invalid. image parameter's size must be equal to a number of template images. |
| Send/Query | false | -4017 | Cannot query messages older than 90 days | You can not search the messages before 90 days. |
| Send/Query | false | -4019 | Invalid recipient number | Invalid recipient number. |
| Send/Query | false | -4022 | Data export failed | Failed to exporting data. |
| Send/Query | false | -4029 | Sender profile marketing consent is not activated | To send 'M/N type' messages to users who are not friends with your profile, you must first apply to have the 'M/N type' messaging feature enabled. Please check if this option is active in your sender profile settings. |
| Send/Query | false | -4104 | RequestId is empty | RequestId is empty value. |
| Send/Query | false | -4200 | Invalid alternative sending message | Resend Message is invalid. |
| Send/Query | false | -8006 | When sending authentication message, template content does not have authentication phrase | The content must contain auth guidement. |
| Send/Query | false | -8008 | Message content contains banned word | The content has banned word. |
| Image | false | -8000 | Image sequence (imageSeq) is missing | The 'imageSeq' is empty. |
| Image | false | -8003 | Image deletion failed | It is failed to delete images. |
| Image | false | -8004 | createUser field exceeded maximum of 100 characters | The 'createUser' is too long. It can be less than 100 characters. |
| Image | false | -8005 | No plus friend registered in project when uploading image | Your project doesn't have any plus friends. Please register it at first. |
| Image | false | -8011 | Image type does not exist | The image type is invalid. |
| Banned Word | false | -8012 | Banned word does not exist | Banned word does not exist. |
| Banned Word | false | -8013 | Banned word already exists | Banned word already exists. |
| Self Verification | false | -1030 | Identity verification not completed | Self verification is required to use this service. |

## AlimTalk/FriendTalk Sending Result Code

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>Code</th>
		<th>Meaning</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td>1000</td>
		<td>Success</td>
	</tr>
	<tr>
		<td>1001</td>
		<td>Request body is not in JSON format</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>Hub partner key is invalid</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>Sender profile key is invalid</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>Cannot find name in request body (JSON)</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>Deleted sender profile (Contact customer service)</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>Blocked sender profile (Contact customer service)</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>Cannot find contract information (Contact customer service)</td>
	</tr>
	<tr>
		<td>1012</td>
		<td>Invalid user key format request</td>
	</tr>
	<tr>
		<td>1013</td>
		<td>Invalid app connection</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>Invalid business registration number</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>Invalid app user ID request</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>Business registration number mismatch</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>Phone number or app user ID is invalid or not entered</td>
	</tr>
	<tr>
		<td>1021</td>
		<td>Blocked KakaoTalk channel</td>
	</tr>
	<tr>
		<td>1022</td>
		<td>Closed KakaoTalk channel</td>
	</tr>
	<tr>
		<td>1023</td>
		<td>Deleted KakaoTalk channel</td>
	</tr>
	<tr>
		<td>1024</td>
		<td>KakaoTalk channel pending deletion</td>
	</tr>
	<tr>
		<td>1025</td>
		<td>Message sending failed due to channel sanctions</td>
	</tr>
	<tr>
		<td>1027</td>
		<td>Message sending failed due to channel message sanctions</td>
	</tr>
	<tr>
		<td>1028</td>
		<td>Cannot use the targeting option</td>
	</tr>
	<tr>
		<td>1030</td>
		<td>Invalid parameter request</td>
	</tr>
	<tr>
		<td>1033</td>
		<td>Template message type and chat_bubble_type parameter mismatch</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>Unable to send message (Unexpected error occurred)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>Message sending failed (KakaoTalk channel not added on test server)</td>
	</tr>
	<tr>
		<td>2005</td>
		<td>Failed to read image information due to Kakao internal system error</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Unexpected error occurred</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Message sent but receipt not confirmed (Success uncertain, stored encrypted on server and can be received within 3 days)</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Message sending failed due to internal system error</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Phone number error</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Unexpected error occurred</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>Message does not exist</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>Kakao communication failed</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>Message is empty</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Message length limit error</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>Cannot find template</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>Message content does not match template</td>
	</tr>
	<tr>
		<td>3022</td>
		<td>Not a valid sending time (FriendTalk messages can be sent from 08:00 to 20:50)</td>
	</tr>
	<tr>
		<td>3023</td>
		<td>Message syntax error (JSON format error)</td>
	</tr>
	<tr>
		<td>3024</td>
		<td>Cannot send image in message (Image URL or link is invalid or image does not meet specifications)</td>
	</tr>
	<tr>
		<td>3025</td>
		<td>Variable character limit exceeded</td>
	</tr>
	<tr>
		<td>3026</td>
		<td>Consultation/bot conversion button extra, event character limit exceeded</td>
	</tr>
	<tr>
		<td>3027</td>
		<td>Message button/quick link does not match template</td>
	</tr>
	<tr>
		<td>3028</td>
		<td>Message emphasis title does not match template</td>
	</tr>
	<tr>
		<td>3029</td>
		<td>Message emphasis title length limit exceeded (50 characters)</td>
	</tr>
	<tr>
		<td>3030</td>
		<td>Message type and template emphasis type do not match</td>
	</tr>
	<tr>
		<td>3031</td>
		<td>Header does not match template</td>
	</tr>
	<tr>
		<td>3032</td>
		<td>Header length limit exceeded (16 characters)</td>
	</tr>
	<tr>
		<td>3033</td>
		<td>Item highlight does not match template</td>
	</tr>
	<tr>
		<td>3034</td>
		<td>Item highlight title length limit exceeded (30 characters without image, 21 characters with image)</td>
	</tr>
	<tr>
		<td>3035</td>
		<td>Item highlight description length limit exceeded (19 characters without image, 13 characters with image)</td>
	</tr>
	<tr>
		<td>3036</td>
		<td>Item list does not match template</td>
	</tr>
	<tr>
		<td>3037</td>
		<td>Item list item description length limit exceeded (23 characters)</td>
	</tr>
	<tr>
		<td>3038</td>
		<td>Item summary information does not match template</td>
	</tr>
	<tr>
		<td>3039</td>
		<td>Item summary description length limit exceeded (14 characters)</td>
	</tr>
	<tr>
		<td>3040</td>
		<td>Item summary description contains disallowed characters (Contains characters other than currency symbols/codes, numbers, commas, decimal points, spaces)</td>
	</tr>
	<tr>
		<td>3041</td>
		<td>Wide item list count min/max mismatch</td>
	</tr>
	<tr>
		<td>3042</td>
		<td>Main link does not match template</td>
	</tr>
	<tr>
		<td>3043</td>
		<td>Image variable count template mismatch</td>
	</tr>
	<tr>
		<td>3044</td>
		<td>Coupon variable template mismatch</td>
	</tr>
	<tr>
		<td>3045</td>
		<td>Commerce information variable template mismatch</td>
	</tr>
	<tr>
		<td>3046</td>
		<td>Additional information maximum length limit error</td>
	</tr>
	<tr>
		<td>3047</td>
		<td>Commerce information product name maximum length limit error</td>
	</tr>
	<tr>
		<td>3048</td>
		<td>Invalid group tag key input</td>
	</tr>
	<tr>
		<td>3050</td>
		<td>Opt-out spec (N type) not supported</td>
	</tr>
	<tr>
		<td>3051</td>
		<td>Carousel item list count min/max mismatch</td>
	</tr>
	<tr>
		<td>3052</td>
		<td>Carousel item message length exceeded</td>
	</tr>
	<tr>
		<td>3053</td>
		<td>Carousel template mismatch</td>
	</tr>
	<tr>
		<td>3054</td>
		<td>Carousel button template mismatch</td>
	</tr>
	<tr>
		<td>3055</td>
		<td>Carousel coupon template mismatch</td>
	</tr>
	<tr>
		<td>3056</td>
		<td>Wide item list title length limit error</td>
	</tr>
	<tr>
		<td>3057</td>
		<td>Carousel commerce template mismatch</td>
	</tr>
	<tr>
		<td>3058</td>
		<td>Carousel header length limit error</td>
	</tr>
	<tr>
		<td>4000</td>
		<td>Cannot find message sending result</td>
	</tr>
	<tr>
		<td>4001</td>
		<td>Unknown message status</td>
	</tr>
	<tr>
		<td>4100</td>
		<td>requestId error</td>
	</tr>
	<tr>
		<td>4101</td>
		<td>Request date error</td>
	</tr>
	<tr>
		<td>4102</td>
		<td>Template request error</td>
	</tr>
	<tr>
		<td>4103</td>
		<td>Cannot find valid hub partner</td>
	</tr>
	<tr>
		<td>4104</td>
		<td>Cannot find valid sender profile</td>
	</tr>
	<tr>
		<td>4110</td>
		<td>Invalid chat bubble type or message type request</td>
	</tr>
	<tr>
		<td>4120</td>
		<td>Message request payload generation error</td>
	</tr>
	<tr>
		<td>4121</td>
		<td>Message sending target error</td>
	</tr>
	<tr>
		<td>4122</td>
		<td>Message result query error</td>
	</tr>
	<tr>
		<td>4130</td>
		<td>requestId pattern error</td>
	</tr>
	<tr>
		<td>4131</td>
		<td>Duplicate requestId</td>
	</tr>
	<tr>
		<td>4132</td>
		<td>Template variable mismatch</td>
	</tr>
	<tr>
		<td>4133</td>
		<td>Stopped template</td>
	</tr>
	<tr>
		<td>4134</td>
		<td>Changed template</td>
	</tr>
	<tr>
		<td>4137</td>
		<td>Message request failed</td>
	</tr>
	<tr>
		<td>4138</td>
		<td>Brand message count limit</td>
	</tr>
	<tr>
		<td>4139</td>
		<td>Message request failed</td>
	</tr>
	<tr>
		<td>4140</td>
		<td>Message request failed</td>
	</tr>
	<tr>
		<td>4141</td>
		<td>Message request failed</td>
	</tr>
	<tr>
		<td>4142</td>
		<td>Message request failed</td>
	</tr>
	<tr>
		<td>4143</td>
		<td>Expired request</td>
	</tr>
	<tr>
		<td>4144</td>
		<td>Body length limit (30KB) exceeded</td>
	</tr>
	<tr>
		<td>4156</td>
		<td>Maximum send count exceeded</td>
	</tr>
	<tr>
		<td>4161</td>
		<td>Message in processing</td>
	</tr>
	<tr>
		<td>9998</td>
		<td>System issue under review by staff (Service currently unavailable)</td>
	</tr>
	<tr>
		<td>9999</td>
		<td>System issue under review by staff (Unknown system error occurred)</td>
	</tr>
</tbody>
</table>
