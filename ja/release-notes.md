## Notification > KakaoTalk Bizmessage > Release Notes
### 2018.06.26
#### 기능 개선
* [Console] 플러스친구 등록 시, 요청 필드 추가
    - 카카오 발급절차 강화로 인해 사업자등록번호, 사업자 카테고리를 입력받도록 수정되었습니다.
    
#### 버그 수정
* [Console] 통계 차트 버그 개선 버전으로 변경
    - IE 브라우져에서 Export xls 기능 버그 개선 버전으로 변경하였습니다.

### 2018.05.29
#### 기능 추가
* [API] 발송 결과 단건 조회 API 추가
    - 특정 발송 결과를 조회할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[API 가이드](./api-guide/#_9)] 를 참고하시기 바랍니다.
* [API] 발송 결과 리스트 조회 API v1.1 추가
    - 응답에 recipientSeq (수신자 시퀀스) 필드가 추가되면서, v1.1 API가 추가되었습니다.

#### 버그 수정
* [API] 치환 발송 API 버그 수정
    - Request Body의 templateParameter 필드가 필수 값으로 인식되는 버그가 개선되었습니다.

### 2018.04.24
#### 기능 추가
* [Console] 템플릿 챗버블 기능 추가
    - 다중 버튼 템플릿 기능이 추가되었습니다.
    - 버튼 타입 추가되었습니다. ( 배송 조회, 웹 링크, 앱 링크, 봇 키워드, 메세지 전달 )
* [API] 템플릿 조회 API 추가
    - 템플릿을 조회할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[API 가이드](./api-guide/#_12)] 를 참고하시기 바랍니다.

#### 기능 개선/변경
* [API] 발송 실패 재발송 기능 수정
    - 본문 길이에 따라 SMS/LMS로 구분지어 발송하도록 수정되었습니다.
    - 재발송 본문 내용이 수정되었습니다. ( 본문 + 웹 링크 버튼명 + 버튼 링크 )

### 2018.03.22
#### 기능 추가
* [Console] 템플릿 수정, 삭제, 문의 등록 기능 추가
    - 템플릿 수정, 삭제 기능이 추가되었습니다.
    - 검수 담당자에게 문의를 등록 기능이 추가되었습니다.
    - 자세한 사항은 [[콘솔 사용 가이드](./console-guide/#_19)] 를 참고하시기 바랍니다.
* [API] 전문 발송 API 추가
    - 치환 데이터가 아닌 전문 내용으로 발송할 수 있는 API가 추가되었습니다.
    - 자세한 사항은 [[API 가이드](./api-guide/#_3)] 를 참고하시기 바랍니다.

### 2018.02.22
#### 기능 추가
* [Console] 대량 발송 기능 추가
    - csv 파일을 이용하여, 대량 발송 기능이 추가되었습니다.

### 2018.01.25
#### 기능 개선/변경
* [Console] 템플릿 등록 개선
    - 버튼명 앞뒤 공백 제거 기능이 추가 되었습니다.
    - 길이 제한 수정되었습니다. ( 템플릿 코드: 10자 -> 20자, 버튼명: 10자 -> 14자)
    - 배송 조회 템플릿 등록 시, 버튼명을 직접 입력할 수 있게 수정되었습니다.

#### 기능 추가
* [API] 발송 결과 조회 API 추가

### 2017.11.23
#### 기능 추가
* [Console] 다중 플러스친구 등록
    - 1개의 플러스친구 등록 구조에서 다중 플러스친구 구조로 변경되었습니다.
* [Console] 템플릿 반려 시, 템플릿 상세보기에서 반려 사유 제공.
* [API] 다중 플러스친구 적용에 따른 발송 API 필드 추가
    - requestBody에 plusFriendId 필드12가 추가되었습니다.
    - plusFriendId 필드를 입력하지 않을 경우, 처음 등록된 플러스친구 아이디로 발송됩니다.

### 2017.08.24
#### 기능 추가
* [Console] 알림톡 발송 통계 화면 제공
    - 날짜별/시간대별/요일별 통계 화면이 제공됩니다.
    - 발송한 일자와 템플릿으로 조회할 수 있습니다.

#### 기능 개선/변경
* [Console] 자유버튼 템플릿 등록 시 URL 검증 변경
    - 자유버튼에 URL 등록 시, http:// or https:// 가 필수로 포함 -> #{url}과 같이 템플릿 치환자도 등록할 수 있게 변경하였습니다.
    - 템플릿 치환자 #{url} 형식의 템플릿 치환자가 아닐 경우 http:// or https:// 검증은 유지됩니다.
* [API] Content-type 에러 응답 메세지 수정
    - 요청 header에 Content-type: application/json이 아닐 경우 실패 응답 메세지 수정되었습니다.

### 2017.07.20
#### 기능 추가
* [Console] 알림톡 발송 결과 조회 추가
    - 발신 일시, 수신번호, 템플릿 등의 조건으로 발신한 메세지의 전송 결과를 조회할 수 있습니다.

### 2017.06.22
#### 기능 개선/변경
* [Console] 발신프로필 관리 페이지 추가
* [Console] 테스트 발송 페이지 추가
* [Console] 템플릿 등록 조회 페이지 추가

#### 기능 추가
* [Console] 발송 실패 설정 추가
    - 알림톡 발송 실패 시, LMS로 대체 발송하는 기능
* [Console] 템플릿 자유 버튼 타입에 링크 치환 기능
    - 템플릿 자유 버튼 타입일 경우, 버튼 링크를 치환자로 등록 가능. ex) buttonURL: #{url}


### 2017.05.25
#### 기능 개선/변경
* [Console] 메인페이지 마크업 개선

### 2017.04.20
#### 신규 상품 출시
* Alimtalk은 휴대폰 번호를 기반으로 친구 추가 없이 카카오톡 사용자에게 배송, 예약 시간 등의 정보성 메시지를 발송할 수 있는 상품입니다.
* 손쉬운 연동을 위한 RESTful API를 제공합니다.