## Notification > KakaoTalk Bizmessage > 알림톡 > API v2.3 Guide

<a id="alimtalk"></a>

## お知らせトーク

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

1. お知らせトークのクイックリプライ、アイテムリストタイプ、トークビズプラグイン、代表リンク、ビジネスフォームボタン機能が追加されました。
2. お知らせトークのアイテムハイライト画像登録 API が追加されました。
3. お知らせトークのプラグイン登録/修正/削除/照会 API が追加されました。
4. メッセージリスト照会 API で buttons フィールドが削除されました。

<a id="general-messages"></a>

## 一般メッセージ

<a id="request-of-sending-replaced-messages"></a>

### メッセージ置換送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前                       | 	タイプ     | 	必須 | 	説明                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | コンソールで生成できます。                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 重複メッセージ送信リクエストの基準 key<br>10分間、同一の key でリクエストした場合、該当リクエストを失敗として処理します。 |

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

| 名前                     | 	タイプ      | 	必須 | 	説明                                                                                           |
|------------------------|----------|-----|-----------------------------------------------------------------------------------------------|
| senderKey              | 	String  | 	O  | 発信キー（40文字）                                                                                     |
| templateCode           | 	String  | 	O  | 登録済み送信テンプレートコード（最大 20 文字）                                                                         |
| requestDate            | String   | X   | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大 60 日後まで予約可能                            |
| senderGroupingKey      | String   | X   | 発信グルーピングキー（最大 100 文字）                                                                             |
| createUser             | String   | X   | 登録者（コンソールから送信する場合、ユーザーの UUID で保存）                                                                   |
| recipientList          | 	List    | 	O  | 	受信者リスト（最大 1000 名）                                                                            |
| - recipientNo          | 	String  | 	O  | 	受信番号（最大 15 文字）                                                                                 |
| - templateParameter    | 	Object  | 	X  | 	テンプレートパラメータ<br>（テンプレートに置換する変数を含む場合は必須）                                                           |
| -- key                 | 	String  | 	X  | 	置換キー（#{key}）                                                                                 |
| -- value               | String   | 	X  | 	置換キーにマッピングされる Value 値                                                                            |
| - resendParameter      | 	Object  | 	X  | 代替送信情報                                                                                      |
| -- isResend            | 	boolean | 	X  | 	送信失敗時、SMS 代替送信の有無<br>コンソールで代替送信を設定した場合、デフォルトで代替送信されます。                                      |
| -- resendType          | 	String  | 	X  | 	代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さに応じてタイプが決まります。                                      |
| -- resendTitle         | 	String  | 	X  | 	LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                              |
| -- resendContent       | 	String  | 	X  | 	代替送信内容<br>（値がない場合、[メッセージ本文とウェブリンクボタン名 - ウェブリンク Mobile リンク] で代替送信されます。）                        |
| -- resendSendNo        | String   | X   | 代替送信の発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信が失敗する可能性があります。）</span> |
| - buttons              | 	List    | 	X  | ボタン追加情報                                                                                      |
| -- ordering            | Integer  | X   | 	ボタン順序（ボタンがある場合は必須）                                                                          |
| -- chatExtra           | 	String  | 	X  | BC（相談トーク転換） / BT（ボット転換）タイプのボタンの場合、転送するメタ情報                                                       |
| -- chatEvent           | 	String  | 	X  | BT（ボット転換）タイプのボタンの場合、接続するボットイベント名                                                                  |
| -- relayId             | 	String  | 	X  | プラグイン実行時に X-Kakao-Plugin-Relay-Id ヘッダーを通じて受け取る値                                               |
| -- oneClickId          | 	String  | 	X  | ワンクリック決済プラグインで使用する決済情報                                                                      |
| -- productId           | 	String  | 	X  | ワンクリック決済プラグインで使用する決済情報                                                                      |
| -- target              | 	String  | 	X  | 	ウェブリンクボタンの場合、`"target":"out"` 属性を追加するとアウトリンク<br>デフォルトはインアプリリンクで送信                                    |
| -- telNumber           | 	String  | 	X  | TN（電話する）タイプのボタンの場合、転送する電話番号                                                                    |
| - quickReplies         | 	List    | 	X  | クイックリプライ情報                                                                                       |
| -- ordering            | Integer  | X   | 	クイックリプライ順序（クイックリプライがある場合は必須）                                                                      |
| -- chatExtra           | 	String  | 	X  | BC（相談トーク転換） / BT（ボット転換）タイプの場合、転送するメタ情報                                                          |
| -- chatEvent           | 	String  | 	X  | BT（ボット転換）タイプの場合、接続するボットイベント名                                                                     |
| -- target              | 	String  | 	X  | 	ウェブリンクタイプの場合、`"target":"out"` 属性を追加するとアウトリンク<br>デフォルトはインアプリリンクで送信                                    |
| - recipientGroupingKey | 	String  | 	X  | 	受信者グルーピングキー（最大 100 文字）                                                                           |
| messageOption          | Object   | 	X  | メッセージオプション                                                                                        |
| - price                | Integer  | 	X  | ユーザーに届くメッセージ内に含まれる価格/金額/決済金額（モーメント広告に該当）                                                   |
| - currencyType         | String   | 	X  | ユーザーに届くメッセージ内に含まれる価格/金額/決済金額の通貨単位。KRW、USD、EUR などの国際通貨コードを使用（モーメント広告に該当）                |
| statsId                | String   | 	X  | 統計 ID（発信検索条件には含まれません。最大 8 文字）                                                            |

* <b>リクエスト日時は、呼び出し時点から 60 日後まで設定できます。</b>
* <b>SMS サービスで代替送信されるため、SMS サービスの送信 API 仕様に従ってフィールドを入力する必要があります。（SMS サービスに登録された発信番号、各種フィールドの長さ制限など）</b>
* <b>代替送信は SMS、LMS で送信可能であり、国際代替送信は SMS のみサポートします。国際受信者番号の場合、resendType（代替送信タイプ）を SMS に変更する必要があります。</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信タイトルや内容は、切り捨てられて代替送信される場合があります。（[[SMS 注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 参考）</b>
* <b>templateTitle および templateItemHighlight.title フィールドの末尾に、置換子と templateParameter を使用して `\s` 文字を追加すると、取り消し線スタイルを適用できます。</b>
    * <b>ただし、テンプレート登録時にあらかじめ \s をフィールドに追加しておく場合は適用されません。</b>

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","templateParameter":{"{置換子フィールド}":"{置換データ}"}}]}'
```

<a id="response"></a>

#### レスポンス

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

| 名前                      | タイプ      | Not Null | 説明           |
|-------------------------|---------|:--------:|--------------|
| header                  | Object  |    O     | ヘッダー領域        |
| - resultCode            | Integer |    O     | 結果コード        |
| - resultMessage         | String  |    O     | 結果メッセージ       |
| - isSuccessful          | Boolean |    O     | 成否        |
| message                 | Object  |    X     | 本文領域        |
| - requestId             | String  |    X     | リクエスト ID       |
| - senderGroupingKey     | String  |    X     | 発信グルーピングキー     |
| - sendResults           | Object  |    O     | 送信リクエスト結果     |
| -- recipientSeq         | Integer |    O     | 受信者シーケンス番号   |
| -- recipientNo          | String  |    X     | 受信番号        |
| -- resultCode           | Integer |    O     | 送信リクエスト結果コード  |
| -- resultMessage        | String  |    O     | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  |    X     | 受信者グルーピングキー    |

<a id="request-of-sending-full-text"></a>

### メッセージ全文送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前                       | 	タイプ     | 	必須 | 	説明                                                        |
|--------------------------|---------|-----|------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | コンソールで生成できます。                                           |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 重複メッセージ送信リクエストの基準 key<br>10分間、同一の key でリクエストした場合、該当リクエストを失敗として処理します。 |

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

| 名前                      | 	タイプ      | 	必須 | 	説明                                                                                                                                                                        |
|-------------------------|----------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| senderKey               | 	String  | 	O  | 発信キー（40文字）                                                                                                                                                                  |
| templateCode            | 	String  | 	O  | 登録済み送信テンプレートコード（最大20文字）                                                                                                                                                      |
| requestDate             | String   | X   | リクエスト日時（yyyy-MM-dd HH:mm）<br>（入力しない場合は即時送信）<br>最大60日後まで予約可能                                                                                                         |
| senderGroupingKey       | String   | X   | 発信グループキー（最大100文字）                                                                                                                                                          |
| createUser              | String   | X   | 登録者（コンソールから送信する場合はユーザーUUIDで保存）                                                                                                                                                |
| recipientList           | 	List    | 	O  | 	受信者リスト（最大1,000名）                                                                                                                                                        |
| - recipientNo           | 	String  | 	O  | 	受信番号（最大15文字）                                                                                                                                                              |
| - content               | 	String  | 	O  | 	内容（最大1,300文字）                                                                                                                                                              |
| - templateTitle         | String   | X   | タイトル（最大50文字）                                                                                                                                                                 |
| - templateHeader        | String   | X   | テンプレートヘッダー（最大16文字）                                                                                                                                                             |
| - templateItem          | Object   | X   | アイテム                                                                                                                                                                        |
| -- list                 | List     | X   | アイテムリスト（最小2個、最大10個）                                                                                                                                                     |
| --- title               | String   | X   | タイトル（最大6文字）                                                                                                                                                                 |
| --- description         | String   | X   | 説明（最大23文字）                                                                                                                                                              |
| -- summary              | Object   | X   | アイテムサマリー情報                                                                                                                                                                  |
| --- title               | String   | X   | タイトル（最大6文字）                                                                                                                                                                 |
| --- description         | String   | X   | 説明（変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大14文字）                                                                                                                              |
| - templateItemHighlight | Object   | X   | アイテムハイライト                                                                                                                                                                  |
| --- title               | String   | X   | タイトル（最大30文字、サムネイル画像がある場合は21文字）                                                                                                                                            |
| --- description         | String   | X   | 説明（最大19文字、サムネイル画像がある場合は13文字）                                                                                                                                          |
| --- imageUrl            | String   | X   | サムネイル画像のURL                                                                                                                                                                 |
| - templateRepresentLink | Object   | X   | 代表リンク                                                                                                                                                                      |
| -- linkMo               | String   | 	X  | 	モバイルWebリンク（最大500文字）                                                                                                                                                         |
| -- linkPc               | String   | 	X  | PCウェブリンク（最大500文字）                                                                                                                                                           |
| -- schemeIos            | String   | X   | 	iOSアプリリンク（最大500文字）                                                                                                                                                         |
| -- schemeAndroid        | String   | X   | 	Androidアプリリンク（最大500文字）                                                                                                                                                       |
| - buttons               | 	List    | 	X  | ボタンリスト（最大5個）                                                                                                                                                              |
| -- ordering             | 	Integer | 	X  | 	ボタン順序（ボタンがある場合は必須）                                                                                                                                                       |
| -- type                 | String   | 	X  | 	ボタンタイプ（WL: Webリンク、AL: アプリリンク、DS: 配送照会、BK: Botキーワード、MD: メッセージ転送、BC: 相談トーク転換、BT: Bot転換、AC: チャンネル追加ボタン、BF: ビジネスフォーム、P1: 画像セキュリティ送信プラグインID、P2: 個人情報利用プラグインID、P3: ワンクリック決済プラグインID、TN: 電話する） |
| -- name                 | String   | 	X  | 	ボタン名（ボタンがある場合は必須、最大14文字）                                                                                                                                               |
| -- linkMo               | String   | 	X  | 	モバイルWebリンク（WLタイプの場合は必須フィールド、最大500文字）                                                                                                                                        |
| -- linkPc               | String   | 	X  | PCウェブリンク（WLタイプの場合は任意フィールド、最大500文字）                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOSアプリリンク（ALタイプの場合は必須フィールド、最大500文字）                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	Androidアプリリンク（ALタイプの場合は必須フィールド、最大500文字）                                                                                                                                      |
| -- chatExtra            | 	String  | 	X  | BC（相談トーク転換）／BT（Bot転換）タイプのボタン使用時に転送するメタ情報                                                                                                                                    |
| -- chatEvent            | 	String  | 	X  | BT（Bot転換）タイプのボタン使用時に接続するBotイベント名                                                                                                                                               |
| -- bizFormId            | 	Integer | 	X  | 	ビジネスフォームID（BFタイプの場合は必須）                                                                                                                                                    |
| -- pluginId             | 	String  | 	X  | 	プラグインID（最大24文字）                                                                                                                                                           |
| -- relayId              | 	String  | 	X  | プラグイン実行時にX-Kakao-Plugin-Relay-Idヘッダーを通じて受け取る値                                                                                                                            |
| -- oneClickId           | 	String  | 	X  | ワンクリック決済プラグインで使用する決済情報                                                                                                                                                   |
| -- productId            | 	String  | 	X  | ワンクリック決済プラグインで使用する決済情報                                                                                                                                                   |
| -- target               | 	String  | 	X  | 	Webリンクボタンの場合、`"target":"out"` 属性を追加するとアウトリンク<br>デフォルトはインアプリリンクで送信                                                                                                                 |
| -- telNumber           | 	String  | 	X  | TN（電話する）タイプのボタン使用時に転送する電話番号                                                                                    |
| - quickReplies          | 	List    | 	X  | クイックリプライリスト（最大5個）                                                                                                                                                            |
| -- ordering             | 	Integer | 	X  | 	クイックリプライの順序（クイックリプライがある場合は必須）                                                                                                                                                   |
| -- type                 | String   | 	X  | 	クイックリプライのタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク切り替え、BT: ボット切り替え、BF: ビジネスフォーム）                                                                                                   |
| -- name                 | String   | 	X  | 	クイックリプライの名前（クイックリプライがある場合は必須、最大 14 文字）                                                                                                                                           |
| -- linkMo               | String   | 	X  | 	モバイルウェブリンク（WL タイプの場合は必須、最大 500 文字）                                                                                                                                        |
| -- linkPc               | String   | 	X  | PC ウェブリンク（WL タイプの場合は任意、最大 500 文字）                                                                                                                                          |
| -- schemeIos            | String   | X   | 	iOS アプリリンク（AL タイプの場合は必須、最大 500 文字）                                                                                                                                        |
| -- schemeAndroid        | String   | X   | 	Android アプリリンク（AL タイプの場合は必須、最大 500 文字）                                                                                                                                      |
| -- bizFormId            | 	Integer | 	X  | 	ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                    |
| -- target               | 	String  | 	X  | 	ウェブリンクタイプの場合、`"target":"out"` 属性を追加するとアウトリンク<br>デフォルトはインアプリリンクで送信されます                                                                                                                 |
| - resendParameter       | 	Object  | 	X  | 代替送信情報                                                                                                                                                                   |
| -- isResend             | 	boolean | 	X  | 	送信失敗時、SMS 代替送信を行うかどうか<br>コンソールで代替送信設定を行った場合、デフォルトで代替送信されます。                                                                                                                   |
| -- resendType           | 	String  | 	X  | 	代替送信タイプ（SMS、LMS）<br>値がない場合、テンプレート本文の長さによってタイプが決まります。                                                                                                                   |
| -- resendTitle          | 	String  | 	X  | 	LMS 代替送信タイトル<br>（値がない場合、プラスフレンド ID で代替送信されます。）                                                                                                                           |
| -- resendContent        | 	String  | 	X  | 	代替送信内容<br>（値がない場合、[メッセージ本文とウェブリンクボタン名 - ウェブリンクモバイルリンク] で代替送信されます。）                                                                                                     |
| -- resendSendNo         | String   | X   | 代替送信の発信番号<br><span style="color:red">（SMS サービスに登録された発信番号でない場合、代替送信に失敗する可能性があります。）</span>                                                                              |
| - recipientGroupingKey  | 	String  | 	X  | 	受信者グルーピングキー（最大 100 文字）                                                                                                                                        |
| messageOption           | Object   | 	X  | メッセージオプション                                                                                                                                                                     |
| - price                 | Integer  | 	X  | ユーザーに送信されるメッセージに含まれる価格/金額/決済金額（モーメント広告に該当）                                                                                                                                |
| - currencyType          | String   | 	X  | ユーザーに送信されるメッセージに含まれる価格/金額/決済金額の通貨単位。KRW、USD、EUR などの国際通貨コードを使用（モーメント広告に該当）                                                                                             |
| statsId                 | String   | 	X  | 統計 ID（送信検索条件には含まれません。最大 8 文字）                                                                                                                                         |

* <b>本文とボタンに置換が完了したデータを入力してください。</b>
* <b>リクエスト日時は、呼び出し時点から60日後まで設定できます。</b>
* <b>SMSサービスで代替送信されるため、SMSサービスの送信API仕様に従ってフィールドを入力する必要があります。（SMSサービスに登録された発信番号、各種フィールドの長さ制限など）</b>
* <b>代替送信はSMS、LMSで送信可能で、国際代替送信はSMSのみサポートします。国際受信者番号の場合、resendType（代替送信タイプ）をSMSに変更する必要があります。</b>
* <b>指定した代替送信タイプのバイト制限を超える代替送信タイトルや内容は、切り捨てられて代替送信される場合があります。（[[SMS 注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)] 参照）</b>
* <b>送信時点でtemplateTitleとtemplateItemHighlight.titleフィールドの末尾に`\s`文字を追加した場合、取り消し線スタイルを適用できます。</b>
    * <b>ただし、テンプレート登録時にあらかじめ\sをフィールドに追加している場合は適用されません。</b>

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/raw-messages -d '{"senderKey":"{発信キー}","templateCode":"{テンプレートコード}","requestDate":"2018-10-01 00:00","recipientList":[{"recipientNo":"{受信番号}","content":"{内容}","buttons":[{"ordering":"{ボタン順序}","type":"{ボタンタイプ}","name":"{ボタン名}","linkMo":"{モバイルWebリンク}"}]}]}'
```

<a id="response-2"></a>

#### レスポンス

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

| 名前                      | タイプ      | Not Null | 説明           |
|-------------------------|---------|:--------:|--------------|
| header                  | Object  |    O     | ヘッダー領域        |
| - resultCode            | Integer |    O     | 結果コード        |
| - resultMessage         | String  |    O     | 結果メッセージ       |
| - isSuccessful          | Boolean |    O     | 成功かどうか        |
| message                 | Object  |    X     | 本文領域        |
| - requestId             | String  |    X     | リクエスト ID       |
| - senderGroupingKey     | String  |    X     | 発信グルーピングキー     |
| - sendResults           | Object  |    O     | 送信リクエスト結果     |
| -- recipientSeq         | Integer |    O     | 受信者シーケンス番号   |
| -- recipientNo          | String  |    X     | 受信番号        |
| -- resultCode           | Integer |    O     | 送信リクエスト結果コード  |
| -- resultMessage        | String  |    O     | 送信リクエスト結果メッセージ |
| -- recipientGroupingKey | String  |    X     | 受信者グルーピングキー    |

<a id="list-messages"></a>

### メッセージリスト照会

<a id="request"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Query parameter] 1番 または(2番、3番)の条件が必須

| 名前                   | 	タイプ      | 	必須        | 	説明                                                   |
|----------------------|----------|------------|-------------------------------------------------------|
| requestId            | 	String  | 	条件必須(1番) | リクエストID                                                |
| startRequestDate     | 	String  | 	条件必須(2番) | 送信リクエスト日時の開始値(yyyy-MM-dd HH:mm)                       |
| endRequestDate       | 	String  | 条件必須(2番)  | 	送信リクエスト日時の終了値(yyyy-MM-dd HH:mm)                       |
| startCreateDate      | String   | 条件必須(3番)  | 登録日時の開始値(yyyy-MM-dd HH:mm)                           |
| endCreateDate        | String   | 条件必須(3番)  | 登録日時の終了値(yyyy-MM-dd HH:mm)                            |
| recipientNo          | 	String  | 	X         | 	受信番号                                                 |
| senderKey            | 	String  | 	X         | 	発信キー                                                 |
| templateCode         | 	String  | 	X         | 	テンプレートコード                                               |
| senderGroupingKey    | String   | X          | 発信グルーピングキー                                              |
| recipientGroupingKey | 	String  | 	X         | 	受信者グルーピングキー                                            |
| messageStatus        | String   | 	X         | リクエスト状態( COMPLETED -> 成功、FAILED -> 失敗、CANCEL -> 取消 )	 |
| resultCode           | String   | 	X         | 送信結果( MRC01 -> 成功 MRC02 -> 失敗 )	                     |
| createUser           | String   | X          | 登録者(コンソールから送信時にユーザー UUID で保存)                           |
| pageNum              | 	Integer | 	X         | 	ページ番号(Default: 1)                                   |
| pageSize             | 	Integer | 	X         | 	照会件数(Default: 15、Max: 1000)                        |

* 90日以前の送信リクエストデータは照会できません。
* 送信リクエスト日時の範囲は最大 30 日です。

<a id="response-3"></a>

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

| 名前                          | タイプ      | Not Null | 説明                                                                                                                                                                     |
|-----------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                      | Object  |    O     | ヘッダ領域                                                                                                                                                                  |
| - resultCode                | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage             | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful              | Boolean |    O     | 成否                                                                                                                                                                  |
| messageSearchResultResponse | Object  |    X     | 本文領域                                                                                                                                                                  |
| - messages                  | List    |    O     | メッセージリスト                                                                                                                                                                |
| -- requestId                | String  |    O     | リクエスト ID                                                                                                                                                                 |
| -- recipientSeq             | Integer |    O     | 受信者シーケンス番号                                                                                                                                                             |
| -- plusFriendId             | String  |    O     | プラスフレンド ID                                                                                                                                                               |
| -- senderKey                | String  |    O     | 発信キー                                                                                                                                                                   |
| -- templateCode             | String  |    O     | テンプレートコード                                                                                                                                                                 |
| -- recipientNo              | String  |    O     | 受信番号                                                                                                                                                                  |
| -- content                  | String  |    X     | 本文                                                                                                                                                                     |
| -- requestDate              | String  |    O     | リクエスト日時                                                                                                                                                                  |
| -- createDate               | String  |    O     | 登録日時                                                                                                                                                                  |
| -- receiveDate              | String  |    X     | 受信日時                                                                                                                                                                  |
| -- resendStatus             | String  |    O     | 代替送信状態コード(RSC01、RSC02、RSC03、RSC04、RSC05)\<br\>([[下記の代替送信状態表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 参照) |
| -- resendStatusName         | String  |    O     | 代替送信状態コード名                                                                                                                                                           |
| -- messageStatus            | String  |    O     | リクエスト状態( COMPLETED \-> 成功、FAILED \-> 失敗、CANCEL \-> 取消 )                                                                                                                |
| -- createUser               | String  |    X     | 登録者(コンソールから送信時にユーザー UUID で保存)                                                                                                                                            |
| -- resultCode               | String  |    X     | 受信結果コード                                                                                                                                                               |
| -- resultCodeName           | String  |    X     | 受信結果コード名                                                                                                                                                              |
| -- senderGroupingKey        | String  |    X     | 発信グルーピングキー                                                                                                                                                               |
| -- recipientGroupingKey     | String  |    X     | 受信者グルーピングキー                                                                                                                                                              |
| - totalCount                | Integer |    X     | 総件数                                                                                                                                                                    |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages?startRequestDate=2018-05-01%20:00&endRequestDate=2018-05-30%20:59"
```

<a id="get-messages"></a>

### メッセージ単件照会

<a id="request-2"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ      | 	説明         |
|--------------|----------|-------------|
| appkey       | 	String  | 	固有のアプリキー     |
| requestId    | 	String  | 	リクエスト ID     |
| recipientSeq | 	Integer | 	受信者シーケンス番号 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}/{recipientSeq}"
```

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

| 名前                      | タイプ      | Not Null | 説明                                                                                                                                                                     |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | ヘッダー領域                                                                                                                                                                  |
| - resultCode            | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage         | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful          | Boolean |    O     | 成否                                                                                                                                                                  |
| message                 | Object  |    X     | メッセージ                                                                                                                                                                    |
| - requestId             | String  |    O     | リクエスト ID                                                                                                                                                                 |
| - recipientSeq          | Integer |    O     | 受信者シーケンス番号                                                                                                                                                             |
| - plusFriendId          | String  |    O     | プラスフレンド ID                                                                                                                                                               |
| - senderKey             | String  |    O     | 発信キー                                                                                                                                                                   |
| - templateCode          | String  |    O     | テンプレートコード                                                                                                                                                                 |
| - recipientNo           | String  |    X     | 受信番号                                                                                                                                                                  |
| - content               | String  |    X     | 本文                                                                                                                                                                     |
| - templateTitle         | String  |    X     | テンプレートタイトル                                                                                                                                                                 |
| - templateSubtitle      | String  |    X     | テンプレート補助文言                                                                                                                                                              |
| - templateExtra         | String  |    X     | テンプレート付加内容                                                                                                                                                              |
| - templateAd            | String  |    X     | テンプレート内の受信同意リクエストまたは簡単な広告文言                                                                                                                                            |
| - templateHeader        | String  |    X     | テンプレートヘッダー（最大 16 文字）                                                                                                                                                         |
| - templateItem          | Object  |    X     | アイテム                                                                                                                                                                    |
| -- list                 | List    |    X     | アイテムリスト（最小 2 個、最大 10 個）                                                                                                                                                 |
| --- title               | String  |    X     | タイトル（最大 6 文字）                                                                                                                                                             |
| --- description         | String  |    X     | 説明（最大 23 文字）                                                                                                                                                          |
| -- summary              | Object  |    X     | アイテム要約情報                                                                                                                                                              |
| --- title               | String  |    X     | タイトル（最大 6 文字）                                                                                                                                                             |
| --- description         | String  |    X     | 説明（変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字）                                                                                                                          |
| - templateItemHighlight | Object  |    X     | アイテムハイライト                                                                                                                                                              |
| --- title               | String  |    X     | タイトル（最大 30 文字、サムネイル画像がある場合は 21 文字）                                                                                                                                        |
| --- description         | String  |    X     | 説明（最大 19 文字、サムネイル画像がある場合は 13 文字）                                                                                                                                      |
| --- imageUrl            | String  |    X     | サムネイル画像 URL                                                                                                                                                             |
| - templateRepresentLink | Object  |    X     | 代表リンク                                                                                                                                                                  |
| -- linkMo               | String  |    X     | モバイル Web リンク（最大 500 文字）                                                                                                                                                      |
| -- linkPc               | String  |    X     | PC Web リンク（最大 500 文字）                                                                                                                                                       |
| -- schemeIos            | String  |    X     | iOS アプリリンク（最大 500 文字）                                                                                                                                                      |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（最大 500 文字）                                                                                                                                                    |
| - requestDate           | String  |    O     | リクエスト日時                                                                                                                                                                  |
| - receiveDate           | String  |    X     | 受信日時                                                                                                                                                                  |
| - createDate            | String  |    O     | 登録日時                                                                                                                                                                  |
| - resendStatus          | String  |    O     | 代替送信ステータスコード（RSC01、RSC02、RSC03、RSC04、RSC05）\<br\>（[[下記の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 参照） |
| - resendStatusName      | String  |    O     | 代替送信ステータスコード名                                                                                                                                                           |
| - resendResultCode      | String  |    X     | 代替送信結果コード [SMS 結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId       | String  |    X     | 代替送信 SMS リクエスト ID                                                                                                                                                        |
| - messageStatus         | String  |    O     | リクエストステータス（COMPLETED -\> 成功、FAILED -\> 失敗、CANCEL -\> 取消）                                                                                                                |
| - resultCode            | String  |    X     | 受信結果コード                                                                                                                                                               |
| - resultCodeName        | String  |    X     | 受信結果コード名                                                                                                                                                              |
| - createUser            | String  |    X     | 登録者（コンソールから送信する場合はユーザー UUID で保存）                                                                                                                                            |
| - buttons               | List    |    X     | ボタンリスト                                                                                                                                                                 |
| -- ordering             | Integer |    X     | ボタン順序                                                                                                                                                                  |
| -- type                 | String  |    X     | ボタンタイプ（WL: Webリンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転達、BC: 相談トーク転換、BT: ボット転換、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ転送プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話する） |
| -- name                 | String  |    X     | ボタン名                                                                                                                                                                  |
| -- linkMo               | String  |    X     | モバイル Web リンク（WL タイプの場合は必須フィールド）                                                                                                                                              |
| -- linkPc               | String  |    X     | PC Web リンク（WL タイプの場合は選択フィールド）                                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                              |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                            |
| -- chatExtra            | String  |    X     | BC（相談トーク転換）/ BT（ボット転換）タイプのボタン時、転達するメタ情報                                                                                                                                |
| -- chatEvent            | String  |    X     | BT（ボット転換）タイプのボタン時、接続するボットイベント名                                                                                                                                           |
| -- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                 |
| -- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                        |
| -- relayId              | String  |    X     | プラグイン実行時に X-Kakao-Plugin-Relay-Id ヘッダーを通じて転達される値                                                                                                                        |
| -- oneClickId           | String  |    X     | ワンクリック決済プラグインで使用する決済情報                                                                                                                                               |
| -- productId            | String  |    X     | ワンクリック決済プラグインで使用する決済情報                                                                                                                                               |
| -- target               | String  |    X     | Web リンクボタンの場合、"target":"out" 属性を追加するとアウトリンク\<br\>デフォルトはインアプリリンクで送信                                                                                                            |
| - quickReplies          | List    |    X     | クイックリプライリスト（最大 5 件）                                                                                                                                                        |
| -- ordering             | Integer |    X     | クイックリプライ順序（クイックリプライがある場合は必須）                                                                                                                                                |
| -- type                 | String  |    X     | クイックリプライタイプ（WL: Web リンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム）                                                                                                |
| -- name                 | String  |    X     | クイックリプライ名（クイックリプライがある場合は必須、最大 14 文字）                                                                                                                                        |
| -- linkMo               | String  |    X     | モバイル Web リンク（WL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| -- linkPc               | String  |    X     | PC Web リンク（WL タイプの場合は選択フィールド、最大 500 文字）                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                   |
| -- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                        |
| -- target               | String  |    X     | Web リンクタイプの場合、"target":"out" 属性を追加するとアウトリンク\<br\>デフォルトはインアプリリンクで送信                                                                                                            |
| -- telNumber           | 	String  | 	X  | TN（電話する）タイプのボタン時、転達する電話番号                                                                    |
| - messageOption         | Object  |    X     | メッセージオプション                                                                                                                                                                 |
| -- price                | Integer |    X     | ユーザーに転達されるメッセージ内に含まれる価格/金額/決済金額（モーメント広告に該当）                                                                                                                            |
| -- currencyType         | String  |    X     | ユーザーに転達されるメッセージ内に含まれる価格/金額/決済金額の通貨単位。KRW、USD、EUR などの国際通貨コードを使用（モーメント広告に該当）                                                                                         |
| - senderGroupingKey     | String  |    X     | 発信グルーピングキー                                                                                                                                                               |
| - recipientGroupingKey  | String  |    X     | 受信者グルーピングキー                                                                                                                                                              |

<a id="authentication-messages"></a>

## 認証メッセージ

<span id="precautions-authword"></span>

1. 認証メッセージ送信時に含める必要がある認証文言のご案内

| 区分       | 認証文言                                          |
|----------|-----------------------------------------------|
| 認証メッセージ | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- 例 1-1) 認証メッセージ API リクエスト時に本文（テンプレート置換変数を含む）に認証文言が含まれていない場合、送信失敗となります。
- 例 1-2) 認証文言が英文の場合、大文字・小文字を区別せずバリデーションが実施されます。

<a id="request-of-sending-replaced-messages-2"></a>

### メッセージ置換送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ   | 	説明        |
|--------|---------|------------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前                       | 	タイプ   | 	必須 | 	説明                                                               |
|--------------------------|---------|-----|-------------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | コンソールで生成できます。                                                     |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 重複メッセージ送信リクエストの基準 key<br>10 分間、同一の key でリクエストした場合、該当リクエストを失敗として処理します。 |

[Request body]
[上記と同様](./alimtalk-api-guide/#_3)

<a id="request-of-sending-full-text-2"></a>

### メッセージ全文送信リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/auth/raw-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ   | 	説明        |
|--------|---------|------------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前                       | 	タイプ   | 	必須 | 	説明                                                               |
|--------------------------|---------|-----|-------------------------------------------------------------------|
| X-Secret-Key             | 	String | O   | コンソールで生成できます。                                                     |
| X-NC-API-IDEMPOTENCY-KEY | 	String | X   | 重複メッセージ送信リクエストの基準 key<br>10 分間、同一の key でリクエストした場合、該当リクエストを失敗として処理します。 |

[Request Body]
[上記と同様](./alimtalk-api-guide/#_5)

<a id="list-messages-2"></a>

### メッセージリスト照会

<a id="request-3"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ   | 	説明        |
|--------|---------|------------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ   | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで生成できます。 |

[Query parameter]
[上記と同様](./alimtalk-api-guide/#_7)

<a id="get-messages-2"></a>

### メッセージ単件照会

<a id="request-4"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| appkey       | 	String  | 	固有のアプリキー   |
| requestId    | 	String  | 	リクエスト ID   |
| recipientSeq | 	Integer | 	受信者シーケンス番号 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ   | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで生成できます。 |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/auth/messages/{requestId}/{recipientSeq}"
```

<a id="response-5"></a>

#### レスポンス

[上記と同様](./alimtalk-api-guide/#_9)

<a id="message"></a>

## メッセージ

<a id="cancel-sending-messages"></a>

### メッセージ送信取消

<a id="request-5"></a>

#### 요청

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	固有のアプリキー |
| requestId | String  | リクエスト ID   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Query parameter]

| 이름           | 	타입     | 	필수 | 	설명                                         |
|--------------|---------|-----|---------------------------------------------|
| recipientSeq | 	String | 	X  | 受信者シーケンス番号<br>(入力しない場合、リクエスト ID のすべての送信件を取消) |

* 一般/認証メッセージのどちらも同一の API で取消できます。

<a id="response-6"></a>

#### 응답

```
{
  "header": {
      "resultCode": Integer,
      "resultMessage": String,
      "isSuccessful": boolean
  }
}
```

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

[예시]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/messages/{requestId}?recipientSeq=1,2,3"
```

<a id="query-updates-of-message-result"></a>

### メッセージ結果アップデート照会

<a id="request-6"></a>

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Query parameter]

| 이름                  | 	타입      | 	필수 | 	설명                                 |
|---------------------|----------|-----|-------------------------------------|
| startUpdateDate     | 	String  | 	O  | 結果アップデート照会開始時間(yyyy-MM-dd HH:mm)  |
| endUpdateDate       | 	String  | O   | 	結果アップデート照会終了時間(yyyy-MM-dd HH:mm) |
| alimtalkMessageType | 	String  | X   | 	お知らせトークメッセージタイプ(NORMAL, AUTH)           |
| pageNum             | 	Integer | 	X  | 	ページ番号(デフォルト: 1)                      |
| pageSize            | 	Integer | 	X  | 	照会件数(Default: 15, Max: 1000)      |

<a id="response-7"></a>

#### 응답

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

| 이름                 | 타입      | Not Null | 설명                                                                                                                                                                     |
|--------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header             | Object  |    O     | ヘッダー領域                                                                                                                                                                  |
| - resultCode       | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage    | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful     | Boolean |    O     | 成否                                                                                                                                                                  |
| messages           | List    |    X     | メッセージリスト                                                                                                                                                                |
| - requestId        | String  |    O     | リクエスト ID                                                                                                                                                                  |
| - recipientSeq     | Integer |    O     | 受信者シーケンス番号                                                                                                                                                             |
| - requestDate      | String  |    O     | リクエスト日時                                                                                                                                                                  |
| - createDate       | String  |    O     | 作成日時                                                                                                                                                                  |
| - receiveDate      | String  |    X     | 受信日時                                                                                                                                                                  |
| - resendStatus     | String  |    O     | 代替送信ステータスコード(RSC01, RSC02, RSC03, RSC04, RSC05)\<br\>([[下記の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 参照) |
| - resendStatusName | String  |    O     | 代替送信ステータスコード名                                                                                                                                                           |
| - resendResultCode | String  |    X     | 代替送信結果コード [SMS 結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId  | String  |    X     | 代替送信 SMS リクエスト ID                                                                                                                                                        |
| - messageStatus    | String  |    O     | リクエストステータス(COMPLETED -\> 成功、FAILED -\> 失敗、CANCEL -\> 取消)                                                                                                                 |
| - resultCode       | String  |    X     | 受信結果コード                                                                                                                                                               |
| - resultCodeName   | String  |    X     | 受信結果コード名                                                                                                                                                              |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

<a id="query-the-number-of-message-result-updates"></a>

### メッセージ結果アップデート件数照会

<a id="request-7"></a>

#### 요청

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/message-results/count
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름     | 	타입     | 	설명     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Query parameter]

| 이름                  | 	타입      | 	필수 | 	설명                                 |
|---------------------|----------|-----|-------------------------------------|
| startUpdateDate     | 	String  | 	O  | 結果アップデート照会開始時間(yyyy-MM-dd HH:mm)  |
| endUpdateDate       | 	String  | O   | 	結果アップデート照会終了時間(yyyy-MM-dd HH:mm) |
| alimtalkMessageType | 	String  | X   | 	お知らせトークメッセージタイプ(NORMAL, AUTH)           |

<a id="response-8"></a>

#### 응답

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

| 이름              | 타입      | Not Null | 설명     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |
| totalCount      | Integer |    O     | 総件数   |

[예시]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/message-results/count?startUpdateDate=2018-05-01%20:00&endUpdateDate=2018-05-30%20:59"
```

<a id="status-code-of-smslms-resending"></a>

### SMS/LMS 代替送信ステータスコード

| 이름    | 	설명                                  |
|-------|--------------------------------------|
| RSC01 | 	代替送信対象外                           |
| RSC02 | 	代替送信対象(送信結果が失敗の場合、代替送信が進行されます。) |
| RSC03 | 	代替送信中                             |
| RSC04 | 	代替送信成功                            |
| RSC05 | 	代替送信失敗                            |

<a id="mass-delivery"></a>

## 一括送信

<a id="list-mass-delivery-requests"></a>

### 一括送信リクエスト一覧照会

<a id="request-8"></a>

#### リクエスト

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appKey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	説明       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestId または startRequestDate + endRequestDate または startCreateDate + endCreateDate は必須です。

| 名前               | 	タイプ               | 最大長 | 	必須 | 	説明       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | リクエスト ID     |
| startRequestDate | String            | -     | O   | 送信日付開始  |
| endRequestDate   | String            | -     | O   | 送信日付終了  |
| startCreateDate  | 	String           | -     | 	O  | 	登録日付開始 |
| endCreateDate    | 	String           | -     | 	O  | 	登録日付終了 |
| pageNum          | optional, Integer | -     | X   | ページ番号    |
| pageSize         | optional, Integer | 1000  | X   | 検索数      |

<a id="curl"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-9"></a>

#### レスポンス

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

| 名前                  | タイプ      | Not Null | 説明                                                                             |
|---------------------|---------|:--------:|--------------------------------------------------------------------------------|
| header              | Object  |    O     | ヘッダー領域                                                                          |
| - resultCode        | Integer |    O     | 結果コード                                                                          |
| - resultMessage     | String  |    O     | 結果メッセージ                                                                         |
| - isSuccessful      | Boolean |    O     | 成否                                                                          |
| body                | Object  |    X     | 本文領域                                                                          |
| - messages          | Object  |    X     | メッセージリスト                                                                        |
| -- requestId        | String  |    O     | リクエスト ID                                                                          |
| -- requestDate      | String  |    O     | リクエスト日付                                                                          |
| -- plusFriendId     | String  |    O     | プラスフレンド ID                                                                      |
| -- senderKey        | String  |    O     | 発信キー（40文字）                                                                      |
| -- masterStatusCode | String  |    O     | 一括送信ステータスコード（WAIT、READY、SENDREADY、SENDWAIT、SENDING、COMPLETE、CANCEL、FAIL） |
| -- content          | String  |    X     | 内容                                                                             |
| -- fileId           | String  |    X     | 添付ファイル ID                                                                       |
| -- templateCode     | String  |    O     | テンプレートコード（最大 20 文字）                                                                 |
| -- autoSendYn       | String  |    X     | 自動送信有無                                                                       |
| -- statsId          | String  |    X     | 統計 ID                                                                          |
| -- createDate       | String  |    O     | 作成日付                                                                          |
| -- createUser       | String  |    X     | 作成ユーザー（コンソールから送信時にユーザー UUID で保存）                                                 |
| - totalCount        | Integer |    X     | 総件数                                                                            |

<a id="list-mass-delivery-recipients"></a>

### 一括送信受信者リスト照会

<a id="request-9"></a>

#### リクエスト

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ     | 	説明     |
|-----------|---------|---------|
| appKey    | 	String | 	固有のアプリキー |
| requestId | 	String | 	リクエスト ID  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	説明       |
|--------------|---------|-----------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

| 名前               | 	タイプ               | 最大長 | 	必須 | 	説明       |
|------------------|-------------------|-------|-----|-----------|
| requestId        | String            | -     | O   | リクエスト ID     |
| startRequestDate | String            | -     | X   | 送信日付の開始  |
| endRequestDate   | String            | -     | X   | 送信日付の終了  |
| startCreateDate  | 	String           | -     | 	X  | 	登録日付の開始 |
| endCreateDate    | 	String           | -     | 	X  | 	登録日付の終了 |
| pageNum          | optional, Integer | -     | X   | ページ番号    |
| pageSize         | optional, Integer | 1000  | X   | 検索件数      |

<a id="curl-2"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/recipients?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-10"></a>

#### レスポンス

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

| 名前                | タイプ      | Not Null | 説明         |
|-------------------|---------|:--------:|------------|
| header            | Object  |    O     | ヘッダー領域      |
| - resultCode      | Integer |    O     | 結果コード      |
| - resultMessage   | String  |    O     | 結果メッセージ     |
| - isSuccessful    | Boolean |    O     | 成否      |
| body              | Object  |    X     | 本文領域      |
| - messages        | Object  |    X     | メッセージリスト    |
| -- requestId      | String  |    O     | リクエスト ID      |
| -- recipientSeq   | String  |    O     | 受信者シーケンス番号 |
| -- recipientNo    | String  |    X     | 受信番号      |
| -- requestDate    | String  |    O     | リクエスト日付      |
| -- receiveDate    | String  |    X     | 受信日付      |
| -- messageStatus  | String  |    O     | メッセージステータス     |
| -- resultCode     | String  |    X     | 結果コード      |
| -- resultCodeName | String  |    X     | 結果コードの内容   |
| - totalCount      | Integer |    X     | 総件数        |

<a id="get-a-mass-delivery-recipient"></a>

### 一括送信受信者照会

<a id="request-10"></a>

#### リクエスト

[URL]

```
GET /alimtalk/v2.3/appkeys/{appKey}/mass-messages/{requestId}/recipients/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明        |
|--------------|---------|------------|
| appKey       | 	String | 固有のアプリキー   |
| requestId    | 	String | リクエスト ID   |
| recipientSeq | String  | 受信者の順序     |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

| 名前               | 	タイプ               | 最大長 | 	必須 | 	説明        |
|------------------|-------------------|-----|-----|------------|
| requestId        | String            | -   | O   | リクエスト ID   |
| startRequestDate | String            | -   | X   | 送信日付の開始    |
| endRequestDate   | String            | -   | X   | 送信日付の終了    |
| startCreateDate  | 	String           | -   | 	X  | 	登録日付の開始   |
| endCreateDate    | 	String           | -   | 	X  | 	登録日付の終了   |
| pageNum          | optional, Integer | -   | X   | ページ番号      |
| pageSize         | optional, Integer | 1000 | X  | 検索件数       |

<a id="curl-3"></a>

#### cURL

```
curl -X GET \
'https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appKey}/'"${APP_KEY}"'/mass-messages/{requestId}/recipients/1" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}'
```

<a id="response-11"></a>

#### レスポンス

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

| 名前                      | タイプ      | Not Null | 説明                                                                                                                                                                     |
|-------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | ヘッダー領域                                                                                                                                                                  |
| - resultCode            | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage         | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful          | Boolean |    O     | 成否                                                                                                                                                                  |
| body                    | Object  |    X     | 本文領域                                                                                                                                                                  |
| - requestId             | String  |    O     | リクエスト ID                                                                                                                                                                  |
| - recipientSeq          | String  |    O     | 受信者シーケンス番号                                                                                                                                                             |
| - plusFriendId          | String  |    O     | プラスフレンド ID                                                                                                                                                              |
| - senderKey             | String  |    O     | 送信者 ID                                                                                                                                                                 |
| - templateCode          | String  |    O     | テンプレートコード（最大 20 文字）                                                                                                                                                         |
| - recipientNo           | String  |    X     | 受信番号                                                                                                                                                                  |
| - content               | String  |    X     | 内容                                                                                                                                                                     |
| - tempalteTitle         | String  |    X     | テンプレートタイトル（最大 50 文字、Android: 2 行、23 文字以上は省略表示、iOS: 2 行、27 文字以上は省略表示）                                                                                                     |
| - templateSubtitle      | String  |    X     | テンプレート補助文言（最大 50 文字、Android: 18 文字以上は省略表示、iOS: 21 文字以上は省略表示）                                                                                                          |
| - templateExtra         | String  |    X     | テンプレート付加情報（テンプレートメッセージタイプが [付加情報型/複合型] の場合は必須）                                                                                                                             |
| - templateAd            | String  |    X     | テンプレート内の受信同意リクエストまたは簡単な広告文言                                                                                                                                            |
| - templateHeader        | String  |    X     | テンプレートヘッダー（最大 16 文字）                                                                                                                                                         |
| - templateItem          | Object  |    X     | アイテム                                                                                                                                                                    |
| -- list                 | List    |    X     | アイテムリスト（最小 2 個、最大 10 個）                                                                                                                                                 |
| --- title               | String  |    X     | タイトル（最大 6 文字）                                                                                                                                                             |
| --- description         | String  |    X     | 説明（最大 23 文字）                                                                                                                                                          |
| -- summary              | Object  |    X     | アイテムサマリー情報                                                                                                                                                              |
| --- title               | String  |    X     | タイトル（最大 6 文字）                                                                                                                                                             |
| --- description         | String  |    X     | 説明（変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字）                                                                                                                          |
| - templateItemHighlight | Object  |    X     | アイテムハイライト                                                                                                                                                              |
| --- title               | String  |    X     | タイトル（最大 30 文字、サムネイル画像がある場合は 21 文字）                                                                                                                                        |
| --- description         | String  |    X     | 説明（最大 19 文字、サムネイル画像がある場合は 13 文字）                                                                                                                                      |
| --- imageUrl            | String  |    X     | サムネイル画像のアドレス                                                                                                                                                             |
| - templateRepresentLink | Object  |    X     | 代表リンク                                                                                                                                                                  |
| -- linkMo               | String  |    X     | モバイル Web リンク（最大 500 文字）                                                                                                                                                      |
| -- linkPc               | String  |    X     | PC Web リンク（最大 500 文字）                                                                                                                                                       |
| -- schemeIos            | String  |    X     | iOS アプリリンク（最大 500 文字）                                                                                                                                                      |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（最大 500 文字）                                                                                                                                                    |
| - requestDate           | String  |    O     | リクエスト日時                                                                                                                                                                  |
| - receiveDate           | String  |    X     | 受信日時                                                                                                                                                                  |
| - createDate            | String  |    O     | 作成日時                                                                                                                                                                  |
| - resendStatus          | String  |    O     | 代替送信ステータスコード（RSC01、RSC02、RSC03、RSC04、RSC05）\<br\>（[[下記の代替送信ステータス表](http://docs.toast.com/ko/Notification/KakaoTalk%20Bizmessage/ko/alimtalk-api-guide/#smslms)] 参照） |
| - resendStatusName      | String  |    O     | 代替送信ステータス名                                                                                                                                                              |
| - resendResultCode      | String  |    O     | 代替送信結果コード [SMS 結果コード](https://docs.toast.com/ko/Notification/SMS/ko/error-code/#api)                                                                                 |
| - resendRequestId       | String  |    X     | 代替送信リクエスト ID                                                                                                                                                            |
| - messageStatus         | String  |    O     | 一括受信者送信ステータスコード（READY、COMPLETED、FAILED、CANCEL）                                                                                                                      |
| - resultCode            | String  |    X     | 結果ステータスコード                                                                                                                                                               |
| - resultCodeName        | String  |    X     | 結果ステータス名                                                                                                                                                                 |
| - createUser            | String  |    X     | 作成ユーザー（コンソールから送信した場合、ユーザーの UUID として保存）                                                                                                                                                         |
| - buttons               | List    |    X     | ボタンリスト                                                                                                                                                                 |
| -- ordering             | Integer |    X     | ボタンの順序                                                                                                                                                                  |
| -- type                 | String  |    X     | ボタンタイプ（WL: ウェブリンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転達、BC: 相談トーク転換、BT: ボット転換、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ送信プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話する） |
| -- name                 | String  |    X     | ボタン名                                                                                                                                                                  |
| -- linkMo               | String  |    X     | モバイルウェブリンク（WL タイプの場合、必須フィールド）                                                                                                                                              |
| -- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合、任意フィールド）                                                                                                                                               |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合、必須フィールド）                                                                                                                                              |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合、必須フィールド）                                                                                                                                            |
| -- chatExtra            | String  |    X     | BC（相談トーク転換）/ BT（ボット転換）タイプのボタン時、転達するメタ情報                                                                                                                                |
| -- chatEvent            | String  |    X     | BT（ボット転換）タイプのボタン時、接続するボットイベント名                                                                                                                                           |
| -- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合、必須）                                                                                                                                                 |
| -- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                        |
| -- relayId              | String  |    X     | プラグイン実行時に X-Kakao-Plugin-Relay-Id ヘッダーを通じて受け取る値                                                                                                                        |
| -- oneClickId           | String  |    X     | ワンクリック決済プラグインで使用する決済情報                                                                                                                                               |
| -- productId            | String  |    X     | ワンクリック決済プラグインで使用する決済情報                                                                                                                                               |
| -- target               | String  |    X     | ウェブリンクボタンの場合、"target":"out" 属性を追加するとアウトリンク\<br\>デフォルトはインアプリリンクで送信                                                                                                            |
| -- telNumber           | 	String  | 	X  | TN（電話する）タイプのボタン時、転達する電話番号                                                                                                                                                    |
| - quickReplies          | List    |    X     | クイックリプライリスト（最大 5 件）                                                                                                                                                        |
| -- ordering             | Integer |    X     | クイックリプライの順序（クイックリプライがある場合、必須）                                                                                                                                                |
| -- type                 | String  |    X     | クイックリプライタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム）                                                                                                |
| -- name                 | String  |    X     | クイックリプライ名（クイックリプライがある場合、必須、最大 14 文字）                                                                                                                                        |
| -- linkMo               | String  |    X     | モバイルウェブリンク（WL タイプの場合、必須フィールド、最大 500 文字）                                                                                                                                     |
| -- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合、任意フィールド、最大 500 文字）                                                                                                                                      |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合、必須フィールド、最大 500 文字）                                                                                                                                     |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合、必須フィールド、最大 500 文字）                                                                                                                                   |
| -- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                        |
| -- target               | String  |    X     | ウェブリンクタイプの場合、"target":"out" 属性を追加するとアウトリンク\<br\>デフォルトはインアプリリンクで送信                                                                                                            |

<a id="templates"></a>

## テンプレート

<a id="list-template-categories"></a>

### テンプレートカテゴリー照会

<a id="request-11"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/template/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

<a id="response-12"></a>

#### レスポンス

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

| 名前              | タイプ      | Not Null | 説明                       |
|-----------------|---------|:--------:|--------------------------|
| header          | Object  |    O     | ヘッダー領域                    |
| - resultCode    | Integer |    O     | 結果コード                    |
| - resultMessage | String  |    O     | 結果メッセージ                   |
| - isSuccessful  | Boolean |    O     | 成否                    |
| categories      | List    |    X     | カテゴリーリスト                 |
| - name          | String  |    X     | カテゴリー名                  |
| - subCategories | List    |    X     | サブカテゴリーリスト              |
| -- code         | String  |    X     | カテゴリーコード（テンプレート登録/修正時に使用） |
| -- name         | String  |    X     | カテゴリー名                  |
| -- groupName    | String  |    X     | カテゴリーグループ名                 |
| -- inclusion    | String  |    X     | カテゴリー適用対象テンプレートの説明        |
| -- exclusion    | String  |    X     | カテゴリー除外対象テンプレートの説明        |

<a id="register-templates"></a>

### テンプレート登録

<a id="request-12"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 이름        | 	타입     | 	설명     |
|-----------|---------|---------|
| appkey    | 	String | 	固有のアプリキー |
| senderKey | 	String | 	発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 	타입     | 	필수 | 	설명              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

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

| 名前                    | 	タイプ      | 	必須 | 	説明                                                                                                                                                                                                                                             |
|-----------------------|----------|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateCode          | 	String  | 	O  | テンプレートコード（最大 20 文字）                                                                                                                                                                                                                                  |
| templateName          | 	String  | 	O  | テンプレート名（最大 150 文字）                                                                                                                                                                                                                                   |
| templateContent       | 	String  | 	O  | テンプレート本文（最大 1300 文字）                                                                                                                                                                                                                                |
| templateMessageType   | String   | X   | テンプレートメッセージタイプ（BA: 基本型、EX: 付加情報型、AD: チャンネル追加型、MI: 複合型、default: BA）                                                                                                                                                                               |
| templateEmphasizeType | String   | X   | テンプレート強調表示タイプ（NONE: 基本、TEXT: 強調表示、IMAGE: 画像型、ITEM_LIST: アイテムリストタイプ、default: NONE）<br>- TEXT: templateTitle、templateSubtitle フィールド必須<br>- IMAGE: templateImageName、templateImageUrl フィールド必須<br>ITEM_LIST: 画像、ヘッダー、アイテムハイライト、アイテムリストのうち 1 つ以上必須 |
| templateExtra         | String   | X   | テンプレート付加情報（テンプレートメッセージタイプが［付加情報型/複合型］の場合は必須）                                                                                                                                                                                                      |
| tempalteTitle         | String   | X   | テンプレートタイトル（最大 50 文字、Android: 2 行、23 文字以上は末尾省略、iOS: 2 行、27 文字以上は末尾省略）                                                                                                                                                                              |
| templateSubtitle      | String   | X   | テンプレートサブ文言（最大 50 文字、Android: 18 文字以上は末尾省略、iOS: 21 文字以上は末尾省略）                                                                                                                                                                                   |
| templateHeader        | String   | X   | テンプレートヘッダー（最大 16 文字）                                                                                                                                                                                                                                  |
| templateItem          | Object   | X   | アイテム                                                                                                                                                                                                                                             |
| - list                | List     | X   | アイテムリスト（最小 2 個、最大 10 個）                                                                                                                                                                                                                          |
| -- title              | String   | X   | タイトル（最大 6 文字）                                                                                                                                                                                                                                      |
| -- description        | String   | X   | 説明（最大 23 文字）                                                                                                                                                                                                                                   |
| - summary             | Object   | X   | アイテムサマリー情報                                                                                                                                                                                                                                       |
| -- title              | String   | X   | タイトル（最大 6 文字）                                                                                                                                                                                                                                      |
| -- description        | String   | X   | 説明（変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字）                                                                                                                                                                                                   |
| templateItemHighlight | Object   | X   | アイテムハイライト                                                                                                                                                                                                                                       |
| - title               | String   | X   | タイトル（最大 30 文字、サムネイル画像がある場合は 21 文字）                                                                                                                                                                                                                 |
| - description         | String   | X   | 説明（最大 19 文字、サムネイル画像がある場合は 13 文字）                                                                                                                                                                                                               |
| - imageUrl            | String   | X   | サムネイル画像 URL                                                                                                                                                                                                                                      |
| templateRepresentLink | Object   | X   | 代表リンク                                                                                                                                                                                                                                           |
| - linkMo              | String   | 	X  | 	モバイル Web リンク（最大 500 文字）                                                                                                                                                                                                                              |
| - linkPc              | String   | 	X  | PC Web リンク（最大 500 文字）                                                                                                                                                                                                                                |
| - schemeIos           | String   | X   | 	iOS アプリリンク（最大 500 文字）                                                                                                                                                                                                                              |
| - schemeAndroid       | String   | X   | 	Android アプリリンク（最大 500 文字）                                                                                                                                                                                                                            |
| templateImageName     | String   | 	X  | 画像名（アップロードしたファイル名）                                                                                                                                                                                                                                  |
| templateImageUrl      | String   | 	X  | 画像 URL                                                                                                                                                                                                                                         |
| securityFlag          | Boolean  | X   | セキュリティテンプレートかどうか<br>OTP などのセキュリティメッセージの場合に設定<br>送信時のメインデバイスを除くすべてのデバイスにメッセージテキストを非表示（default: false）                                                                                                                                                    |
| categoryCode          | String   | X   | テンプレートカテゴリコード（テンプレートカテゴリ照会 API 参照、default: 999999）<br>カテゴリがその他の場合、最低優先順位で審査                                                                                                                                                                   |
| buttons               | 	List    | 	X  | ボタンリスト（最大 5 個）                                                                                                                                                                                                                                   |
| - ordering            | 	Integer | 	X  | ボタン順序（1〜5）                                                                                                                                                                                                                                      |
| - type                | String   | 	X  | 	ボタンタイプ（WL: Web リンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転送、BC: 相談トーク転換、BT: ボット転換、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ送信プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話をかける）                                                                      |
| - name                | String   | 	X  | 	ボタン名（ボタンがある場合は必須、最大 14 文字）                                                                                                                                                                                                                    |
| - linkMo              | String   | 	X  | 	モバイル Web リンク (WL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC Web リンク (WL タイプの場合は選択フィールド、最大 500 文字)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS アプリリンク (AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	Android アプリリンク (AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	ビジネスフォーム ID (BF タイプの場合は必須)                                                                                                                                                                                                                         |
| - pluginId            | 	String  | 	X  | 	プラグイン ID (最大 24 文字)                                                                                                                                                                                                                                |
| -- telNumber           | 	String  | 	X  | TN (電話をかける) タイプのボタンの場合、転送する電話番号                                                                                    |
| quickReplies          | 	List    | 	X  | クイックリプライリスト (最大 5 個)                                                                                                                                                                                                                                 |
| - ordering            | 	Integer | 	X  | 	クイックリプライの順序 (クイックリプライがある場合は必須)                                                                                                                                                                                                                        |
| - type                | String   | 	X  | 	クイックリプライタイプ (WL: Web リンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク切替、BT: ボット切替、BF: ビジネスフォーム)                                                                                                                                                                        |
| - name                | String   | 	X  | 	クイックリプライ名 (クイックリプライがある場合は必須、最大 14 文字)                                                                                                                                                                                                                |
| - linkMo              | String   | 	X  | 	モバイル Web リンク (WL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                             |
| - linkPc              | String   | 	X  | PC Web リンク (WL タイプの場合は選択フィールド、最大 500 文字)                                                                                                                                                                                                               |
| - schemeIos           | String   | X   | 	iOS アプリリンク (AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                             |
| - schemeAndroid       | String   | X   | 	Android アプリリンク (AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                                                                                           |
| - bizFormId           | 	Integer | 	X  | 	ビジネスフォーム ID (BF タイプの場合は必須)                                                                                                                                                                                                                         |

* チャンネル追加型(AD)または複合型(MI)メッセージタイプのテンプレート登録時、templateAd の値が固定されます。
* チャンネル追加型(AD)または複合型(MI)メッセージタイプのテンプレート登録時、チャンネル追加(AC)ボタンが最初の順序に配置される必要があります。
* チャンネル追加(AC)ボタンのボタン名は「チャンネル追加」で固定して登録する必要があります。

フィールドごとの置換変数(`#{変数}`)の使用可否については、以下を参照してください。

| 区分 | フィールド | 置換可否 |
|------|------|---------|
| 基本 | templateContent | O |
| 基本 | templateTitle | O |
| 基本 | templateSubtitle | X |
| 基本 | templateHeader | O |
| 基本 | templateExtra | X |
| 基本 | templateAd | X |
| ボタン | name | X |
| ボタン | linkMo, linkPc, schemeIos, schemeAndroid | O |
| クイックリプライ | name | X |
| クイックリプライ | linkMo, linkPc, schemeIos, schemeAndroid | O |
| テンプレートアイテム | title | X |
| テンプレートアイテム | description | O |
| テンプレートアイテム summary | title | X |
| テンプレートアイテム summary | description | O |
| テンプレートアイテムハイライト | title | O |
| テンプレートアイテムハイライト | description | O |
| テンプレートアイテムハイライト | imageUrl | X |
| 代表リンク | linkMo, linkPc, schemeIos, schemeAndroid | O |

<a id="response-13"></a>

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
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

<a id="modify-templates"></a>

### テンプレート修正

<a id="request-13"></a>

#### リクエスト

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明     |
|--------------|---------|---------|
| appkey       | 	String | 	固有のアプリキー |
| senderKey    | 	String | 	発信キー   |
| templateCode | 	String | 	テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

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

| 名前                    | 	タイプ      | 	必須 | 	説明                                                                                                                                                                        |
|-----------------------|----------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| templateName          | 	String  | 	O  | テンプレート名(最大 150 文字)                                                                                                                                                              |
| templateContent       | 	String  | 	O  | テンプレート本文(最大 1300 文字)                                                                                                                                                           |
| templateMessageType   | String   | X   | テンプレートメッセージタイプ(BA: 基本型、EX: 付加情報型、AD: チャンネル追加型、MI: 複合型)                                                                                                                       |
| templateEmphasizeType | String   | X   | テンプレート強調表示タイプ(NONE: 基本、TEXT: 強調表示、IMAGE: 画像型、default: NONE)<br>- TEXT: templateTitle、templateSubtitle フィールド必須<br>- IMAGE: templateImageName、templateImageUrl フィールド必須     |
| templateExtra         | String   | X   | テンプレート付加情報(テンプレートメッセージタイプが[付加情報型/複合型]の場合は必須)                                                                                                                                 |
| tempalteTitle         | String   | X   | テンプレートタイトル(最大 50 文字、Android: 2 行、23 文字以上は省略表示、iOS: 2 行、27 文字以上は省略表示)                                                                                                         |
| templateSubtitle      | String   | X   | テンプレート補助文言(最大 50 文字、Android: 18 文字以上は省略表示、iOS: 21 文字以上は省略表示)                                                                                                              |
| templateHeader        | String   | X   | テンプレートヘッダー(最大 16 文字)                                                                                                                                                             |
| templateItem          | Object   | X   | アイテム                                                                                                                                                                        |
| - list                | List     | X   | アイテムリスト(最小 2 個、最大 10 個)                                                                                                                                                     |
| -- title              | String   | X   | タイトル(最大 6 文字)                                                                                                                                                                 |
| -- description        | String   | X   | 説明(最大 23 文字)                                                                                                                                                              |
| - summary             | Object   | X   | アイテムサマリー情報                                                                                                                                                                  |
| -- title              | String   | X   | タイトル(最大 6 文字)                                                                                                                                                                 |
| -- description        | String   | X   | 説明(変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字)                                                                                                                              |
| templateItemHighlight | Object   | X   | アイテムハイライト                                                                                                                                                                  |
| - title               | String   | X   | タイトル(最大 30 文字、サムネイル画像がある場合は 21 文字)                                                                                                                                            |
| - description         | String   | X   | 説明(最大 19 文字、サムネイル画像がある場合は 13 文字)                                                                                                                                          |
| - imageUrl            | String   | X   | サムネイル画像 URL                                                                                                                                                                 |
| templateRepresentLink | Object   | X   | 代表リンク                                                                                                                                                                      |
| - linkMo              | String   | 	X  | 	モバイル Web リンク(最大 500 文字)                                                                                                                                                         |
| - linkPc              | String   | 	X  | PC Web リンク(最大 500 文字)                                                                                                                                                           |
| - schemeIos           | String   | X   | 	iOS アプリリンク(最大 500 文字)                                                                                                                                                         |
| - schemeAndroid       | String   | X   | 	Android アプリリンク(最大 500 文字)                                                                                                                                                       |
| templateImageName     | String   | 	X  | 画像名(アップロードしたファイル名)                                                                                                                                                             |
| templateImageUrl      | String   | 	X  | 画像 URL                                                                                                                                                                    |
| securityFlag          | Boolean  | X   | セキュリティテンプレート使用有無<br>OTP などのセキュリティメッセージの場合に設定<br>送信時のメインデバイスを除くすべてのデバイスにメッセージテキストを非表示(default: false)                                                                               |
| categoryCode          | String   | X   | テンプレートカテゴリコード(テンプレートカテゴリ照会 API 参照、default: 999999)<br>カテゴリがその他の場合、最低優先度で審査                                                                                              |
| buttons               | 	List    | 	X  | ボタンリスト(最大 5 個)                                                                                                                                                              |
| - ordering            | 	Integer | 	X  | ボタン順序(1〜5)                                                                                                                                                                 |
| - type                | String   | 	X  | 	ボタンタイプ(WL: Web リンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転達、BC: 相談トーク転換、BT: ボット転換、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ転送プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話) |
| - name                | String   | 	X  | 	ボタン名(ボタンがある場合は必須、最大 14 文字)                                                                                                                                               |
| - linkMo              | String   | 	X  | 	モバイル Web リンク(WL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                        |
| - linkPc              | String   | 	X  | PC Web リンク(WL タイプの場合は任意フィールド、最大 500 文字)                                                                                                                                          |
| - schemeIos           | String   | X   | 	iOS アプリリンク(AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                        |
| - schemeAndroid       | String   | X   | 	Android アプリリンク(AL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                      |
| - bizFormId           | 	Integer | 	X  | 	ビジネスフォーム ID(BF タイプの場合は必須)                                                                                                                                                    |
| - pluginId            | 	String  | 	X  | 	プラグイン ID(最大 24 文字)                                                                                                                                                           |
| -- telNumber           | 	String  | 	X  | TN(電話)タイプボタン使用時に転達する電話番号                                                                    |
| quickReplies          | 	List    | 	X  | クイックリプライリスト(最大 5 個)                                                                                                                                                            |
| - ordering            | 	Integer | 	X  | 	クイックリプライ順序(クイックリプライがある場合は必須)                                                                                                                                                   |
| - type                | String   | 	X  | 	クイックリプライタイプ(WL: Web リンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム)                                                                                                   |
| - name                | String   | 	X  | 	クイックリプライ名(クイックリプライがある場合は必須、最大 14 文字)                                                                                                                                           |
| - linkMo              | String   | 	X  | 	モバイル Web リンク(WL タイプの場合は必須フィールド、最大 500 文字)                                                                                                                                        |
| - linkPc              | String   | 	X  | PC Webリンク（WLタイプの場合は選択フィールド、最大500文字）                                                                                                                                          |
| - schemeIos           | String   | X   | 	iOSアプリリンク（ALタイプの場合は必須フィールド、最大500文字）                                                                                                                                        |
| - schemeAndroid       | String   | X   | 	Androidアプリリンク（ALタイプの場合は必須フィールド、最大500文字）                                                                                                                                      |
| - bizFormId           | 	Integer | 	X  | 	ビジネスフォームID（BFタイプの場合は必須）                                                                                                                                                    |

* 「チャンネル追加型（AD）」および「複合型（MI）」メッセージタイプのテンプレートを修正する際、templateAd の値が固定されます。
* チャンネル追加型（AD）および複合型（MI）メッセージタイプのテンプレートを修正する際、チャンネル追加（AC）ボタンを先頭の順番に配置する必要があります。
* チャンネル追加（AC）ボタンのボタン名は「채널 추가」に固定されているため、修正する必要があります。

<a id="response-14"></a>

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
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

<a id="delete-templates"></a>

### テンプレート削除

<a id="request-14"></a>

#### リクエスト

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明     |
|--------------|---------|---------|
| appkey       | 	String | 	固有のアプリキー |
| senderKey    | 	String | 	発信キー   |
| templateCode | 	String | 	テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

* 承認済みのテンプレートを削除した場合、NHN Cloud 内でのみ削除されます（3日間送信されていないテンプレートのみ削除可能）。
* 承認済みテンプレートの場合、カカオトークビズメッセージの制約により、カカオ内部データは削除できません。
* カカオに残存しているテンプレートは、1年間未使用の場合に休眠処理され、休眠状態が1年間継続すると削除処理されます（カカオでテンプレートが休眠に移行または削除された場合、担当者に通知が送信されます）。

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

| 名前              | タイプ      | Not Null | 説明     |
|-----------------|---------|:--------:|--------|
| header          | Object  |    O     | ヘッダ領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

<a id="inquire-of-templates"></a>

### テンプレートへのお問い合わせ

<a id="request-15"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明         |
|--------------|---------|-------------|
| appkey       | 	String | 	固有のアプリキー   |
| senderKey    | 	String | 	発信キー       |
| templateCode | 	String | 	テンプレートコード  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明                  |
|--------------|---------|-----|----------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request Body]

```
{
  "comment": String
}
```

| 名前      | 	タイプ     | 	必須 | 	説明       |
|---------|---------|-----|-----------|
| comment | 	String | 	O  | お問い合わせ内容  |

* 反映状態のテンプレートにお問い合わせを送信した場合、検収中 (REQ) の状態に変更されます。

<a id="response-16"></a>

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
|-----------------|---------|:--------:|----------|
| header          | Object  |    O     | ヘッダー領域   |
| - resultCode    | Integer |    O     | 結果コード    |
| - resultMessage | String  |    O     | 結果メッセージ  |
| - isSuccessful  | Boolean |    O     | 成功可否     |

<a id="send-inquiry-on-templates-with-file-attachment"></a>

### ファイルを添付してテンプレートに問い合わせる

<a id="request-16"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/comments_file
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明        |
|--------------|---------|------------|
| appkey       | 	String | 	固有のアプリキー  |
| senderKey    | 	String | 	発信キー      |
| templateCode | 	String | 	テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明               |
|--------------|---------|-----|-------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request Body]

```
{
  "comment": String,
  "attachments": File
}
```

| 名前          | 	タイプ        | 	必須 | 	説明                                                                                                                                                |
|-------------|------------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------|
| comment     | 	String    | 	O  | 問い合わせ内容                                                                                                                                            |
| attachments | List<File> | X   | 添付ファイルリスト（最大10個）<br>- サポートされる拡張子（.png、.jpg、.jpeg、.gif、.pdf、.hwp、.doc、.docx）<br>- 各ファイルの最大サイズ：50MB<br>- アップロード可能なファイルの合計最大サイズ：100MB |

* 反映済みのテンプレートに問い合わせを残した場合、検収中（REQ）の状態に変更されます。

<a id="response-17"></a>

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
|-----------------|---------|:--------:|----------|
| header          | Object  |    O     | ヘッダー領域   |
| - resultCode    | Integer |    O     | 結果コード    |
| - resultMessage | String  |    O     | 結果メッセージ  |
| - isSuccessful  | Boolean |    O     | 成功かどうか   |

<a id="change-template-to-channel-add-type"></a>

### テンプレートをチャンネル追加型に変更

<a id="request-17"></a>

#### リクエスト

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appKey}/senders/{senderKey}/templates/{templateCode}/convert-add-channel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明     |
|--------------|---------|---------|
| appkey       | 	String | 	固有のアプリキー |
| senderKey    | 	String | 	発信キー   |
| templateCode | 	String | 	テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

* グループ発信プロフィールに登録されたテンプレートは、チャンネル追加型に変換することはできません。

<a id="response-18"></a>

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
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

<a id="single-query-for-template"></a>

### テンプレート単件照会

<a id="request-18"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明     |
|--------------|---------|---------|
| appkey       | 	String | 	固有のアプリキー |
| senderKey    | 	String | 	発信キー   |
| templateCode | String  | テンプレートコード  |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

| テンプレート状態コード | 説明   |
|-----------|------|
| TSC01     | リクエスト   |
| TSC02     | 検収中 |
| TSC03     | 承認   |
| TSC04     | 反映   |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}"
```

<a id="response-19"></a>

#### 応答

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

| 名前                      | タイプ      | Not Null | 説明                                                                                                                                                                               |
|-------------------------|---------|:--------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                  | Object  |    O     | ヘッダー領域                                                                                                                                                                            |
| - resultCode            | Integer |    O     | 結果コード                                                                                                                                                                            |
| - resultMessage         | String  |    O     | 結果メッセージ                                                                                                                                                                           |
| - isSuccessful          | Boolean |    O     | 成功の可否                                                                                                                                                                            |
| templates               | Object    |    X     | テンプレートリスト                                                                                                                                                                          |
| - plusFriendId          | String  |    O     | カカオトークチャンネル検索用 ID または発信プロフィールグループ名                                                                                                                                                     |
| - senderKey             | String  |    O     | 発信キー                                                                                                                                                                             |
| - plusFriendType        | String  |    O     | プラスフレンドタイプ (NORMAL, GROUP)                                                                                                                                                          |
| - templateCode          | String  |    O     | テンプレートコード                                                                                                                                                                           |
| - kakaoTemplateCode     | String  |    O     | 元のテンプレートコード                                                                                                                                                                        |
| - templateName          | String  |    O     | テンプレート名                                                                                                                                                                             |
| - templateMessageType   | String  |    X     | テンプレートメッセージタイプ (BA: 基本型、EX: 付加情報型、AD: チャンネル追加型、MI: 複合型)                                                                                                                             |
| - templateEmphasizeType | String  |    X     | テンプレート強調表示タイプ (NONE: 基本、TEXT: 強調表示、IMAGE: 画像型、ITEM_LIST: アイテムリストタイプ)                                                                                                             |
| - templateContent       | String  |    X     | テンプレート本文                                                                                                                                                                           |
| - templateExtra         | String  |    X     | テンプレート付加情報                                                                                                                                                                        |
| - templateAd            | String  |    X     | テンプレート内の受信同意リクエストまたは簡単な広告文                                                                                                                                                      |
| - tempalteTitle         | String  |    X     | テンプレートタイトル                                                                                                                                                                           |
| - templateSubtitle      | String  |    X     | テンプレート補助文言                                                                                                                                                                        |
| - templateHeader        | String  |    X     | テンプレートヘッダー (最大 16 文字)                                                                                                                                                                   |
| - templateItem          | Object  |    X     | アイテム                                                                                                                                                                              |
| -- list                 | List    |    X     | アイテムリスト (最小 2 件、最大 10 件)                                                                                                                                                           |
| --- title               | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                                       |
| --- description         | String  |    X     | 説明 (最大 23 文字)                                                                                                                                                                    |
| -- summary              | Object  |    X     | アイテム要約情報                                                                                                                                                                        |
| --- title               | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                                       |
| --- description         | String  |    X     | 説明 (変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字)                                                                                                                                    |
| - templateItemHighlight | Object  |    X     | アイテムハイライト                                                                                                                                                                        |
| -- title                | String  |    X     | タイトル (最大 30 文字、サムネイル画像がある場合は 21 文字)                                                                                                                                                  |
| -- description          | String  |    X     | 説明 (最大 19 文字、サムネイル画像がある場合は 13 文字)                                                                                                                                                |
| -- imageUrl             | String  |    X     | サムネイル画像 URL                                                                                                                                                                       |
| - templateRepresentLink | Object  |    X     | 代表リンク                                                                                                                                                                            |
| -- linkMo               | String  |    X     | モバイル Web リンク (最大 500 文字)                                                                                                                                                                |
| -- linkPc               | String  |    X     | PC Web リンク (最大 500 文字)                                                                                                                                                                 |
| -- schemeIos            | String  |    X     | iOS アプリリンク (最大 500 文字)                                                                                                                                                                |
| -- schemeAndroid        | String  |    X     | Android アプリリンク (最大 500 文字)                                                                                                                                                              |
| - templateImageName     | String  |    X     | 画像名 (アップロードしたファイル名)                                                                                                                                                                   |
| - templateImageUrl      | String  |    X     | 画像 URL                                                                                                                                                                          |
| - buttons               | List    |    X     | ボタンリスト                                                                                                                                                                           |
| -- ordering             | Integer |    X     | ボタン順序 (1〜5)                                                                                                                                                                       |
| -- type                 | String  |    X     | ボタンタイプ (WL: Web リンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転送、BC: 相談トーク切替、BT: ボット切替、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ転送プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話発信) |
| -- name                 | String  |    X     | ボタン名                                                                                                                                                                            |
| -- linkMo               | String  |    X     | モバイルウェブリンク（WL タイプの場合は必須フィールド）                                                                                                                                                    |
| -- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合は任意フィールド）                                                                                                                                                     |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                                    |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                                |
| -- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                         |
| -- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                               |
| -- telNumber            | 	String  | 	X  | TN（電話する）タイプのボタンの場合、送信する電話番号                                                                                                                                                      |
| - quickReplies          | List    |    X     | クイックリプライリスト（最大 5 件）                                                                                                                                                               |
| -- ordering             | Integer |    X     | クイックリプライの順序（クイックリプライがある場合は必須）                                                                                                                                                    |
| -- type                 | String  |    X     | クイックリプライタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム）                                                                                               |
| -- name                 | String  |    X     | クイックリプライ名（クイックリプライがある場合は必須、最大 14 文字）                                                                                                                                             |
| -- linkMo               | String  |    X     | モバイルウェブリンク（WL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                          |
| -- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合は任意フィールド、最大 500 文字）                                                                                                                                           |
| -- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                          |
| -- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                      |
| -- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                         |
| - comments              | List    |    X     | 検収結果                                                                                                                                                                             |
| -- id                   | Integer |    X     | 問い合わせ ID                                                                                                                                                                         |
| -- content              | String  |    X     | 問い合わせ内容                                                                                                                                                                          |
| -- userName             | String  |    X     | 作成者                                                                                                                                                                              |
| -- createAt             | String  |    O     | 登録日時                                                                                                                                                                             |
| -- attachment           | List    |    X     | 添付ファイル                                                                                                                                                                           |
| --- originalFileName    | String  |    X     | 添付ファイル名                                                                                                                                                                          |
| --- filePath            | String  |    X     | 添付ファイルパス                                                                                                                                                                         |
| -- status               | String  |    X     | コメント状態（INQ: 問い合わせ、APR: 承認、REJ: 反映、REP: 返答、REQ: 検収中）                                                                                                                              |
| - status                | String  |    O     | テンプレート状態                                                                                                                                                                         |
| - statusName            | String  |    X     | テンプレート状態名                                                                                                                                                                        |
| - securityFlag          | Boolean |    X     | セキュリティテンプレートかどうか                                                                                                                                                                 |
| - categoryCode          | String  |    X     | テンプレートカテゴリコード                                                                                                                                                                    |
| - block                 | Boolean |    X     | ブロックかどうか                                                                                                                                                                         |
| - dormant               | Boolean |    X     | 休眠かどうか                                                                                                                                                                           |
| - createDate            | String  |    O     | 作成日時                                                                                                                                                                             |
| - updateDate            | String  |    X     | 修正日時                                                                                                                                                                             |

<a id="list-templates"></a>

### テンプレートリスト照会

<a id="request-19"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ     | 	説明     |
|-----------|---------|---------|
| appkey    | 	String | 	固有のアプリキー |
| senderKey | 	String | 	発信キー   |

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

| 名前             | 	タイプ      | 	必須 | 	説明                            |
|----------------|----------|-----|--------------------------------|
| templateCode   | 	String  | 	X  | 	テンプレートコード                        |
| templateName   | 	String  | 	X  | 	テンプレート名                        |
| templateStatus | String   | 	X  | テンプレート状態コード                      |
| pageNum        | 	Integer | 	X  | 	ページ番号（Default: 1）            |
| pageSize       | 	Integer | 	X  | 	照会件数（Default: 15、Max: 1000） |

| テンプレート状態コード | 説明   |
|-----------|------|
| TSC01     | リクエスト   |
| TSC02     | 検収中 |
| TSC03     | 承認   |
| TSC04     | 反映   |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates?templateStatus={テンプレート状態コード}"
```

<a id="response-20"></a>

#### 応答

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

| 名前                       | タイプ      | Not Null | 説明                                                                                                                                                                     |
|--------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                   | Object  |    O     | ヘッダー領域                                                                                                                                                                  |
| - resultCode             | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage          | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful           | Boolean |    O     | 成否                                                                                                                                                                  |
| templateListResponse     | Object  |    X     | 本文領域                                                                                                                                                                  |
| - templates              | List    |    X     | テンプレートリスト                                                                                                                                                                |
| -- plusFriendId          | String  |    O     | カカオトークチャンネル検索用 ID または発信プロフィールグループ名                                                                                                                                           |
| -- senderKey             | String  |    O     | 発信キー                                                                                                                                                                   |
| -- plusFriendType        | String  |    O     | プラスフレンドタイプ (NORMAL, GROUP)                                                                                                                                                |
| -- templateCode          | String  |    O     | テンプレートコード                                                                                                                                                                 |
| -- kakaoTemplateCode     | String  |    O     | 元のテンプレートコード                                                                                                                                                              |
| -- templateName          | String  |    O     | テンプレート名                                                                                                                                                                   |
| -- templateMessageType   | String  |    X     | テンプレートメッセージタイプ (BA: 基本型、EX: 付加情報型、AD: チャンネル追加型、MI: 複合型)                                                                                                                   |
| -- templateEmphasizeType | String  |    X     | テンプレート강조表記型タイプ (NONE: デフォルト、TEXT: 강조表示、IMAGE: 画像型、ITEM_LIST: アイテムリストタイプ)                                                                                                   |
| -- templateContent       | String  |    X     | テンプレート本文                                                                                                                                                                 |
| -- templateExtra         | String  |    X     | テンプレート付加情報                                                                                                                                                              |
| -- templateAd            | String  |    X     | テンプレート内の受信同意要求または簡単な広告文                                                                                                                                            |
| -- tempalteTitle         | String  |    X     | テンプレートタイトル                                                                                                                                                                 |
| -- templateSubtitle      | String  |    X     | テンプレート補助文                                                                                                                                                              |
| - templateHeader         | String  |    X     | テンプレートヘッダー (最大 16 文字)                                                                                                                                                         |
| - templateItem           | Object  |    X     | アイテム                                                                                                                                                                    |
| -- list                  | List    |    X     | アイテムリスト (最小 2 個、最大 10 個)                                                                                                                                                 |
| --- title                | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                             |
| --- description          | String  |    X     | 説明 (最大 23 文字)                                                                                                                                                          |
| -- summary               | Object  |    X     | アイテム概要情報                                                                                                                                                              |
| --- title                | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                             |
| --- description          | String  |    X     | 説明 (変数および通貨単位、数字、カンマ、ピリオドのみ使用可能、最大 14 文字)                                                                                                                          |
| - templateItemHighlight  | Object  |    X     | アイテムハイライト                                                                                                                                                              |
| -- title                 | String  |    X     | タイトル (最大 30 文字、サムネイル画像がある場合は 21 文字)                                                                                                                                        |
| -- description           | String  |    X     | 説明 (最大 19 文字、サムネイル画像がある場合は 13 文字)                                                                                                                                      |
| -- imageUrl              | String  |    X     | サムネイル画像 URL                                                                                                                                                             |
| - templateRepresentLink  | Object  |    X     | 代表リンク                                                                                                                                                                  |
| -- linkMo                | String  |    X     | モバイル Web リンク (最大 500 文字)                                                                                                                                                      |
| -- linkPc                | String  |    X     | PC Web リンク (最大 500 文字)                                                                                                                                                       |
| -- schemeIos             | String  |    X     | iOS アプリリンク (最大 500 文字)                                                                                                                                                      |
| -- schemeAndroid         | String  |    X     | Android アプリリンク (最大 500 文字)                                                                                                                                                    |
| -- templateImageName     | String  |    X     | 画像名 (アップロードしたファイル名)                                                                                                                                                         |
| -- templateImageUrl      | String  |    X     | 画像 URL                                                                                                                                                                |
| -- buttons               | List    |    X     | ボタンリスト                                                                                                                                                                 |
| --- ordering             | Integer |    X     | ボタン順序 (1〜5)                                                                                                                                                             |
| --- type                 | String  |    X     | ボタンタイプ (WL: Web リンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転送、BC: 相談トーク転換、BT: ボット転換、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ転送プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話をかける) |
| --- name                 | String  |    X     | ボタン名                                                                                                                                                                  |
| --- linkMo               | String  |    X     | モバイル Web リンク (WL タイプの場合は必須フィールド)                                                                                                                                              |
| --- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合は選択フィールド）                                                                                                                                               |
| --- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                              |
| --- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                            |
| --- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                 |
| --- pluginId             | String  |    X     | プラグイン ID（最大 24 文字）                                                                                                                                                        |
| --- telNumber            | 	String  | 	X  | TN（電話する）タイプのボタンの場合に送信する電話番号                                                                                                                                    |
| -- quickReplies          | List    |    X     | クイックリプライリスト（最大 5 件）                                                                                                                                                        |
| --- ordering             | Integer |    X     | クイックリプライの順序（クイックリプライがある場合は必須）                                                                                                                                                |
| --- type                 | String  |    X     | クイックリプライタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム）                                                                                                |
| --- name                 | String  |    X     | クイックリプライ名（クイックリプライがある場合は必須、最大 14 文字）                                                                                                                                        |
| --- linkMo               | String  |    X     | モバイルウェブリンク（WL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| --- linkPc               | String  |    X     | PC ウェブリンク（WL タイプの場合は選択フィールド、最大 500 文字）                                                                                                                                      |
| --- schemeIos            | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| --- schemeAndroid        | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                   |
| --- bizFormId            | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                 |
| -- comments              | List    |    X     | 検収結果                                                                                                                                                                  |
| --- id                   | Integer |    X     | お問い合わせ ID                                                                                                                                                                 |
| --- content              | String  |    X     | お問い合わせ内容                                                                                                                                                                  |
| --- userName             | String  |    X     | 作成者                                                                                                                                                                    |
| --- createAt             | String  |    O     | 登録日                                                                                                                                                                  |
| --- attachment           | List    |    X     | 添付ファイル                                                                                                                                                                  |
| ---- originalFileName    | String  |    X     | 添付ファイル名                                                                                                                                                                 |
| ---- filePath            | String  |    X     | 添付ファイルパス                                                                                                                                                               |
| --- status               | String  |    X     | コメント状態（INQ: 問い合わせ、APR: 承認、REJ: 反映、REP: 返答、REQ: 検収中）                                                                                                                   |
| -- status                | String  |    O     | テンプレート状態                                                                                                                                                                 |
| -- statusName            | String  |    X     | テンプレート状態名                                                                                                                                                                |
| -- securityFlag          | Boolean |    X     | セキュリティテンプレートかどうか                                                                                                                                              |
| -- categoryCode          | String  |    X     | テンプレートカテゴリコード                                                                                                                                                            |
| -- createDate            | String  |    O     | 作成日                                                                                                                                                                   |
| -- updateDate            | String  |    X     | 修正日                                                                                                                                                                   |
| - totalCount             | Integer |    X     | 総件数                                                                                                                                                                    |

<a id="list-template-modifications"></a>

### テンプレート修正リスト照会

<a id="request-20"></a>

#### リクエスト

[URL]

```
GET  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前           | 	タイプ     | 	説明     |
|--------------|---------|---------|
| appkey       | 	String | 	固有のアプリキー |
| senderKey    | 	String | 	発信キー   |
| templateCode | 	String | 	テンプレートコード |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[例]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/templates/{templateCode}/modifications"
```

<a id="response-21"></a>

#### 応答

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

| 名前                            | タイプ      | Not Null | 説明                                                                                                                                                                     |
|-------------------------------|---------|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| header                        | Object  |    O     | ヘッダー領域                                                                                                                                                                  |
| - resultCode                  | Integer |    O     | 結果コード                                                                                                                                                                  |
| - resultMessage               | String  |    O     | 結果メッセージ                                                                                                                                                                 |
| - isSuccessful                | Boolean |    O     | 成功かどうか                                                                                                                                                                  |
| templateModificationsResponse | Object  |    X     | 本文領域                                                                                                                                                                  |
| - templates                   | List    |    X     | テンプレートリスト                                                                                                                                                                |
| -- plusFriendId               | String  |    O     | カカオトークチャンネル検索用 ID または発信プロフィールグループ名                                                                                                                                           |
| -- senderKey                  | String  |    O     | 発信キー                                                                                                                                                                   |
| -- plusFriendType             | String  |    O     | プラスフレンドタイプ (NORMAL, GROUP)                                                                                                                                                |
| -- templateCode               | String  |    O     | テンプレートコード                                                                                                                                                                 |
| -- templateName               | String  |    O     | テンプレート名                                                                                                                                                                   |
| -- templateMessageType        | String  |    X     | テンプレートメッセージタイプ (BA: 基本型、EX: 付加情報型、AD: チャンネル追加型、MI: 複合型)                                                                                                                   |
| -- templateEmphasizeType      | String  |    X     | テンプレート強調表示タイプ (NONE: 基本、TEXT: 強調表示、IMAGE: 画像型)                                                                                                                     |
| -- templateContent            | String  |    O     | テンプレート本文                                                                                                                                                                 |
| -- templateExtra              | String  |    X     | テンプレート付加情報                                                                                                                                                              |
| -- templateAd                 | String  |    X     | テンプレート内の受信同意要求または簡単な広告文                                                                                                                                            |
| -- tempalteTitle              | String  |    X     | テンプレートタイトル                                                                                                                                                                 |
| -- templateSubtitle           | String  |    X     | テンプレート補助文言                                                                                                                                                              |
| - templateHeader              | String  |    X     | テンプレートヘッダー (最大 16 文字)                                                                                                                                                         |
| - templateItem                | Object  |    X     | アイテム                                                                                                                                                                    |
| -- list                       | List    |    X     | アイテムリスト (最小 2 個、最大 10 個)                                                                                                                                                 |
| --- title                     | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                             |
| --- description               | String  |    X     | 説明 (最大 23 文字)                                                                                                                                                          |
| -- summary                    | Object  |    X     | アイテム要約情報                                                                                                                                                              |
| --- title                     | String  |    X     | タイトル (最大 6 文字)                                                                                                                                                             |
| --- description               | String  |    X     | 説明 (変数および通貨単位、数字、コンマ、ピリオドのみ使用可能、最大 14 文字)                                                                                                                          |
| - templateItemHighlight       | Object  |    X     | アイテムハイライト                                                                                                                                                              |
| -- title                      | String  |    X     | タイトル (最大 30 文字、サムネイル画像がある場合は 21 文字)                                                                                                                                        |
| -- description                | String  |    X     | 説明 (最大 19 文字、サムネイル画像がある場合は 13 文字)                                                                                                                                      |
| -- imageUrl                   | String  |    X     | サムネイル画像 URL                                                                                                                                                             |
| - templateRepresentLink       | Object  |    X     | 代表リンク                                                                                                                                                                  |
| -- linkMo                     | String  |    X     | モバイル Web リンク (最大 500 文字)                                                                                                                                                      |
| -- linkPc                     | String  |    X     | PC Web リンク (最大 500 文字)                                                                                                                                                       |
| -- schemeIos                  | String  |    X     | iOS アプリリンク (最大 500 文字)                                                                                                                                                      |
| -- schemeAndroid              | String  |    X     | Android アプリリンク (最大 500 文字)                                                                                                                                                    |
| -- templateImageName          | String  |    X     | 画像名 (アップロードしたファイル名)                                                                                                                                                         |
| -- templateImageUrl           | String  |    X     | 画像 URL                                                                                                                                                                |
| -- buttons                    | List    |    X     | ボタンリスト                                                                                                                                                                 |
| --- ordering                  | Integer |    X     | ボタン順序 (1〜5)                                                                                                                                                             |
| --- type                      | String  |    X     | ボタンタイプ (WL: Web リンク、AL: アプリリンク、DS: 配送照会、BK: ボットキーワード、MD: メッセージ転送、BC: 相談トーク切替、BT: ボット切替、AC: チャンネル追加、BF: ビジネスフォーム、P1: 画像セキュリティ送信プラグイン ID、P2: 個人情報利用プラグイン ID、P3: ワンクリック決済プラグイン ID、TN: 電話発信) |
| --- name                      | String  |    X     | ボタン名                                                                                                                                                                  |
| --- linkMo                    | String  |    X     | モバイル Web リンク (WL タイプの場合は必須フィールド)                                                                                                                                              |
| --- linkPc                    | String  |    X     | PC ウェブリンク（WL タイプの場合は選択フィールド）                                                                                                                                               |
| --- schemeIos                 | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                              |
| --- schemeAndroid             | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド）                                                                                                                                            |
| --- telNumber                 | 	String  | 	X  | TN（電話する）タイプのボタンの場合、送信する電話番号                                                                                    |
| -- quickReplies               | List    |    X     | クイックリプライリスト（最大 5 件）                                                                                                                                                        |
| --- ordering                  | Integer |    X     | クイックリプライの順序（クイックリプライがある場合は必須）                                                                                                                                                |
| --- type                      | String  |    X     | クイックリプライタイプ（WL: ウェブリンク、AL: アプリリンク、BK: ボットキーワード、BC: 相談トーク転換、BT: ボット転換、BF: ビジネスフォーム）                                                                                                |
| --- name                      | String  |    X     | クイックリプライ名（クイックリプライがある場合は必須、最大 14 文字）                                                                                                                                        |
| --- linkMo                    | String  |    X     | モバイルウェブリンク（WL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| --- linkPc                    | String  |    X     | PC ウェブリンク（WL タイプの場合は選択フィールド、最大 500 文字）                                                                                                                                      |
| --- schemeIos                 | String  |    X     | iOS アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                     |
| --- schemeAndroid             | String  |    X     | Android アプリリンク（AL タイプの場合は必須フィールド、最大 500 文字）                                                                                                                                   |
| --- bizFormId                 | Integer |    X     | ビジネスフォーム ID（BF タイプの場合は必須）                                                                                                                                                 |
| -- comments                   | List    |    X     | 検収結果                                                                                                                                                                  |
| --- id                        | Integer |    X     | 問い合わせ ID                                                                                                                                                                 |
| --- content                   | String  |    X     | 問い合わせ内容                                                                                                                                                                  |
| --- userName                  | String  |    X     | 作成者                                                                                                                                                                    |
| --- createAt                  | String  |    O     | 登録日時                                                                                                                                                                  |
| --- attachment                | List    |    X     | 添付ファイル                                                                                                                                                                  |
| ---- originalFileName         | String  |    X     | 添付ファイル名                                                                                                                                                                 |
| ---- filePath                 | String  |    X     | 添付ファイルパス                                                                                                                                                               |
| --- status                    | String  |    X     | コメントステータス（INQ: 問い合わせ、APR: 承認、REJ: 反映、REP: 回答、REQ: 検収中）                                                                                                                   |
| -- status                     | String  |    O     | テンプレート状態                                                                                                                                                                 |
| -- statusName                 | String  |    O     | テンプレート状態名                                                                                                                                                                |
| -- securityFlag               | Boolean |    X     | セキュリティテンプレートかどうか                                                                                                                                              |
| -- categoryCode               | String  |    X     | テンプレートカテゴリコード                                                                                                                                                            |
| -- activated                  | Boolean |    X     | 有効化かどうか                                                                                                                                                                 |
| -- createDate                 | String  |    O     | 作成日時                                                                                                                                                                   |
| -- updateDate                 | String  |    X     | 修正日時                                                                                                                                                                   |
| - totalCount                  | Integer |    X     | 総件数                                                                                                                                                                    |

<a id="register-template-image"></a>

### テンプレート画像登録

<a id="request-21"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image
Content-Type: multipart/form-data
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request parameter]

| 名前   | 	タイプ   | 	必須 | 	説明     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	画像ファイル |

[例]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image" -F "file=@alimtalk-template-image.jpeg"
```

<a id="response-22"></a>

#### レスポンス

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

| 名前                  | タイプ      | Not Null | 説明             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | ヘッダ領域          |
| - resultCode        | Integer |    O     | 結果コード          |
| - resultMessage     | String  |    O     | 結果メッセージ         |
| - isSuccessful      | Boolean |    O     | 成否          |
| templateImage       | Object  |    X     | 本文領域          |
| - templateImageName | String  |    X     | 画像名（アップロードしたファイル名） |
| - templateImageUrl  | String  |    X     | 画像 URL        |

<a id="register-template-item-highlight-images"></a>

### テンプレートアイテムハイライト画像登録

<a id="request-22"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight
Content-Type: multipart/form-data
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request parameter]

| 名前   | 	タイプ   | 	必須 | 	説明     |
|------|-------|-----|---------|
| file | 	File | 	O  | 	画像ファイル |

[例]

```
curl -X POST -H "Content-Type: multipart/form-data" -H "X-Secret-Key:{secretkey}" "https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/template-image/item-highlight" -F "file=@alimtalk-template-image.jpeg"
```

<a id="response-23"></a>

#### レスポンス

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

| 名前                  | タイプ      | Not Null | 説明             |
|---------------------|---------|:--------:|----------------|
| header              | Object  |    O     | ヘッダー領域          |
| - resultCode        | Integer |    O     | 結果コード          |
| - resultMessage     | String  |    O     | 結果メッセージ         |
| - isSuccessful      | Boolean |    O     | 成否          |
| templateImage       | Object  |    X     | 本文領域          |
| - templateImageName | String  |    X     | 画像名（アップロードしたファイル名） |
| - templateImageUrl  | String  |    X     | 画像 URL        |

<a id="register-template-plugin"></a>

### テンプレートプラグイン登録

<a id="request-23"></a>

#### リクエスト

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ   | 	説明      |
|-----------|---------|---------|
| appkey    | 	String | 	アプリキー  |
| senderKey | 	String | 	発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ   | 	必須 | 	説明                  |
|--------------|---------|-----|----------------------|
| X-Secret-Key | 	String | O   | コンソールで生成できます。 |

[Request Body]

```
{
  "pluginType": String,
  "pluginId": String,
  "callbackUrl": String
}
```

| 名前          | 	タイプ   | 	必須 | 	説明                                                                      |
|-------------|---------|-----|--------------------------------------------------------------------------|
| pluginType  | 	String | 	O  | プラグインタイプ (SECURE_IMAGE: セキュリティ画像送信、ONE_TIME_PROFILE: 個人情報利用) |
| pluginId    | 	String | 	O  | プラグイン ID                                                                 |
| callbackUrl | 	String | 	O  | プラグインボタンクリック時に受信するコールバック URL                                           |

<a id="response-24"></a>

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
|-----------------|---------|:--------:|----------|
| header          | Object  |    O     | ヘッダー領域   |
| - resultCode    | Integer |    O     | 結果コード    |
| - resultMessage | String  |    O     | 結果メッセージ  |
| - isSuccessful  | Boolean |    O     | 成功かどうか   |

<a id="modify-template-plugin"></a>

### テンプレートプラグインの修正

<a id="request-24"></a>

#### リクエスト

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ     | 	説明          |
|-----------|---------|--------------|
| appkey    | 	String | 	固有のアプリキー    |
| senderKey | 	String | 	発信キー        |
| pluginId  | 	String | 	プラグイン ID    |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明                  |
|--------------|---------|-----|----------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

[Request Body]

```
{
  "pluginType": String,
  "callbackUrl": String
}
```

| 名前          | 	タイプ     | 	必須 | 	説明                                                                              |
|-------------|---------|-----|----------------------------------------------------------------------------------|
| pluginType  | 	String | 	O  | プラグインタイプ (SECURE_IMAGE: セキュリティ画像送信、ONE_TIME_PROFILE: 個人情報利用) |
| callbackUrl | 	String | 	O  | プラグインボタンクリック時に受信するコールバック URL                                     |

<a id="response-25"></a>

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
|-----------------|---------|:--------:|----------|
| header          | Object  |    O     | ヘッダー領域   |
| - resultCode    | Integer |    O     | 結果コード    |
| - resultMessage | String  |    O     | 結果メッセージ  |
| - isSuccessful  | Boolean |    O     | 成否       |

<a id="modify-template-plugin-2"></a>

### テンプレートプラグイン削除

<a id="request-25"></a>

#### リクエスト

[URL]

```
DELETE  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins/{pluginId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ   | 	説明          |
|-----------|---------|--------------|
| appkey    | 	String | 	固有のアプリキー    |
| senderKey | 	String | 	発信キー        |
| pluginId  | 	String | 	プラグイン ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ   | 	必須 | 	説明                  |
|--------------|---------|-----|----------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

<a id="response-26"></a>

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
|-----------------|---------|:--------:|----------|
| header          | Object  |    O     | ヘッダー領域   |
| - resultCode    | Integer |    O     | 結果コード    |
| - resultMessage | String  |    O     | 結果メッセージ  |
| - isSuccessful  | Boolean |    O     | 成功かどうか   |

<a id="retrieve-template-plugin"></a>

### テンプレートプラグイン照会

<a id="request-26"></a>

#### リクエスト

[URL]

```
PUT  /alimtalk/v2.3/appkeys/{appkey}/senders/{senderKey}/plugins
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前        | 	タイプ     | 	説明     |
|-----------|---------|---------|
| appkey    | 	String | 	固有のアプリキー |
| senderKey | 	String | 	発信キー   |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで作成できます。 |

<a id="response-27"></a>

#### レスポンス

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

| 名前               | タイプ      | Not Null | 説明                                                         |
|------------------|---------|:--------:|------------------------------------------------------------|
| header           | Object  |    O     | ヘッダー領域                                                      |
| - resultCode     | Integer |    O     | 結果コード                                                      |
| - resultMessage  | String  |    O     | 結果メッセージ                                                     |
| - isSuccessful   | Boolean |    O     | 成否                                                      |
| plugins          | List    |    X     | プラグインリスト                                                   |
| - pluginId       | String  |    X     | プラグイン ID                                                   |
| - pluginType     | String  |    X     | プラグインタイプ（SECURE_IMAGE: セキュリティ画像送信、ONE_TIME_PROFILE: 個人情報利用） |
| - pluginTypeName | String  |    X     | プラグイン名                                                    |
| - callbackUrl    | String  |    X     | プラグインボタンクリック時に受信するコールバック URL                                  |
| - modifiable     | Boolean |    X     | 修正可否                                                   |
| - deletable      | Boolean |    X     | 削除可否                                                   |

<a id="manage-alternative-delivery"></a>

## 代替送信管理

<a id="register-an-sms-appkey"></a>

### SMS AppKey 登録

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback/appkey
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで生成できます。 |

[Request body]

```
{
    "resendAppKey": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明                    |
|--------------|---------|-----|------------------------|
| resendAppKey | 	String | 	O  | 代替送信に設定する SMS サービスアプリキー |

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"resendAppKey": "smsAppKey"}
```

<a id="response-28"></a>

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
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |

<a id="register-alternative-delivery-settings"></a>

### 代替送信設定登録

[URL]

```
POST  /alimtalk/v2.3/appkeys/{appkey}/failback
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 名前     | 	タイプ     | 	説明     |
|--------|---------|---------|
| appkey | 	String | 	固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前           | 	タイプ     | 	必須 | 	説明              |
|--------------|---------|-----|------------------|
| X-Secret-Key | 	String | O   | コンソールで生成できます。 |

[Request body]

```
{  
   "senderKey": String,
   "isResend": Boolean,
   "resendSendNo": String
}
```

| 名前           | 	タイプ      | 	必須 | 	説明                                                                                       |
|--------------|----------|-----|-------------------------------------------------------------------------------------------|
| senderKey    | 	String  | 	O  | 発信キー                                                                                      |
| isResend     | 	Boolean | 	O  | 送信失敗時、SMS 代替送信を行うかどうか<br>コンソールで代替送信設定時、デフォルトで代替送信されます。                          |
| resendSendNo | 	String  | 	O  | 代替送信の発信番号<br><span style="color:red">(SMS サービスに登録された発信番号でない場合、代替送信が失敗する可能性があります。)</span> |

[例]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" -H "X-Secret-Key:{secretkey}" https://kakaotalk-bizmessage.api.nhncloudservice.com/alimtalk/v2.3/appkeys/{appkey}/failback/appkey -d '{"senderKey": "0be23c29de88d6888798aeda57062516354d74ba","isResend": true,"resendSendNo": "01012341234" }
```

<a id="response-29"></a>

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
| header          | Object  |    O     | ヘッダー領域  |
| - resultCode    | Integer |    O     | 結果コード  |
| - resultMessage | String  |    O     | 結果メッセージ |
| - isSuccessful  | Boolean |    O     | 成否  |