## Notification > KakaoTalk Bizmessage > Common > API v2.2 Guide

## 통계

### [API 도메인]

<table>
<thead>
<tr>
<th>도메인</th>
</tr>
</thead>
<tbody>
<tr>
<td>https://kakaotalk-bizmessage.api.nhncloudservice.com</td>
</tr>
</tbody>
</table>


### 통계 검색 - 이벤트 기반
* 이벤트 발생 시간 기준으로 수집된 통계입니다.
* 다음 시간 기준으로 통계가 수집됩니다.
    * 요청 개수(REQUESTED): 예약 발송 등록 시간
    * 발송 개수(SENT): 벤더로 발송 시점(예약 발송 시간)
    * 성공 개수(RECEIVED): 발송 결과 성공(수신 시간)
    * 실패 개수(SENT_FAILED): 발송 요청 실패 or 발송 결과 실패 시점
    * 대체 발송 요청 개수(RESENT): 대체 발송 요청 시점
    * 대체 발송 실패 개수(RESENT_FAILED): 대체 발송 요청 실패 시점

### 통계 정보 조회

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats |

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름           | 타입     | 필수 | 설명               |
|--------------|--------|----|------------------|
| X-Secret-Key | String | O  | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름 | 타입 | 최대 길이 | 필수 | 설명 |
|---|---|---|---|---|
| statsType | String | - | 필수 | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:요일별 |
| from | String | - | 필수 | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | 필수 | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | 옵션 | 통계 데이터 조회 시, 트리 형태로 제공할지 설정 |
| extra1s | List<String> | - | 옵션 | 하위 상품 구분<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | 옵션 | senderKey |
| eventTypes | List<String> | - | 옵션 | 이벤트 종류<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | 옵션 | 이벤트 목록(현재 `MESSAGE`만 지원)<br/> MESSAGE |
| templateCodes | List<String> | - | 옵션 | 템플릿 코드 목록(친구톡 미지원) |
| requestIds | List<String> | 5 | 옵션 | 요청 ID 목록 |
| statsIds | List<String> | - | 옵션 | 통계 ID 목록 |
| statsCriteria | List<String> | - | 옵션 | 통계 기준<br/>- EVENT: 이벤트(기본값)<br/>- EXTRA_1, EVENT: 하위 상품 구분, 이벤트<br/>- EXTRA_2, EVENT: senderKey, 이벤트 |

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

### 이벤트별 개수 조회

[URL]

|Http method|	URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/stats/total |

[Path parameter]

| 이름 |	타입|	설명|
|---|---|---|
| appKey | String | 고유의 앱키 |

[Query parameter]

| 이름 | 타입 | 최대 길이 | 필수 | 설명 |
|---|---|---|---|---|
| statsType | String | - | 필수 | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:요일별 |
| from | String | - | 필수 | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss |
| to | String | - | 필수 | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss |
| join | Boolean | - | 옵션 | 통계 데이터 조회 시, 트리 형태로 제공할지 설정 |
| extra1s | List<String> | - | 옵션 | 하위 상품 구분<br/> ALIMTALK, ALIMTALK_AUTH, FRIENDTALK, BRAND_MESSAGE |
| extra2s | List<String> | - | 옵션 | senderKey |
| eventTypes | List<String> | - | 옵션 | 이벤트 종류<br/> REQUESTED, SENT, RECEIVED, SENT_FAILED, RESENT, RESENT_FAILED |
| eventCategory | String | - | 옵션 | 이벤트 목록(현재 `MESSAGE`만 지원)<br/> MESSAGE |
| templateCodes | List<String> | - | 옵션 | 템플릿 코드 목록(친구톡 미지원) |
| requestIds | List<String> | 5 | 옵션 | 요청 ID 목록 |
| statsIds | List<String> | - | 옵션 | 통계 ID 목록 |
| statsCriteria | List<String> | - | 옵션 | 통계 기준<br/>- EVENT: 이벤트(기본값)<br/>- EXTRA_1, EVENT: 하위 상품 구분, 이벤트<br/>- EXTRA_2, EVENT: senderKey, 이벤트 |

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

## 카카오 통계

* 카카오비즈센터에서 제공하는 통계 데이터를 조회합니다.
* 통계 데이터는 발신 키 기준으로 일별(DAILY) 또는 월별(MONTHLY)로 조회할 수 있습니다.
* DAILY: 최근 90일 이내 데이터만 조회 가능하며, 조회 범위는 최대 90일입니다.
* MONTHLY: 최근 3개월 이내 데이터만 조회 가능하며, 조회 범위는 최대 3개월입니다.

발신 프로필 관리에서 **카카오 통계 바로가기**를 클릭하면 새 창에서 카카오 통계를 조회할 수 있습니다. 통계 기준은 발송 통계와 템플릿 통계가 있으며, 메시지 채널에 따라 조회 조건이 달라집니다. 조회 결과를 차트와 표로 확인할 수 있습니다.

* 실시간 통계는 제공하지 않으며, 전날 수집한 데이터를 매일 오전 7시경 제공합니다.
* 알림톡 통계는 D+1에 최초 제공하며, D+2에 확정합니다.
* 유효 읽음 수는 같은 메시지에 대해 중복 집계하지 않습니다.
* 클릭 수는 같은 메시지에 대해 중복 집계합니다.
* 발송 성공 건수가 10건 이하이면 유효 읽음 수와 클릭 수를 제공하지 않습니다.

### 발송 통계

발신 프로필을 기준으로 일별 발송 수, 유효 읽음 수, 클릭 수를 조회합니다. 기간, 발송 식별자, 메시지 타입 등을 설정해 조회할 수 있습니다.

### 템플릿 통계

템플릿 및 그룹 태그를 기준으로 일별 발송 수, 유효 읽음 수, 클릭 수를 조회합니다. 기간, 메시지 타입 등을 설정해 조회할 수 있습니다.

* 브랜드 메시지 자유형은 그룹 태그를 사용한 경우에만 제공합니다.

### 알림톡 발송 통계 조회

#### 요청

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/ALIMTALK |

[Path parameter]

| 이름 | 타입 | 설명 |
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-Secret-Key | String | O | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| senderKey | String | O | 발신 키 |
| periodType | String | O | 통계 구분(DAILY: 일별, MONTHLY: 월별) |
| startDate | String | O | 조회 시작 날짜<br/>DAILY: yyyy-MM-dd(최근 90일 이내), MONTHLY: yyyy-MM(최근 3개월 이내) |
| endDate | String | O | 조회 종료 날짜<br/>DAILY: yyyy-MM-dd(최대 범위 90일), MONTHLY: yyyy-MM(최대 범위 3개월) |
| messageType | String | X | 메시지 유형(AT: 일반 알림톡, AI: 이미지 알림톡) |
| receiveUserType | String | X | 수신자 유형(PhoneNumber: 전화번호, None: 수신자 식별자 없음) |
| limit | Integer | X | 조회 건수(Default: 500, Max: 1000) |
| offset | Integer | X | 시작 위치(Default: 0) |

#### 응답

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

| 이름 | 타입 | Not Null | 설명 |
|---|---|:---:|---|
| header | Object | O | 헤더 영역 |
| - resultCode | Integer | O | 결과 코드 |
| - resultMessage | String | O | 결과 메시지 |
| - isSuccessful | Boolean | O | 성공 여부 |
| totalCount | Integer | O | 총 개수 |
| alimtalkDeliveryStatistics | List | O | 알림톡 발송 통계 리스트 |
| - date | String | O | 날짜 |
| - messageType | String | O | 메시지 유형(AT: 일반 알림톡, AI: 이미지 알림톡) |
| - receiveUserType | String | O | 수신자 유형(PhoneNumber: 전화번호, None: 수신자 식별자 없음) |
| - totalSendRequestCount | Integer | O | 총 발송 요청 수 |
| - validSendRequestCount | Integer | O | 유효 발송 요청 수 |
| - validReadCount | Integer | O | 유효 열람 수 |

### 알림톡 템플릿 통계 조회

#### 요청

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/ALIMTALK |

[Path parameter]

| 이름 | 타입 | 설명 |
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-Secret-Key | String | O | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| senderKey | String | O | 발신 키 |
| periodType | String | O | 통계 구분(DAILY: 일별, MONTHLY: 월별) |
| startDate | String | O | 조회 시작 날짜<br/>DAILY: yyyy-MM-dd(최근 90일 이내), MONTHLY: yyyy-MM(최근 3개월 이내) |
| endDate | String | O | 조회 종료 날짜<br/>DAILY: yyyy-MM-dd(최대 범위 90일), MONTHLY: yyyy-MM(최대 범위 3개월) |
| kakaoTemplateCode | String | X | 카카오 템플릿 코드 |
| messageType | String | X | 메시지 유형(AT: 일반 알림톡, AI: 이미지 알림톡) |
| limit | Integer | X | 조회 건수(Default: 500, Max: 1000) |
| offset | Integer | X | 시작 위치(Default: 0) |

#### 응답

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

| 이름 | 타입 | Not Null | 설명 |
|---|---|:---:|---|
| header | Object | O | 헤더 영역 |
| - resultCode | Integer | O | 결과 코드 |
| - resultMessage | String | O | 결과 메시지 |
| - isSuccessful | Boolean | O | 성공 여부 |
| totalCount | Integer | O | 총 개수 |
| alimtalkTemplateStatistics | List | O | 알림톡 템플릿 통계 리스트 |
| - date | String | O | 날짜 |
| - messageType | String | O | 메시지 유형(AT: 일반 알림톡, AI: 이미지 알림톡) |
| - templateCode | String | O | 템플릿 코드 |
| - totalSendSuccessCount | Integer | O | 총 발송 성공 수 |
| - validReadCount | Integer | O | 유효 열람 수 |
| - totalClickCount | Integer | O | 총 클릭 수 |

### 브랜드 메시지 발송 통계 조회

#### 요청

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/delivery-statistics/BRANDMESSAGE |

[Path parameter]

| 이름 | 타입 | 설명 |
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-Secret-Key | String | O | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| senderKey | String | O | 발신 키 |
| periodType | String | O | 통계 구분(DAILY: 일별, MONTHLY: 월별) |
| startDate | String | O | 조회 시작 날짜<br/>DAILY: yyyy-MM-dd(최근 90일 이내), MONTHLY: yyyy-MM(최근 3개월 이내) |
| endDate | String | O | 조회 종료 날짜<br/>DAILY: yyyy-MM-dd(최대 범위 90일), MONTHLY: yyyy-MM(최대 범위 3개월) |
| messageSpec | String | X | 메시지 스펙(BASIC: 기본형, FREESTYLE: 자유형) |
| chatBubbleType | String | X | 말풍선 유형(TEXT: 텍스트형, IMAGE: 이미지형, WIDE: 와이드 이미지형, WIDE_ITEM_LIST: 와이드 아이템리스트형, CAROUSEL_FEED: 캐러셀 피드형, PREMIUM_VIDEO: 프리미엄 비디오형, COMMERCE: 커머스형, CAROUSEL_COMMERCE: 캐러셀 커머스형) |
| targeting | String | X | 타겟팅(M: 마케팅 수신동의 유저 전체, N: 채널 친구 제외, I: 채널 친구만, F: 채널 친구 전체) |
| friendType | String | X | 친구 유형(F: 친구, N: 비친구) |
| receiveUserType | String | X | 수신자 유형(PhoneNumber: 전화번호, None: 수신자 식별자 없음) |
| limit | Integer | X | 조회 건수(Default: 500, Max: 1000) |
| offset | Integer | X | 시작 위치(Default: 0) |

#### 응답

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

| 이름 | 타입 | Not Null | 설명 |
|---|---|:---:|---|
| header | Object | O | 헤더 영역 |
| - resultCode | Integer | O | 결과 코드 |
| - resultMessage | String | O | 결과 메시지 |
| - isSuccessful | Boolean | O | 성공 여부 |
| totalCount | Integer | O | 총 개수 |
| brandmessageDeliveryStatistics | List | O | 브랜드 메시지 발송 통계 리스트 |
| - date | String | O | 날짜 |
| - messageSpec | String | O | 메시지 스펙(BASIC: 기본형, FREESTYLE: 자유형) |
| - chatBubbleType | String | O | 말풍선 유형(TEXT: 텍스트형, IMAGE: 이미지형, WIDE: 와이드 이미지형, WIDE_ITEM_LIST: 와이드 아이템리스트형, CAROUSEL_FEED: 캐러셀 피드형, PREMIUM_VIDEO: 프리미엄 비디오형, COMMERCE: 커머스형, CAROUSEL_COMMERCE: 캐러셀 커머스형) |
| - targeting | String | O | 타겟팅(M: 마케팅 수신동의 유저 전체, N: 채널 친구 제외, I: 채널 친구만, F: 채널 친구 전체) |
| - friendType | String | O | 친구 유형(F: 친구, N: 비친구) |
| - receiveUserType | String | O | 수신자 유형(PhoneNumber: 전화번호, None: 수신자 식별자 없음) |
| - totalSendRequestCount | Integer | O | 총 발송 요청 수 |
| - validSendRequestCount | Integer | O | 유효 발송 요청 수 |
| - validReadCount | Integer | O | 유효 열람 수 |
| - totalClickCount | Integer | O | 총 클릭 수 |

### 브랜드 메시지 템플릿 통계 조회

#### 요청

[URL]

|Http method| URI|
|---|---|
|GET| /common/v2.2/appkeys/{appKey}/kakao-statistics/template-statistics/BRANDMESSAGE |

[Path parameter]

| 이름 | 타입 | 설명 |
|---|---|---|
| appKey | String | 고유의 앱키 |

[Header]

```
{
  "X-Secret-Key": String
}
```

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| X-Secret-Key | String | O | 콘솔에서 생성할 수 있습니다. |

[Query parameter]

| 이름 | 타입 | 필수 | 설명 |
|---|---|---|---|
| senderKey | String | O | 발신 키 |
| periodType | String | O | 통계 구분(DAILY: 일별, MONTHLY: 월별) |
| startDate | String | O | 조회 시작 날짜<br/>DAILY: yyyy-MM-dd(최근 90일 이내), MONTHLY: yyyy-MM(최근 3개월 이내) |
| endDate | String | O | 조회 종료 날짜<br/>DAILY: yyyy-MM-dd(최대 범위 90일), MONTHLY: yyyy-MM(최대 범위 3개월) |
| kakaoTemplateCode | String | X | 카카오 템플릿 코드 |
| groupTagKey | String | X | 그룹 태그 키 |
| messageSpec | String | X | 메시지 스펙(BASIC: 기본형, FREESTYLE: 자유형) |
| chatBubbleType | String | X | 말풍선 유형(TEXT: 텍스트형, IMAGE: 이미지형, WIDE: 와이드 이미지형, WIDE_ITEM_LIST: 와이드 아이템리스트형, CAROUSEL_FEED: 캐러셀 피드형, PREMIUM_VIDEO: 프리미엄 비디오형, COMMERCE: 커머스형, CAROUSEL_COMMERCE: 캐러셀 커머스형) |
| targeting | String | X | 타겟팅(M: 마케팅 수신동의 유저 전체, N: 채널 친구 제외, I: 채널 친구만, F: 채널 친구 전체) |
| friendType | String | X | 친구 유형(F: 친구, N: 비친구) |
| limit | Integer | X | 조회 건수(Default: 500, Max: 1000) |
| offset | Integer | X | 시작 위치(Default: 0) |

#### 응답

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

| 이름 | 타입 | Not Null | 설명 |
|---|---|:---:|---|
| header | Object | O | 헤더 영역 |
| - resultCode | Integer | O | 결과 코드 |
| - resultMessage | String | O | 결과 메시지 |
| - isSuccessful | Boolean | O | 성공 여부 |
| totalCount | Integer | O | 총 개수 |
| brandmessageTemplateStatistics | List | O | 브랜드 메시지 템플릿 통계 리스트 |
| - date | String | O | 날짜 |
| - templateCode | String | O | 템플릿 코드 |
| - groupTagKey | String | X | 그룹 태그 키 |
| - messageSpec | String | O | 메시지 스펙(BASIC: 기본형, FREESTYLE: 자유형) |
| - chatBubbleType | String | O | 말풍선 유형(TEXT: 텍스트형, IMAGE: 이미지형, WIDE: 와이드 이미지형, WIDE_ITEM_LIST: 와이드 아이템리스트형, CAROUSEL_FEED: 캐러셀 피드형, PREMIUM_VIDEO: 프리미엄 비디오형, COMMERCE: 커머스형, CAROUSEL_COMMERCE: 캐러셀 커머스형) |
| - targeting | String | O | 타겟팅(M: 마케팅 수신동의 유저 전체, N: 채널 친구 제외, I: 채널 친구만, F: 채널 친구 전체) |
| - friendType | String | O | 친구 유형(F: 친구, N: 비친구) |
| - totalSendSuccessCount | Integer | O | 총 발송 성공 수 |
| - validReadCount | Integer | O | 유효 열람 수 |
| - totalClickCount | Integer | O | 총 클릭 수 |
