---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-27"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 보안
{: #sec-index}

클라우드에서 호스트되는 서비스로서 {{site.data.keyword.iot_full}}에는 아키텍처의 중요한 요소로 보안이 임베드되어 있습니다.
{: shortdesc}

다음 문서에서는 특정 영역에 초점을 맞춰 조직 데이터 보호 방식에 대한 몇 가지 일반 질문에 응답합니다.

* 규제 준수: 보안을 위한 기준을 설정하는 외부 표준입니다.
* 인증: 조직의 정보에 액세스하는 사용자, 디바이스 또는 애플리케이션의 ID를 보증합니다.
* 권한: 사용자, 디바이스 및 애플리케이션이 조직의 정보에 액세스하는 데 필요한 권한을 가지고 있음을 보증합니다.
* 암호화: 인증된 당사자만 데이터를 읽을 수 있고 가로채지는 못하게 합니다.

## {{site.data.keyword.iot_short_notm}} 및 {{site.data.keyword.Bluemix_notm}}
{: #iot-bluemix-sec}

{{site.data.keyword.iot_short_notm}}은 {{site.data.keyword.Bluemix_notm}} 플랫폼 내에서 실행되며, 따라서 액세스 및 연결에 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.BluSoftlayer_notm}} 인프라를 둘 다 사용합니다. {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.BluSoftlayer_notm}} 인프라에 대한 의존도 때문에 {{site.data.keyword.Bluemix_notm}} 및 {{site.data.keyword.BluSoftlayer_notm}} 인프라 보안 및 안정성은 {{site.data.keyword.iot_short_notm}}의 사용자에게 매우 중요합니다. 

{{site.data.keyword.Bluemix_notm}}의 보안에 대한 세부사항은 [{{site.data.keyword.Bluemix_notm}} 플랫폼 보안](index.html#platform-security)을 참조하십시오. 

## {{site.data.keyword.iot_short_notm}} 보안 준수
{: #compliance}  
![ISO 27K 아이콘](../../images/icon_iso27k1.png "ISO 27K 아이콘")   
{{site.data.keyword.iot_short_notm}}은 정보 보안 관리 프로세스의 우수 사례를 정의하는 ISO(International Organization for Standardization) 27001 표준에 따라 인증됩니다. ISO 27001 표준은 개별 조직의 요구에 따라 정보 보안 관리 시스템(ISMS)의 설정, 구현 및 문서화를 위한 요구사항과 보안 제어 구현을 위한 요구사항을 지정합니다. ISO 27000 표준 모음은 서면, 구두 또는 전자식 정보의 신뢰성, 무결성 및 가용성 보호를 목적으로 자산의 가치와 위험을 척도화하는 프로세스를 통합한 것입니다.

{{site.data.keyword.iot_short_notm}}은 서드파티 보안 업체에서 감사하며 ISO 27001({{site.data.keyword.iot_short_notm}} ISO 27001:2013 등록 인증)의 모든 요구사항을 만족합니다.


## {{site.data.keyword.iot_short_notm}} 용어
{: #terminology}

![이미지](terminology_platform.svg)


## 조직에서 IoT 정보 관리를 보호하는 방법
{: #secure-org}

브라우저 기반 GUI 및 REST API 앞에는 HTTPS와 DigiCert에서 서명한 인증서가 오므로 진본의 {{site.data.keyword.iot_short_notm}}에 연결 중임을 신뢰할 수 있습니다. 웹 기반 GUI에 대한 액세스는 IBM ID로 인증됩니다. REST API를 사용하려면 GUI를 통해 생성된 API 키가 필요하므로 이 키를 사용하여 조직에 대해 인증된 REST API를 호출할 수 있습니다.

![이미지](management_platform.svg)


## 디바이스 및 애플리케이션 인증 정보에 보안을 설정하는 방법
{: #secure-credentials}

디바이스가 등록되거나 API 키가 생성되면 인증 토큰이 솔트(salt)되고 해시됩니다. 즉, 조직의 인증 정보를 시스템에서 복구할 수 없습니다. {{site.data.keyword.iot_short_notm}}이 손상되는 매우 드문 경우에도 마찬가지입니다.

디바이스 인증 정보 및 API 키는 위태롭게 되는 경우 개별적으로 취소될 수 있습니다.

![이미지](authentication_platform.svg)

## {{site.data.keyword.iot_short_notm}}에 디바이스를 안전하게 연결하는 방법
{: #secure-device-connection}

디바이스가 플랫폼에 추가될 때 생성되는 인증 토큰 및 클라이언트 ID를 사용하여 디바이스가 연결됩니다. MQTT는 여러 플랫폼과 언어 간에 단순 상호 운용성을 지원하기 위해 사용됩니다. {{site.data.keyword.iot_short_notm}}에서는 TLS v1.2를 통한 연결을 지원합니다.

**중요:** 새 조직은 기본적으로 TLS 보안을 사용하여 디바이스가 연결되도록 자동 구성되며, 이로 인해 디바이스는 오직 암호화된 보안 채널을 사용하여 연결될 수 있습니다. 하지만 {{site.data.keyword.iot_short_notm}}은 조직이 TLS 없이 디바이스가 연결 가능하도록 해야 하는 경우도 지원합니다. 예를 들어, 조직은 TLS 지원이 부족한 디바이스 또는 전송을 암호화하거나 복호화하는 데 필요한 처리 전력을 할애할 수 없는 저전력 IoT 디바이스를 사용할 수 있습니다. 조직의 플랜은 이러한 경우에 사용할 수 있는 설정을 판별합니다.

연결 보안 구성 방법에 대한 자세한 정보는 [보안 정책 구성](set_up_policies.html)을 참조하십시오.

![이미지](connectivity_platform.svg)


TLS 및 암호 스위트 요구사항에 대한 자세한 정보는 `Watson IoT Platform에 애플리케이션, 디바이스 및 게이트웨이 연결` 문서의 [TLS 요구사항](connect_devices_apps_gw.html#tls_requirements) 섹션을 참조하십시오.

인증서 및 보안 정책을 사용하여 디바이스 연결 보안을 강화할 수 있습니다. 암호화되지 않은 연결을 허용하고 TLS(Transport Layer Security) 연결만 적용하며 디바이스가 토큰 없이 클라이언트 측 인증서로 인증을 수행할 수 있도록 보안 정책을 설정할 수 있습니다. 블랙리스트는 연결이 허용되지 않는 디바이스를 지정하는 데 사용할 수 있으며, 화이트리스트는 특정 디바이스의 연결을 허용하는 데 사용할 수 있습니다. 보안 강화에 대한 자세한 정보는 [위험 및 보안 관리](RM_security.html)를 참조하십시오.

### 디바이스 및 게이트웨이를 사용 및 사용 안함으로 설정
{: #disable-devices}

**권한 - 디바이스 관리** HTTP API를 사용하여 디바이스가 플랫폼에 직접 연결되지 않도록 하거나 게이트웨이 뒤에서 연결되지 않도록 할 수 있습니다. 예를 들어, 올바르게 작동하지 않으며 스팸으로 인한 원하지 않는 데이터 사용과 같은 문제의 원인이 되는 디바이스나 악의적인 사용자에 대한 연결은 강제로 끊을 수 있습니다. API를 사용하여 디바이스의 현재 연결을 끊고 이 디바이스가 플랫폼에 다시 연결되지 않도록 할 수 있습니다.

디바이스를 사용 또는 사용 안함으로 설정하려면 다음 API를 사용하십시오. 여기서 *${clientId}*는 *d:${orgId}:${typeId}:${deviceId}* 형식(디바이스용) 또는 *g:${orgId}:${typeId}:${deviceId}*(게이트웨이용) 형식의 URL 인코딩 클라이언트 ID입니다.

    PUT /api/v0002/authorization/devices/${clientId}
    
요청 본문에서 0 상태 값을 사용하여 디바이스를 사용 안함으로 설정하거나 1 상태 값을 사용하여 디바이스를 사용으로 설정하십시오. 예를 들어, 다음 상태 값은 디바이스가 사용되지 않음을 표시합니다.

    { "status": 0 }

성공 시 응답 코드는 200입니다. 

게이트웨이가 사용되지 않는 디바이스에 대해 공개하는 경우 응답 코드 180인 오류 알림을 수신합니다. 자세한 정보는 [게이트웨이 알림](../../gateways/mqtt.html#notification)을 참조하십시오. 

API에 대한 자세한 정보는 [디바이스 보안 베타 API(![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){:new_window}를 참조하고, **권한 - 디바이스 관리**로 이동하십시오.

## IoT 디바이스 간 데이터 누출 방지 방법
{: #prevent-leak-devices}

보안 메시징 패턴이 빌드됩니다. 인증되고 나면 디바이스에는 제한된 주제 공간을 공개하고 구독할 수 있는 권한만 부여됩니다.

* '/iot-2/evt/<event_id>/fmt/<format_string>'
* '/iot-2/cmd/<command_id>/fmt/<format_string>'

모든 디바이스가 동일한 주제 공간에 대해 작업합니다. 클라이언트에서 제공하는 인증 인증 정보를 통해 {{site.data.keyword.iot_short_notm}}에서 이 주제 영역의 범위로 지정될 디바이스를 나타냅니다.  이를 수행하면 디바이스가 다른 디바이스를 위장하는 것이 방지됩니다.

다른 디바이스를 위장하는 유일한 방법은 해당 디바이스에 대해 안전을 위협하는 보안 인증 정보를 얻는 것입니다.


![이미지](device_scope_platform.svg)


애플리케이션은 조직의 모든 디바이스에 대한 이벤트 및 명령 주제 모두에서 구독하고 공개할 수 있습니다. 애플리케이션에서 동시에 여러 디바이스의 데이터를 분석할 수 있으며 전이중 통신 루프의 보완 사항을 형성하는 외에도 디바이스를 시뮬레이션하거나 프록시할 수 있습니다.


## 조직 간 IoT 데이터 누출 방지 방법
{: #prevent-leak-org}

디바이스 및 애플리케이션이 작동하는 주제 공간은 단일 조직 내에서 범위가 지정됩니다. 인증되면 {{site.data.keyword.iot_short_notm}}이 클라이언트 인증을 기반으로 조직 ID를 사용하여 주제 구조를 변환하므로 한 조직의 데이터를 다른 조직에서 액세스할 수 없게 됩니다.

![이미지](org_scope_platform.svg)

# 관련 링크
{: #rellinks}
## 관련 링크
{: #general}
* [{{site.data.keyword.iot_short_notm}} 시작하기](https://console.ng.bluemix.net/docs/services/IoT/index.html)
* [{{site.data.keyword.Bluemix_notm}} 보안 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/docs/security/index.html#security){:new_window}
* [{{site.data.keyword.Bluemix_notm}} 플랫폼 보안 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/docs/security/index.html#platform-security){:new_window}
* [{{site.data.keyword.Bluemix_notm}} 규제 준수](https://console.ng.bluemix.net/docs/security/index.html#compliance){:new_window}
* [{{site.data.keyword.BluSoftlayer_notm}} 보안 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.softlayer.com/security){:new_window}
* [{{site.data.keyword.BluSoftlayer_notm}} 준수 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.softlayer.com/compliance){:new_window}
