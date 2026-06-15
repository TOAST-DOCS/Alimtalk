<!-- pre-align:aligned sig=ebf094815abd -->

## Notification > KakaoTalk Bizmessage > Brand Message > Console User Guide

<a id="brand-message-sending"></a>

## Brand Message Delivery

<a id="regular-send"></a>

### General Delivery

You can send Brand Message-type messages by configuring sender profiles and entering content.
To send Brand Messages, select **Notification > KakaoTalk Bizmessage > Brand Message** in the console.

<a id="when-using-a-template"></a>

### When Using a Template

![friendtalkupgrade_04_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_04_20250729.png)

1. Select a sender profile.
    * You can check sender profiles in the **Notification > KakaoTalk Bizmessage > Sender Profile Management** tab.
2. Select whether to use push notifications.
    * If you select **Disable** for push notifications, message push notifications will not be sent.
3. Select **Use** for template usage and select the template to send.
    * Templates can be registered in the **Notification > KakaoTalk Bizmessage > Brand Message > Template Management** tab.
4. Select whether to configure alternative delivery messages.
    * Alternative delivery is only available when you set alternative delivery to **Use** in the **080 Opt-out Management** tab and integrate the SMS service.
5. Select whether to configure the 080 opt-out number.
    * If you select **Not Configured (Send with sender profile settings)** for the 080 opt-out number configuration, the 080 opt-out number configured in the sender profile will be used.
    * If you select **Set with Common Content** or **Set by User**, the entered 080 opt-out number will be used for delivery.
    * When using the SMS service in the **080 Opt-out Management** tab, only 080 opt-out numbers registered in the SMS service can be used.
6. Add recipients.
    * Recipients can be entered in mobile phone number format.
7. Select targeting type for each recipient.
    * Brand Messages are advertising messages that can be sent to users who have consented to receive marketing messages from advertisers (hereinafter referred to as "marketing consent") and channel friend users.
    * Message reception by specified recipients may vary depending on the targeting type.
        * M: Advertiser marketing consent users (KakaoTalk reception consent)
            * Sends advertising messages to advertiser marketing consent users (KakaoTalk reception consent).
        * N: Advertiser marketing consent users (KakaoTalk reception consent) - Channel friends
            * Sends advertising messages to advertiser marketing consent users (KakaoTalk reception consent) excluding channel friends.
        * I: Advertiser delivery request targets ∩ Channel friends
            * Sends advertising messages only to channel friends among advertiser delivery request targets.
8. After completing the input, click **Send** to transmit.

<a id="when-not-using-a-template"></a>

### When Not Using a Template

![friendtalkupgrade_05_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_05_20250729.png)

1. Select a sender profile.
    * You can check sender profiles in the **Notification > KakaoTalk Bizmessage > Sender Profile Management** tab.
2. Select whether to use push notifications.
    * If you select **Disable** for push notifications, message push notifications will not be sent.
3. Select whether this is an adult message.
    * For adult messages, chat room messages are covered until age verification, and only users 20 years or older can view the content.
4. Select message type.
    * Text
        * 1,300 characters of text, including spaces both for Korean and English + up to 5 link buttons (vertically arranged)
    * Image
        * 1,300 characters of text, including spaces both for Korean and English + 1 image + up to 5 link buttons (vertically arranged)
    * Wide Image
        * 76 characters of text, including spaces both for Korean and English + 1 image + up to 2 link buttons
    * Wide Item List
        * An advertising type product that can add 3-4 lists (image + item) to one title.
        * 25 characters for the first item title, 30 characters for the 2nd-4th item titles, including spaces both for Korean and English + 3-4 image items + up to 2 link buttons (horizontally aligned)
    * Carousel Feed
        * An advertising type product that can contain up to 10 images and various text information.
        * Up to 6 items consisting of 20-character title text + 180-character phrase text + image + 2 link buttons (horizontally aligned), including spaces both for Korean and English
    * Premium Video
        * A type where attached videos automatically play in speech bubbles.
        * Video links can only use videos uploaded to KakaoTV (e.g., [https://tv.kakao.com/v/#{숫자}](https://tv.kakao.com/v/#%7B%EC%88%AB%EC%9E%90%7D) / [https://tv.kakao.com/channel/#{숫자}/cliplink/#{숫자}](https://tv.kakao.com/channel/#%7B%EC%88%AB%EC%9E%90%7D/cliplink/#%7B%EC%88%AB%EC%9E%90%7D)).
        * 20-character header text + 76-character phrase text + 1 video uploaded to KakaoTV + 1 link button, including spaces both for Korean and English
    * Commerce
        * A speech bubble that can emphasize product prices and discount information.
        * 20-character title text + 34-character additional information text + up to 2 link buttons (horizontally aligned), including spaces both for Korean and English
    * Carousel Commerce
        * A speech bubble that can organize various product information in catalog format.
        * Up to 6 items consisting of 30-character title text + 34-character additional information text + up to 2 link buttons (horizontally aligned), including spaces both for Korean and English
        * All images used in Carousel Commerce must have the same ratio.
5. If there are images, select the image.
    * To attach an image to a message, you must first register the image in the **Image Management** tab.
    * Image link: Enter the link that connects when the image is clicked (URL including http:// or https://).
    * All images used in Carousel Commerce must have the same ratio.
6. You can insert web links, app links, bot keywords, message forwarding, channel addition, consultation chat conversion, bot conversion, and business form buttons.
    * Basic type up to 5, carousel/wide item list up to 2
7. If you need to emphasize coupons in messages, you can add buttons to redirect to attached coupons when clicked.
8. Select whether to configure alternative delivery messages.
    * Alternative delivery is only available when you set alternative delivery to **Use** in the **080 Opt-out Management** tab and integrate the SMS service.
9. Add recipients.
    * Recipients can be entered in mobile phone number format.
10. Select targeting type for each recipient.
    * Brand Messages are advertising messages that can be sent to users who have consented to receive marketing messages from advertisers (hereinafter referred to as "marketing consent") and channel friend users.
    * Message reception by specified recipients may vary depending on the targeting type.
        * M: Advertiser marketing consent users (KakaoTalk reception consent)
            * Sends advertising messages to advertiser marketing consent users (KakaoTalk reception consent).
        * N: Advertiser marketing consent users (KakaoTalk reception consent) - Channel friends
            * Sends advertising messages to advertiser marketing consent users (KakaoTalk reception consent) excluding channel friends.
        * I: Advertiser delivery request targets ∩ Channel friends
            * Sends advertising messages only to channel friends among advertiser delivery request targets.
11. After completing the input, click **Send** to transmit.

<a id="mass-delivery"></a>

### Mass Delivery

This function allows you to send Brand Messages to multiple recipient numbers using Excel/CSV format template files. Select **Mass Delivery** in the bottom tab.

![friendtalkupgrade_mass01.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade_mass01.png)

* After entering template replacement variables in the content in '#{name}' format and clicking the **Download Template** button, you can download CSV and XLSX files containing template replacement variables.
* CSV files may not save Korean characters properly when opened and saved in Excel.
* Only CSV and XLSX files can be uploaded. The maximum size is 30MB, and the maximum number of recipients for delivery is 1,000,000.

![friendtalkupgrade_mass02.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade_mass02.png)

* You can enter individual targeting values and template replacement variable values for each recipient.
* If you enter content without template replacement variables, the same content will be sent to all recipients.

When clicking the **Send** button, you can select **Proceed After Review** or **Send Immediately**.

* Proceed After Review: After confirming message recipients within 7 days in the **Mass Delivery Query** tab, AlimTalk is sent. (Not supported for scheduled delivery.)
* Send Immediately: AlimTalk is sent immediately without confirming recipients. You can check the delivery status in the **Mass Delivery Query** tab.

<a id="fallback"></a>

### Alternative Delivery
This function allows alternative delivery of content as text messages when Brand Message delivery fails.
You can send by selecting **Send with Common Content** or **Set Different Messages per User**.

![friendtalkupgrade_06_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_06_20250729.png)

* Alternative delivery is only available when you set alternative delivery to **Use** in the **080 Opt-out Management** tab and integrate the SMS service.
* A sender number is required to send alternative messages. If alternative delivery settings or sender number are not configured, the default settings specified in the **Alternative Delivery Management** tab are applied.
* Alternative delivery occurs after the KakaoTalk delivery result changes to failure, and it may take up to several tens of seconds to receive the text message.
* Depending on the message, some content such as buttons and links may appear differently from KakaoTalk messages.
* Alternative delivery will be sent as SMS/LMS depending on message length (separate usage fees apply for each type of text delivery).

<a id="set-as-common-content"></a>

#### Send with Common Content

![friendtalkupgrade_resend01_20250729.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_resend01_20250729.png)

* After selecting **Sender Profile**, click **Send with Common Content** to configure the alternative delivery message.
* If no alternative delivery message is entered, it will be sent as [Message Content].

<a id="set-by-different-messages-per-user"></a>

#### Set Different Messages per User
* After selecting **Sender Profile**, click **Set Different Messages per User** to configure the alternative delivery message in the **Add Recipient** tab below.
* Clicking the [pen icon] next to added recipient numbers allows you to check and modify the configured content.

!!! danger "Precautions When Sending Advertising Messages"
Advertising messages refer to messages about information, goods, or services sent by the sender for economic gain.

    **[Advertising Message Delivery Guide]**
    1. Advertisement Display
        ① You must display (Advertisement) at the beginning of messages containing advertising content.
        ② Criteria for determining advertising messages
            a. Special price/discount product information
            b. Promotions or events for product and service promotion
            c. Cases where content from [a, b above] is mixed, even if information is presented as the 'main' content
    2. Sender Contact Information Display Requirements
       ① Sender name and sender contact information must be written above the message body.
       ② Sender name and sender contact information are displayed at the top of the chat room.
       ③ For contact information, you must choose and enter either a phone number or address.
    3. Requirements for Displaying Measures and Methods to Easily Express Intent to Refuse Reception
       ① You must clearly specify in the advertisement body the measures and methods that allow easy expression of intent to refuse reception and withdraw reception consent.
       ② If refusal of reception or withdrawal of reception consent cannot be easily achieved or is impossible through such measures and methods, it is considered as not having displayed them.
       ③ Requiring additional measures such as logging into a website to refuse email reception constitutes a law violation as it makes refusal of reception or withdrawal of reception consent difficult.

![\[Figure 3\] FriendTalk Advertisement Message](https://static.toastoven.net/prod_alimtalk/friendtalk_02.png)

<a id="view-send"></a>

## View Send

<a id="view-send-result"></a>

### View Send Result

![friendtalkupgrade_07_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_07_20250729.png)

* You can view detailed information about each sent message by clicking on the search results item in the **View Details** screen.
* **Request Date** can be searched for up to one month.
* You can check the status of the sending request in the **Request Status** column.
* You can check the sending process status in the **Send Result** column.

<a id="cancel-send"></a>

### Cancel send

Cancellation is possible for scheduled send with a sending request date set to the future during regular send.

* When you view a scheduled send request, you can see a checkbox to the left of the request ID.
* The checkbox only appears for reservation requests that have not been canceled.
* Select the checkbox for the request you wish to cancel and click the Cancel Selected Schedule button at the top to cancel the schedule.
* You can select or deselect the entire list by checking the checkboxes in the header of the list.

<a id="view-mass-delivery"></a>

## View Mass Delivery

<a id="view-send-result-2"></a>

### View Send Result

![friendtalkupgrade_masssearch01_20250729.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_masssearch01_20250729.png)

* You can select each mass delivery request and check the recipient’s send results.
* You can view detailed information about each sent message by clicking on the search results item in the **View Details** screen.
* **Request Date** can be searched for up to one month.
* You can check the status of the sending request in the **Request Status** column.
* You can check the sending process status in the **Send Result** column.

<a id="manage-image"></a>

## Image Management

![friendtalkupgrade_8_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_8_20250616.png)

You can register or delete images for use in brand messages and check the information of registered images.

* You can select and register images by type (basic/wide, wide item list, carousel feed, carousel commerce, business form).
* When you delete an image, the image becomes unavailable.
* When updating a template, if the image is changed to a different one, the existing image is deleted from the Kakao CDN and the URL becomes invalid. Other templates using the same image are also affected, so caution is required. Although image information is retained in the image retrieval API or the console, the actual image cannot be accessed, so it is recommended to keep the original file separately on your own server.
* You must comply with file specifications and recommended/restricted sizes.
* You can copy the image address URL. This URL is used when sending via API.
* Business forms are business tools that support the design of events such as reservations, surveys, and applications that KakaoTalk users can easily participate in. Create a business form and enter its ID.
* [\[Go to Business Form Registration\]](https://business.kakao.com/talkbizform/)
* [\[Go to Business Form Registration Guide\]](https://kakaobusiness.gitbook.io/main/tool/bizform)

<a id="image-upload-allowance"></a>

#### Image Upload Allowance

* File format: JPG, PNG
* Check the specifications of each image upload API.

<a id="manage-video"></a>

## Video Management

![friendtalkupgrade_video_management.png](TODO: 콘솔 캡처 후 NHN static 호스트 URL 교체)

You can register or delete videos for use in Brand Messages and view information about registered videos.

* You can register or delete videos after selecting a sender profile.
* For video upload, select a file in the console screen and click the upload button to display the progress. Be careful not to refresh or close the page while uploading. If a refresh occurs during the process, the upload will be interrupted and you will need to upload again.
* Registered videos can be used for sending after encoding is completed at Kakao Biz Center. Encoding usually takes 5 to 10 minutes and can take up to 3 days depending on the video length. If it exceeds 3 days, it is automatically processed as `ERROR` status.
* Video status is synchronized periodically with Kakao Biz Center. Only videos with `PUBLIC` status can be used for template registration and sending, while videos with `PRIVATE` status can only be used for template registration.
* Registered videos are permanently stored on Kakao's side, and even if you delete a video from the console, the video in Kakao Biz Center is not automatically cleaned up. Kakao Channel administrators can delete them directly from the management screen of the Channel Business Home.
* When deleting a video, be careful as templates that use that video cannot be sent.

<a id="video-upload-allowance"></a>

#### Video Upload Requirements

* File formats: MP4, MOV, AVI
* Maximum file size: 4 GB
* Maximum video length: 4 hours
* Maximum resolution: 8K

<a id="manage-templates"></a>

## Manage Templates

![friendtalkupgrade_09_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_09_20250729.png)

* You can manage templates and use them for sending in a similar way to existing AlimTalk messages.
* ******Brand messages are free to create, edit, and delete, with no review process.******
* Unlike AlimTalk, this is a method where users do not register a template code, but instead receive a random identifier from Kakao.

<a id="080-opt-out-management"></a>

## 080 Opt-Out Management

* In Brand Message, the '080 Opt-Out Management' and 'Fallback' features are integrated and managed with a single smsAppkey through integration with NHN Cloud's SMS service.

<a id="register-and-manage-080-opt-out-numbers"></a>

### Register and Manage 080 Opt-Out Numbers
* For Brand Message manual user sending, messages can be sent to recipients who are not friends with the sender profile, so you must register an 080 opt-out number in the sender profile.
    * Since the sender profile 080 opt-out number is data that applies collectively to all sender profiles of other organizations, other projects, and other dealers in the same talk channel, it must be changed carefully.
* If you need to register or modify an 080 opt-out number in the sender profile, contact us through **NHN Cloud Customer Support > Contact Us**.
* When an 080 opt-out number is registered, the sender profile owner has the responsibility to manage so that messages are not sent to recipients who have expressed their intention to opt out to that number.
* By integrating with NHN Cloud SMS service, you can easily manage opt-out for 080 opt-out numbers.
  ![sms 080](https://static.toastoven.net/prod_sms/SMS_14_20230818.png)
* To manage 080 opt-out numbers with the SMS service, you must first activate the SMS service.
* Register the 080 opt-out number you want to manage in the SMS service.
    * If the 080 opt-out number registered in the sender profile is not registered in the SMS service, all sending requests will fail.
* For more information, see [SMS User Guide](https://docs.nhncloud.com/ko/Notification/SMS/ko/console-guide/#080).
  ![friendtalkupgrade_10_20250616.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_10_20250729.png)
* Enter the app key of the SMS service that registered the 080 opt-out number in the 080 Opt-Out tab and save.
* Whether recipient management is possible with the 080 opt-out number registered in the sender profile from the SMS service designated as the management target can be checked in the **SMS Project 080 Number Management Status** item of the corresponding sender profile in the **080 Opt-Out Management** tab of the console.
* When the SMS service app key is emptied and saved
    * You must directly manage opt-out recipients and no verification is performed for recipients during the Brand Message sending process.

<a id="manage-fallback"></a>

### Manage Fallback
* When Brand Message sending fails, you can set it to send the content as a text message as fallback.
* You must be using NHN Cloud SMS service, and messages are sent as SMS/LMS depending on message length (separate fees apply for each type of texting).
* When you modify the SMS app key, the fallback settings for channels of all search IDs are reset.
* Since fallback is made in the SMS service, field values must follow the API specifications for SMS (e.g., Sender number registered at the SMS service, or restriction in the field length).
* Fallback titles or content that exceed the byte limit of the specified fallback type may be truncated when sent as fallback (refer to [SMS API Guide > Cautions](https://docs.nhncloud.com/ko/Notification/SMS/ko/api-guide/#_1)).
* The content for fallback is sent in EUC-KR, and unsupported emoji will fail for fallback.
* For channels of the search ID whose fallback settings you want to modify, click **Edit** to modify **Fallback Status** and **Sender Number**.
* When **Fallback Status** is set to **Disabled**, channels of that search ID cannot send fallback when sending Brand Messages.
* Brand Message advertising messages are sent as fallback through the advertising SMS API, so you must register an 080 opt-out number for fallback to work.
* When entering the resendContent field for Brand Message advertising messages, you must enter the advertising text of the SMS advertising API for fallback to work. `(Ad)Content[Free opt-out]080XXXXXXX`
* If there is no resendContent field for Brand Message advertising messages, the advertising text is automatically generated with the registered 080 opt-out number and sent as fallback.

<a id="apply-for-using-non-friend-message-sending-targeting-m-n"></a>

## Apply for using non-friend message sending (targeting M, N)

* If you wish to use non-friend message sending (targeting M, N), you must apply for use. If you do not apply for use, the M/N type will not be displayed during send.
* Applications for use will be approved if they meet the following conditions:
  * Business verification channel
  * Register a business registration number
  * Register a channel customer service center phone number
  * 50,000 or more channel friends
  * Successfully send notification messages within the past three months

<a id="cautions"></a>

### Cautions
* If your business verification is canceled, the permission to send non-friend messages (targeting M, N) will be canceled. You will need to reapply for use after your business verification has been re-reviewed.
* The consent evidence file for receiving advertising information is saved per Talk channel, so any changes will be applied to all profiles sent by other dealers in the same Talk channel.
* If a file uploaded by a dealer already exists, you can skip the file upload process and apply to send non-friend messages (targeting M, N).

![friendtalkupgrade_11_20250715.png](https://static.toastoven.net/prod_alimtalk/friendtalkupgrade/friendtalkupgrade_11_20250729.png)