---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 단계별 안내서 1: 공통 인터페이스를 통한 디바이스 관련 작업 방법에 대한 자세한 예제
{: #scenario}

다음 정보를 사용하여 두 개의 온도 디바이스가 {{site.data.keyword.iot_full}}에 이벤트를 공개하는 시나리오를 작성할 수 있습니다. 하나의 디바이스는 섭씨로 온도를 측정합니다. 다른 디바이스는 화씨로 온도를 측정합니다. 이러한 측정값은 섭씨인 하나의 온도 측정값으로 맵핑됩니다. 새 온도 측정값이 이러한 디바이스에 의해 공개되는 경우, 디바이스 상태와 연관된 특성의 값이 변경됩니다.
{: shortdesc}

## 시작하기 전에

[리소스](ga_im_definitions.html#definitions_resources)를 작성할 때 해당 리소스는 드래프트 버전으로 작성됩니다. 드래프트 버전은 API를 사용하여 직접 조회하고 업데이트하고 삭제할 수 있는 리소스의 작업 사본입니다. REST API를 사용할 때는 **draft/** 접두부가 드래프트 상태에 있는 리소스를 식별하는 데 사용됩니다.  

리소스의 드래프트 및 활성 버전에 대한 자세한 정보는 [데이터 관리 이해](ga_im_definitions.html)를 참조하십시오. 

## 전제조건

요청을 인증하려면 {{site.data.keyword.iot_short_notm}} [조직 인스턴스](../iotplatform_overview.html#organizations) 및 해당 조직에 대한 API 키와 인증 토큰이 있어야 합니다.  

API 키 생성에 대한 정보는 [API](../reference/api.html) 문서를 참조하십시오. API 토큰 생성에 대한 자세한 정보는 [시작하기 튜토리얼](../getting-started.html) 문서를 참조하십시오. 


## 이 태스크에 관한 정보

이 시나리오에서는 두 개의 디바이스가 작성됩니다. 

하나의 디바이스를 *tSensor*라고 합니다. 이 디바이스는 섭씨로 측정되는 온도 이벤트를 공개합니다. 온도 이벤트는 주제 `iot-2/evt/tevt/fmt/json`에서 공개되며 다음 예제 페이로드를 갖습니다.
```
{
  "t" : 34.5
}
```

**참고:** 이벤트 ID는 *tevt*입니다. 이 ID는 이 유형의 온도 이벤트를 실제 인터페이스에 추가할 때와 이 유형의 인바운드 이벤트와 연관된 특성을 논리 인터페이스의 특성에 맵핑하는 맵핑을 정의할 때 필요합니다. 이 시나리오에서는 논리 인터페이스에 정의된 특성을 **temperature**라고 합니다.

다른 디바이스를 *tempSensor*라고 합니다. 이 디바이스는 화씨로 측정되는 온도 이벤트를 공개합니다. 온도 이벤트는 주제 `iot-2/evt/tempevt/fmt/json`에서 공개되며 다음 예제 페이로드를 갖습니다.
```
{
  "temp" : 72.55
}
```

**참고:** 이벤트 ID는 *tempevt*입니다. 이 ID는 이 유형의 온도 이벤트를 실제 인터페이스에 추가할 때와 이 유형의 인바운드 이벤트와 연관된 특성을 논리 인터페이스의 특성에 맵핑하는 맵핑을 정의할 때 필요합니다. 이 시나리오에서는 논리 인터페이스에 정의된 특성을 **temperature**라고 합니다.

논리 인터페이스도 구성됩니다. 이 논리 인터페이스는 다음 구조로 이 유형의 디바이스에 대한 상태를 표시합니다.
```
{
  "temperature" : <current temperature value in Celsius>
}
```
이 구성은 해당 값을 섭씨로 변환한 후에 **t**와 연관된 값을 처리하고 **temp**와 연관된 값을 처리하는 애플리케이션을 구성하는 대신에 **temperature**와 연관된 값을 처리하는 애플리케이션을 구성할 수 있음을 의미합니다.

![ {{site.data.keyword.iot_short_notm}}에서 온도 센서 디바이스 및 애플리케이션 간의 맵핑.](../information_management/images/Information)  

다음 예제 시나리오를 사용하여 자체 인터페이스 환경을 설정할 수 있습니다.

**중요 참고:** 기타 태스크를 완료하는 데 ID가 필요하므로 curl 응답에서 생성된 ID를 저장해야 합니다.
이 안내서에서 사용되는 리소스 특성 이름, 값 및 ID를 나열하는 표는 [단계별 안내서 1 및 2 - 리소스 이름 및 ID의 추가 정보](../information_management/im_id_reference.html)에 설명되어 있습니다. 

## 필요하면 디바이스 유형 및 디바이스 추가
{: #step14}

이 시나리오에서는 두 개의 디바이스 유형과 두 개의 디바이스 인스턴스를 가정합니다. 디바이스 인스턴스 *tSensor*는 디바이스 유형 *TSensor*와 연관되어 있습니다. 디바이스 인스턴스 *tempSensor*는 디바이스 유형 *TempSensor*와 연관되어 있습니다.  

[{{site.data.keyword.iot_short_notm}} 대시보드 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://internetofthings.ibmcloud.com){: new_window}를 사용하거나 REST API를 사용하여 디바이스 유형 및 디바이스를 작성할 수 있습니다. {{site.data.keyword.iot_short_notm}} 대시보드를 사용한 디바이스 유형 및 디바이스 추가에 대한 자세한 정보는 [웹 인터페이스를 사용하여 데이터 관리 시작하기](im_ui_flow.html) 문서를 참조하십시오. 

다음 예제는 REST API를 사용하여 *TSensor*라고 하는 디바이스 유형을 작성하는 방법을 표시합니다. 

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 다음 예제는 REST API를 사용하여 *TempSensor*라고 하는 디바이스 유형을 작성하는 방법을 표시합니다. 

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**참고:** 디바이스 유형 및 디바이스를 작성할 때 메타데이터를 추가할 수 있습니다. 이 시나리오에서는 다음 메타데이터가 디바이스 유형 *TSensor*에 추가됩니다. 
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
이 메타데이터는 디바이스 상태의 *temperature* 특성이 섭씨 44도를 초과하도록 하는 온도 이벤트를 *tSensor* 디바이스에서 {{site.data.keyword.iot_short_notm}}이 수신할 때 트리거되는 [규칙](../information_management/im_rules.html)을 작성할 때 사용됩니다.  


그리고 사용자는 디바이스 유형과 연관된 디바이스 인스턴스를 등록해야 합니다. 다음 예제는 REST API를 사용하여 디바이스 유형 *TSensor*와 연관된 *tSensor*라고 하는 디바이스 인스턴스를 등록하는 방법을 표시합니다. 
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

다음 예제는 REST API를 사용하여 디바이스 유형 *TempSensor*와 연관된 *tempSensor*라고 하는 디바이스 인스턴스를 등록하는 방법을 표시합니다. 
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

REST API를 사용한 디바이스 유형 및 디바이스의 추가에 대한 정보는 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window} 문서를 참조하십시오. 

**참고:** 디바이스가 Watson IoT Platform HTTP REST API를 통해 HTTP 요청을 작성하는 경우에는 사용자 이름과 비밀번호가 필요합니다. 비밀번호는 디바이스가 등록될 때 자동으로 생성되거나 수동으로 지정된 인증 토큰의 값입니다. MQTT 클라이언트를 사용하는 경우, 주제 문자열을 구독하여 디바이스 또는 사물 상태를 검색하려면 토큰이 필요하므로 디바이스의 인증 토큰을 기록해 두어야 합니다. 

## 1단계: 이벤트 스키마 파일 작성
{: #step1}

이 시나리오의 경우에는 각각의 인바운드 온도 이벤트의 구조를 정의하는 두 개의 이벤트 스키마 파일을 작성하십시오.

다음 예제는 *tEventSchema.json*이라고 하는 스키마 파일을 작성하는 방법을 표시합니다. 이 파일은 섭씨로 온도를 측정하는 온도 디바이스의 인바운드 이벤트에 대한 구조를 정의합니다. 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**팁:** **필수** 매개변수를 사용하여 하나 이상의 특성을 필수로 표시할 수 있습니다. {{site.data.keyword.iot_short_notm}}이 디바이스 데이터로 디바이스 상태를 업데이트할 수 있도록 필수 특성이 디바이스 메시지에 포함되어야 합니다. 필수 특성이 포함되지 않은 메시지는 처리되지 않습니다.   

스키마 파일 이름 *tEventSchema*는 이벤트 유형에 대한 이벤트 스키마 리소스를 작성할 때 사용됩니다.

다음 예제는 *tempEventSchema.json*이라고 하는 스키마 파일을 작성하는 방법을 표시합니다. 이 파일은 화씨로 온도를 측정하는 온도 디바이스의 인바운드 이벤트에 대한 구조를 정의합니다. 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
스키마 파일 이름 *tempEventSchema*는 이벤트 유형에 대한 이벤트 스키마 리소스를 작성할 때 사용됩니다.   

## 2단계: 이벤트 유형에 대한 이벤트 스키마 리소스 작성
{: #step2}

이벤트 스키마 리소스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/schemas
```

스키마 정의 파일은 다중 파트 POST(multipart/form-data) 내에서 Watson IoT Platform으로 전달됩니다. POST의 본문에는 최소한 두 개의 파트가 포함되어 있어야 합니다. 

- 하나의 이름은 **schemaFile**이며, 여기에는 파트의 본문으로서 파일의 실제 컨텐츠가 포함되어 있습니다. 
- 하나의 이름은 **name**이며, 여기에는 파트의 본문에서 스키마 리소스의 이름을 정의하는 문자열이 포함되어 있습니다. 

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 문서를 참조하십시오.

다음 예제는 cURL을 사용하여 이벤트 스키마 리소스 *tEventSchema.json*을 작성하는 방법을 표시합니다.

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**팁:** 예제 권한 부여 값 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=`은 다음 정보로 구성되어 있습니다.
`{API Key}:{authorization token}` (이는 다시 Base64 인코딩됨) 

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
POST 메소드에 대한 응답으로 리턴된 스키마 ID *5846cd7c6522050001db0e0d*는 이벤트 유형에 이벤트 스키마를 추가할 때 필요합니다.

다음 예제는 cURL을 사용하여 이벤트 스키마 리소스 *tempEventSchema.json*을 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
POST 메소드에 대한 응답으로 리턴된 스키마 ID *5846cee36522050001db0e0e*는 이벤트 유형에 이벤트 스키마를 추가할 때 필요합니다.

## 3단계: 이벤트 스키마를 참조하는 이벤트 유형 작성
{: #step3}

각각의 이벤트 유형은 이벤트 스키마 리소스의 작성에 사용된 POST 메소드에 대한 응답으로 리턴된 스키마 ID를 사용하여 이전 예제에서 작성된 관련 이벤트 스키마를 참조합니다.

이벤트 유형을 작성하려면 다음 API를 사용하십시오.

```
POST /draft/event/types
```
드래프트 이벤트 유형은 인바운드 MQTT 이벤트의 구조를 정의하는 스키마 정의를 참조해야 합니다. 


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 문서를 참조하십시오.


다음 예제는 cURL을 사용하여 섭씨로 측정되는 온도 이벤트에 대한 이벤트 유형을 작성합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

스키마 ID *5846cd7c6522050001db0e0d*는 이벤트 유형에 이벤트 스키마를 추가하는 데 사용됩니다. 이 ID는 이벤트 스키마 리소스 *tEventSchema.json*을 작성하는 데 사용된 POST 메소드에 대한 응답으로 리턴되었습니다.

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

POST 메소드에 대한 응답으로 리턴된 이벤트 유형 ID *5846d0fd6522050001db0e0f*는 실제 인터페이스에 이벤트 유형을 추가하는 데 사용됩니다.

다음 예제는 cURL을 사용하여 화씨로 측정되는 온도 이벤트에 대한 이벤트 유형을 작성합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
스키마 ID *5846cee36522050001db0e0e*는 이벤트 유형에 이벤트 스키마를 추가하는 데 사용됩니다. 이 ID는 이벤트 스키마 리소스 *tempEventSchema.json*을 작성하는 데 사용된 POST 메소드에 대한 응답으로 리턴되었습니다.

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
POST 메소드에 대한 응답으로 리턴된 이벤트 유형 ID *5846d2846522050001db0e10*는 실제 인터페이스에 이벤트 유형을 추가하는 데 사용됩니다.

## 4단계: 실제 인터페이스 작성
{: #step7}

실제 인터페이스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/physicalinterfaces
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 문서를 참조하십시오.

이 시나리오에서는 각 이벤트 유형마다 하나씩 두 개의 실제 인터페이스가 필요합니다.

다음 예제는 cURL을 사용하여 첫 번째 실제 인터페이스를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

응답으로 리턴된 실제 인터페이스 ID *5847d1df6522050001db0e1a*는 섭씨로 측정되는 온도 이벤트를 실제 인터페이스에 추가하기 위해 호출된 POST 메소드의 URL에서 사용됩니다.

다음 예제는 cURL을 사용하여 두 번째 실제 인터페이스를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

응답으로 리턴된 실제 인터페이스 ID *5847d1df6522050001db0e1b*는 화씨로 측정되는 온도 이벤트를 실제 인터페이스에 추가하기 위해 호출된 POST 메소드의 URL에서 사용됩니다.   

## 5단계: 실제 인터페이스에 이벤트 유형 추가
{: #step8}

실제 인터페이스에 이벤트 유형을 추가하려면 다음 API를 사용하십시오.

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 문서를 참조하십시오.

이 시나리오에서는 다음 이벤트 유형이 지정된 실제 인터페이스에 추가됩니다.
- 섭씨 온도 이벤트 *tevt*가 주제의 *eventId* 및 이벤트 스키마 리소스 작성의 *eventTypeId*를 사용하여 ID *5847d1df6522050001db0e1a*의 실제 인터페이스에 추가됩니다.
- 화씨 온도 이벤트 *tempevt*가 주제의 *eventId* 및 이벤트 스키마 리소스 작성의 *eventTypeId*를 사용하여 ID *5847d1df6522050001db0e1b*의 실제 인터페이스에 추가됩니다.


다음 예제는 cURL을 사용하여 온도 이벤트 *tevt*를 ID *5847d1df6522050001db0e1a*의 실제 인터페이스에 추가하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

다음 예제는 cURL을 사용하여 온도 이벤트 *tempevt*를 ID *5847d1df6522050001db0e1b*의 실제 인터페이스에 추가하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## 6단계: 실제 인터페이스에 연결하도록 디바이스 유형 업데이트
{: #step9}

디바이스 유형를 업데이트하려면 다음 API를 사용하십시오.

```
POST /draft/device/types/{typeId}/physicalinterface
```

여기서 *typeId*는 디바이스 유형 ID입니다.


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.

이 시나리오에서 디바이스 유형 *TSensor*는 실제 인터페이스 *5847d1df6522050001db0e1b*에 연결하기 위해 업데이트되며, 디바이스 유형 *TempSensor*는 실제 인터페이스 *5847d1df6522050001db0e1a*에 연결하기 위해 업데이트됩니다. 


다음 예제는 cURL을 사용하여 디바이스 유형 *TSensor*를 업데이트하는 방법을 표시합니다. 

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
디바이스 ID *TSensor*는 실제 인터페이스와 논리 인터페이스를 추가할 때 필요합니다. 

다음 예제는 cURL을 사용하여 디바이스 유형 *TempSensor*를 업데이트하는 방법을 표시합니다. 

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
디바이스 ID *TempSensor*는 실제 인터페이스와 논리 인터페이스를 추가할 때 필요합니다. 


## 7단계: 논리 인터페이스 스키마 파일 작성
{: #step4}

다음 예제는 *thermometer.json*이라고 하는 논리 인터페이스 스키마 파일을 작성하는 방법을 표시합니다. 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## 8단계: 논리 인터페이스 스키마 리소스 작성
{: #step5}

논리 인터페이스 스키마 리소스를 작성하려면 다음 API를 사용하십시오. 

```
POST /draft/schemas
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 문서를 참조하십시오.

다음 예제는 cURL을 사용하여 논리 인터페이스 스키마를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
논리 인터페이스 스키마를 논리 인터페이스에 추가하려면 POST 메소드에 대한 응답으로 리턴된 스키마 ID *5846ec826522050001db0e11*을 사용하십시오.

## 9단계: 논리 인터페이스 스키마를 참조하는 논리 인터페이스 작성
{: #step6}

논리 인터페이스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/logicalinterfaces
```

선택적으로 논리 인터페이스의 의미 있는 별명 이름을 지정할 수 있습니다. 별명은 자동 생성된 논리 인터페이스 ID를 사용하는 대신에 디바이스의 상태를 검색하는 데 사용되는 API 호출 또는 주제 문자열 구독에서 참조될 수 있습니다. 

**참고:** 별명 이름의 길이는 1-36자여야 하며 영숫자, 하이픈, 마침표, 밑줄 문자를 포함할 수 있습니다. 별명 이름은 24자의 16진 문자열일 수 없습니다. 

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 문서를 참조하십시오.

이 시나리오에서 논리 인터페이스 스키마를 논리 인터페이스에 추가하려면 이전 응답에 리턴된 스키마 ID *5846ec826522050001db0e11*을 사용하십시오.

다음 예제는 cURL을 사용하여 논리 인터페이스를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
이 시나리오에서 논리 인터페이스를 디바이스 유형에 추가하려면 POST 메소드에 대한 응답으로 리턴된 논리 인터페이스 ID *5846ed076522050001db0e12*를 사용하십시오. 또한 이 ID를 사용하여 논리 인터페이스에서 정의한 특성에 인바운드 디바이스 이벤트를 맵핑합니다. 논리 인터페이스 별명 *IThermometer*를 사용하면 HTTP REST API를 사용하거나 주제 문자열을 구독하여 [디바이스의 상태를 검색](##step13)할 수 있습니다. 

## 10단계: 디바이스 유형에 논리 인터페이스 추가
{: #step10}

디바이스 유형에 논리 인터페이스를 추가하려면 다음 API를 사용하십시오. 

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

여기서 *typeId*는 디바이스 유형의 이름입니다. 

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.  
**참고:** 이 시나리오에서 동일한 논리 인터페이스 *5846ed076522050001db0e12*는 *TSensor* 및 *TempSensor*와 연관되어 있습니다. 

다음 예제는 cURL을 사용하여 논리 스키마 ID *5846ec826522050001db0e11*과 연관된 논리 인터페이스 *5846ed076522050001db0e12*를 디바이스 유형 *TSensor*에 추가하는 방법을 표시합니다. 

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

다음 예제는 cURL을 사용하여 논리 인터페이스 스키마 ID *5846ec826522050001db0e11*을 참조하는 논리 인터페이스 *5846ed076522050001db0e12*를 디바이스 유형 *TempSensor*에 추가하는 방법을 표시합니다. 

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## 11단계: 인바운드 이벤트의 특성을 논리 인터페이스의 특성에 맵핑하기 위한 맵핑 정의
{: #step11}  
**중요:** MQTT 클라이언트 애플리케이션을 사용 중이고 디바이스 상태 업데이트에 대한 알림을 수신하기 위해 구독하려는 경우, 맵핑 정의 시 알림 설정을 구성해야 합니다. 이 방식으로 알림 전략을 구성하여 여러 디바이스 유형에서 논리 인터페이스를 재사용할 수 있으며, 각 디바이스 유형은 선택한 방법으로 상태 변경 알림을 수신하도록 구성됩니다. 알림 전략은 MQTT 클라이언트가 지정된 주제 문자열을 구독하는지 여부에 관계없이 정의될 수 있습니다.  
  
다음 알림 설정을 구성할 수 있습니다.  

매개변수	|	설명
------	|	-----
never	|	알림이 전송되지 않음 - 이 설정은 기본값임
on-every-event	|	디바이스 상태 변경 여부에 관계없이 알림이 이벤트마다 전송됨
on-state-change	|	이벤트로 인해 디바이스 상태가 변경되는 경우에만 알림이 전송됨  

**참고:** 알림 설정 *never*를 선택하는 경우 애플리케이션이 디바이스 상태의 변경에 대한 알림을 결코 수신하지 않습니다. 따라서 알림 전략 *on-every-event* 또는 *on-state-change*를 선택할 수 있습니다.  

이벤트를 맵핑하려면 다음 API를 사용하십시오.

```
POST /draft/device/types/{typeId}/mappings
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.

이 시나리오에서는 인바운드 이벤트 *tevt*의 **t** 특성을 논리 인터페이스의 **temperature** 특성에 맵핑하기 위한 디바이스 유형 *TSensor*의 맵핑을 정의합니다. 또한 인바운드 이벤트 *tempevt*의 **temp** 특성을 논리 인터페이스의 **temperature** 특성에 맵핑하기 위한 디바이스 유형 *TempSensor*의 맵핑도 정의합니다. 알림 설정은 "on-state-change"로 설정됩니다. 

다음 예제는 cURL을 사용하여 디바이스 유형 *TSensor*에 대한 맵핑을 추가하는 방법을 표시합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**중요:** 구조화된 [JSON ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://json-schema.org/){:new_window} 이벤트 페이로드에 대해 $event.*property* 항목이 사용자 특성의 JSON 구조와 올바르게 일치하는지 확인하십시오. 예를 들어, 페이로드가 `{"d":{"t":<temp>}}`인 경우 `$event.d.t`를 사용하십시오.

논리 인터페이스 및 디바이스 유형 *TSensor*의 작성에 사용된 POST 메소드에 대한 응답으로 리턴된 논리 인터페이스 ID *5846ed076522050001db0e12*를 지정하십시오. 

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt": {
      "temperature": "$event.t"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```
다음 예제는 cURL을 사용하여 디바이스 유형 *TempSensor*에 대한 맵핑을 추가하는 방법을 표시합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

논리 인터페이스 및 디바이스 유형 *TempSensor*의 작성에 사용된 POST 메소드에 대한 응답으로 리턴된 논리 인터페이스 ID *5846ed076522050001db0e12*를 지정하십시오. 변환을 적용하여 값을 화씨의 측정값에서 섭씨의 측정값으로 변경할 수 있습니다.


다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## 12단계: 구성 유효성 검증 및 활성화
{: #step15}

각 디바이스 유형의 디바이스 상태 업데이트와 관련된 구성을 유효성 검증하고 활성화하십시오. 이 구성에는 스키마, 이벤트 유형, 실제 인터페이스, 논리 인터페이스 및 맵핑이 포함됩니다. 

디바이스 유형 구성을 유효성 검증하고 활성화하려면 다음 API를 사용하십시오. 

```
PATCH /draft/device/types/{typeId}
```

여기서 *typeId*는 디바이스 유형 ID입니다.

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.

이 시나리오에서는 두 개의 디바이스 유형에 대한 구성을 유효성 검증하고 활성화해야 합니다. 

다음 예제는 cURL을 사용하여 디바이스 유형 *TSensor*에 대한 구성을 유효성 검증하고 활성화하는 방법을 표시합니다. 

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
다음 예제는 PATCH 메소드에 대한 응답을 표시합니다.

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

다음 예제는 cURL을 사용하여 디바이스 유형 *TempSensor*에 대한 구성을 유효성 검증하고 활성화하는 방법을 표시합니다. 

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

다음 예제는 PATCH 메소드에 대한 응답을 표시합니다.

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## 13단계: 인바운드 디바이스 이벤트 공개
{: #step12}

다음 예제 페이로드로 주제 `iot-2/evt/tevt/fmt/json`에 *tSensor*의 온도 이벤트를 공개하십시오. 
```
{
  "t" : 34.5
}
```
다음 예제 페이로드로 주제 `iot-2/evt/tempevt/fmt/json`에 *tempSensor*의 온도 이벤트를 공개하십시오.
```
{
  "temp" : 72.55
}
```

디바이스에서 인바운드 이벤트의 공개에 대한 정보는 [애플리케이션용 MQTT 연결](../applications/mqtt.html#publishing_device_events)을 참조하십시오.


## 14단계: 디바이스의 상태 검색
{: #step13}

HTTP REST API를 사용하거나 주제 문자열을 구독하여 디바이스의 상태를 검색할 수 있습니다. 논리 인터페이스를 작성할 때 별명 이름을 지정한 경우에는 *logicalInterfaceId* 매개변수 대신 별명 이름을 사용하여 디바이스의 상태를 검색할 수 있습니다.  

- MQTT 클라이언트 애플리케이션이 있는 경우 다음 주제 문자열을 구독할 수 있습니다.  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- 또는 다음 HTTP REST API를 사용하여 최신 디바이스 상태를 검색할 수 있습니다.  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

필수 매개변수는 다음과 같습니다.  

매개변수	|	설명
------	|	-----
typeId	|	디바이스 유형 ID입니다.
deviceId	|	디바이스 ID입니다.
logicalInterfaceId 또는 alias|	논리 인터페이스 또는 사용자 지정 별명 이름에 대해 작성된 ID입니다. 


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.

다음 예제는 cURL을 사용하여 작성된 논리 인터페이스의 ID를 참조함으로써 *tSensor*의 현재 상태를 검색하는 방법을 표시합니다. 
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

논리 인터페이스 ID *5846ed076522050001db0e12*는 GET 메소드에 사용됩니다. 이 ID는 논리 인터페이스 작성에 사용된 POST 메소드에 대한 응답으로 리턴됩니다.
다음 예제는 GET 메소드에 대한 응답을 표시합니다.
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```

다음 예제는 cURL을 사용하여 작성된 논리 인터페이스의 별명을 참조함으로써 *tempSensor*의 현재 상태를 검색하는 방법을 표시합니다. 
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

논리 인터페이스 별명 *IThermometer*는 GET 메소드에서 사용됩니다. 이 ID는 논리 인터페이스 작성에 사용된 POST 메소드에 대한 응답으로 리턴됩니다.
다음 예제는 GET 메소드에 대한 응답을 표시합니다.
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```

참고로, 리턴되는 온도 측정값은 화씨가 아닌 섭씨입니다.

애플리케이션은 서로 다른 온도 스케일을 파악하거나 변환하기 위한 구성을 요구하지 않고도 이 정규화된 데이터를 처리할 수 있습니다.

## 다음 단계

사용자가 이 시나리오를 기반으로 사물 유형 및 사물 인스턴스를 구성하여 데이터 관리의 쌍둥이 자산 기능을 활용하고자 할 수 있습니다. 사물 구성에 대한 정보는 [단계별 안내서 2: 공통 인터페이스를 통한 사물 관련 작업 방법에 대한 자세한 예제(베타)](../information_management/im_index_scenario_thing.html)를 참조하십시오. 

{{site.data.keyword.iot_short_notm}}에서 수신한 이벤트로 인해 디바이스 또는 사물 상태가 변경될 때 조치를 트리거하기 위해 사용할 수 있는 규칙을 작성할 수도 있습니다. 규칙 작성에 대한 정보는 [임베디드 규칙 작성(베타)](../information_management/im_rules.html)을 참조하십시오. 

