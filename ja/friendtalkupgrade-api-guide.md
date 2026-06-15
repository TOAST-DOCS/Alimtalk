## Notification > KakaoTalk Bizmessage > ブランドメッセージ > API v1.0 Guide

<a id="brand-message"></a>

## ブランドメッセージ

<a id="api-domain"></a>

#### [API ドメイン]

| ドメイン                                                                          |
|------------------------------------------------------------------------------|
| [https://kakaotalk-bizmessage.api.nhncloudservice.com](https://kakaotalk-bizmessage.api.nhncloudservice.com) |

<a id="introduce-v10-api"></a>

## v1.0 API 紹介

<a id="manage-non-friend-message-sending-targeting-m-n"></a>

## フレンド以外へのメッセージ送信（ターゲティング M、N）管理

フレンド以外へのメッセージ送信（ターゲティング M、N）は、以下の条件をすべて満たす場合に送信できます。

- ビジネス認証チャンネル
- 事業者番号登録
- チャンネルカスタマーセンター電話番号登録
- チャンネルフレンド数 5 万以上
- 3 か月以内のお知らせトーク送信成功履歴保有

<a id="upload-marketing-consent-evidence"></a>

### マーケティング受信同意根拠資料アップロード

<a id="requested"></a>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/upload-marketing-agreement
Content-Type: multipart/form-data
```

[Path parameter]

| 名前        | 種類     | 説明     |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 種類     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request parameter]

| 名前   | 種類   | 必須 | 説明            |
|------|------|----|---------------|
| file | File | O  | マーケティング受信同意根拠資料 |

<a id="response"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | 種類      | Not Null | 説明     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダー領域  |
- resultCode    | Integer | O        | 結果コード  |
- resultMessage | String  | O        | 結果メッセージ |
- isSuccessful  | boolean | O        | 成功可否  |

<a id="apply-for-using-non-friend-message-sending-targeting-m-n"></a>

### フレンド以外へのメッセージ送信（ターゲティング M、N）使用申請

<a id="requested-2"></a>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/marketing-agreement
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 種類     | 説明     |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 種類     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="response-2"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | 種類      | Not Null | 説明     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダー領域  |
- resultCode    | Integer | O        | 結果コード  |
- resultMessage | String  | O        | 結果メッセージ |
- isSuccessful  | boolean | O        | 成功可否  |

<a id="request-to-send-a-free-form-message"></a>

## メッセージ自由型送信リクエスト

* マーケティング受信同意送信を使用できます。
    * targeting フィールドを指定してメッセージ対象のタイプを指定できます。
        * M: 顧客の広告性情報受信同意ユーザー（カカオトーク受信同意）
        * N: 顧客の広告性情報受信同意ユーザー（カカオトーク受信同意） - チャンネルフレンド
        * I: 顧客の送信リクエスト対象 ∩ チャンネルフレンド
* 既存フレンドトークの8つのメッセージタイプをすべて使用できます。
* BT、AC ボタンタイプを使用できます。
* AC（チャンネル追加）ボタン使用時、次の制約事項があります。
    * 強調型ボタン（黄色）で表記されます。
    * ボタンが複数ある場合、指定された位置で使用する必要があります。
        * TEXT、IMAGE: 1番目のボタン（最上段）
        * その他: 2番目のボタン（右側）
    * ボタン名（name）は「チャンネル追加」で固定されます。
    * カルーセル型は全体カルーセルを通じて1個のみ使用可能です。
    * ターゲティング M、N のみ使用可能です。
* BF ボタン使用時、カカオから発行されたビジネスフォーム ID を使用できます。
* 代替送信は受信者別 resendParameter で設定できます。
    * 代替送信を利用する場合、代替送信管理 API で SMS AppKey 登録および送信設定が必要です。
* **夜間送信制限（20:50〜翌日08:00）**

<a id="requested-3"></a>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/freestyle-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | タイプ   | 説明      |
|--------|--------|---------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ   | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="request-text-type-sending"></a>

#### テキスト型送信リクエスト

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

| 名前                     | 型      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信の有無（デフォルト値：true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 要求日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信されます）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信されます）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| adult                  | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT タイプの場合、最大1,300文字（改行：最大99個、URL形式入力可能）<br>- IMAGE タイプの場合、最大1,300文字（改行：最大99個、URL形式入力可能）<br>- WIDE タイプの場合、最大76文字（改行：最大5個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションで使用できます。最大76文字（改行：最大5個）<br>- その他のタイプの場合、このフィールドは使用しません                           |
| buttons                | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大4個、その他最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大2個<br>- PREMIUM_VIDEO タイプの場合最大1個<br>- COMMERCE タイプの場合最小1個、最大2個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンでのみ使用でき、nameには次の3つの文言のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合に転送するメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合に接続するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合のビズフォームキー                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | titleの場合、5つの形式に制限されます<br>- 「${数字}円割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「配送費割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字。改行：不可<br>- その他のタイプの場合最大12文字。改行：不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大1,000人）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信の有無<br>コンソールで代替送信設定時、デフォルト値で代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信080受信拒否番号<br><span style="color:red">（SMS サービスに登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドのユーザー）                                                                |
| - unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信されます）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信されます）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 発信者グルーピングキー（発信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時に使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールでの送信時にユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | X | 統計 ID（送信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                            |

<a id="request-image-type-sending"></a>

#### 画像型送信リクエスト

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

| 名前                     | 型       | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値: true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 要求日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大 60 日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例: 1234        |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値: false）                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT タイプの場合最大 1,300 文字（改行: 最大 99 個、URL 形式入力可能）<br>- IMAGE タイプの場合最大 1,300 文字（改行: 最大 99 個、URL 形式入力可能）<br>- WIDE タイプの場合最大 76 文字（改行: 最大 5 個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションとして使用できます。最大 76 文字（改行: 最大 5 個）<br>- その他のタイプの場合、このフィールドは使用しません                           |
| image                  | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合必須フィールド                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 画像 URL。一般画像としてアップロードされた画像 URL を使用                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | 画像クリック時に移動する URL。1,000 文字制限<br>未設定時はカカオトーク内画像ビューア使用                                                                                                                                                                                                                            |
| buttons                | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大 4 個、その他最大 5 個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大 2 個<br>- PREMIUM_VIDEO タイプの場合最大 1 個<br>- COMMERCE タイプの場合最少 1 個、最大 2 個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大 14 文字<br>- その他のタイプの場合最大 8 文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、MD: メッセージ転送、AC: チャンネル追加、BT: チャットボット転換、BF: ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンとしてのみ使用でき、name には次の 3 つの文言のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000 文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合に転送するメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合に接続するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合のビズフォームキー                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title の場合 5 つの形式に制限される<br>- "${数字}円割引クーポン" 数字は 1 以上 99,999,999 以下<br>- "${数字}% 割引クーポン" 数字は 1 以上 100 以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大 18 文字。改行: 不可<br>- その他のタイプの場合最大 12 文字。改行: 不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                   |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000 文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大 1,000 名）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信可否<br>コンソールで代替送信設定時、デフォルト値で代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信 080 受信拒否番号<br><span style="color:red">（SMS サービスに登録された 080 受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M: マーケティング受信同意ユーザー、N: フレンドでないマーケティング受信同意ユーザーのみ、I: フレンドユーザー）                                                                |
| - unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例: 1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者ごとにグルーピングキーを指定できます。最大 100 文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 発信者グルーピングキー（発信者ごとにグルーピングキーを指定できます。最大 100 文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時に使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時にユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計 ID（送信検索条件には含まれません。最大 8 文字）                                                                                                                                                                                                                                            |

<a id="request-wide-image-type-sending"></a>

#### ワイド画像型送信リクエスト

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

| 名前                     | 種類      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力は不可<br>例：1234        |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                                                                       |
| content                | String  | O  | - TEXT タイプの場合最大1,300文字（改行：最大99個、URL形式入力可能）<br>- IMAGE タイプの場合最大1,300文字（改行：最大99個、URL形式入力可能）<br>- WIDE タイプの場合最大76文字（改行：最大5個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションで使用できます。最大76文字（改行：最大5個）<br>- その他のタイプの場合、このフィールドは使用しません                           |
| image                  | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合必須フィールド                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 画像 URL、ワイド画像としてアップロードされた画像 URL を使用                                                                                                                                                                                                                                             |
| - imageLink            | String  | X  | 画像クリック時に移動する URL。1,000文字制限<br>未設定時はカカオトーク内画像ビューアを使用                                                                                                                                                                                                                            |
| buttons                | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合クーポン適用時は最大4個、その他は最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大2個<br>- PREMIUM_VIDEO タイプの場合最大1個<br>- COMMERCE タイプの場合最小1個、最大2個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンとしてのみ使用可能で、name には次の3つの文句のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合に渡すメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合に接続するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合のビズフォームキー                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title の場合5つの形式に制限<br>- 「${数字}円割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}％割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UP クーポン」                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字。改行：不可<br>- その他のタイプの場合最大12文字。改行：不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大1,000人）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、SMS 代替送信可否<br>コンソールで代替送信設定時、デフォルト値で代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、［メッセージ本文］で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信080受信拒否番号<br><span style="color:red">（SMS サービスに登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドユーザー）                                                                |
| - unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力は不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別でグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 発信者グルーピングキー（発信者別でグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時に使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時にユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計 ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                            |

<a id="request-to-send-wide-item-list-type"></a>

#### ワイドアイテムリストタイプ送信リクエスト

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

| 名前                     | タイプ      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                                                                       |
| header                 | String  | O  | ヘッダー<br>- WIDE_ITEM_LIST タイプの場合は必須フィールドで最大20文字（改行：不可）<br>- PREMIUM_VIDEO タイプの場合は選択フィールドで最大20文字（改行：不可）                                                                                                                                                                     |
| item                   | Object  | O  | ワイドリスト要素（WIDE_ITEM_LIST タイプでのみ使用可能）                                                                                                                                                                                                                                       |
| - list                 | List    | O  | ワイドリスト（最小：3、最大4）                                                                                                                                                                                                                                                         |
| -- title               | String  | O  | アイテムタイトル<br>- 1番目のアイテムは最大25文字制限（改行：最大1個、1番目のアイテムの場合titleは必須値ではない）<br>- 2〜4番目のアイテムは最大30文字制限（改行：最大1個）                                                                                                                                                                |
| -- imageUrl            | String  | O  | アイテム画像URL<br>- 1番目のアイテムには1番目のワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>- 2〜4番目のアイテムは一般ワイドアイテムリスト画像としてアップロードされた画像URLを使用                                                                                                                                                             |
| -- linkMo              | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | Android アプリリンク、1,000文字制限                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | iOS アプリリンク、1,000文字制限                                                                                                                                                                                                                                                           |
| buttons                | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合はクーポン適用時最大4個、その他最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合は最大2個<br>- PREMIUM_VIDEO タイプの場合は最大1個<br>- COMMERCE タイプの場合は最小1個、最大2個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合は最大14文字<br>- その他のタイプの場合は最大8文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは1番目のボタンとしてのみ使用でき、name には次の3種類の文言のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルWebリンク（WL タイプの場合は必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC Webリンク（WL タイプの場合は選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合は必須フィールド）、1,000文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合は必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合に転送するメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合に連結するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合のビジネスフォームキー                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | titleの場合は5種類の形式で制限される<br>- "${数字}円 割引 クーポン" 数字は1以上99,999,999以下<br>- "${数字}% 割引 クーポン" 数字は1以上100以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合は最大18文字。改行：不可<br>- その他のタイプの場合は最大12文字。改行：不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルWebリンク（WL タイプの場合は必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| - linkPc               | String  | X  | PC Webリンク（WL タイプの場合は選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合は必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合は必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大1,000名）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信可否<br>コンソールで代替送信設定時、デフォルト値として代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンドIDで代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信080受信拒否番号<br><span style="color:red">（SMS サービスに登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドであるユーザー）                                                                |
| - unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 送信者グルーピングキー（送信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時に使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時はユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                            |

<a id="request-to-send-premium-video-type"></a>

#### プレミアム動画型送信リクエスト

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

| 名前                     | タイプ      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値: true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力の場合、発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力の場合、発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                                                                       |
| content                | String  | X  | - TEXT タイプの場合最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- IMAGE タイプの場合最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- WIDE タイプの場合最大 76 文字（改行：最大 5 個）<br>- PREMIUM_VIDEO タイプの場合当該フィールドをオプションで使用可能、最大 76 文字（改行：最大 5 個）<br>- その他のタイプの場合当該フィールドを使用しない                           |
| header                 | String  | X  | ヘッダー<br>- WIDE_ITEM_LIST タイプの場合必須フィールドで最大 20 文字（改行：不可）<br>- PREMIUM_VIDEO タイプの場合選択フィールドで最大 20 文字（改行：不可）                                                                                                                                                                     |
| video                  | Object  | O  | 動画要素（PREMIUM_VIDEO タイプのみ使用可能）                                                                                                                                                                                                                                              |
| - videoUrl             | String  | O  | カカオTV 動画 URL（カカオTV にアップロードされた動画アドレスのみ使用可能）、最大 500 文字制限                                                                                                                                                                                                                         |
| - thumbnailUrl         | String  | X  | 動画サムネイル用画像 URL、一般画像としてアップロードされた URL のみ使用可能（ない場合カカオTV 動画デフォルトサムネイル使用）、最大 500 文字制限                                                                                                                                                                            |
| buttons                | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合クーポン適用時最大 4 個、その他最大 5 個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大 2 個<br>- PREMIUM_VIDEO タイプの場合最大 1 個<br>- COMMERCE タイプの場合最小 1 個、最大 2 個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大 14 文字<br>- その他のタイプの場合最大 8 文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンとしてのみ使用でき、name には次の 3 つの文句のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000 文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合転送するメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合接続するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合ビズフォームキー                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title の場合 5 つの形式に制限される<br>- "${数字}円 割引 クーポン" 数字は 1 以上 99,999,999 以下<br>- "${数字}% 割引 クーポン" 数字は 1 以上 100 以下<br>- "配送費 割引 クーポン"<br>- "${7 文字以内} 無料 クーポン"<br>- "${7 文字以内} UP クーポン"                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大 18 文字。改行：不可<br>- その他のタイプの場合最大 12 文字。改行：不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）となります。                                                                                   |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000 文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）となります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）となります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大 1,000 名）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信可否<br>コンソールで代替送信設定時、デフォルト値として代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗することがあります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信 080 受信拒否番号<br><span style="color:red">（SMS サービスに登録された 080 受信拒否番号でない場合、代替送信に失敗することがあります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドのユーザー）                                                |
| - unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力の場合、発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力の場合、発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別にグルーピングキーを指定できます。最大 100 文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 送信者グルーピングキー（送信者別にグルーピングキーを指定できます。最大 100 文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時ユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計 ID（発信検索条件には含まれません。最大 8 文字）                                                                                                                                                                                                                                            |

<a id="request-to-send-commerce"></a>

#### コマース型送信リクエスト

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

| 名前                     | 種類      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信有無（デフォルト値: true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合、即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| adult                  | boolean | X  | 成人用メッセージ有無（デフォルト値: false）                                                                                                                                                                                                                                                       |
| additionalContent      | String  | X  | 付加情報（最大34文字、改行：最大1個）、コマースタイプでのみ使用可能                                                                                                                                                                                                                                      |
| image                  | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合必須フィールド                                                                                                                                                                                                                                |
| - imageUrl             | String  | O  | 画像 URL。一般画像としてアップロードされた画像 URL を使用                                                                                                                                                                                                                                              |
| - imageLink            | String  | X  | 画像クリック時に移動する URL。1,000文字制限<br>未設定時、カカオトーク内画像ビューアー使用                                                                                                                                                                                                                            |
| commerce               | Object  | O  | コマース（COMMERCE タイプでのみ使用可能）                                                                                                                                                                                                                                                    |
| title                  | String  | O  | 商品タイトル（最大30文字、改行：不可）                                                                                                                                                                                                                                                       |
| regularPrice           | Integer | O  | 正常価格（0〜99,999,999）                                                                                                                                                                                                                                                        |
| discountPrice          | Integer | X  | 割引価格（0〜99,999,999）                                                                                                                                                                                                                                                          |
| discountRate           | Integer | X  | 割引率（0〜100）、割引価格存在時、割引率、定額割引価格のいずれか一つは必須                                                                                                                                                                                                                                   |
| discountFixed          | Integer | X  | 定額割引価格（0〜999,999）、割引価格存在時、割引率、定額割引価格のいずれか一つは必須                                                                                                                                                                                                                            |
| buttons                | List    | O  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大4個、その他最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大2個<br>- PREMIUM_VIDEO タイプの場合最大1個<br>- COMMERCE タイプの場合最小1個、最大2個                                                                                                                 |
| - name                 | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字                                                                                                                                                                                                                    |
| - type                 | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンのみに使用でき、name には次の3つの文句のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                       |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| - chatExtra            | String  | X  | BT タイプボタンの場合、転送するメタ情報                                                                                                                                                                                                                                                   |
| - chatEvent            | String  | X  | BT タイプボタンの場合、連携するボットイベント名                                                                                                                                                                                                                                                       |
| - bizFormKey           | String  | X  | BF タイプボタンの場合、ビズフォームキー                                                                                                                                                                                                                                                            |
| coupon                 | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| - title                | String  | O  | title の場合、5つの形式に制限<br>- "${数字}円 割引クーポン" 数字は1以上99,999,999以下<br>- "${数字}% 割引クーポン" 数字は1以上100以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                                                            |
| - description          | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字。改行：不可<br>- その他のタイプの場合最大12文字。改行：不可                                                                                                                                                                      |
| - linkMo               | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| - linkPc               | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| - schemeAndroid        | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                 |
| - schemeIos            | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                                                   |
| recipientList          | List    | O  | 受信者リスト（最大1,000名）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、SMS 代替送信有無<br>コンソールで代替送信設定時、デフォルト値として代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信080受信拒否番号<br><span style="color:red">（SMS サービスに登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドではないマーケティング受信同意ユーザーのみ、I：フレンドであるユーザー）                                                |
| - unsubscribeNo       | String  | X  | 080 無料受信拒否電話番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080 無料受信拒否認証番号（すべて未入力時、発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グループ化キー（受信者別にグループ化キーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 発信者グループ化キー（発信者別にグループ化キーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時、ユーザー UUID で保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計 ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                            |

<a id="request-to-send-carousel-feed-type"></a>

#### カルーセルフィード型送信リクエスト

##### カルーセル固定置換子（path ベーステンプレートパラメータ）

カルーセルタイプでは、各アイテムに異なる置換子値を適用できます。

* **使用形式**: `キー@$.carousel.list[インデックス]`（例: `productName@$.carousel.list[0]`）
* head（カルーセルイントロ）がある場合、head が `list[0]` を占有し、実際のアイテムは `list[1]` から開始します。
* path ベースキーで値が見つからない場合は、一般キーで fallback されます。

| 区分 | 置換可能フィールド | path 形式 |
|------|--------------|----------|
| カルーセルイントロ（head） | header, content, linkMo, linkPc, schemeAndroid, schemeIos | `キー@$.carousel.list[0]`（head 存在時） |
| カルーセルアイテム | header, message, additionalContent | `キー@$.carousel.list[インデックス]` |
| カルーセルアイテムボタン | linkMo, linkPc, schemeAndroid, schemeIos | `キー@$.carousel.list[インデックス]` |
| カルーセルアイテムクーポン | title, description, linkMo, linkPc, schemeAndroid, schemeIos | `キー@$.carousel.list[インデックス]` |
| カルーセルアイテムコマース | title | `キー@$.carousel.list[インデックス]` |

* **Tail**: 置換子使用不可
* ボタンインデックスは path に含まれません。同じアイテム内のボタンは同一の path 値で置換されます。

> [注意] 置換子キー（`#{key}`）には `@` 文字を使用できません。`@` はシステムで path 区切り文字として使用されます。

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

| 名前                     | 種類      | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）。グループ発信キーは使用不可                                                                                                                                                                                                                                                       |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                         |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                                                                   |
| requestDate            | String  | X  | 要求日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即座に送信）<br>最大60日後まで予約可能                                                                                                                                                                                                                                                   |
| unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNoなしでunsubscribeAuthNoのみ入力不可<br>例：1234        |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                                                                       |
| carousel               | Object  | O  | カルーセル                                                                                                                                                                                                                                                                           |
| - list                 | List    | O  | カルーセルリスト（最小2個、最大6個）                                                                                                                                                                                                                                                        |
| -- header              | String  | O  | カルーセルアイテムタイトル（最大20文字）。カルーセルフィード型でのみ使用可能                                                                                                                                                                                                                                          |
| -- message             | String  | O  | カルーセルアイテムタイトル（最大20文字）、カルーセルアイテムメッセージ（最大180文字）。カルーセルフィード型でのみ使用可能                                                                                                                                                                                                                                    |
| -- imageUrl            | String  | O  | 画像URL（カルーセルフィード型画像でアップロードされた画像のみ使用可能）                                                                                                                                                                                                                                        |
| -- imageLink           | String  | X  | 画像リンク、1,000文字制限                                                                                                                                                                                                                                                              |
| -- buttons             | List    | O  | カルーセルリストボタン一覧 最小1個、最大2個                                                                                                                                                                                                                                                    |
| --- name               | String  | O  | ボタンタイトル<br>- TEXT、IMAGEタイプの場合最大14文字<br>- その他のタイプの場合最大8文字                                                                                                                                                                                                                    |
| --- type               | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BTタイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BFタイプは最初のボタンとしてのみ使用でき、nameには次の3つの文句のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| --- linkMo             | String  | X  | モバイルWebリンク（WLタイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| --- linkPc             | String  | X  | PC Webリンク（WLタイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | AndroidアプリリンクSchemeAndroid（ALタイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                       |
| --- schemeIos          | String  | X  | iOSアプリリンク（ALタイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                                         |
| --- chatExtra          | String  | X  | BTタイプボタンの場合転送するメタ情報                                                                                                                                                                                                                                                   |
| --- chatEvent          | String  | X  | BTタイプボタンの場合接続するボットイベント名                                                                                                                                                                                                                                                       |
| --- bizFormKey         | String  | X  | BFタイプボタンの場合ビズフォームキー                                                                                                                                                                                                                                                            |
| -- coupon              | Object  | X  | クーポン要素                                                                                                                                                                                                                                                                         |
| --- title              | String  | O  | titleの場合5つの形式に制限<br>- "${数字}円割引クーポン" 数字は1以上99,999,999以下<br>- "${数字}% 割引クーポン" 数字は1以上100以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UPクーポン"                                                                                                            |
| --- description        | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合最大18文字、改行：不可<br>- その他のタイプの場合最大12文字、改行：不可                                                                                                                                                                      |
| --- linkMo             | String  | X  | モバイルWebリンク（WLタイプの場合必須フィールド）、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                   |
| --- linkPc             | String  | X  | PC Webリンク（WLタイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | AndroidアプリリンクSchemeAndroid（ALタイプの場合必須フィールド）、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                 |
| --- schemeIos          | String  | X  | iOSアプリリンク（ALタイプの場合必須フィールド）、1,000文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                                                   |
| - tail                 | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                                                                     |
| -- linkMo              | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                                           |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                                            |
| -- schemeAndroid       | String  | X  | AndroidアプリリンクSchemeAndroid、1,000文字制限                                                                                                                                                                                                                                                         |
| -- schemeIos           | String  | X  | iOSアプリリンク、1,000文字制限                                                                                                                                                                                                                                                           |
| recipientList          | List    | O  | 受信者一覧（最大1,000名）                                                                                                                                                                                                                                                             |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                                         |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                                      |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信可否<br>コンソールで代替送信設定時、デフォルト値として代替送信されます。                                                                                                                                                                                                                       |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                                       |
| -- resendTitle         | String  | X  | LMS代替送信タイトル<br>（値がない場合、プラスフレンドIDで代替送信されます。）                                                                                                                                                                                                                               |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                                  |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMSサービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                                 |
| -- resendUnsubscribeNo | String  | X  | 代替送信080受信拒否番号<br><span style="color:red">（SMSサービスに登録された080受信拒否番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                                       |
| - targeting         | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドであるユーザー）                                                |
| - unsubscribeNo       | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx         |
| - unsubscribeAuthNo   | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信）<br>unsubscribeNoなしでunsubscribeAuthNoのみ入力不可<br>例：1234        |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| senderGroupingKey    | String  | X  | 発信者グルーピングキー（発信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                   |
| resellerCode          | String  | X  | リセラーコード（リセラーが送信時使用）                                                                                                                                                                                                                                                       |
| createUser             | String  | X  | 登録者（コンソールで送信時ユーザーUUIDで保存）                                                                                                                                                                                                                                                   |
| statsId                | String  | 	X | 統計ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                            |

<a id="request-to-send-carousel-commerce-type"></a>

#### カルーセルコマース型送信リクエスト

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

| 名前                     | タイプ     | 必須 | 説明                                                                                                                                                                                                                                                                            |
|------------------------|---------|----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）、グループ発信キー使用不可                                                                                                                                                                                                                                               |
| chatBubbleType         | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                                                     |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                                                       |
| requestDate            | String  | X  | 要求日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                                                                                                                          |
| unsubscribeNo          | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                                          |
| unsubscribeAuthNo      | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234                                                                                                                                           |
| adult                  | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                                                           |
| carousel               | Object  | O  | カルーセル                                                                                                                                                                                                                                                                 |
| - head                 | Object  | X  | カルーセルイントロ                                                                                                                                                                                                                                                             |
| -- header              | String  | O  | カルーセルイントロヘッダー（最大20文字）                                                                                                                                                                                                                                              |
| -- content             | String  | O  | カルーセルイントロ内容（最大50文字）                                                                                                                                                                                                                                               |
| -- imageUrl            | String  | O  | カルーセルイントロ画像アドレス（カルーセルコマース型画像でアップロードされた画像を使用、使用する画像はカルーセルの画像と比率が同じである必要があります）                                                                                                                                                                 |
| -- linkMo              | String  | X  | モバイルWebリンク（linkMo、linkPc、schemeAndroid、schemeIos のうちいずれかを使用する場合、linkMo は必須値）、1,000文字制限                                                                                                                                                                      |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                               |
| -- schemeAndroid       | String  | X  | Android アプリリンク、1,000文字制限                                                                                                                                                                                                                                           |
| -- schemeIos           | String  | X  | iOS アプリリンク、1,000文字制限                                                                                                                                                                                                                                             |
| - list                 | List    | O  | カルーセルリスト（head が存在する場合最小1個、最大5個 / その他は最小2個、最大6個）                                                                                                                                                                                                                |
| -- additionalContent   | String  | X  | 付加情報（最大34文字）、カルーセルコマース型でのみ使用可能                                                                                                                                                                                                                                |
| -- imageUrl            | String  | O  | 画像URL（カルーセルコマース型画像でアップロードされた画像を使用）                                                                                                                                                                                                                               |
| -- imageLink           | String  | X  | 画像リンク、1,000文字制限                                                                                                                                                                                                                                                    |
| -- commerce            | Object  | O  | コマース（CAROUSEL_COMMERCE タイプでのみ使用可能）                                                                                                                                                                                                                                 |
| --- title              | String  | O  | 商品タイトル（最大30文字、改行：不可）                                                                                                                                                                                                                                             |
| --- regularPrice       | Integer | O  | 正常価格（0〜99,999,999）                                                                                                                                                                                                                                                |
| --- discountPrice      | Integer | X  | 割引価格（0〜99,999,999）                                                                                                                                                                                                                                                |
| --- discountRate       | Integer | X  | 割引率（0〜100）、割引価格存在時は割引率、定額割引価格のうちいずれかは必須                                                                                                                                                                                                                     |
| --- discountFixed      | Integer | X  | 定額割引価格（0〜999,999）、割引価格存在時は割引率、定額割引価格のうちいずれかは必須                                                                                                                                                                                                             |
| -- buttons             | List    | O  | カルーセルリストボタンリスト 最小1個、最大2個                                                                                                                                                                                                                                        |
| --- name               | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字                                                                                                                                                                                                |
| --- type               | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- BT タイプはカカオオープンビルダーのチャットボットを使用するチャンネルのみ利用可能<br>- BF タイプは最初のボタンのみ使用可能で、name には次の3つの文句のみ使用可能<br>  - トークで予約する<br>  - トークでアンケートする<br>  - トークで応募する |
| --- linkMo             | String  | X  | モバイルWebリンク（WL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                         |
| --- linkPc             | String  | X  | PC Webリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                       |
| --- schemeIos          | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限                                                                                                                                                                                                                         |
| --- chatExtra          | String  | X  | BT タイプボタンの場合転送するメタ情報                                                                                                                                                                                                                                             |
| --- chatEvent          | String  | X  | BT タイプボタンの場合接続するボットイベント名                                                                                                                                                                                                                                         |
| --- bizFormKey         | String  | X  | BF タイプボタンの場合ビズフォームキー                                                                                                                                                                                                                                              |
| -- coupon              | Object  | X  | クーポン要素                                                                                                                                                                                                                                                               |
| --- title              | String  | O  | title の場合5つの形式に制限されます<br>- "${数字}円 割引クーポン" 数字は1以上99,999,999以下<br>- "${数字}% 割引クーポン" 数字は1以上100以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                                 |
| --- description        | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字、改行：不可<br>- その他のタイプの場合最大12文字、改行：不可                                                                                                                                                        |
| --- linkMo             | String  | X  | モバイルWebリンク（WL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                     |
| --- linkPc             | String  | X  | PC Webリンク（WL タイプの場合選択フィールド）、1,000文字制限                                                                                                                                                                                                                          |
| --- schemeAndroid      | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                   |
| --- schemeIos          | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、1,000文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                     |
| - tail                 | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                                                         |
| -- linkMo              | String  | O  | モバイルWebリンク、1,000文字制限                                                                                                                                                                                                                                             |
| -- linkPc              | String  | X  | PC Webリンク、1,000文字制限                                                                                                                                                                                                                                              |
| -- schemeAndroid       | String  | X  | Android アプリリンク、1,000文字制限                                                                                                                                                                                                                                           |
| -- schemeIos           | String  | X  | iOS アプリリンク、1,000文字制限                                                                                                                                                                                                                                             |
| recipientList          | List    | O  | 受信者リスト（最大1,000名）                                                                                                                                                                                                                                                 |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                                               |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                                             |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信可否<br>コンソールで代替送信設定時、デフォルト値で代替送信されます。                                                                                                                                                                                                         |
| -- resendType          | String  | X  | 代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが区分されます。                                                                                                                                                                                                   |
| -- resendTitle         | String  | X  | LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                                                                                                             |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                                                      |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                                               |
| - targeting            | String  | X  | メッセージ対象のタイプ（M：マーケティング受信同意ユーザー、N：フレンドでないマーケティング受信同意ユーザーのみ、I：フレンドであるユーザー）                                                                                                                                                                         |
| - unsubscribeNo        | String  | X  | 080 無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                                          |
| - unsubscribeAuthNo    | String  | X  | 080 無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力不可<br>例：1234                                                                                                                                           |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                 |
| senderGroupingKey      | String  | X  | 発信者グルーピングキー（発信者別にグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                                                 |
| resellerCode           | String  | X  | リセラーコード（リセラーが送信時に使用）                                                                                                                                                                                                                                           |
| createUser             | String  | X  | 登録者（コンソールで送信時はユーザー UUID で保存）                                                                                                                                                                                                                                   |
| statsId                | String  | X  | 統計 ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                                                    |

<a id="response-3"></a>

#### 応答

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

| 名前               | タイプ     | Not Null | 説明                                                           |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | 応答ヘッダー情報                                                     |
| - isSuccessful   | boolean | O        | API 呼び出し成功有無                                                 |
| - resultCode     | Integer | O        | API 呼び出し結果コード（成功: 0、失敗時エラーコード）                           |
| - resultMessage  | String  | O        | API 呼び出し結果メッセージ（成功時 "success" または関連成功メッセージ、失敗時失敗原因詳細メッセージ）  |
| message          | Object  | X        | メッセージ送信結果情報（送信要求がある場合にのみ存在）                             |
| - requestId      | String  | X        | 送信要求 ID（各送信要求を一意に識別する ID）                             |
| - sendResults    | Array   | O        | 受信者別送信結果リスト                                                |
| -- recipientSeq  | Integer | O        | 受信者リストの順番                                                   |
| -- recipientNo   | String  | X        | 受信者電話番号                                                     |
| -- resultCode    | Integer | O        | 受信者別送信結果コード（成功および様々な失敗コードが存在する可能性があります）                     |
| -- resultMessage | String  | O        | 受信者別送信結果メッセージ（成功時 "success" または関連メッセージ、失敗時失敗原因詳細メッセージ） |

<a id="request-to-send-basic-message"></a>

## メッセージ基本型送信リクエスト

* テンプレートを利用した送信です。
* マーケティング受信同意送信を使用できます。
    * targeting フィールドを指定してメッセージ対象のタイプを指定できます。
        * M: 顧客企業の広告性情報受信同意ユーザー（カカオトーク受信同意）
        * N: 顧客企業の広告性情報受信同意ユーザー（カカオトーク受信同意）- チャンネルフレンド
        * I: 顧客企業の送信リクエスト対象 ∩ チャンネルフレンド
* 既存フレンドトークの8種類のメッセージタイプをすべて使用できます。
* BT ボタンタイプを使用することはできません。
* AC（チャンネル追加）ボタンを使用できます。
* BF ボタン使用時はカカオで発行されたビジネスフォーム ID をアップロードしてビズフォームキーを発行して使用できます。
* 代替送信は受信者別 resendParameter で設定できます。
    * 代替送信を利用する場合、代替送信管理 API で SMS AppKey 登録および送信設定が必要です。
* **夜間送信制限（20:50〜翌日 08:00）**

<a id="cautions-for-use"></a>

### 使用時の注意事項

- unsubscribeNo、unsubscribeAuthNo は 080 無料受信拒否電話番号と認証番号で、どちらか一つでも入力しないと発信プロフィールに登録された無料受信拒否情報で送信されます。
- 送信間で unsubscribeNo、unsubscribeAuthNo を入力する場合、発信プロフィールに登録された無料受信拒否情報ではなく、入力した値で送信されます。
- 送信間で unsubscribeNo、unsubscribeAuthNo を入力しない場合、発信プロフィールに登録された無料受信拒否情報で送信されます。
- unsubscribeNo、unsubscribeAuthNo は受信者別に入力でき、共通フィールドと受信者別フィールドの両方を入力する場合、共通フィールドが優先適用されます。

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/basic-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | タイプ    | 説明      |
|--------|--------|---------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。    |

<a id="requested-4"></a>

#### 送信リクエスト

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

| 名前                     | タイプ     | 必須 | 説明                                                                                                                                                                                                                                      |
|------------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey              | String  | O  | 発信キー（40文字）、グループ発信キーは使用不可                                                                                                                                                                                                                |
| templateCode           | String  | O  | 使用するテンプレートコード                                                                                                                                                                                                                            |
| pushAlarm              | boolean | X  | メッセージプッシュアラーム送信の有無（デフォルト値: true）                                                                                                                                                                                                              |
| requestDate            | String  | X  | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                                                                                      |
| unsubscribeNo          | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                           |
| unsubscribeAuthNo      | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNoなしでunsubscribeAuthNoのみ入力不可<br>例: 1234                                                                                                           |
| recipientList          | List    | O  | 受信者リスト（最大1,000名）                                                                                                                                                                                                                       |
| - recipientNo          | String  | O  | 受信番号                                                                                                                                                                                                                                   |
| - targeting            | String  | O  | メッセージ対象のタイプ（M: マーケティング受信同意ユーザー、N: フレンドではないマーケティング受信同意ユーザーのみ、I: フレンドであるユーザー）                                                                                                                                                       |
| - templateParameter    | Object  | X  | テンプレートパラメータ（テンプレートに置換する変数を含む場合は必須）<br>- カルーセルタイプ: アイテムごとに異なる置換値を適用する場合は `キー@$.carousel.list[インデックス]` 形式を使用（例: `productName@$.carousel.list[0]`）<br>- headがあればheadがlist[0]を占有し、実際のアイテムはlist[1]から開始<br>- 置換キーに `@` 文字使用不可<br>- キー/値それぞれ最大1,300文字 |
| - imageParameters      | List    | X  | テンプレート画像フィールド値を変更できる動的パラメータ（テンプレートに存在する画像数と同一サイズのJSONリストのみ使用可能、使用する場合は変更しない画像は空のJSONオブジェクトを入力する必要がある）                                                                                                                      |
| -- imageUrl            | String  | O  | 画像URL                                                                                                                                                                                                                                 |
| -- imageLink           | String  | X  | 画像リンク                                                                                                                                                                                                                                  |
| - videoParameter       | Object  | X  | テンプレート動画フィールド値を変更できる動的パラメータ                                                                                                                                                                                                          |
| -- videoUrl            | String  | O  | カカオTV動画URL                                                                                                                                                                                                                           |
| -- thumbnailUrl        | String  | X  | 動画サムネイル用画像URL                                                                                                                                                                                                                        |
| - resendParameter      | Object  | X  | 代替送信情報                                                                                                                                                                                                                                |
| -- isResend            | boolean | X  | 送信失敗時、文字代替送信の有無<br>コンソールで代替送信設定時、デフォルト値で代替送信されます。                                                                                                                                                                                 |
| -- resendType          | String  | X  | 代替送信タイプ（SMS,LMS）<br>値がない場合、テンプレート本文の長さに応じてタイプが区分されます。                                                                                                                                                                                 |
| -- resendTitle         | String  | X  | LMS代替送信タイトル<br>（値がない場合、プラスフレンドIDで代替送信されます。）                                                                                                                                                                                         |
| -- resendContent       | String  | X  | 代替送信内容<br>（値がない場合、[メッセージ本文]で代替送信されます。）                                                                                                                                                                                            |
| -- resendSendNo        | String  | X  | 代替送信発信番号<br><span style="color:red">（SMSサービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                                                                                           |
| - unsubscribeNo        | String  | X  | 080無料受信拒否電話番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx                                                                                                           |
| - unsubscribeAuthNo    | String  | X  | 080無料受信拒否認証番号（すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信される）<br>unsubscribeNoなしでunsubscribeAuthNoのみ入力不可<br>例: 1234                                                                                                           |
| - recipientGroupingKey | String  | X  | 受信者グルーピングキー（受信者ごとにグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                             |
| senderGroupingKey      | String  | X  | 発信者グルーピングキー（発信者ごとにグルーピングキーを指定できます。最大100文字）                                                                                                                                                                                             |
| resellerCode           | String  | X  | リセラーコード（リセラーが送信時使用）                                                                                                                                                                                                                    |
| createUser             | String  | X  | 登録者（コンソールで送信時はユーザーUUIDで保存）                                                                                                                                                                                                             |
| statsId                | String  | X  | 統計ID（発信検索条件には含まれません。最大8文字）                                                                                                                                                                                                      |

<a id="response-4"></a>

#### 応答

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

| 名前               | タイプ     | Not Null | 説明                                                           |
|:-----------------|:--------|:---------|:-------------------------------------------------------------|
| header           | Object  | O        | 応答ヘッダー情報                                                     |
| - isSuccessful   | boolean | O        | API 呼び出し成功可否                                                 |
| - resultCode     | Integer | O        | API 呼び出し結果コード（成功: 0、失敗時エラーコード）                           |
| - resultMessage  | String  | O        | API 呼び出し結果メッセージ（成功時「success」または関連成功メッセージ、失敗時失敗原因詳細メッセージ）  |
| message          | Object  | X        | メッセージ送信結果情報（送信要求がある場合のみ存在）                             |
| - requestId      | String  | X        | 送信要求 ID（各送信要求を一意に識別する ID）                             |
| - sendResults    | Array   | O        | 受信者別送信結果リスト                                                |
| -- recipientSeq  | Integer | O        | 受信者リストの順番                                                   |
| -- recipientNo   | String  | X        | 受信者電話番号                                                     |
| -- resultCode    | Integer | O        | 受信者別送信結果コード（成功および各種失敗コードが存在する可能性があります）                     |
| -- resultMessage | String  | O        | 受信者別送信結果メッセージ（成功時「success」または関連メッセージ、失敗時失敗原因詳細メッセージ） |

<a id="view-sending-list"></a>

## 送信リスト照会

<a id="requested-5"></a>

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | タイプ    | 説明       |
|--------|--------|----------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                    |
|--------------|--------|----|---------------------|
| X-Secret-Key | String | O  | コンソールで作成することができます。 |

[Query parameter] 1番または2番条件必須

| 名前               | タイプ    | 必須        | 説明                                  |
|------------------|--------|-----------|-------------------------------------|
| requestId        | String | 条件必須(1番) | リクエストID                            |
| startRequestDate | String | 条件必須(2番) | 送信リクエスト日付開始値(yyyy-MM-dd HH:mm)  |
| endRequestDate   | String | 条件必須(2番) | 送信リクエスト日付終了値(yyyy-MM-dd HH:mm)  |
| senderKey        | String | X         | 発信キー                               |
| templateCode     | String | X         | テンプレートコード                         |
| recipientNo      | String | X         | 受信番号                               |
| messageStatus    | String | X         | リクエスト状態(COMPLETED: 成功、FAILED: 失敗) |
| resultCode       | String | X         | 送信結果(MRC01: 成功 MRC02: 失敗)         |
| senderGroupingKey    | String   | X          | 発信グルーピングキー                        |
| recipientGroupingKey | String  | X         | 受信者グルーピングキー                       |
| pageNum          | String | X         | ページ番号(Default: 1)                |
| pageSize         | String | X         | 照会件数(Default: 15、Max: 1000)      |

<a id="response-5"></a>

#### レスポンス

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

| 名前                          | タイプ     | Not Null | 説明                                                                        |
|:----------------------------|:--------|:---------|:--------------------------------------------------------------------------|
| header                      | Object  | O        | ヘッダー領域                                                                    |
| - resultCode                | Integer | O        | 結果コード                                                                     |
| - resultMessage             | String  | O        | 結果メッセージ                                                                   |
| - isSuccessful              | boolean | O        | 成功可否                                                                      |
| messageSearchResultResponse | Object  | X        | 本文領域                                                                      |
| - messages                  | Array   | O        | メッセージリスト                                                                  |
| -- requestId                | String  | O        | リクエストID                                                                   |
| -- recipientSeq             | Integer | O        | 受信者シーケンス番号                                                               |
| -- plusFriendId             | String  | O        | 発信プロフィールID                                                              |
| -- senderKey                | String  | O        | 発信キー                                                                      |
| -- templateCode             | String  | X        | テンプレートコード                                                                |
| -- recipientNo              | String  | O        | 受信番号                                                                      |
| -- targeting                | String  | O        | メッセージ対象のタイプ(M: マーケティング受信同意ユーザー、N: フレンドではないマーケティング受信同意ユーザーのみ、I: フレンドのユーザー) |
| -- requestDate              | String  | O        | リクエスト日時(yyyy-MM-dd HH:mm)<br>(入力しない場合は即時送信)<br>最大60日後まで予約可能                                                                     |
| -- createDate               | String  | O        | 登録日時                                                                      |
| -- receiveDate              | String  | X        | 受信日時                                                                      |
| -- chatBubbleType           | String  | O        | メッセージタイプ                                                                 |
| -- pushAlarm                | boolean | O        | プッシュアラーム可否                                                               |
| -- messageStatus            | String  | O        | リクエスト状態(COMPLETED: 成功、FAILED: 失敗)                                      |
| -- resendStatusCode         | String  | X        | 代替送信状態コード                                                                |
| -- resendStatusName         | String  | X        | 代替送信状態名                                                                  |
| -- resendResultCode         | String  | X        | 代替送信結果コード                                                                |
| -- resendRequestId          | String  | X        | 代替送信リクエストID                                                             |
| -- isAddedChannel           | boolean | O        | チャンネルフレンド可否                                                              |
| -- resultCode               | String  | X        | 受信結果コード                                                                  |
| -- resultCodeName           | String  | X        | 受信結果コード名                                                                 |
| -- createUser               | String  | X        | 登録者(コンソールで送信時、ユーザー UUID で保存)                                           |
| -- senderGroupingKey        | String  | X        | 発信グルーピングキー                                                     |
| -- recipientGroupingKey     | String  | X        | 受信者グルーピングキー                                               |
| - totalCount                | Integer | O        | 総件数                                                                       |

<a id="view-single-sending"></a>

## 送信単件照会

<a id="requested-6"></a>

#### 要求

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 種類      | 説明         |
|--------------|---------|------------|
| appkey       | String  | 固有のアプリキー     |
| requestId    | String  | 要求 ID      |
| recipientSeq | Integer | 受信者シーケンス番号 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 種類     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="response-6"></a>

#### 応答

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

| 名前                    | タイプ      | Not Null | 説明                                                                                               | 
 |:----------------------|:--------|:---------|:-------------------------------------------------------------------------------------------------| 
| header                | Object  | O        | ヘッダー領域                                                                                            | 
| - resultCode          | Integer | O        | 結果コード                                                                                            | 
| - resultMessage       | String  | O        | 結果メッセージ                                                                                           | 
| - isSuccessful        | boolean | O        | 成功可否                                                                                            | 
| message               | Object  | X        | メッセージ本文領域（メッセージ失敗時に存在しない場合があります）                                                                     | 
| - requestId           | String  | O        | リクエスト ID（message オブジェクト存在時 Not Null）                                                                 | 
| - recipientSeq        | Integer | O        | 受信者シーケンス番号（message オブジェクト存在時 Not Null）                                                            | 
| - plusFriendId        | String  | O        | 発信プロフィール ID（message オブジェクト存在時 Not Null）                                                             | 
| - senderKey           | String  | O        | 発信キー（message オブジェクト存在時 Not Null）                                                                  | 
| - templateCode        | String  | X        | テンプレートコード                                                                                           | 
| - recipientNo         | String  | O        | 受信番号（message オブジェクト存在時 Not Null）                                                                 | 
| - targeting           | String  | O        | メッセージ対象のタイプ（M: マーケティング受信同意ユーザー、N: フレンドでないマーケティング受信同意ユーザーのみ、I: フレンドのユーザー）（message オブジェクト存在時 Not Null） | 
| - requestDate         | String  | O        | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大 60 日後まで予約可能（message オブジェクト存在時 Not Null）                                                                 | 
| - createDate          | String  | O        | 登録日時（message オブジェクト存在時 Not Null）                                                                 | 
| - receiveDate         | String  | X        | 受信日時                                                                                            | 
| - chatBubbleType      | String  | O        | メッセージタイプ（message オブジェクト存在時 Not Null）                                                                | 
| - content             | String  | X        | メッセージ内容                                                                                           | 
| - adult               | boolean | O        | 成人向けメッセージ可否（message オブジェクト存在時 Not Null）                                                            | 
| - header              | String  | X        | ヘッダー（メッセージ内）                                                                                       | 
| - additionalContent   | String  | X        | 付加情報（メッセージ内）                                                                                    |
| - image               | Object  | X        | 画像要素                                                                                           | 
| -- imageUrl           | String  | O        | 画像 URL（image オブジェクト存在時 Not Null）                                                                 | 
| -- imageLink          | String  | X        | 画像リンク                                                                                           | 
| - buttons             | Array   | X        | ボタンリスト                                                                                            | 
| -- name               | String  | O        | ボタンタイトル（buttons 配列アイテム存在時 Not Null）                                                              | 
| -- type               | String  | O        | ボタンタイプ（buttons 配列アイテム存在時 Not Null）                                                              | 
| -- linkMo             | String  | X        | モバイルウェブリンク                                                                                         | 
| -- linkPc             | String  | X        | PC ウェブリンク                                                                                          | 
| -- schemeIos          | String  | X        | iOS アプリリンク                                                                                         | 
| -- schemeAndroid      | String  | X        | Android アプリリンク                                                                                       | 
| -- chatExtra          | String  | X        | BT タイプボタンの場合に渡すメタ情報                                                                      | 
| -- chatEvent          | String  | X        | BT タイプボタンの場合に接続するボットイベント名                                                                          | 
| -- bizFormKey         | String  | X        | BF タイプボタンの場合のビズフォームキー                                                                               | 
| - item                | Object  | X        | ワイドリスト要素                                                                                       | 
| -- list               | Array   | X        | ワイドリスト（item オブジェクト存在時 Nullable）                                                                  | 
| --- title             | String  | X        | アイテムタイトル                                                                                           | 
| --- imageUrl          | String  | O        | アイテム画像 URL（item.list アイテム存在時 Not Null）                                                         | 
| --- linkMo            | String  | O        | モバイルウェブリンク（item.list アイテム存在時 Not Null）                                                            | 
| --- linkPc            | String  | X        | PC ウェブリンク                                                                                          | 
| --- schemeIos         | String  | X        | iOS アプリリンク                                                                                         | 
| --- schemeAndroid     | String  | X        | Android アプリリンク                                                                                       | 
| - coupon              | Object  | X        | クーポン要素                                                                                            | 
| -- title              | String  | O        | クーポンタイトル（coupon オブジェクト存在時 Not Null）                                                                  | 
| -- description        | String  | O        | クーポン詳細説明（coupon オブジェクト存在時 Not Null）                                                               | 
| -- linkMo             | String  | X        | モバイルウェブリンク                                                                                         | 
| -- linkPc             | String  | X        | PC ウェブリンク                                                                                          | 
| -- schemeAndroid      | String  | X        | Android アプリリンク                                                                                       | 
| -- schemeIos          | String  | X        | iOS アプリリンク                                                                                         | 
| - commerce            | Object  | X        | コマース要素                                                                                           | 
| -- title              | String  | O        | 商品タイトル（commerce オブジェクト存在時 Not Null）                                                                | 
| -- regularPrice       | Integer | X        | 定価                                                                                            | 
| -- discountPrice      | Integer | X        | 割引価格                                                                                             | 
| -- discountRate       | Integer | X        | 割引率                                                                                              | 
| -- discountFixed      | Integer | X        | 定額割引価格                                                                                           | 
| - video               | Object  | X        | 動画要素                                                                                           | 
| -- videoUrl           | String  | O        | カカオ TV 動画 URL（video オブジェクト存在時 Not Null）                                                           | 
| -- thumbnailUrl       | String  | X        | 動画サムネイル用画像 URL                                                                                 | 
| - carousel            | Object  | X        | カルーセル                                                                                              | 
| -- head               | Object  | X        | カルーセルイントロ（carousel オブジェクト存在時 Nullable）                                                              | 
| --- header            | String  | O        | カルーセルイントロヘッダー（head オブジェクト存在時 Not Null）                                                               | 
| --- content           | String  | O        | カルーセルイントロ内容（head オブジェクト存在時 Not Null）                                                               | 
| --- imageUrl          | String  | O        | カルーセルイントロ画像アドレス（head オブジェクト存在時 Not Null）                                                           | 
| --- linkMo            | String  | X        | モバイルウェブリンク                                                                                         | 
| --- linkPc            | String  | X        | PC ウェブリンク                                                                                          | 
| --- schemeIos         | String  | X        | iOS アプリリンク                                                                                         | 
| --- schemeAndroid     | String  | X        | Android アプリリンク                                                                                       | 
| -- list               | Array   | O        | カルーセルリスト（carousel オブジェクト存在時 Not Null）                                                              | 
| --- header            | String  | X        | カルーセルアイテムヘッダー                                                                                       | 
| --- message           | String  | O        | カルーセルアイテムメッセージ（list アイテム存在時 Not Null）                                                              | 
| --- additionalContent | String  | X        | 付加情報                                                                                            | 
| --- imageUrl          | String  | X        | 画像 URL                                                                                          | 
| --- imageLink         | String  | X        | 画像リンク                                                                                           | 
| --- commerce          | Object  | X        | コマース（カルーセル内）                                                                                      | 
| ---- title            | String  | O        | 商品タイトル（carousel.list.commerce 存在時 Not Null）                                                     | 
| ---- regularPrice     | Integer | X        | 定価                                                                                            | 
| ---- discountPrice    | Integer | X        | 割引価格                                                                                             | 
| ---- discountRate     | Integer | X        | 割引率                                                                                              | 
| ---- discountFixed    | Integer | X        | 定額割引価格                                                                                           | 
| --- buttons           | Array   | X        | ボタンリスト（カルーセル内）                                                                                    | 
| ---- name             | String  | O        | ボタンタイトル（carousel.list.buttons アイテム存在時 Not Null）                                                   | 
| ---- type             | String  | O        | ボタンタイプ（carousel.list.buttons アイテム存在時 Not Null）                                                   | 
| ---- linkMo           | String  | X        | モバイルウェブリンク                                                                                         | 
| ---- linkPc           | String  | X        | PC ウェブリンク                                                                                          | 
| ---- schemeAndroid    | String  | X        | Android アプリリンク                                                                                       | 
| ---- schemeIos        | String  | X        | iOS アプリリンク                                                                                         | 
| ---- chatExtra        | String  | X        | BT タイプボタンの場合に渡すメタ情報                                                                      | 
| ---- chatEvent        | String  | X        | BT タイプボタンの場合に接続するボットイベント名                                                                          | 
| ---- bizFormKey       | String  | X        | BF タイプボタンの場合のビズフォームキー                                                                               | 
| --- coupon            | Object  | X        | クーポン（カルーセル内）                                                                                       | 
| ---- title            | String  | O        | クーポンタイトル（carousel.list.coupon 存在時 Not Null）                                                       | 
| ---- description      | String  | O        | クーポン詳細説明（carousel.list.coupon 存在時 Not Null）                                                    | 
| ---- linkMo           | String  | X        | モバイルウェブリンク                                                                                         | 
| ---- linkPc           | String  | X        | PC ウェブリンク                                                                                          | 
| ---- schemeAndroid    | String  | X        | Android アプリリンク                                                                                       | 
| ---- schemeIos        | String  | X        | iOS アプリリンク                                                                                         | 
| -- tail               | Object  | X        | もっと見るボタン情報（carousel オブジェクト存在時 Nullable）                                                            | 
| --- linkMo            | String  | O        | モバイルウェブリンク（tail オブジェクト存在時 Not Null）                                                                 | 
| --- linkPc            | String  | X        | PC ウェブリンク                                                                                          | 
| --- schemeAndroid     | String  | X        | Android アプリリンク                                                                                       | 
| --- schemeIos         | String  | X        | iOS アプリリンク                                                                                         | 
| - templateParameter   | String  | X        | テンプレートパラメータ                                                                                         | 
| - pushAlarm           | boolean | O        | プッシュアラーム可否（message オブジェクト存在時 Not Null）                                                              | 
| - messageStatus       | String  | O        | リクエスト状態（COMPLETED: 成功、FAILED: 失敗）（message オブジェクト存在時 Not Null）                                     | 
| - isAddedChannel      | boolean | O        | チャンネルフレンド可否（message オブジェクト存在時 Not Null）                                                              | 
| - resultCode          | String  | X        | 受信結果コード（メッセージ内）                                                                                 | 
| - resultCodeName      | String  | X        | 受信結果コード名（メッセージ内）                                                                                | 
| - resendStatusCode    | String  | X        | 代替送信状態コード                                                                                      | 
| - resendStatusName    | String  | X        | 代替送信状態コード名                                                                                     | 
| - resendResultCode    | String  | X        | 代替送信結果コード                                                                                      | 
| - resendRequestId     | String  | X        | 代替送信リクエスト ID                                                                                      | 
| - createUser          | String  | X        | 登録者（コンソールから送信時はユーザー UUID で保存）                                                                      |
| - senderGroupingKey   | String  | X        | 送信者グルーピングキー                                                 |
| - recipientGroupingKey | String  | X        | 受信者グルーピングキー                                           |

<a id="cancel-message-sending"></a>

## メッセージ送信取消

<a id="requested-7"></a>

#### 要求

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ     | 	説明     |
|-----------|---------|---------|
| appkey    | 	String | 	固有のアプリキー |
| requestId | String  | 要求 ID   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Query parameter]

| 名前           | 	タイプ     | 	必須 | 	説明                                         |
|--------------|---------|-----|---------------------------------------------|
| recipientSeq | 	String | 	X  | 受信者シーケンス番号<br>（入力しない場合、要求 ID のすべての送信件を取消） |

<a id="response-7"></a>

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 名前              | タイプ      | Not Null | 説明     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    X     | ヘッダー領域  |
| - resultCode    | Integer |    X     | 結果コード  |
| - resultMessage | String  |    X     | 結果メッセージ |
| - isSuccessful  | Boolean |    X     | 成功可否  |

[例示]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

<a id="manage-templates"></a>

## テンプレート管理

<a id="view-template-list"></a>

### テンプレートリスト照会

<a id="requested-8"></a>

#### 요청

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | タイプ     | 説明     |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Query parameter]

| 名前           | タイプ      | 必須 | 説明                            |
|--------------|---------|----|-------------------------------|
| templateCode | String  | X  | テンプレートコード                        |
| templateName | String  | X  | テンプレート名                        |
| status       | String  | X  | テンプレート状態コード                     |
| pageNum      | Integer | X  | ページ番号(Default: 1)            |
| pageSize     | Integer | X  | 照会件数(Default: 15, Max: 1000) |

<a id="response-8"></a>

#### レスポンス

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

| 名前                      | タイプ      | Not Null | 説明                                     |
|:------------------------|:--------|:---------|:---------------------------------------|
| header                  | Object  | O        | ヘッダー領域                                  |
| - resultCode            | Integer | O        | 結果コード                                  |
| - resultMessage         | String  | O        | 結果メッセージ                                 |
| - isSuccessful          | boolean | O        | 成功可否                                  |
| templateListResponse    | Object  | O        | 本文領域                                  |
| - templates             | Array   | O        | テンプレートリスト                                |
| -- plusFriendId         | String  | O        | 発信プロフィール ID                              |
| -- plusFriendType       | String  | O        | 発信プロフィールタイプ                              |
| -- templateCode         | String  | O        | テンプレートコード                                 |
| -- templateName         | String  | O        | テンプレート名                                  |
| -- chatBubbleType       | String  | O        | メッセージタイプ                                 |
| -- content              | String  | X        | メッセージ内容                                 |
| -- header               | String  | X        | ヘッダー                                     |
| -- additionalContent    | String  | X        |（説明テーブル参照）                            |
| -- adult                | boolean | O        | アダルトメッセージ可否                             |
| -- image                | Object  | X        | 画像情報                                 |
| --- imageUrl            | String  | O        | 画像 URL                                |
| --- imageLink           | String  | X        | 画像リンク                                 |
| -- buttons              | Array   | X        | ボタンリスト                                  |
| --- name                | String  | O        | ボタンタイトル                                  |
| --- type                | String  | O        | ボタンタイプ                                  |
| --- linkMo              | String  | X        | モバイルウェブリンク                               |
| --- linkPc              | String  | X        | PC ウェブリンク                                |
| --- schemeIos           | String  | X        | iOS アプリリンク                               |
| --- schemeAndroid       | String  | X        | Android アプリリンク                             |
| --- bizFormId           | Integer | X        | ビズフォーム ID（JSON 基準）                       |
| -- item                 | Object  | X        | ワイドリスト要素                             |
| --- list                | Array   | X        | ワイドリスト                                |
| ---- title              | String  | X        | アイテムタイトル                                 |
| ---- imageUrl           | String  | O        | アイテム画像 URL                            |
| ---- linkMo             | String  | O        | モバイルウェブリンク                               |
| ---- linkPc             | String  | X        | PC ウェブリンク                                |
| ---- schemeAndroid      | String  | X        | Android アプリリンク                             |
| ---- schemeIos          | String  | X        | iOS アプリリンク                               |
| -- coupon               | Object  | X        | クーポン要素                                  |
| --- title               | String  | O        | クーポンタイトル                                  |
| --- description         | String  | O        | クーポン詳細説明                               |
| --- linkMo              | String  | X        | モバイルウェブリンク                               |
| --- linkPc              | String  | X        | PC ウェブリンク                                |
| --- schemeAndroid       | String  | X        | Android アプリリンク                             |
| --- schemeIos           | String  | X        | iOS アプリリンク                               |
| -- commerce             | Object  | X        | コマース要素                                 |
| --- title               | String  | O        | 商品タイトル                                  |
| --- regularPrice        | Integer | X        | 通常価格                                  |
| --- discountPrice       | Integer | X        | 割引価格                                   |
| --- discountRate        | Integer | X        | 割引率                                    |
| --- discountFixed       | Integer | X        | 定額割引価格                                 |
| -- video                | Object  | X        | 動画要素                                 |
| --- videoUrl            | String  | O        | カカオ TV 動画 URL                          |
| --- thumbnailUrl        | String  | X        | 動画サムネイル用画像 URL                       |
| -- carousel             | Object  | X        | カルーセル                                    |
| --- head                | Object  | X        | カルーセルイントロ                                |
| ---- header             | String  | O        | カルーセルイントロヘッダー                             |
| ---- content            | String  | O        | カルーセルイントロ内容                             |
| ---- imageUrl           | String  | O        | カルーセルイントロ画像アドレス                         |
| ---- linkMo             | String  | X        | モバイルウェブリンク                               |
| ---- linkPc             | String  | X        | PC ウェブリンク                                |
| ----- schemeAndroid     | String  | X        | Android アプリリンク                             |
| ----- schemeIos         | String  | X        | iOS アプリリンク                               |
| ---- list               | Array   | O        | カルーセルリスト                                |
| ----- header            | String  | O        | カルーセルアイテムヘッダー                             |
| ----- message           | String  | O        | カルーセルアイテムメッセージ（Not Null リストの content マッピング） |
| ----- additionalContent | String  | X        | 追加情報                                  |
| ----- imageUrl          | String  | O        | 画像 URL（カルーセルアイテム内）                    |
| ----- imageLink         | String  | X        | 画像リンク（カルーセルアイテム内）                     |
| ----- commerce          | Object  | O        | コマース（カルーセルアイテム内）                        |
| ------ title            | String  | O        | 商品タイトル                                  |
| ------ regularPrice     | Integer | X        | 通常価格                                  |
| ------ discountPrice    | Integer | X        | 割引価格                                   |
| ------ discountRate     | Integer | X        | 割引率                                    |
| ------ discountFixed    | Integer | X        | 定額割引価格                                 |
| ----- buttons           | Array   | O        | カルーセルリストボタンリスト                          |
| ------ name             | String  | O        | ボタンタイトル                                  |
| ------ type             | String  | O        | ボタンタイプ                                  |
| ------ linkMo           | String  | X        | モバイルウェブリンク                               |
| ------ linkPc           | String  | X        | PC ウェブリンク                                |
| ------ schemeIos        | String  | X        | iOS アプリリンク                               |
| ------ schemeAndroid    | String  | X        | Android アプリリンク                             |
| ------ bizFormId        | Integer | X        | ビズフォーム ID（JSON 基準）                       |
| ----- coupon            | Object  | X        | クーポン要素（カルーセルアイテム内）                      |
| ------ title            | String  | X        | クーポンタイトル                                  |
| ------ description      | String  | X        | クーポン詳細説明                               |
| ------ linkMo           | String  | X        | モバイルウェブリンク                               |
| ------ linkPc           | String  | X        | PC ウェブリンク                                |
| ------ schemeAndroid    | String  | X        | Android アプリリンク                             |
| ------ schemeIos        | String  | X        | iOS アプリリンク                               |
| ---- tail               | Object  | X        | もっと見るボタン情報                              |
| ----- linkMo            | String  | O        | モバイルウェブリンク                               |
| ----- linkPc            | String  | X        | PC ウェブリンク                                |
| ----- schemeAndroid     | String  | X        | Android アプリリンク                             |
| ----- schemeIos         | String  | X        | iOS アプリリンク                               |
| --- status              | String  | O        | テンプレート状態（A: 登録、S: ブロック）                   |
| --- createDate          | String  | O        | 登録日時                                  |
| --- updateDate          | String  | X        | 修正日時                                  |
| - totalCount            | Integer | O        | 総数                                   |

<a id="view-single-template"></a>

### テンプレート単件照会

[URL]

```
GET  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | タイプ     | 説明     |
|--------------|--------|--------|
| appkey       | String | 固有のアプリキー |
| senderKey    | String | 発信キー   |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |
|X-NC-API-IDEMPOTENCY-KEY|	String| X | 重複メッセージ送信要求基準key<br>10分間同一のkeyでリクエスト時、該当リクエストを失敗処理します。 |

<a id="response-9"></a>

#### 応答

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

| 名前                    | タイプ      | Not Null | 説明                   |
|:----------------------|:--------|:---------|:---------------------|
| header                | Object  | O        | ヘッダー領域                |
| - resultCode          | Integer | O        | 結果コード                |
| - resultMessage       | String  | O        | 結果メッセージ               |
| - isSuccessful        | boolean | O        | 成功可否                |
| template              | Object  | O        | テンプレート本文領域            |
| - plusFriendId        | String  | O        | 発信プロフィール ID            |
| - plusFriendType      | String  | O        | 発信プロフィール タイプ            |
| - templateCode        | String  | O        | テンプレートコード               |
| - templateName        | String  | O        | テンプレート名                |
| - chatBubbleType      | String  | O        | メッセージタイプ               |
| - pushAlarm           | boolean | X        | プッシュアラーム可否             |
| - content             | String  | X        | メッセージ内容               |
| - adult               | boolean | O        | 成人向けメッセージ可否           |
| - header              | String  | X        | ヘッダー（テンプレート内）           |
| - additionalContent   | String  | X        | 補助情報（テンプレート内）        |
| - image               | Object  | X        | 画像情報               |
| -- imageUrl           | String  | O        | 画像 URL              |
| -- imageLink          | String  | X        | 画像リンク               |
| - buttons             | Array   | X        | ボタンリスト                |
| -- name               | String  | O        | ボタン名                |
| -- type               | String  | O        | ボタンタイプ                |
| -- linkMo             | String  | X        | モバイル Web リンク             |
| -- linkPc             | String  | X        | PC Web リンク              |
| -- schemeIos          | String  | X        | iOS アプリリンク             |
| -- schemeAndroid      | String  | X        | Android アプリリンク           |
| -- bizFormId          | Integer | X        | ビズフォーム ID（JSON 基準）     |
| - item                | Object  | X        | ワイドリスト要素           |
| -- list               | Array   | X        | ワイドリスト              |
| --- title             | String  | X        | アイテムタイトル               |
| --- imageUrl          | String  | O        | アイテム画像 URL          |
| --- linkMo            | String  | O        | モバイル Web リンク             |
| --- linkPc            | String  | X        | PC Web リンク              |
| --- schemeIos         | String  | X        | iOS アプリリンク             |
| --- schemeAndroid     | String  | X        | Android アプリリンク           |
| - coupon              | Object  | X        | クーポン要素                |
| -- title              | String  | O        | クーポンタイトル                |
| -- description        | String  | O        | クーポン詳細説明             |
| -- linkMo             | String  | X        | モバイル Web リンク             |
| -- linkPc             | String  | X        | PC Web リンク              |
| -- schemeIos          | String  | X        | iOS アプリリンク             |
| -- schemeAndroid      | String  | X        | Android アプリリンク           |
| - commerce            | Object  | X        | コマース要素               |
| -- title              | String  | O        | 商品タイトル                |
| -- regularPrice       | Integer | X        | 通常価格                |
| -- discountPrice      | Integer | X        | 割引価格                 |
| -- discountRate       | Integer | X        | 割引率                  |
| -- discountFixed      | Integer | X        | 定額割引価格               |
| - video               | Object  | X        | 動画要素               |
| -- videoUrl           | String  | O        | カカオ TV 動画 URL        |
| -- thumbnailUrl       | String  | X        | 動画サムネイル用画像 URL     |
| - carousel            | Object  | X        | カルーセル                  |
| -- head               | Object  | X        | カルーセルイントロ              |
| --- header            | String  | O        | カルーセルイントロヘッダー           |
| --- content           | String  | O        | カルーセルイントロ内容           |
| --- imageUrl          | String  | O        | カルーセルイントロ画像アドレス       |
| --- linkMo            | String  | X        | モバイル Web リンク             |
| --- linkPc            | String  | X        | PC Web リンク              |
| --- schemeIos         | String  | X        | iOS アプリリンク             |
| --- schemeAndroid     | String  | X        | Android アプリリンク           |
| -- list               | Array   | O        | カルーセルリスト              |
| --- header            | String  | O        | カルーセルアイテムヘッダー           |
| --- message           | String  | O        | カルーセルアイテムメッセージ          |
| --- additionalContent | String  | X        | 補助情報（カルーセルアイテム内）    |
| --- imageUrl          | String  | O        | 画像 URL（カルーセルアイテム内）  |
| --- imageLink         | String  | X        | 画像リンク（カルーセルアイテム内）   |
| --- commerce          | Object  | O        | コマース（カルーセルアイテム内）      |
| ---- title            | String  | O        | 商品タイトル                |
| ---- regularPrice     | Integer | X        | 通常価格                |
| ---- discountPrice    | Integer | X        | 割引価格                 |
| ---- discountRate     | Integer | X        | 割引率                  |
| ---- discountFixed    | Integer | X        | 定額割引価格               |
| --- buttons           | Array   | O        | カルーセルリストボタンリスト        |
| ---- name             | String  | O        | ボタン名                |
| ---- type             | String  | O        | ボタンタイプ                |
| ---- linkMo           | String  | X        | モバイル Web リンク             |
| ---- linkPc           | String  | X        | PC Web リンク              |
| ---- schemeIos        | String  | X        | iOS アプリリンク             |
| ---- schemeAndroid    | String  | X        | Android アプリリンク           |
| ---- bizFormId        | Integer | X        | ビズフォーム ID（JSON 基準）     |
| --- coupon            | Object  | X        | クーポン要素（カルーセルアイテム内）    |
| ---- title            | String  | X        | クーポンタイトル                |
| ---- description      | String  | X        | クーポン詳細説明             |
| ---- linkMo           | String  | X        | モバイル Web リンク             |
| ---- linkPc           | String  | X        | PC Web リンク              |
| ---- schemeIos        | String  | X        | iOS アプリリンク             |
| ---- schemeAndroid    | String  | X        | Android アプリリンク           |
| -- tail               | Object  | X        | もっと見るボタン情報            |
| --- linkMo            | String  | O        | モバイル Web リンク             |
| --- linkPc            | String  | X        | PC Web リンク              |
| --- schemeIos         | String  | X        | iOS アプリリンク             |
| --- schemeAndroid     | String  | X        | Android アプリリンク           |
| - status              | String  | O        | テンプレート状態（A：登録、S：ブロック） |
| - createDate          | String  | O        | 登録日時                |
| - updateDate          | String  | X        | 修正日時                |

<a id="register-template"></a>

### テンプレート登録

<a id="requested-9"></a>

#### 要求

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | タイプ     | 説明     |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="note"></a>

#### 注意事項

* クーポンタイトルに置換子を適用する場合、次のような固定置換子を使用する必要があります。

```
- #{할인금액}원 할인 쿠폰(#{할인금액} 범위는 1 ~ 99,999,999)
- #{할인율}% 할인 쿠폰(#{할인율} 범위는 1 ~ 100)
- 배송비 할인 쿠폰
- #{상품명} 무료 쿠폰(#{상품명}은 최대 7자)
- #{상품명} UP 쿠폰(#{상품명}은 최대 7자)
```

* コマースで商品タイトルを除く regularPrice、discountPrice、discountRate、discountFixed フィールドには、ユーザーが置換子を指定することはできません。
    * 商品タイトルを除くすべてのフィールドの値を空にした場合、自動的に固定置換子が入力されて保存されます。
    * regularPrice -> #{정상가격}
    * discountPrice -> #{할인가격}
    * discountRate -> #{할인율}
    * discountFixed -> #{정액할인가격}

<a id="request-to-register-text-type-template"></a>

#### テキスト型テンプレート登録要求

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

| 名前              | タイプ     | 必須 | 説明                                                                                                                                                                                                                                             |
|-----------------|---------|----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名（最大200文字）                                                                                                                                                                                                                                 |
| chatBubbleType  | String  | O  | メッセージタイプ（TEXT, IMAGE, WIDE, WIDE_ITEM_LIST, PREMIUM_VIDEO, COMMERCE, CAROUSEL_FEED, CAROUSEL_COMMERCE）                                                                                                                                           |
| adult           | boolean | X  | アダルトメッセージかどうか（デフォルト値: false）                                                                                                                                                                                                                   |
| content         | String  | O  | - TEXT タイプの場合最大1,300文字（改行: 最大99個、URL形式入力可能）<br>- IMAGE タイプの場合最大1,300文字（改行: 最大99個、URL形式入力可能）<br>- WIDE タイプの場合最大76文字（改行: 最大5個）<br>- PREMIUM_VIDEO タイプの場合該当フィールドをオプションとして使用可能、最大76文字（改行: 最大5個）<br>- その他のタイプの場合該当フィールドは使用しません |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合クーポン適用時最大4個、その他最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大2個<br>- PREMIUM_VIDEO タイプの場合最大1個<br>- COMMERCE タイプの場合最小1個最大2個                                                                                   |
| - name          | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字<br>置換変数使用不可                                                                                                                                                                      |
| - type          | String  | O  | ボタンタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、MD: メッセージ転送、AC: チャンネル追加、BT: チャットボット転換、BF: ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合最初のボタンとして、その他のメッセージタイプの場合最後のボタンとして登録する必要があります             |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                            |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500文字制限                                                                                                                                                                                                           |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                        |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                          |
| - bizFormKey    | String  | X  | BF タイプボタンの場合ビジネスフォームキー<br>置換変数使用不可                                                                                                                                                                                                              |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                         |
| - title         | String  | O  | title の場合5つの形式に制限されます<br>- "${数字}円 割引クーポン" 数字は1以上99,999,999以下<br>- "${数字}% 割引クーポン" 数字は1以上100以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                            |
| - description   | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字、改行: 不可<br>- その他のタイプの場合最大12文字、改行: 不可                                                                                                                                       |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）になります。                                                    |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500文字制限                                                                                                                                                                                                           |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）になります。                                                  |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式: alimtalk=coupon://）を入力する場合残りのフィールドが選択事項（オプション）になります。                                                    |

<a id="request-to-register-image-type-template"></a>

#### 画像型テンプレート登録リクエスト

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

| 名前              | タイプ     | 必須 | 説明                                                                                                                                                                                                                                                  |
|-----------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名（最大200文字）                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXT タイプの場合最大1,300文字（改行：最大99個、URL形式入力可能）<br>- IMAGE タイプの場合最大1,300文字（改行：最大99個、URL形式入力可能）<br>- WIDE タイプの場合最大76文字（改行：最大5個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションとして使用可能、最大76文字（改行：最大5個）<br>- その他のタイプの場合、このフィールドは使用しません |
| image           | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合必須フィールド                                                                                                                                                                                                      |
| - imageUrl      | String  | O  | 画像URL、一般画像としてアップロードされた画像URLを使用<br>置換子使用不可                                                                                                                                                                                                      |
| - imageLink     | String  | X  | 画像クリック時に移動するURL、500文字制限<br>未設定時はカカオトーク内画像ビューアを使用<br>置換子使用不可                                                                                                                                                                                    |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大4個、その他最大5個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大2個<br>- PREMIUM_VIDEO タイプの場合最大1個<br>- COMMERCE タイプの場合最小1個最大2個                                                                                       |
| - name          | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大14文字<br>- その他のタイプの場合最大8文字<br>置換子使用不可                                                                                                                                                                            |
| - type          | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合最初のボタンとして、その他のメッセージタイプの場合最後のボタンとして登録する必要があります                   |
| - linkMo        | String  | X  | モバイルWebリンク（WL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC Webリンク（WL タイプの場合オプションフィールド）、500文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500文字制限                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF タイプボタンの場合、ビジネスフォームキー                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                               |
| - title         | String  | O  | title の場合5つの形式に制限されます<br>- 「${数字}円割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                                  |
| - description   | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大18文字、改行：不可<br>- その他のタイプの場合最大12文字、改行：不可                                                                                                                                            |
| - linkMo        | String  | X  | モバイルWebリンク（WL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドはオプション項目となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドがオプション項目となります。                                         |
| - linkPc        | String  | X  | PC Webリンク（WL タイプの場合オプションフィールド）、500文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドはオプション項目となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドがオプション項目となります。                                       |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドはオプション項目となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドがオプション項目となります。                                         |

<a id="request-to-register-wide-image-type-template"></a>

#### ワイド画像型テンプレート登録要求

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

| 名前              | 種類      | 必須 | 説明                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名（最大 200 文字）                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                                             |
| content         | String  | O  | - TEXT タイプの場合最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- IMAGE タイプの場合最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- WIDE タイプの場合最大 76 文字（改行：最大 5 個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションとして使用可能、最大 76 文字（改行：最大 5 個）<br>- その他のタイプの場合、このフィールドは使用しません |
| image           | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合必須フィールド                                                                                                                                                                                                      |
| - imageUrl      | String  | O  | 画像 URL、ワイド画像としてアップロードされた画像 URL を使用<br>置換値使用不可                                                                                                                                                                                                     |
| - imageLink     | String  | X  | 画像クリック時に移動する URL、500 文字制限<br>未設定時はカカオトーク内画像ビューアを使用<br>置換値使用不可                                                                                                                                                                                    |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大 4 個、その他最大 5 個<br>- WIDE、WIDE_ITEM_LIST タイプの場合最大 2 個<br>- PREMIUM_VIDEO タイプの場合最大 1 個<br>- COMMERCE タイプの場合最小 1 個最大 2 個                                                                                       |
| - name          | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大 14 文字<br>- その他のタイプの場合最大 8 文字<br>置換値使用不可                                                                                                                                                                            |
| - type          | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合最初のボタンとして、その他のメッセージタイプの場合最後のボタンとして登録する必要があります                   |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF タイプボタンの場合、ビジネスフォームキー                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                               |
| - title         | String  | O  | title の場合、5つの形式に制限されます<br>- "${数字}円割引クーポン" 数字は 1 以上 99,999,999 以下<br>- "${数字}% 割引クーポン" 数字は 1 以上 100 以下<br>- "送料割引クーポン"<br>- "${7文字以内} 無料クーポン"<br>- "${7文字以内} UP クーポン"                                                                                  |
| - description   | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大 18 文字、改行：不可<br>- その他のタイプの場合最大 12 文字、改行：不可                                                                                                                                            |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                         |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                       |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）となります。                                                         |

<a id="request-to-register-wide-item-list-type-template"></a>

#### ワイドアイテムリストタイプテンプレート登録リクエスト

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

| 名前               | 種類      | 必須 | 説明                                                                                                                                                                                                                                |
|------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName     | String  | O  | テンプレート名（最大200文字）                                                                                                                                                                                                                   |
| chatBubbleType   | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                             |
| adult            | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                           |
| header           | String  | O  | ヘッダー<br>- WIDE_ITEM_LIST タイプの場合、必須フィールドで最大20文字（改行：不可）<br>- PREMIUM_VIDEO タイプの場合、選択フィールドで最大20文字（改行：不可）                                                                                                                         |
| item             | Object  | O  | ワイドリスト要素（WIDE_ITEM_LIST タイプでのみ使用可能）                                                                                                                                                                                           |
| - list           | List    | O  | ワイドリスト（最小：3、最大4）                                                                                                                                                                                                             |
| -- title         | String  | O  | アイテムタイトル<br>- 最初のアイテムは最大25文字制限（改行：最大1つ、最初のアイテムの場合、title は必須値ではない）<br>- 最初のアイテムは title が必須フィールドではない<br>- 2〜4番目のアイテムは最大30文字制限（改行：最大1つ）                                                                                     |
| -- imageUrl      | String  | O  | アイテム画像URL<br>- 最初のアイテムには最初のワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>- 2〜4番目のアイテムは一般ワイドアイテムリスト画像としてアップロードされた画像URLを使用<br>置換値使用不可                                                                                                   |
| -- linkMo        | String  | O  | モバイルWebリンク、500文字制限                                                                                                                                                                                                               |
| -- linkPc        | String  | X  | PC Webリンク、500文字制限                                                                                                                                                                                                                |
| -- schemeAndroid | String  | X  | Androidアプリリンク、500文字制限                                                                                                                                                                                                             |
| -- schemeIos     | String  | X  | iOSアプリリンク、500文字制限                                                                                                                                                                                                               |
| buttons          | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時は最大4つ、それ以外は最大5つ<br>- WIDE、WIDE_ITEM_LIST タイプの場合、最大2つ<br>- PREMIUM_VIDEO タイプの場合、最大1つ<br>- COMMERCE タイプの場合、最小1つ最大2つ                                                                     |
| - name           | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合、最大14文字<br>- その他のタイプの場合、最大8文字<br>置換値使用不可                                                                                                                                                          |
| - type           | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合は最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります |
| - linkMo         | String  | X  | モバイルWebリンク（WL タイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                             |
| - linkPc         | String  | X  | PC Webリンク（WL タイプの場合は選択フィールド）、500文字制限                                                                                                                                                                                              |
| - schemeAndroid  | String  | X  | Androidアプリリンク（AL タイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                           |
| - schemeIos      | String  | X  | iOSアプリリンク（AL タイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                             |
| - bizFormKey     | String  | X  | BF タイプボタンの場合のビジネスフォームキー                                                                                                                                                                                                                |
| coupon           | Object  | X  | クーポン要素                                                                                                                                                                                                                             |
| - title          | String  | O  | title の場合、5つの形式に制限されます<br>- 「${数字}円割引クーポン」数字は1以上99,999,999以下<br>- 「${数字}%割引クーポン」数字は1以上100以下<br>- 「送料割引クーポン」<br>- 「${7文字以内}無料クーポン」<br>- 「${7文字以内}UPクーポン」                                                                |
| - description    | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合、最大18文字、改行：不可<br>- その他のタイプの場合、最大12文字、改行：不可                                                                                                                          |
| - linkMo         | String  | X  | モバイルWebリンク（WL タイプの場合は必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| - linkPc         | String  | X  | PC Webリンク（WL タイプの場合は選択フィールド）、500文字制限                                                                                                                                                                                              |
| - schemeAndroid  | String  | X  | Androidアプリリンク（AL タイプの場合は必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                     |
| - schemeIos      | String  | X  | iOSアプリリンク（AL タイプの場合は必須フィールド）、500文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |

<a id="request-to-register-premium-video-type-template"></a>

#### プレミアム動画型テンプレート登録要求

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

| 名前              | タイプ     | 必須 | 説明                                                                                                                                                                                                                                                  |
|-----------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName    | String  | O  | テンプレート名（最大 200 文字）                                                                                                                                                                                                                                     |
| chatBubbleType  | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                                               |
| adult           | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                                             |
| content         | String  | X  | - TEXT タイプの場合、最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- IMAGE タイプの場合、最大 1,300 文字（改行：最大 99 個、URL 形式入力可能）<br>- WIDE タイプの場合、最大 76 文字（改行：最大 5 個）<br>- PREMIUM_VIDEO タイプの場合、このフィールドをオプションで使用可能、最大 76 文字（改行：最大 5 個）<br>- その他のタイプの場合、このフィールドは使用しません |
| header          | String  | X  | ヘッダー<br>- WIDE_ITEM_LIST タイプの場合、必須フィールドで最大 20 文字（改行：不可）<br>- PREMIUM_VIDEO タイプの場合、選択フィールドで最大 20 文字（改行：不可）                                                                                                                                           |
| video           | Object  | O  | 動画要素（PREMIUM_VIDEO タイプでのみ使用可能）                                                                                                                                                                                                                    |
| - videoUrl      | String  | O  | カカオTV 動画 URL（カカオTV にアップロードされた動画アドレスのみ使用可能）、最大 500 文字制限<br>置換変数使用不可                                                                                                                                                                                 |
| - thumbnailUrl  | String  | X  | 動画サムネイル用画像 URL。一般画像でアップロードされた URL のみ使用可能（ない場合はカカオTV 動画のデフォルトサムネイルを使用）、最大 500 文字制限<br>置換変数使用不可                                                                                                                                                    |
| buttons         | List    | X  | ボタンリスト<br>- TEXT、IMAGE タイプの場合、クーポン適用時最大 4 個、その他最大 5 個<br>- WIDE、WIDE_ITEM_LIST タイプの場合、最大 2 個<br>- PREMIUM_VIDEO タイプの場合、最大 1 個<br>- COMMERCE タイプの場合、最小 1 個最大 2 個                                                                                       |
| - name          | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合、最大 14 文字<br>- その他のタイプの場合、最大 8 文字<br>置換変数使用不可                                                                                                                                                                            |
| - type          | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合は最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります                   |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                                               |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、1,000 文字制限                                                                                                                                                                                                             |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                                               |
| - bizFormKey    | String  | X  | BF タイプボタンの場合、ビジネスフォームキー                                                                                                                                                                                                                                  |
| coupon          | Object  | X  | クーポン要素                                                                                                                                                                                                                                               |
| - title         | String  | O  | title の場合、5 つの形式に制限されます<br>- "${数字}円 割引クーポン" 数字は 1 以上 99,999,999 以下<br>- "${数字}% 割引クーポン" 数字は 1 以上 100 以下<br>- "送料割引クーポン"<br>- "${7 文字以内} 無料クーポン"<br>- "${7 文字以内} UP クーポン"                                                                                  |
| - description   | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合、最大 18 文字、改行：不可<br>- その他のタイプの場合、最大 12 文字、改行：不可                                                                                                                                            |
| - linkMo        | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                         |
| - linkPc        | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                                                |
| - schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                       |
| - schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                                         |

<a id="request-to-register-commerce-type-template"></a>

#### コマーステンプレート登録リクエスト

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

| 名前                | 型      | 必須 | 説明                                                                                                                                                                                                                                |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | テンプレート名（最大200文字）                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                             |
| adult             | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                           |
| additionalContent | String  | X  | 付加情報（最大34文字、改行：最大1個）、コマース型でのみ使用可能                                                                                                                                                                                          |
| image             | Object  | O  | 画像要素<br>- IMAGE、WIDE、COMMERCE タイプの場合は必須フィールド                                                                                                                                                                                    |
| - imageUrl        | String  | O  | 画像URL、一般画像としてアップロードされた画像URLを使用<br>置換子使用不可                                                                                                                                                                                    |
| - imageLink       | String  | X  | 画像クリック時に移動するURL、500文字制限<br>未設定時はカカオトーク内画像ビューアーを使用<br>置換子使用不可                                                                                                                                                                  |
| commerce          | Object  | O  | コマース（COMMERCEタイプでのみ使用可能）                                                                                                                                                                                                        |
| title             | String  | O  | 商品タイトル（最大30文字、改行：不可）                                                                                                                                                                                                           |
| regularPrice      | Integer | O  | 通常価格（0〜99,999,999）<br>置換子のユーザー指定不可、値を空にすると固定置換子`#{정상가격}`として保存される                                                                                                                                                          |
| discountPrice     | Integer | X  | 割引価格（0〜99,999,999）<br>置換子のユーザー指定不可、値を空にすると固定置換子`#{할인가격}`として保存される                                                                                                                                                            |
| discountRate      | Integer | X  | 割引率（0〜100）、割引価格が存在する場合は割引率、定額割引価格のいずれかが必須<br>置換子のユーザー指定不可、値を空にすると固定置換子`#{할인율}`として保存される                                                                                                                                      |
| discountFixed     | Integer | X  | 定額割引価格（0〜999,999）、割引価格が存在する場合は割引率、定額割引価格のいずれかが必須<br>置換子のユーザー指定不可、値を空にすると固定置換子`#{정액할인가격}`として保存される                                                                                                                            |
| buttons           | List    | O  | ボタンリスト<br>- TEXT、IMAGEタイプの場合、クーポン適用時は最大4個、その他は最大5個<br>- WIDE、WIDE_ITEM_LISTタイプの場合は最大2個<br>- PREMIUM_VIDEOタイプの場合は最大1個<br>- COMMERCEタイプの場合は最小1個、最大2個                                                                     |
| - name            | String  | O  | ボタンタイトル<br>- TEXT、IMAGEタイプの場合は最大14文字<br>- その他のタイプの場合は最大8文字<br>置換子使用不可                                                                                                                                                          |
| - type            | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- ACタイプはTEXT、IMAGEの場合は最初のボタンとして、その他のメッセージタイプの場合は最後のボタンとして登録する必要があります |
| - linkMo          | String  | X  | モバイルウェブリンク（WLタイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                             |
| - linkPc          | String  | X  | PCウェブリンク（WLタイプの場合は選択フィールド）、500文字制限                                                                                                                                                                                              |
| - schemeAndroid   | String  | X  | Androidアプリリンク（ALタイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                           |
| - schemeIos       | String  | X  | iOSアプリリンク（ALタイプの場合は必須フィールド）、500文字制限                                                                                                                                                                                             |
| - bizFormKey      | String  | X  | BFタイプボタンの場合のビジネスフォームキー                                                                                                                                                                                                                |
| coupon            | Object  | X  | クーポン要素                                                                                                                                                                                                                             |
| - title           | String  | O  | titleの場合、5つの形式に制限される<br>- 「${数字}원 할인 쿠폰」数字は1以上99,999,999以下<br>- 「${数字}% 할인 쿠폰」数字は1以上100以下<br>- 「배송비 할인 쿠폰」<br>- 「${7文字以内} 무료 쿠폰」<br>- 「${7文字以内} UP 쿠폰」                                                                |
| - description     | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEOタイプの場合は最大18文字、改行：不可<br>- その他のタイプの場合は最大12文字、改行：不可                                                                                                                          |
| - linkMo          | String  | X  | モバイルウェブリンク（WLタイプの場合は必須フィールド）、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| - linkPc          | String  | X  | PCウェブリンク（WLタイプの場合は選択フィールド）、500文字制限                                                                                                                                                                                              |
| - schemeAndroid   | String  | X  | Androidアプリリンク（ALタイプの場合は必須フィールド）、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                     |
| - schemeIos       | String  | X  | iOSアプリリンク（ALタイプの場合は必須フィールド）、500文字制限<br>クーポンにlinkMoフィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_androidまたはscheme_iosフィールドにチャンネルクーポンURL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |

<a id="request-to-register-carousel-feed-type-template"></a>

#### カルーセルフィード型テンプレート登録要求

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

| 名前                | タイプ      | 必須 | 説明                                                                                                                                                                                                                                |
|-------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName      | String  | O  | テンプレート名（最大 200 文字）                                                                                                                                                                                                                   |
| chatBubbleType    | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                             |
| pushAlarm         | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                       |
| adult             | boolean | X  | 成人向けメッセージ可否（デフォルト値：false）                                                                                                                                                                                                           |
| carousel          | Object  | O  | カルーセル                                                                                                                                                                                                                               |
| - list            | List    | O  | カルーセルリスト（最小 2 個、最大 6 個）                                                                                                                                                                                                            |
| -- header         | String  | O  | カルーセルアイテムタイトル（最大 20 文字）、カルーセルフィードタイプでのみ使用可能                                                                                                                                                                                              |
| -- message        | String  | O  | カルーセルアイテムタイトル（最大 20 文字）、カルーセルアイテムメッセージ（最大 180 文字）、カルーセルフィードタイプでのみ使用可能                                                                                                                                                        |
| -- imageUrl       | String  | O  | 画像 URL（カルーセルフィードタイプ画像としてアップロードされた画像のみ使用可能）<br>置換値使用不可                                                                                                                                                                              |
| -- imageLink      | String  | X  | 画像リンク、500 文字制限<br>置換値使用不可                                                                                                                                                                                                    |
| -- buttons        | List    | O  | カルーセルリストボタン一覧最小 1 個、最大 2 個                                                                                                                                                                                                        |
| --- name          | String  | O  | ボタンタイトル<br>- TEXT、IMAGEタイプの場合最大 14 文字<br>- それ以外のタイプの場合最大 8 文字<br>置換値使用不可                                                                                                                                                          |
| --- type          | String  | O  | ボタンタイプ（WL：Webリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合 1 番目のボタンとして、その他のメッセージタイプの場合最後のボタンとして登録する必要があります |
| --- linkMo        | String  | X  | モバイル Web リンク（WL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                             |
| --- linkPc        | String  | X  | PC Web リンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                              |
| --- schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                           |
| --- schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                             |
| --- bizFormKey    | String  | X  | BF タイプボタンの場合ビジネスフォームキー                                                                                                                                                                                                                |
| -- coupon         | Object  | X  | クーポン要素                                                                                                                                                                                                                             |
| --- title         | String  | O  | title の場合 5 つの形式に制限されます<br>- 「${数字}円割引クーポン」数字は 1 以上 99,999,999 以下<br>- 「${数字}%割引クーポン」数字は 1 以上 100 以下<br>- 「送料割引クーポン」<br>- 「${7 文字以内}無料クーポン」<br>- 「${7 文字以内}UPクーポン」                                                                |
| --- description   | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大 18 文字、改行：不可<br>- それ以外のタイプの場合最大 12 文字、改行：不可                                                                                                                          |
| --- linkMo        | String  | X  | モバイル Web リンク（WL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| --- linkPc        | String  | X  | PC Web リンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                              |
| --- schemeAndroid | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                     |
| --- schemeIos     | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）になり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| - tail            | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                         |
| -- linkMo         | String  | O  | モバイル Web リンク、500 文字制限<br>置換値使用不可                                                                                                                                                                                                 |
| -- linkPc         | String  | X  | PC Web リンク、500 文字制限<br>置換値使用不可                                                                                                                                                                                                  |
| -- schemeAndroid  | String  | X  | Android アプリリンク、500 文字制限<br>置換値使用不可                                                                                                                                                                                               |
| -- schemeIos      | String  | X  | iOS アプリリンク、500 文字制限<br>置換値使用不可                                                                                                                                                                                                 |
| recipientList     | List    | O  | 受信者一覧（最大 1,000 名）                                                                                                                                                                                                                 |
| - recipientNo     | String  | O  | 受信番号                                                                                                                                                                                                                             |
| createUser        | String  | X  | 登録者（コンソールから送信時、ユーザー UUID で保存）                                                                                                                                                                                                       |

<a id="request-to-register-carousel-commerce-type-template"></a>

#### カルーセルコマース型テンプレート登録リクエスト

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

| 名前                   | 種類      | 必須 | 説明                                                                                                                                                                                                                                |
|----------------------|---------|----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName         | String  | O  | テンプレート名（最大 200 文字）                                                                                                                                                                                                                   |
| chatBubbleType       | String  | O  | メッセージタイプ（TEXT、IMAGE、WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO、COMMERCE、CAROUSEL_FEED、CAROUSEL_COMMERCE）                                                                                                                             |
| pushAlarm            | boolean | X  | メッセージプッシュアラーム送信可否（デフォルト値：true）                                                                                                                                                                                                       |
| adult                | boolean | X  | 成人向けメッセージかどうか（デフォルト値：false）                                                                                                                                                                                                           |
| carousel             | Object  | O  | カルーセル                                                                                                                                                                                                                               |
| - head               | Object  | X  | カルーセルイントロ                                                                                                                                                                                                                           |
| -- header            | String  | O  | カルーセルイントロヘッダー（最大 20 文字）                                                                                                                                                                                                                |
| -- content           | String  | O  | カルーセルイントロ内容（最大 50 文字）                                                                                                                                                                                                                |
| -- imageUrl          | String  | O  | カルーセルイントロ画像アドレス（カルーセルコマース型画像としてアップロードされた画像使用、使用される画像はカルーセルの画像と比率が同一である必要があります）<br>置換子使用不可能                                                                                                                                          |
| -- linkMo            | String  | X  | モバイルウェブリンク（linkMo、linkPc、schemeAndroid、schemeIos のいずれかを使用する場合、linkMo は必須値）、500 文字制限                                                                                                                                        |
| -- linkPc            | String  | X  | PC ウェブリンク、500 文字制限                                                                                                                                                                                                               |
| -- schemeAndroid     | String  | X  | Android アプリリンク、500 文字制限                                                                                                                                                                                                             |
| -- schemeIos         | String  | X  | iOS アプリリンク、500 文字制限                                                                                                                                                                                                               |
| - list               | List    | O  | カルーセルリスト（head が存在する場合、最小 1 個、最大 5 個 / それ以外は最小 2 個、最大 6 個）                                                                                                                                                                          |
| -- additionalContent | String  | X  | 付加情報（最大 34 文字）、カルーセルコマース型でのみ使用可能                                                                                                                                                                                                  |
| -- imageUrl          | String  | O  | 画像 URL（カルーセルコマース型画像としてアップロードされた画像使用）<br>置換子使用不可能                                                                                                                                                                                 |
| -- imageLink         | String  | X  | 画像リンク、500 文字制限<br>置換子使用不可能                                                                                                                                                                                                    |
| -- commerce          | Object  | O  | コマース（CAROUSEL_COMMERCE タイプでのみ使用可能）                                                                                                                                                                                               |
| --- title            | String  | O  | 商品タイトル（最大 30 文字、改行：不可）                                                                                                                                                                                                           |
| --- regularPrice     | Integer | O  | 正規価格（0〜99,999,999）<br>置換子ユーザー指定不可能、値を空にすると固定置換子 `#{정상가격}` として保存されます                                                                                                                                                          |
| --- discountPrice    | Integer | X  | 割引価格（0〜99,999,999）<br>置換子ユーザー指定不可能、値を空にすると固定置換子 `#{할인가격}` として保存されます                                                                                                                                                            |
| --- discountRate     | Integer | X  | 割引率（0〜100）、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>置換子ユーザー指定不可能、値を空にすると固定置換子 `#{할인율}` として保存されます                                                                                                                                      |
| --- discountFixed    | Integer | X  | 定額割引価格（0〜999,999）、割引価格が存在する場合、割引率、定額割引価格のいずれかは必須<br>置換子ユーザー指定不可能、値を空にすると固定置換子 `#{정액할인가격}` として保存されます                                                                                                                            |
| -- buttons           | List    | O  | カルーセルリストボタンリスト最小 1 個、最大 2 個                                                                                                                                                                                                        |
| --- name             | String  | O  | ボタンタイトル<br>- TEXT、IMAGE タイプの場合最大 14 文字<br>- それ以外のタイプの場合最大 8 文字<br>置換子使用不可能                                                                                                                                                          |
| --- type             | String  | O  | ボタンタイプ（WL：ウェブリンク、AL：アプリリンク、BK：ボットキーワード、MD：メッセージ転送、AC：チャンネル追加、BT：チャットボット転換、BF：ビジネスフォーム）<br>- AC タイプは TEXT、IMAGE の場合、最初のボタンとして、それ以外のメッセージタイプの場合、最後のボタンとして登録する必要があります |
| --- linkMo           | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                             |
| --- linkPc           | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                           |
| --- schemeIos        | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限                                                                                                                                                                                             |
| --- bizFormKey       | String  | X  | BF タイプボタンの場合、ビズフォームキー                                                                                                                                                                                                                |
| -- coupon            | Object  | X  | クーポン要素                                                                                                                                                                                                                             |
| --- title            | String  | O  | title の場合、5 種類の形式に制限されます<br>- "${数字}円割引クーポン" 数字は 1 以上 99,999,999 以下<br>- "${数字}% 割引クーポン" 数字は 1 以上 100 以下<br>- "送料割引クーポン"<br>- "${7 文字以内} 無料クーポン"<br>- "${7 文字以内} UP クーポン"                                                                |
| --- description      | String  | O  | クーポン詳細説明<br>- WIDE、WIDE_ITEM_LIST、PREMIUM_VIDEO タイプの場合最大 18 文字、改行：不可<br>- それ以外のタイプの場合最大 12 文字、改行：不可                                                                                                                          |
| --- linkMo           | String  | X  | モバイルウェブリンク（WL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| --- linkPc           | String  | X  | PC ウェブリンク（WL タイプの場合選択フィールド）、500 文字制限                                                                                                                                                                                              |
| --- schemeAndroid    | String  | X  | Android アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                     |
| --- schemeIos        | String  | X  | iOS アプリリンク（AL タイプの場合必須フィールド）、500 文字制限<br>クーポンに linkMo フィールドを入力する場合、残りのフィールドは選択事項（オプション）となり、<br>scheme_android または scheme_ios フィールドにチャンネルクーポン URL（形式：alimtalk=coupon://）を入力する場合、残りのフィールドが選択事項（オプション）になります。                                       |
| - tail               | Object  | X  | もっと見るボタン情報                                                                                                                                                                                                                         |
| -- linkMo            | String  | O  | モバイルウェブリンク、500 文字制限<br>置換子使用不可能                                                                                                                                                                                                 |
| -- linkPc            | String  | X  | PC ウェブリンク、500 文字制限<br>置換子使用不可能                                                                                                                                                                                                  |
| -- schemeAndroid     | String  | X  | Android アプリリンク、500 文字制限<br>置換子使用不可能                                                                                                                                                                                               |
| -- schemeIos         | String  | X  | iOS アプリリンク、500 文字制限<br>置換子使用不可能                                                                                                                                                                                                 |

<a id="response-10"></a>

#### レスポンス

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

| 名前              | タイプ     | Not Null | 説明       |
|:----------------|:--------|:---------|:---------|
| header          | Object  | O        | ヘッダー領域   |
| - resultCode    | Integer | O        | 結果コード    |
| - resultMessage | String  | O        | 結果メッセージ  |
| - isSuccessful  | boolean | O        | 成功可否     |
| template        | Object  | X        | テンプレート情報 |
| - templateCode  | String  | O        | テンプレートコード |

<a id="modify-template"></a>

### テンプレート修正

<a id="requested-10"></a>

#### リクエスト

[URL]

```
PUT  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | タイプ    | 説明       |
|--------------|--------|----------|
| appkey       | String | 固有のアプリケーションキー |
| senderKey    | String | 発信キー     |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                 |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成することができます。 |

[Request Body]

* テンプレート登録と仕様が同じ

<a id="response-11"></a>

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 名前              | タイプ      | Not Null | 説明       |
|:----------------|:--------|:---------|:---------|
| header          | Object  | O        | ヘッダー領域   |
| - resultCode    | Integer | O        | 結果コード    |
| - resultMessage | String  | O        | 結果メッセージ  |
| - isSuccessful  | boolean | O        | 成功可否     |

<a id="delete-template"></a>

### テンプレート削除

<a id="requested-11"></a>

#### リクエスト

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | タイプ    | 説明      |
|--------------|--------|---------|
| appkey       | String | 固有のアプリキー |
| senderKey    | String | 発信キー    |
| templateCode | String | テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="response-12"></a>

#### レスポンス

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 名前              | タイプ     | Not Null | 説明      |
|:----------------|:--------|:---------|:--------|
| header          | Object  | O        | ヘッダー領域  |
| - resultCode    | Integer | O        | 結果コード   |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成功可否    |

<a id="manage-image"></a>

## 画像管理

<a id="upload-image"></a>

### 画像アップロード

<a id="requested-12"></a>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: multipart/form-data
```

[Path parameter]

| 名前     | タイプ    | 説明       |
|--------|--------|----------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。   |

[Request parameter]

| 名前        | タイプ    | 必須 | 説明                                                                                                                             |
|-----------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| image     | File   | O  | 画像                                                                                                                            |
| imageType | String | O  | 画像タイプ <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |

<a id="response-13"></a>

#### レスポンス

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

| 名前              | タイプ     | Not Null | 説明        |
|:----------------|:--------|:---------|:----------|
| header          | Object  | O        | ヘッダー領域    |
| - resultCode    | Integer | O        | 結果コード     |
| - resultMessage | String  | O        | 結果メッセージ   |
| - isSuccessful  | boolean | O        | 成功可否      |
| image           | Object  | X        | 画像領域      |
| - imageSeq      | Integer | O        | 画像シーケンス   |
| - imageUrl      | String  | O        | 画像 URL    |
| - imageName     | String  | X        | 画像名       |

<a id="upload-image-specifications"></a>

#### アップロード画像仕様
| 画像タイプ                     | 使用先                                                     | アップロード画像仕様                                                                                                                              |
|:---------------------------|:--------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------|
| IMAGE                      | 画像型画像、コマース型画像、プレミアム動画型サムネイル                        | 推奨サイズ: 800 X 400px (横 500px 以上)<br/>画像比率: 0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>ファイル形式および容量制限: jpg, png / 最大 5MB                                |
| WIDE_IMAGE                 | ワイド画像型画像                                            | 推奨サイズ: 800 X 600px (横 500px 以上)<br/>画像比率: 0.5 ≤ 縦 ÷ 横 ≤ 1<br>ファイル形式および容量制限: jpg, png / 最大 5MB                                     |
| MAIN_WIDE_ITEMLIST_IMAGE   | ワイドアイテムリストタイプ最初のアイテム画像                                | 制限サイズ: 横 500px 以上<br/>画像比率: 縦 ÷ 横 = 0.5<br/>ファイル形式および容量制限: jpg, png / 最大 5MB                                                      |
| NORMAL_WIDE_ITEMLIST_IMAGE | ワイドアイテムリストタイプ2〜4番目のアイテム画像                              | 制限サイズ: 横 500px 以上<br/>画像比率: 縦 ÷ 横 = 1<br/>ファイル形式およびサイズ : jpg, png / 各ファイル最大 5MB                                                      |
| CAROUSEL_FEED_IMAGE        | カルーセルフィードタイプセル別画像                                          | 推奨サイズ: 800 X 600px または 800 X 400px (横 500px 以上)<br/>画像比率: 0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>ファイル形式および容量制限: jpg, png / 最大 5MB                 |
| CAROUSEL_COMMERCE_IMAGE    | カルーセルコマースタイプイントロ画像、カルーセルコマースタイプセル別画像 | 推奨サイズ: 800 X 600px または 800 X 400px (横 500px 以上)<br/>画像比率: 0.5 ≤ 縦 ÷ 横 ≤ 1.333<br/>ファイル形式および容量制限: jpg, png / 最大 5MB                 |

* テンプレート修正時に画像を別の画像に変更すると、既存の画像がカカオ CDN から削除され、URL が無効になります。同じ画像を使用する他のテンプレートにも影響するため注意が必要です。画像照会 API では画像情報が維持されますが、実際の画像にはアクセスできないため、元のファイルは自社サーバーに別途保管することをお勧めします。

<a id="view-image"></a>

### 画像照会

<a id="requested-13"></a>

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | タイプ    | 説明       |
|--------|--------|----------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。   |

[Request parameter]

| 名前         | タイプ    | 必須 | 説明                                                                                                                             |
|------------|--------|----|--------------------------------------------------------------------------------------------------------------------------------|
| imageTypes | List   | O  | 画像タイプ <br>(IMAGE, WIDE_IMAGE,MAIN_WIDE_ITEMLIST_IMAGE,NORMAL_WIDE_ITEMLIST_IMAGE,CAROUSEL_FEED_IMAGE,CAROUSEL_COMMERCE_IMAGE) |
| pageNum    | String | X  | ページ番号（デフォルト: 1）                                                                                                                  |
| pageSize   | String | X  | 照会件数（デフォルト: 15）                                                                                                                  |

<a id="response-14"></a>

#### レスポンス

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

| 名前              | タイプ     | Not Null | 説明        |
|:----------------|:--------|:---------|:----------|
| header          | Object  | O        | ヘッダー領域    |
| - resultCode    | Integer | O        | 結果コード     |
| - resultMessage | String  | O        | 結果メッセージ   |
| - isSuccessful  | boolean | O        | 成功可否      |
| image           | Object  | X        | 画像領域      |
| - imageSeq      | Integer | O        | 画像シーケンス   |
| - imageUrl      | String  | O        | 画像 URL    |
| - imageName     | String  | X        | 画像名       |

<a id="delete-image"></a>

### 画像削除

<a id="requested-14"></a>

#### リクエスト

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/images
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | タイプ    | 説明       |
|--------|--------|----------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                |
|--------------|--------|----|-------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。   |

[Query parameter]

| 名前       | タイプ    | 必須 | 説明     |
|----------|--------|----|--------|
| imageSeq | String | O  | 画像番号   |

<a id="response-15"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | タイプ     | Not Null | 説明       |
|:----------------|:--------|:---------|:---------|
| header          | Object  | O        | ヘッダー領域   |
| - resultCode    | Integer | O        | 結果コード    |
| - resultMessage | String  | O        | 結果メッセージ  |
| - isSuccessful  | boolean | O        | 成功可否     |

<a id="manage-video"></a>

## 動画管理

ブランドメッセージに使用する動画を登録・照会・削除する API です。登録された動画はカカオビズセンターでエンコーディング処理後、送信に使用でき、ステータスが `PUBLIC` の動画のみテンプレート登録および送信が可能です（`PRIVATE` はテンプレート登録のみ可能）。

<a id="video-upload-flow"></a>

### 動画アップロードフロー

動画アップロードは2段階で進行されます。

1. **動画アップロード登録** — 本API（`POST /brand-message/v1.0/appkeys/{appKey}/videos`）に動画メタ情報（ファイル名・ファイルサイズ）をJSONで送信します。レスポンスとして`video`（NHN Cloud側登録情報）と`uploadInfo`（カカオアップロードURL・トークン）を受け取ります。
2. **動画ファイルアップロード** — レスポンスで受け取った`uploadInfo.uploadUrl`に`multipart/form-data`で動画ファイルを直接アップロードします。認証は`uploadInfo.token`を`x-kamp-upload-token`ヘッダーで送信します。

動画ファイルはNHN Cloudサーバーを経由せず、カカオ側アップロードサーバーに直接送信されます。したがって、本APIのリクエストボディはメタ情報のみJSONで送信し、実際のファイルは2段階で別途送信します。

> **注意**
> * `uploadInfo.token`は発行後5分間有効です。5分が経過すると1段階登録を再度呼び出して新しいトークンを受け取る必要があります。
> * 1段階リクエストの`fileSize`は、実際に2段階でアップロードするファイルのサイズと正確に一致する必要があります（不一致の場合、カカオ側でerrCode 109で拒否）。

<a id="register-video-upload"></a>

### 動画アップロード登録

<a id="requested-15"></a>

#### リクエスト

[URL]

```
POST  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json
```

[Path parameter]

| 名前     | タイプ    | 説明      |
|--------|--------|---------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                  |
|--------------|--------|----|---------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。       |

[Request body]

```
{
  "senderKey": String,
  "fileName": String,
  "fileSize": Long,
  "createUser": String
}
```

| 名前         | タイプ    | 必須 | 説明                                                                    |
|------------|--------|----|-----------------------------------------------------------------------|
| senderKey  | String | O  | 発信プロフィールキー（40文字）                                                       |
| fileName   | String | O  | 動画ファイル名（拡張子含む、MP4・MOV・AVIのいずれか、最大250文字）                            |
| fileSize   | Long   | O  | 動画ファイルサイズ（byte、最大4GB = 4294967296）                                  |
| createUser | String | X  | アップロードユーザー識別子（最大100文字）                                              |

<a id="response-16"></a>

#### レスポンス

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

| 名前              | タイプ      | Not Null | 説明                                                                                |
|:----------------|:--------|:---------|:----------------------------------------------------------------------------------|
| header          | Object  | O        | ヘッダー領域                                                                            |
| - resultCode    | Integer | O        | 結果コード                                                                             |
| - resultMessage | String  | O        | 結果メッセージ                                                                           |
| - isSuccessful  | boolean | O        | 成功可否                                                                              |
| video           | Object  | X        | NHN Cloud 側に登録された動画情報（status = `REGISTERED`）                                    |
| - videoSeq      | Long    | O        | 動画シーケンス                                                                           |
| - vid           | String  | O        | カカオ動画ID                                                                           |
| - senderKey     | String  | O        | 発信プロフィールキー                                                                        |
| - title         | String  | X        | 動画タイトル（アップロード直後はファイル名、エンコーディング完了後にカカオビズセンターで修正した値に同期される）                 |
| - fileName      | String  | O        | アップロードファイル名                                                                       |
| - fileSize      | Long    | O        | ファイルサイズ（byte）                                                                     |
| - status        | String  | O        | 動画状態（[動画状態](#動画-状態) 参照）。アップロード登録レスポンスでは常に `REGISTERED`                      |
| uploadInfo      | Object  | O        | カカオアップロード情報。2段階で使用                                                              |
| - uploadUrl     | String  | O        | 動画ファイルを直接アップロードするカカオ側エンドポイント                                                   |
| - token         | String  | O        | アップロード認証トークン。`x-kamp-upload-token` ヘッダーで送信                                     |

> エンコーディング完了後に入力される `thumbnailUrl`、`videoUrl`、`playUrl`、`updateDate` フィールドは[動画照会](#動画-照会) APIで取得できます。

<a id="upload-video-file"></a>

### 動画ファイルアップロード (2段階)

上記のレスポンスの `uploadInfo.uploadUrl` で動画ファイルを直接呼び出します。このリクエストは NHN Cloud サーバーではなく、カカオ側のアップロードサーバーに直接送信されます。

<a id="requested-16"></a>

#### リクエスト

[URL]

```
POST  {uploadInfo.uploadUrl}
Content-Type: multipart/form-data
```

[Header]

| 名前                  | タイプ    | 必須 | 説明                                          |
|---------------------|--------|----|---------------------------------------------|
| x-kamp-upload-token | String | O  | 1段階レスポンスの `uploadInfo.token` 値をそのまま送信        |

[Request body (multipart)]

| 名前   | タイプ  | 必須 | 説明                                                  |
|------|------|----|-----------------------------------------------------|
| file | File | O  | 動画ファイル。1段階リクエストの `fileSize` と正確に一致する必要があります             |

<a id="response-17"></a>

#### レスポンス

```
{
  "vid": String,
  "playUrl": String,
  "errCode": Integer,
  "message": String
}
```

* 成功時は `vid`（1段階レスポンスと同一）と `playUrl` を返し、`errCode`/`message` フィールドは含まれません。
* 失敗時は HTTP 4xx とともに `errCode`（100～110）と `message` を返します。詳細なエラーコードはカカオビズメッセージガイドを参照してください。

<a id="upload-video-specifications"></a>

#### アップロード動画仕様

| 項目       | 制限                                          |
|:---------|:--------------------------------------------|
| ファイル形式    | MP4、MOV、AVI                               |
| 最大ファイルサイズ | 4GB                                         |
| 最大映像長   | 4時間                                         |
| 最大解像度   | 8K                                          |
| ファイル名長   | 250文字以内                                     |

* アップロードした動画はカカオビズセンターでエンコードが完了した後に使用できます。エンコード時間は映像長によって異なり、通常5〜10分かかります。
* アップロード直後の動画状態は `REGISTERED` で開始し、`ENCODING` を経て `PUBLIC` または `PRIVATE` に転換されます。状態はコンソールまたは[動画照会](#動画照会) API で確認できます。
* 登録された動画はカカオ側で永続保存され、テンプレートが削除されてもカカオビズセンターの動画は自動的に整理されません。カカオチャンネル管理者がチャンネルビジネスホームの管理画面で直接削除できます。
* 1段階登録後、2段階ファイルアップロードが失敗するか遅延してトークン（5分）が期限切れになると、新規登録を再度呼び出す必要があります。登録のみ行われ実際のアップロードが行われなかった動画は、一定時間が経過した後、状態が `ERROR` に自動的にマーキングされます。

<a id="view-video"></a>

### 動画照会

<a id="requested-17"></a>

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 種類     | 説明     |
|--------|--------|--------|
| appkey | String | 固有のアプリケーションキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 種類     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Request parameter]

| 名前         | 種類     | 必須 | 説明                |
|------------|--------|----|-------------------|
| senderKey  | String | X  | 発信プロフィールキー（40文字）     |
| pageNum    | String | X  | ページ番号（デフォルト：1）    |
| pageSize   | String | X  | 照会件数（デフォルト：15）    |

<a id="response-18"></a>

#### レスポンス

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

| 名前                | 種類      | Not Null | 説明         |
|:------------------|:--------|:---------|:-----------|
| header            | Object  | O        | ヘッダー領域      |
| - resultCode      | Integer | O        | 結果コード      |
| - resultMessage   | String  | O        | 結果メッセージ     |
| - isSuccessful    | boolean | O        | 成功可否      |
| videosResponse    | Object  | X        | 動画リスト領域  |
| - videos          | Array   | O        | 動画配列     |
| - totalCount      | Integer | O        | 全体動画数   |

> 各 `videos` 項目のフィールドは[動画アップロードレスポンス](#応答)と同じです。

<a id="delete-video"></a>

### 動画削除

<a id="requested-18"></a>

#### リクエスト

[URL]

```
DELETE  /brand-message/v1.0/appkeys/{appKey}/videos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 型      | 説明      |
|--------|--------|---------|
| appkey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 型      | 必須 | 説明              |
|--------------|--------|----|-----------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

[Query parameter]

| 名前       | 型      | 必須 | 説明                                 |
|----------|--------|----|-----------------------------------|
| videoSeq | String | O  | 動画シーケンス（カンマで区切って複数件を送信可能） |

<a id="response-19"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | 型       | Not Null | 説明     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダー領域 |
| - resultCode    | Integer | O        | 結果コード  |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成功可否   |

<a id="video-status"></a>

### 動画ステータス

動画照会レスポンスの `status` フィールド値を説明します。

| status     | 説明                                  |
|:-----------|:------------------------------------|
| REGISTERED | アップロード登録                              |
| ENCODING   | エンコード中                               |
| PUBLIC     | 公開状態（送信およびテンプレート登録可能）              |
| PRIVATE    | 非公開状態（テンプレート登録可能）                  |
| VIOLATED   | 違反動画                              |
| ILLEGAL    | 不法撮影物動画                           |
| DELETED    | 削除された動画                             |
| ERROR      | アップロードおよびエンコード中にエラーが発生                   |

<a id="upload"></a>

## アップロード

<a id="upload-bizform-key"></a>

### ビズフォームキーアップロード

<a id="requested-19"></a>

#### リクエスト

[URL]

```
POST /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/biz-form
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 種類     | 説明     |
|-----------|--------|--------|
| appkey    | String | 固有のアプリキー |
| senderKey | String | 発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 種類     | 必須 | 説明               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | コンソールで作成できます。 |

<a id="response-20"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | 種類      | Not Null | 説明     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダー領域  |
| - resultCode    | Integer | O        | 結果コード  |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成功可否  |

<a id="manage-outgoing-profiles"></a>

## 発信プロフィール管理

<a id="view-outgoing-profile"></a>

### 発信プロフィール照会

<a id="requested-20"></a>

#### リクエスト

[URL]

```
GET  /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | タイプ    | 説明        |
|-----------|--------|-----------|
| appkey    | String | 固有のアプリキー  |
| senderKey | String | 発信キー      |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                   |
|--------------|--------|----|----------------------|
| X-Secret-Key | String | O  | コンソールで作成することができます。 |

<a id="response-21"></a>

#### レスポンス

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

| 名前                        | タイプ     | Not Null | 説明                                                                                                                       |
|:--------------------------|:--------|:---------|:-------------------------------------------------------------------------------------------------------------------------|
| header                    | Object  | O        | ヘッダー領域                                                                                                                   |
| - resultCode              | Integer | O        | 結果コード                                                                                                                    |
| - resultMessage           | String  | O        | 結果メッセージ                                                                                                                  |
| - isSuccessful            | boolean | O        | 成功可否                                                                                                                     |
| sender                    | Object  | X        | 発信プロフィール                                                                                                                 |
| - plusFriendId            | String  | O        | プラスフレンド ID                                                                                                               |
| - senderKey               | String  | O        | 発信キー                                                                                                                     |
| - categoryCode            | String  | X        | カテゴリーコード                                                                                                                 |
| - unsubscribePhoneNumber  | String  | X        | 無料受信拒否電話番号                                                                                                               |
| - unsubscribeAuthNumber   | String  | X        | 無料受信拒否認証番号                                                                                                               |
| - status                  | String  | X        | NHN Cloud プラスフレンド状態コード <br>(YSC02: 登録待機中、YSC03: 正常登録)                                                                 |
| - statusName              | String  | X        | NHN Cloud プラスフレンド状態名(登録待機中、正常登録)                                                                                       |
| - kakaoStatus             | String  | X        | カカオプラスフレンド状態コード<br>(A: 正常、S: ブロック)<br>status が YSC02 の場合、kakaoStatus は null 値を持ちます。                               |
| - kakaoStatusName         | String  | X        | カカオプラスフレンド状態名(正常、ブロック)<br>status が YSC02 の場合、kakaoStatusName は null 値を持ちます。                                        |
| - kakaoProfileStatus      | String  | X        | カカオプラスフレンドプロフィール状態コード<br>(A: アクティブ化、B:ブロック、C: 非アクティブ化、D:削除 E:削除処理中)<br>status が YSC02 の場合、kakaoProfileStatus は null 値を持ちます。 |
| - kakaoProfileStatusName  | String  | X        | カカオプラスフレンドプロフィール状態名(アクティブ化、非アクティブ化、ブロック、削除処理中、削除)<br>status が YSC02 の場合、kakaoProfileStatusName は null 値を持ちます。         |
| - profileSpamLevel        | String  | X        | カカオトークチャンネルスパム状態名(永久制限、警告制限、正常)<br>発信プロフィール状態が正常でない場合、null 値を持つことがあります。                                            |
| - profileMessageSpamLevel | String  | X        | カカオトークメッセージスパム状態名(活動制限、警告制限、正常)<br>発信プロフィール状態が正常でない場合、null 値を持つことがあります。                                           |
| - block                   | boolean | O        | 発信プロフィールブロック可否                                                                                                           |
| - brandMessage            | Object  | X        | ブランドメッセージ設定情報                                                                                                            |
| -- resendAppKey           | String  | X        | 代替送信として設定する SMS サービスアプリキー                                                                                              |
| -- isResend               | boolean | O        | 代替送信設定(再送信)可否                                                                                                            |
| -- resendSendNo           | String  | X        | 再送信時、tc-sms 発信番号                                                                                                        |
| -- resendUnsubscribeNo    | String  | X        | 再送信時、tc-sms 080 受信拒否番号                                                                                                 |
| - dormant                 | boolean | O        | 発信プロフィール休眠可否                                                                                                             |
| - marketingAgreement      | boolean | O        | M/N タイプ使用申請可否                                                                                                           |
| - createDate              | String  | X        | 登録日時                                                                                                                     |
| - initialUserRestriction  | boolean | O        | 最初のユーザー制限可否                                                                                                              |

<a id="modify-outgoing-profile-080-opt-out-number"></a>

### 発信プロフィール 080 受信拒否番号修正

<a id="requested-21"></a>

#### リクエスト

[URL]

```
PUT /brand-message/v1.0/appkeys/{appKey}/senders/{senderKey}/unsubscribe-content
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | タイプ    | 説明        |
|-----------|--------|-----------|
| appkey    | String | 固有のアプリキー  |
| senderKey | String | 発信キー      |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | タイプ    | 必須 | 説明                   |
|--------------|--------|----|----------------------|
| X-Secret-Key | String | O  | コンソールで作成することができます。 |

[Request body]

```
{
    "unsubscribeNo": String,
    "unsubscribeAuthNo": String
}
```

| 名前                | 	タイプ    | 	必須 | 	説明                                                                                                                              |
|-------------------|---------|-----|----------------------------------------------------------------------------------------------------------------------------------|
| unsubscribeNo     | 	String | 	O  | 080 無料受信拒否電話番号(すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信されます)<br>- 080-xxx-xxxx <br>- 080-xxxx-xxxx <br>- 080xxxxxxx <br>- 080xxxxxxxx  |
| unsubscribeAuthNo | 	String | 	X  | 080 無料受信拒否認証番号(すべて未入力時は発信プロフィールに登録された無料受信拒否情報で送信されます)<br>unsubscribeNo なしで unsubscribeAuthNo のみ入力は不可<br>例: 1234 |

<a id="response-22"></a>

#### レスポンス

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | タイプ     | Not Null | 説明       |
|:----------------|:--------|:---------|:---------|
| header          | Object  | O        | ヘッダー領域   |
| - resultCode    | Integer | O        | 結果コード    |
| - resultMessage | String  | O        | 結果メッセージ  |
| - isSuccessful  | boolean | O        | 成功可否     |

<a id="manage-fallback"></a>

## 代替送信管理

<a id="register-sms-appkey"></a>

### SMS AppKey 登録

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリケーションキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request body]

```
{
    "resendAppKey": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明                    |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | 代替送信に設定する SMS サービスアプリケーションキー |

[例示]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

<a id="response-23"></a>

#### 応答

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

### 代替送信設定登録

[URL]

```
POST  /brand-message/v1.0/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリケーションキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String,
   "resendUnsubscribeNo": String
}
```

| 名前                  | 	タイプ      | 	必須 | 	説明                                                                                                        |
|---------------------|----------|-----|------------------------------------------------------------------------------------------------------------|
| senderKey           | 	String  | 	O  | 発信キー                                                                                                       |
| isResend            | 	Boolean | 	O  | 送信失敗時、SMS 代替送信の可否<br>コンソールで代替送信を設定すると、デフォルトで代替送信されます。                                           |
| resendSendNo        | 	String  | 	X  | 代替送信発信番号<br><span style="color:red">(SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。)</span>                  |
| resendUnsubscribeNo | 	String  | 	X  | 代替送信 080 受信拒否番号<br><span style="color:red">(SMS サービスに登録された 080 受信拒否番号でない場合、代替送信に失敗する可能性があります。)</span> |

[例示]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/brand-message/v1.0/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

<a id="response-24"></a>

#### 応答

```
{
  "header": {
    "resultCode": Integer,
    "resultMessage": String,
    "isSuccessful": boolean
  }
}
```

| 名前              | タイプ      | Not Null | 説明     |
|:----------------|:--------|:---------|:-------|
| header          | Object  | O        | ヘッダー領域  |
| - resultCode    | Integer | O        | 結果コード  |
| - resultMessage | String  | O        | 結果メッセージ |
| - isSuccessful  | boolean | O        | 成功可否  |