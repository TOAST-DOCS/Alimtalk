## Notification > KakaoTalk Bizmessage > Release Notes
### April 26, 2022
* [API] Added public fields for SMS alternative delivery
    * Made improvements so that the statsId, senderGroupingKey, and recipientGroupingKey fields are added when resending SMS messages for Alimtalk/Friendtalk.
* [API] Improved validation of template for Alimtalk full text delivery
    * Improved validation to allow whitespaces for template replacement when sending Alimtalk full text.

### Mar 29, 2022
* [Console] Changed the base date for backup of  delivery results
    * The base date for backup of Alimtalk delivery results has been changed from 180 days to 90 days.
* [Console] Improved the feature to download general, mass, and tag delivery results
    * For Excel download, changed to create a .zip file for more than 1 million results.

### January 11, 2022
* [Console] Fade-out of (Old)Statistics
    * Due to introduction of new statistics, the (Old)Statistics tab has been deleted.
* [Console] Limit of maximum 5,000 members for the sender profile group
    * In accordance with Kakao policy, applied the limit of 5,000 to the maximum number of members in a sender profile group.
* [Console] CloudTrail applied
    * CloudTrail has been applied, so you can check the usage history.

### December 2, 2021
* [API] Changed the Alimtalk template inquiry API
    * Due to changes in the Kakao API specification, if you inquire about a template that is in the Rejected status, the template will be changed to the 'Inspection Underway' status.
* [API] Fixed a bug in the API to list templates
    * Modified so that the categoryCode field value is responded normally.

### October 26, 2021
* [API] Added Alimtalk/Friendtalk APIs to list mass delivery information
    * Alimtalk/Friendtalk APIs to list mass delivery information have been added.
* [API] Added statistics API
    * A statistics API has been added.
* [API] Added chatExtra and chatEvent fields
    * A chatExtra field has been added to the BC (Bot for Consultation) type button when sending a message.
    * chatExtra and chatEvent fields have been added to the BT (Bot Transfer) type button when sending a message.
* [API] Added an outlink feature to the web link type button
    * This is a feature to open the link with the browser of the mobile device instead of the in-app browser when clicking the button.
    * An outlink feature has been added, by adding a target field when sending a message.
* [API] Added linkMo and linkPc fields to the app link type button
    * linkMo and linkPc fields have been added to the app link type button.

### August 24, 2021
* [Console] Image Alimtalk feature
    * An image Alimtalk feature has been added.

### July 27, 2021
* [Console] New statistics feature
    * New statistics has been added to enhance the functionality.
    * The collection for (Old)Statistics service will be performed until July 31, 2021, and will end on December 31, 2021.

### June 29, 2021
* [Console] Webhook feature for the update of the sending result
    * A webhook feature for the update of the sending result has been added.

### May 25, 2021
* [Console] Added a Kakao template code field
    * A field for template code that is actually registered in Kakao has been added.
* [Console] Webhook feature for the change of template status/inquiry content
    * A webhook feature for the change of template status/inquiry content has been added.
* [API] Added price and currencyType fields for delivery
    * price and currencyType fields, which are Alimtalk advertisement moment fields, have been added.
    * A messageOption field has been added to the Alimtalk delivery API and single message retrieval API.

### April 27, 2021
* [Console] Added a feature to register the same sender's profile
    * Improved the system so that the same sender's profile can be registered for other projects.
    * Even though it is the same sender's profile, its data such as template/sender profile per project and profile group/send history is independently treated.

### March 23, 2021
* [Console] Added a sender profile group feature
    * Templates evaluated and approved as a group can be used by the sender profile belonging to the group.
    * It is useful when the same template is shared among multiple sender profiles.
* [Console] Added a feature to delete sender profiles
    * Added a feature that deletes sender profiles regardless of their status.

### January 26, 2021
* [Console] Added a feature to back up the Friendtalk delivery results.
    * Added a feature to back up the Friendtalk delivery results.
    * An Excel file containing the delivery results can be created based on the search conditions used on console.
    * Delivery result files are deleted in 7 days.

### November 24, 2020
* [API] Alimtalk template category code added
    * Category Code field added for registration or modification of Alimtalk template.
    * The template with a category entered is screened first.
* [Console] Backup function of Alimtalk sent list data more than 180 days old added
    * A function that adds a backup file to customers' object storage or AWS S3 for a sent list (normal/batch) more than 180 days old has been added.
    * Backup settings can be set in the **Send Settings** tab.
* [API] Alimtalk template code limit changed
    * Changed to allow the following in the Alimtalk templateCode field: (alphabet letters, numbers, -, _).

### October 27, 2020
* [API] Changed the fields of the Alimtalk that are exposed to/hidden from PC
    * The pcFlag field has been changed to securityFlag field. (Default: false)
    * A field used to show whether there is a security template. It must be configured for security messages like OTP.
    * If it is true, the message text will not be displayed on any devices except the main device.
* [API] Changed the type of the Add Channel button for Alimtalk
    * Changed the type of the **Add Channel** button from CA to AC.
    * The **Add Channel** button can be registered only when it is on top of the buttons list or it is the only button available.
      *The name of the **Add Channel** button can only be 'Add Channel.'
    * Buttons of which type is changed to AC will be displayed as a new button with the highlight effect and new icon.
* [Console] Added a feature to back up the delivery results of Alimtalk.
    * Added a feature to back up the delivery results of Alimtalk.
    * An Excel file containing the delivery results can be created based on the search conditions used on console.
    * Delivery result files are deleted in 7 days.
* [Console] Improved the detailed information modal for Friendtalk delivery results
    * The detailed report of Friendtalk delivery results now provides additional information such as the presence of ads and the result of alternative delivery request.

### August 25, 2020
* [API] Show/Not Show Alimtalk on PC
    * Added the feature of selecting Show/Not Show on PC, when registering a template
* [Console] Supports Excel Files for Bulk Delivery
    * Allows excel extension for sending bulk messages, or for uploading recipients' file

### July 28, 2020
* [API] Template Emphasizing Alimtalk Messages
    * Officially added as a feature, with CBT closed.
* [API] More Types for Alimtalk Template Messages
    * Expanded types for Alimtalk template messages (BA: Basic, EX: Extra Information, AD: Ads Included, MI: Mixed Purposes)
* [API] Query of Attachments for Alimtalk Templates
    * Added the feature of querying on Alimtalk templates with files attached

### June 23, 2020
* [API] Allowed Alimtalk Emphasized template
    * It has been changed to allow emphasized template for Register Template API

### May 26, 2020
* [API] Friendtalk in Wide Images
    * Added the feature of uploading and sending Friendtalk messages in wide images.
* [Console] Delete Plus Friends with Unregistered Tokens
    * Added the feature of deleting Plus Friends with unregistered tokens

### November 26, 2019
* [Console] Template Registration Using File Uploads
    * Added the feature of file uploading for mass templates
* [Console] Template Query Upgrades
    * Added the feature of querying both original template status and approval status for final templates
* [Console] Query by Registered Date for Delivery Results
    * Added the feature of querying by registered date for the query of delivery results

### October 29, 2019
* [API] Tighter validity checks for the delivery of certification messages
    * Message delivery is unavailable when authentication message is not included
    * For more details, see [[API User Guide](./alimtalk-api-guide/#precautions-authword)].

### September 24, 2019
* [Console] Canceling Scheduled Delivery of Alimtalk/Friendtalk
    - Added the feature of canceling scheduled delivery of Alimtalk/Friendtalk from the **Query Delivery Result** tab, if it is yet to be delivered.
    - Canceling is available by querying time after scheduled delivery is requested.
* [Console] Validity Checks Added for Uploading Bulk Files for Alimtalk/Friendtalk Delivery
    - With validity checks for uploading bulk delivery files, you can receive feedbacks before delivery.
* [Console] Maximum Recipients Raised for Bulk Alimtalk/Friendtalk Delivery
    - Increased the number of maximum recipients for Alimtalk/Friendtalk from 10,000 to 100,000.
* [Console] Name Change from Kakaotalk PlusFriend to Kakaotalk Channel
    - As of September 17 of 2019, the service name has changed from 'PlusFriend' to 'Kakaotalk Channel'.

### July 30, 2019
* [Console] Field Added for Result Code of Alternative SMS Delivery Request
    - To query details of alternative delivery message, result code of SMS request has been added.
* [System] Server Replacement for Service Stabilization

### June 27, 2019
* [Console] Allowed alternative delivery, and added split delivery, for mass delivery of Friendtalk messages
    - Fields related to alternative delivery can be specified, such as content of alternative delivery/sender number/alternative delivery.
    - Features have been added to send in splits by specifying split times/interval.
* [API] Added API to cancel scheduled delivery of Friendtalk
    - Scheduled Friendtalk message can be cancelled, if it is yet to be delivered.
* [API] Added API to cancel scheduled delivery of Alimtalk for authentication
    - Scheduled Alimtalk message for authentication can be cancelled, if it is yet to be delivered
* [Console] Improved mass delivery of Alimtalk/Friendtalk
    - With [Proceed after Inspect], notification mail is sent, unless Send is clicked.
      + Email receiving targets: All project members
      + Mail delivery condition: Click [Proceed after Inspect], and send two times in total, including one time after a day, and another in 6 days
    - For mass scheduled delivery, Proceed after Inspect is not available.
* [Console] Improved Search of Plus Friends
    - To search for a Plus Friend, search by conditions has been added.
* [Console] Fixed bugs in messages
    - Fixed errors in messages for the status of Plus Friend, and deleting templates.
* [Console] <b>Introduced business certification</b> when registering PlusFriend
    - When registering PlusFriend, the PlusFriend <b>must be certified for business</b> [[Related announcement](https://center-pf.kakao.com/notices/311)]


### May 28, 2019
* [API] For delivery, a country code can be included in recipient numbers.
    - The recipientNo field can now include country code for delivery.
    - Available to send to users authenticated for overseas mobile phone on the Kakaotalk application.
* [API] Added v1.3 for Alimtalk Delivery
    - The field configuration related for alternative delivery for Alimtalk Delivery API has been updated to the same level of Friendtalk Delivery API.
* [API] Added Set Alternative Delivery API
    - Set Alternative Delivery API for PlusFriend has been added.
* [API] Updated Query PlusFriend API
    - Pagination has been added to Query PlusFriend API.
    - Response field of Alimtalk/Friendtalk alternative delivery has been added to Query PlusFriend API. (v1.3)


### April 30, 2019
* [Console] Rolled back the business verification method used when adding a Plus friend
    - Reverted the way to verify a Plus friend's business to the previous method because the previous method took too much time to verify.

### April 23, 2019
* [Console] Added a feature to be used to specify <b>alternative delivery and split delivery </b>when mass sending Alimtalk messages
    - Added a feature that specifies the fields related to alternative delivery, including the alternative delivery content/sender number/whether to apply alternative delivery fields.
    - Added a feature of split delivery over a specific number of times in a specific interval.
* [Console] Separated the <b>alternative delivery settings</b> of Alimtalk/Friendtalk
    - Added a feature that configures the alternative delivery for each Plus friend on Alimtalk/Friendtalk.
	<br>For example,) when only Alimtalk is set to deliver alternatively for the same Plus friend, Friendtalk will not use the alternative delivery even if it fails to deliver.
* [Console] Introduced the <b>business verification feature</b> when adding a Plus friend
    - Only the friends whose <b>business is verified</b> can now be added as Plus friends. [[Related notice:](https://center-pf.kakao.com/notices/311)]
* [Console] Added the Like search feature in the template selection modal window
    - Added the Like search feature to the template selection modal window so that templates can easily be selected. (Use template code and name to search templates)
* [API] Improved the system so that advertisement Friendtalk and <b>advertisement SMS API</b> can be used for alternative delivery
    - Improved the system so that advertisement SMS API can be used as an alternative when advertisement Friendtalk delivery fails.
    - For alternative delivery to work, an 080 opt-out number must be registered when configuring the advertisement Friendtalk alternative delivery.
* [API] Advanced Friendtalk's alternative delivery API
    - Added the alternative delivery title field for delivery.
    - The title/content/sender number/080 opt-out number/whether to use alternative delivery can be selected using the alternative delivery field.
* [API] Improved the system so that it will not store the Alimtalk/Friendtalk delivery failure messages
    - Improved the system so that it will not store delivery recipient's length limit, invalid receiver number and other request error messages.
    - API response field sendResults can be used to check if the request succeeded/failed.

### March 26, 2019
* [Console] Added the Alimtalk preview UI
    - Added the UI for Alimtalk inbox screen preview.
* [API] Added the Auth API for sending an Alimtalk for verification
    - The send pool has been separated as the Auth API for sending a verification Alimtalk.

### February 26, 2019
* [Console] Fixed the bug in which delivery fails when mass sending Alimtalk messages
    - Fixed a bug in which delivery would fail due to some invalid recipient numbers.
* [Console] Fixed a bug in which batch delivery recipient numbers would be masked when mass sending Friendtalk messages.
    - Fixed a bug in which recipient numbers are masked when a Friendtalk message is delivered to a large number of recipients and their details are looked up.

### January 29, 2019
* [API] Added Friendtalk v1.2 API
    - Added the sender/receiver grouping key field when sending a message.
    - Added the <b>request success/failure</b> field per recipient in the delivery response field.
* [API] Added an API that views Friendtalk delivery result update
    - Added a new API that views the results based on the result-updated time.
* [Console] Advanced the Alimtalk delivery screen
    - Improved the system so that messages can be sent to multiple recipients.
    - Improved the system so that users can now schedule a delivery.
    - Improved the system so that uses can now select the body text for alternative delivery.
* [Console] Added a feature of mass sending Friendtalk messages
    - Users can now send a Friendtalk message to a large number of recipients using a csv file.
* [Console] Added a feature that views Friendtalk messages that were sent to a large number of recipients
    - Users can now look up messages that they sent to a large number of recipients from the Friendtalk bulk delivery tab.
* [Console] Advanced the Alimtalk template edit feature
    - Previously, only rejected templates could be edited. Approved templates can also be edited now.
    - When an approved template is edited, it will be reviewed again. After that, its content will <b>replace the content of the existing template</b>.
    - While the template is in the review stage, the previously approved template can be normally delivered.
* [Console] Added a feature that is used to mask recipient numbers
    - Added a feature that is used to mask recipient numbers for projects that were individually asked to do so.
* [Console] Fixed a bug that would occur while counting the number of words in the Alimtalk template body
    - Fixed a bug in which a blank space is counted as 2 characters in the template body.

### December 4, 2018
* [API] Added Alimtalk v1.2 API
    - Added the sender/receiver grouping key field when sending a message.
    - Added the <b>request success/failure</b> field per recipient in the delivery response field.
* [API] Added an API that views the updated Alimtalk delivery result
    - Added a new API that views the results based on the result-updated time.
* [API] Added the alternative delivery sender number field when sending a message
    - Added a feature that specifies an alternative sender number via the resendSendNo field when sending a message.
* [API] Improved the system so that it will retain sent messages up to 90 days
    - Improved the system so that the data created more than 90 days ago will not be displayed when retrieving data.
* [Console] Added the detailed status of a Plus friend
    - Added the TOAST Plus friend, Kakao Plus friend, Kakao Plus friend profile status fields to the Plus friend view screen.

### November 13, 2018
* [API] Advanced Alimtalk delivery API alternative delivery
    - Added the alternative delivery title field for delivery.
    - The name of LMS can be specified when performing alternative delivery with LMS using the alternative delivery title field.
    - Changed to use the alternative delivery when the request fails with template context mismatch (-3010) or template button mismatch (-3011). Messages that don't need alternative delivery can be controlled using the isResend field.
* [API] Fixed a bug regarding scheduled delivery
    - Fixed a bug in which scheduled deliveries could not normally be canceled due to an error in the date data of the request ID when sending a scheduled message.

### October 23, 2018
* [API] Added a feature that schedules Alimtalk delivery API
    - A message can be sent at any time using the schedule feature.
    - The scheduled message can be canceled at any time before it is sent.
    - For more information, see [[Alimtalk delivery API](./alimtalk-api-guide/#_3)].
* [API] Advanced Alimtalk delivery API alternative delivery
    - Added the alternative delivery type (SMS/LMS), whether to use alternative delivery (true/false), alternative delivery content fields for delivery.
    - The alternative delivery can be used at scale using the fields.
* [API] Added the API related to Plus friends and templates
    - Added the APIs for Plus friend category view, business license upload, registration, token authentication, and list view.
    - Added the API for template registration, edit, delete, and query.
* [Console] Added a feature that searches for Likes to the template code
    - Added the Like search feature when viewing templates on console.

### August 28, 2018
* [Console] Improved the delivery result view
    - Improved the system so that template code can be manually entered when viewing Alimtalk delivery results.

### July 24, 2018
* [Console] Changed the name of the service
    - The service name of Alimtalk has been changed to KakaoTalkBizmessage.
* [Console] Added a Friendtalk feature
    - Friendtalk targets users who became friends, and can be used to send advertisement messages.
    - The message delivery, view, image management, statistics features are provided on console.
    - For more information, see [[Friendtalk overview ](./friendtalk-overview/)].
* [API] Added the Friendtalk API
    - The Friendtalk API supports the message delivery, list view, single item view, image management features.
    - For more information, see [[Friendtalk API guide](./friendtalk-api-guide/)].
* [API] Added a limit to the number of Alimtalk and Friendtalk messages sent
    - Added a feature to limit the number of messages to 1,000 a day, which are sent to the Plus friends who became friends since the maintenance on July 24.
    - Delivery requests exceeding this limit will fail.
* [API] Edited the resending feature after a failed delivery
    - The body text of the resending was changed as below:

```
Delivery body text
- Web link button name: Web link
- Web link button name 2: Web link 2
...
```

### June 26, 2018
#### Feature Updates
* [Console] Added the request field when adding a Plus friend
    - To comply with the latest Kakao issuance procedure, a business license number and business category are needed when adding a Plus friend.

#### Bug Fixes
* [Console] Changed the statistics chart to a version where bugs are fixed
    - Changed the statistics chart to a version where the bugs related to exporting .xls files on IE are fixed.

### May 29, 2018
#### More Features
* [API] Added the API that views a single delivery result
    - Added the API that views a specific delivery result.
    - For more information, see [[Alimtalk API guide](./alimtalk-api-guide/#_14)].
* [API] Added delivery result list view API v1.1
    - Added v1.1 API as the recipientSeq (Recipient sequence) field is added to response.

#### Bug Fixes
* [API] Fixed a bug related to the substitution delivery API
    - Fixed a bug in which the templateParameter field of Request Body would be recognized as a required field.

### April 24, 2018
#### More Features
* [Console] Added a feature that is used for template chat bubble
    - Added a feature for multiple button template.
    - Added new button types: (delivery view, web link, app link, bot keyword, and message forwarding)
* [API] Added the API for viewing templates
    - Added the API that views templates.
    - For more information, see [[ Alimtalk API guide ](./alimtalk-api-guide/#_46)].

#### Feature Updates
* [API] Edited the resending feature after a failed delivery
    - Fixed the system so that messages are sent as SMS or LMS according to their length.
    - Changed the body text of resending message: (Body + web link button name + button link)

### March 22, 2018
#### More Features
* [Console] Added a feature that is used to edit or delete templates and register queries
    - Added a feature that is used to edit or delete templates.
    - Added a feature that is used to register a query to reviewer.
    - For more information, see [[Alimtalk console user guide ](./alimtalk-console-guide/#_12)].
* [API] Added the API that is used to send full body text
    - Added the API that is used to send a message in full text, not in substituted data.
    - For more information, see [[ Alimtalk API guide ](./alimtalk-api-guide/#_4)].

### February 22, 2018
#### More Features
* [Console] Added a feature that is used to send messages to a large number of recipients
    - Users can now send a Friendtalk message to a large number of recipients using a csv file.

### January 25, 2018
#### Feature Updates
* [Console] Improved template registration
    - Added a feature that is used to remove spaces in front and back of a button name.
    - Edited the length limit: (Template code: 10 characters -> 20 characters, button name: 10 characters -> 14 characters)
    - Modified the system so that user can manually enter the button name when registering a delivery view template.

#### More Features
* [API] Added the API to view delivery result

### November 23, 2017
#### More Features
* [Console] Register multiple Plus friends
    - Previously, only 1 Plus friend could be added at a time. Now, multiple Plus friends can be added at a time.
* [Console] When a template is rejected, provides the reason.
* [API] Added the delivery API field as now multiple Plus friends can be added at a time
    - Added 12 plusFriendId fields to requestBody.
    - The message will be sent to the Plus friend ID who was added first when plusFriendId is left empty.

### August 24, 2017
#### More Features
* [Console] Provided the Alimtalk delivery statistics screen
    - Provides the by date/by time/by day of a week statistics screen.
    - Allows you to look up by delivery date and template.

#### Feature Updates
* [Console] Changed the URL verification when adding a free button template
    - http:// or https:// must be included when adding a URL to a free button -> Like #{url}, the template substituter can now be added.
    - If it is not a template substituter in the form of #{url}, the http:// or https:// verification will be maintained.
* [API] Modified the error response message for content-type
    - Modified the failure response message if it's not content-type: application/json in the request header.

### July 20, 2017
#### More Features
* [Console] Added Alimtalk delivery result view
    - The message delivery result can be viewed with conditions such as date of delivery, recipient number, and template.

### June 22, 2017
#### Feature Updates
* [Console] Added the sender profile management page
* [Console] Added the test delivery page
* [Console] Added the added template view page

#### More Features
* [Console] Added the delivery failure setting
    - This is a feature that is used to send the message via LMS when Alimtalk delivery fails
* [Console] Added a feature that is used to substitute the free button type of a template
    - For template free button type, the button link can be added as a substituter (e.g. buttonURL: #{url})

### May 25, 2017
#### Feature Updates
* [Console] Improved the main page markup

### April 20, 2017
#### New Product Release
    * Alimtalk is a product based on mobile phones with which users can send informative messages such as delivery message, schedule notification, and others without adding the recipient as a friend.
    * It provides RESTful API for users to easily link it to apps.
