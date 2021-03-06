---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 게이트웨이 역할의 액세스 레벨

다음 표에서는 각 게이트웨이 역할의 액세스 레벨을 보여줍니다.

이 표에는 다음의 액세스 레벨이 표시되어 있습니다.
- [디바이스 오퍼레이션](#gateway-device-ops)
- [로그 오퍼레이션](#gateway-log-ops)
- [캐시 오퍼레이션](#gateway-cache-ops)
<!-- [Historian Operations](#gateway-historian) -->
- [조직 오퍼레이션](#gateway-org-ops)
- [액세스 제어 오퍼레이션](#gateway-access-ops)
- [분석 오퍼레이션](#gateway-analytics-ops)
- [서드파티 오퍼레이션](#gateway-third-party)  
<!-- - [Risk Management Operations](#gateway-risk-mgt) -->

## 게이트웨이 역할
{: #gateway-roles}

### 디바이스 오퍼레이션 {: #gateway-device-ops}

디바이스 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
디바이스 작성, 업데이트 또는 삭제|-|X
디바이스 보기|X|X
디바이스 활성화|-|X
이벤트 공개|X|X
이벤트 구독|-|-
명령 공개|-|-
명령 구독|X|X
디바이스 관리 조치 시작|X|X
디바이스 관리 조치 보기|X|X
디바이스 관리 조치 지우기|-|-
디바이스 관리 조치 번들 관리|-|X
디바이스 유형 작성, 업데이트 또는 삭제|-|-
디바이스 유형 보기|X|X
진단 로그 관리|-|-
진단 로그 보기|-|-

### 로그 오퍼레이션 {: #gateway-log-ops}

로그 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
서버 로그 보기|-|-

### 캐시 오퍼레이션 {: #gateway-cache-ops}

캐시 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
라이브 데이터 보기(이벤트 캐시)|-|-
라이브 데이터 관리(이벤트 캐시)|-|-


### 조직 오퍼레이션 {: #gateway-org-ops}

조직 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
스토리지 매개변수 구성|-|-
인증 제공업체 구성|-|-
메일 구성 작성, 보기, 업데이트 또는 삭제|-|-
사용 가능한 메일 제공업체 보기|-|-
메일 템플리트 작성, 보기, 업데이트 또는 삭제|-|-
사용자 작성, 업데이트 또는 삭제|-|-
사용자 보기|-|-
사용자 초대 작성, 업데이트, 삭제|-|-
사용자 초대 보기|-|-
초대 완료|-|-
API 키 작성, 업데이트 또는 삭제|-|-
API 키 보기|-|-
조직 사용 정보 보기|-|-

### 액세스 제어 오퍼레이션 {: #gateway-access-ops}

액세스 제어 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
사용자 특성 보기(액세스 권한 포함)|-|-
사용자 고유 특성 보기(액세스 권한 포함)|-|-
사용자 관리(액세스 권한 포함)|-|-
API 키 특성 보기(액세스 권한 포함)|-|-
API 키 고유 특성 보기(액세스 권한 포함)|-|-
API 키 작성, 업데이트 또는 삭제(액세스 권한 포함)|-|-
디바이스 특성 보기(액세스 권한 포함)|X|X
디바이스의 고유 특성 보기(액세스 권한 포함)|X|X
디바이스 작성, 업데이트, 삭제(액세스 권한 포함)|-|X
역할 보기|-|-
사용자 정의 역할 작성, 업데이트, 삭제|-|-
오퍼레이션 보기|-|-

### 분석 오퍼레이션 {: #gateway-analytics-ops}

분석 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------|
           |**표준 게이트웨이** |**권한 부여된 게이트웨이** |
분석 규칙 보기|-|-
분석 규칙 관리|-|-
분석 조치 보기|-|-
분석 조치 관리|-|-
분석 경보 보기|-|-
분석 메시지 스키마 보기|-|-
분석 메시지 스키마 관리|-|-

### 서드파티 서비스 오퍼레이션 {: #gateway-third-party}

서드파티 서비스 오퍼레이션 ||게이트웨이 역할|
:--------: | ---------------------|------------------------
           |**표준 게이트웨이** |**권한 부여된 게이트웨이**
외부 플랫폼에서 일괄처리 알림 처리|-|-
일괄처리 알림을 처리하고 외부 플랫폼으로 전송|-|-
디바이스에 대한 이벤트 공개|-|-
디바이스의 이벤트 구독|-|-
외부 플랫폼에 대한 콜백 URL 설정|-|-
외부 플랫폼의 구독 레벨 설정|-|-
커넥터의 상태 가져오기|-|-
외부 시스템의 작동 여부 확인 및 인증 정보의 유효성 검증|-|-
