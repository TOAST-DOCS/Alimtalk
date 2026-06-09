## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## 統計

### [APIドメイン]

<table>
<thead>
<tr>
<th>ドメイン</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://kakaotalk-bizmessage.api.nhncloudservice.com</td>
</tr>
</tbody>
</table>


### 統計検索 - イベントベース
* イベント発生時間基準で収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(REQUESTED)：予約送信登録時間
    * 送信数(SENT)：ベンダーに送信時点(予約送信時間)
    * 成功数(RECEIVED)：送信結果成功(受信時間)
    * 失敗数(SENT_FAILED)：送信リクエスト失敗or送信結果失敗時点
    * 代替送信リクエスト数(RESENT)：代替送信リクエスト時点
    * 代替送信失敗数(RESENT_FAILED)：代替送信リクエスト失敗時点

### 統計情報照会

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey | String | 固有のアプリケーションキー |

[Header]
```
{
  "X-Secret-Key": String
}
```
| 値      | タイプ | 必須 | 説明                                 |
| ------------ | ------ | ---- | ---------------------------------------- |
| X-Secret-Key | String | O    | コンソールで作成できます。  |

[Query parameter]

|値| タイプ | 最大長さ | 必須 | 説明 |
|---|---|---|---|---|
| statsType | String | - | 必須 | 統計区分<br/>NORMAL：基本、MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：時間別 |
| from | String | - | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | オプション | 統計データ照会時、ツリー形式で提供するか設定 |
| extra1s | List<String> | - | オプション | 下位商品区分<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | オプション | senderKey |
| eventTypes | List<String> | - | オプション | イベント種類<br/> REQUESTED、SENT、RECEIVED、SENT_FAILED、RESENT、RESENT_FAILED |
| eventCategory | String | - | オプション | イベントリスト(現在`MESSAGE`のみサポート)<br/> MESSAGE |
| templateCodes | List<String> | - | オプション | テンプレートコードリスト(カカともへのメッセージ未サポート) |
| requestIds | List<String> | 5 | オプション | リクエストIDリスト |
| statsIds | List<String> | - | オプション | 統計IDリスト |
| statsCriteria | List<String> | - | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- EXTRA_1、EVENT：下位商品区分、イベント<br/>- EXTRA_2、EVENT：senderKey、イベント |

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

### イベント別数照会

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

|値| タイプ | 最大長さ | 必須 | 説明 |
|---|---|---|---|---|
| statsType | String | - | 必須 | 統計区分<br/>NORMAL：基本、MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：時間別 |
| from | String | - | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | オプション | 統計データ照会時、ツリー形式で提供するか設定 |
| extra1s | List<String> | - | オプション | 下位商品区分<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | オプション | senderKey |
| eventTypes | List<String> | - | オプション | イベント種類<br/> REQUESTED、SENT、RECEIVED、SENT_FAILED、RESENT、RESENT_FAILED |
| eventCategory | String | - | オプション | イベントリスト(現在`MESSAGE`のみサポート)<br/> MESSAGE |
| templateCodes | List<String> | - | オプション | テンプレートコードリスト(カカともへのメッセージ未サポート) |
| requestIds | List<String> | 5 | オプション | リクエストIDリスト |
| statsIds | List<String> | - | オプション | 統計IDリスト |
| statsCriteria | List<String> | - | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- EXTRA_1、EVENT：下位商品区分、イベント<br/>- EXTRA_2、EVENT：senderKey、イベント |

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
## カカオ統計

* カカオビズセンターで提供される統計データを照会します。
* 統計データは発信キー基準で日別（DAILY）または月別（MONTHLY）で照会できます。
* DAILY：最近 90 日以内のデータのみ照会可能で、照会範囲は最大 90 日です。
* MONTHLY：最近 3 か月以内のデータのみ照会可能で、照会範囲は最大 3 か月です。

発信プロフィール管理で **[カカオ統計ショートカット]** をクリックすると、新しいウィンドウでカカオ統計を照会できます。統計基準には送信統計とテンプレート統計があり、メッセージチャンネルによって照会条件が異なります。照会結果をチャートと表で確認できます。

* リアルタイム統計は提供せず、前日に収集したデータを毎日午前 7 時頃に提供します。
* お知らせトーク統計は D+1 で最初に提供され、D+2 で確定します。
* 有効読み取り数は同じメッセージについて重複集計しません。
* クリック数は同じメッセージについて重複集計します。
* 送信成功件数が 10 件以下の場合、有効読み取り数とクリック数は提供しません。

### 送信統計

発信プロフィールを基準として、日別の送信数、有効読み取り数、クリック数を照会します。期間、送信識別子、メッセージタイプなどを設定して照会できます。

### テンプレート統計

テンプレートおよびグループタグを基準に日別送信数、有効開封数、クリック数を照会します。期間、メッセージタイプなどを設定して照会できます。

* ブランドメッセージ自由型はグループタグを使用した場合のみ提供します。

### お知らせトーク送信統計照会

<!-- TODO: translate body -->

#### 要請

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/ALIMTALK |

[Path parameter]

| 名前 | 種類 | 説明 |
|---|---|---|
| appKey | String | 固有のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前 | 種類 | 必須 | 説明 |
|---|---|---|---|
| X-Secret-Key | String | O | コンソールで作成できます。 |

[Query parameter]

| 名前 | 種類 | 必須 | 説明 |
|---|---|---|---|
| senderKey | String | O | 発信キー |
| periodType | String | O | 統計区分（DAILY：日別、MONTHLY：月別） |
| startDate | String | O | 照会開始日付<br/>DAILY：yyyy-MM-dd（最近90日以内）、MONTHLY：yyyy-MM（最近3か月以内） |
| endDate | String | O | 照会終了日付<br/>DAILY：yyyy-MM-dd（最大範囲90日）、MONTHLY：yyyy-MM（最大範囲3か月） |
| messageType | String | X | メッセージタイプ（AT：一般お知らせトーク、AI：画像お知らせトーク） |
| receiveUserType | String | X | 受信者タイプ（PhoneNumber：電話番号、None：受信者識別子なし） |
| limit | Integer | X | 照会件数（Default：500、Max：1000） |
| offset | Integer | X | 開始位置（Default：0） |

#### レスポンス

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

| 名前 | タイプ | Not Null | 説明 |
|---|---|:---:|---|
| header | Object | O | ヘッダー領域 |
| - resultCode | Integer | O | 結果コード |
| - resultMessage | String | O | 結果メッセージ |
| - isSuccessful | Boolean | O | 成功可否 |
| totalCount | Integer | O | 総数 |
| alimtalkDeliveryStatistics | List | O | お知らせトーク送信統計リスト |
| - date | String | O | 日付 |
| - messageType | String | O | メッセージタイプ(AT: 一般お知らせトーク、AI: 画像お知らせトーク) |
| - receiveUserType | String | O | 受信者タイプ(PhoneNumber: 電話番号、None: 受信者識別子なし) |
| - totalSendRequestCount | Integer | O | 総送信リクエスト数 |
| - validSendRequestCount | Integer | O | 有効送信リクエスト数 |
| - validReadCount | Integer | O | 有効閲覧数 |

### お知らせトークテンプレート統計照会

<!-- TODO: translate body -->

#### 要請

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/ALIMTALK |

[パスパラメータ]

| 名前 | タイプ | 説明 |
|---|---|---|
| appKey | String | 固有のアプリキー |

[ヘッダー]

```
{
  "X-Secret-Key": String
}
```

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| X-Secret-Key | String | O | コンソールで作成できます。 |

[クエリパラメータ]

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| senderKey | String | O | 発信キー |
| periodType | String | O | 統計区分（DAILY: 日別、MONTHLY: 月別） |
| startDate | String | O | 照会開始日付<br/>DAILY: yyyy-MM-dd（最近 90 日以内）、MONTHLY: yyyy-MM（最近 3 か月以内） |
| endDate | String | O | 照会終了日付<br/>DAILY: yyyy-MM-dd（最大範囲 90 日）、MONTHLY: yyyy-MM（最大範囲 3 か月） |
| kakaoTemplateCode | String | X | カカオテンプレートコード |
| messageType | String | X | メッセージタイプ（AT: 一般お知らせトーク、AI: 画像お知らせトーク） |
| limit | Integer | X | 照会件数（Default: 500、Max: 1000） |
| offset | Integer | X | 開始位置（Default: 0） |

#### レスポンス

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

| 名前 | タイプ | Not Null | 説明 |
|---|---|:---:|---|
| header | Object | O | ヘッダー領域 |
| - resultCode | Integer | O | 結果コード |
| - resultMessage | String | O | 結果メッセージ |
| - isSuccessful | Boolean | O | 成功可否 |
| totalCount | Integer | O | 総個数 |
| alimtalkTemplateStatistics | List | O | お知らせトークテンプレート統計リスト |
| - date | String | O | 日付 |
| - messageType | String | O | メッセージタイプ（AT: 一般お知らせトーク、AI: 画像お知らせトーク） |
| - templateCode | String | O | テンプレートコード |
| - totalSendSuccessCount | Integer | O | 総送信成功数 |
| - validReadCount | Integer | O | 有効閲覧数 |
| - totalClickCount | Integer | O | 総クリック数 |

### ブランドメッセージ送信統計照会

<!-- TODO: translate body -->

#### 要請

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/BRANDMESSAGE |

[Path parameter]

| 名前 | 種類 | 説明 |
|---|---|---|
| appKey | String | 一意のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前 | 種類 | 必須 | 説明 |
|---|---|---|---|
| X-Secret-Key | String | O | コンソールで作成できます。 |

[Query parameter]

| 名前 | 種類 | 必須 | 説明 |
|---|---|---|---|
| senderKey | String | O | 発信キー |
| periodType | String | O | 統計区分（DAILY: 日別、MONTHLY: 月別） |
| startDate | String | O | 照会開始日<br/>DAILY: yyyy-MM-dd（最近90日以内）、MONTHLY: yyyy-MM（最近3か月以内） |
| endDate | String | O | 照会終了日<br/>DAILY: yyyy-MM-dd（最大範囲90日）、MONTHLY: yyyy-MM（最大範囲3か月） |
| messageSpec | String | X | メッセージスペック（BASIC: 基本型、FREESTYLE: 自由型） |
| chatBubbleType | String | X | 吹き出しタイプ（TEXT: テキスト型、IMAGE: 画像型、WIDE: ワイド画像型、WIDE_ITEM_LIST: ワイドアイテムリストタイプ、CAROUSEL_FEED: カルーセルフィード型、PREMIUM_VIDEO: プレミアム動画型、COMMERCE: コマース型、CAROUSEL_COMMERCE: カルーセルコマース型） |
| targeting | String | X | ターゲティング（M: マーケティング受信同意ユーザー全体、N: チャンネルフレンド除外、I: チャンネルフレンドのみ、F: チャンネルフレンド全体） |
| friendType | String | X | フレンドタイプ（F: フレンド、N: 非フレンド） |
| receiveUserType | String | X | 受信者タイプ（PhoneNumber: 電話番号、None: 受信者識別子なし） |
| limit | Integer | X | 照会件数（Default: 500、Max: 1000） |
| offset | Integer | X | 開始位置（Default: 0） |

#### レスポンス

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

| 名前 | 種類 | Not Null | 説明 |
|---|---|:---:|---|
| header | Object | O | ヘッダー領域 |
| - resultCode | Integer | O | 結果コード |
| - resultMessage | String | O | 結果メッセージ |
| - isSuccessful | Boolean | O | 成功可否 |
| totalCount | Integer | O | 総数 |
| brandmessageDeliveryStatistics | List | O | ブランドメッセージ送信統計リスト |
| - date | String | O | 日付 |
| - messageSpec | String | O | メッセージスペック（BASIC: 基本型、FREESTYLE: 自由型） |
| - chatBubbleType | String | O | 吹き出しタイプ（TEXT: テキスト型、IMAGE: 画像型、WIDE: ワイド画像型、WIDE_ITEM_LIST: ワイドアイテムリストタイプ、CAROUSEL_FEED: カルーセルフィードタイプ、PREMIUM_VIDEO: プレミアム動画型、COMMERCE: コマースタイプ、CAROUSEL_COMMERCE: カルーセルコマースタイプ） |
| - targeting | String | O | ターゲティング（M: マーケティング受信同意ユーザー全体、N: チャンネルフレンド除外、I: チャンネルフレンドのみ、F: チャンネルフレンド全体） |
| - friendType | String | O | フレンドタイプ（F: フレンド、N: 非フレンド） |
| - receiveUserType | String | O | 受信者タイプ（PhoneNumber: 電話番号、None: 受信者識別子なし） |
| - totalSendRequestCount | Integer | O | 総送信リクエスト数 |
| - validSendRequestCount | Integer | O | 有効送信リクエスト数 |
| - validReadCount | Integer | O | 有効閲覧数 |
| - totalClickCount | Integer | O | 総クリック数 |

### ブランドメッセージテンプレート統計照会

<!-- TODO: translate body -->

#### 要請

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/BRANDMESSAGE |

[Path parameter]

| 名前 | タイプ | 説明 |
|---|---|---|
| appKey | String | 一意のアプリキー |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| X-Secret-Key | String | O | コンソールで作成できます。 |

[Query parameter]

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| senderKey | String | O | 発信キー |
| periodType | String | O | 統計区分（DAILY: 日別、MONTHLY: 月別） |
| startDate | String | O | 照会開始日付<br/>DAILY: yyyy-MM-dd（直近90日以内）、MONTHLY: yyyy-MM（直近3ヶ月以内） |
| endDate | String | O | 照会終了日付<br/>DAILY: yyyy-MM-dd（最大範囲90日）、MONTHLY: yyyy-MM（最大範囲3ヶ月） |
| kakaoTemplateCode | String | X | カカオテンプレートコード |
| groupTagKey | String | X | グループタグキー |
| messageSpec | String | X | メッセージスペック（BASIC: 基本型、FREESTYLE: 自由型） |
| chatBubbleType | String | X | 吹き出しタイプ（TEXT: テキスト型、IMAGE: 画像型、WIDE: ワイド画像型、WIDE_ITEM_LIST: ワイドアイテムリスト型、CAROUSEL_FEED: カルーセルフィード型、PREMIUM_VIDEO: プレミアム動画型、COMMERCE: コマース型、CAROUSEL_COMMERCE: カルーセルコマース型） |
| targeting | String | X | ターゲティング（M: マーケティング受信同意ユーザー全体、N: チャンネルフレンド除外、I: チャンネルフレンドのみ、F: チャンネルフレンド全体） |
| friendType | String | X | フレンドタイプ（F: フレンド、N: 非フレンド） |
| limit | Integer | X | 照会件数（Default: 500、Max: 1000） |
| offset | Integer | X | 開始位置（Default: 0） |

#### レスポンス

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

| 名前 | タイプ | Not Null | 説明 |
|---|---|:---:|---|
| header | Object | O | ヘッダー領域 |
| - resultCode | Integer | O | 結果コード |
| - resultMessage | String | O | 結果メッセージ |
| - isSuccessful | Boolean | O | 成功可否 |
| totalCount | Integer | O | 総件数 |
| brandmessageTemplateStatistics | List | O | ブランドメッセージテンプレート統計リスト |
| - date | String | O | 日付 |
| - templateCode | String | O | テンプレートコード |
| - groupTagKey | String | X | グループタグキー |
| - messageSpec | String | O | メッセージ仕様（BASIC：基本型、FREESTYLE：自由型） |
| - chatBubbleType | String | O | チャットバブルタイプ（TEXT：テキスト型、IMAGE：画像型、WIDE：ワイド画像型、WIDE_ITEM_LIST：ワイドアイテムリスト型、CAROUSEL_FEED：カルーセルフィード型、PREMIUM_VIDEO：プレミアムビデオ型、COMMERCE：コマース型、CAROUSEL_COMMERCE：カルーセルコマース型） |
| - targeting | String | O | ターゲティング（M：マーケティング受信同意ユーザー全体、N：チャンネルフレンド除外、I：チャンネルフレンドのみ、F：チャンネルフレンド全体） |
| - friendType | String | O | フレンドタイプ（F：フレンド、N：非フレンド） |
| - totalSendSuccessCount | Integer | O | 総送信成功件数 |
| - validReadCount | Integer | O | 有効閲覧件数 |
| - totalClickCount | Integer | O | 総クリック件数 |

