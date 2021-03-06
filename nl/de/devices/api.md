---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# HTTP-APIs für Geräte
{: #api}

Es gibt zwei Arten von HTTP-APIs, die mit {{site.data.keyword.iot_full}} verwendet werden können. Mit HTTP-REST-APIs können Sie
Geräte erstellen, aktualisieren, auflisten und löschen. Mit HTTP-Messaging-APIs können Sie Ereignisinformationen von Geräten in die Cloud senden und
Befehle von Anwendungen in der Cloud entgegennehmen. 

## Clientverbindungen
{: #client_connections}

Informationen zur Clientsicherheit und zur Vorgehensweise beim Herstellen von Verbindungen zwischen Clients und Geräten in {{site.data.keyword.iot_short_notm}} finden Sie in [Anwendungen, Geräte und Gateways mit {{site.data.keyword.iot_short_notm}} verbinden](../reference/security/connect_devices_apps_gw.html).

**Hinweis:** Der HTTP-Port 1883 ist standardmäßig inaktiviert. Informationen zum Ändern der Standardeinstellung finden Sie unter [Sicherheitsrichtlinien konfigurieren](../reference/security/set_up_policies.html#set_up_policies.md).

## Authentifizierung

Alle Authentifizierungsanforderungen müssen einen Berechtigungsheader enthalten. Die Basisauthentifizierung ist die einzige Methode, die unterstützt ist. Wenn ein Gerät eine HTTP-Anforderung über die {{site.data.keyword.iot_short_notm}}-HTTP-REST-API übergibt, sind folgende Berechtigungsnachweise erforderlich:

|Berechtigungsnachweis|Erforderliche Eingabe|
|:---|:---|
|Benutzername|`use-token-auth`
|Kennwort| Das Authentifizierungstoken, das beim Registrieren des Geräts entweder automatisch generiert oder manuell angegeben wurde.


## Anforderungsheader des Typs 'Content-Type'

Zusammen mit der Anforderung muss ein Anforderungsheader des Typs `Content-Type` ('Inhaltstyp') angegeben werden. Die folgende Tabelle zeigt die Zuordnung der unterstützten Typen zu den internen {{site.data.keyword.iot_short_notm}}-Formaten.

|Header des Typs 'Content-Type'|Format in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## Servicequalität

Ähnlich der Servicestufe 0 der MQTT-Servicequalität (Zustellung höchstens einmal) bietet das HTTP-REST-Messaging die Zustellung nicht persistenter Nachrichten, prüft aber, ob die Anforderung richtig ist und an den Server gesendet werden kann, bevor es die HTTP-Antwort sendet. Durch eine Antwort, die den HTTP-Statuscode 200 enthält, wird bestätigt, dass die Nachricht an den Server übermittelt wurde. Wenn Sie entweder die MQTT-Servicequalitätsstufe 'höchstens einmal' oder das HTTP-Äquivalent zum Zustellen von Ereignisnachrichten verwenden, muss das Gerät oder die Anwendung Wiederholungslogik implementieren, damit die Zustellung garantiert ist.

Weitere Informationen zum MQTT-Protokoll und den Servicequalitätsstufen für {{site.data.keyword.iot_short_notm}} finden Sie in [MQTT-Messaging](../reference/mqtt/index.html).

Mit HTTP-Messsaging-APIs können Sie Ereignisse von Geräten in der Cloud publizieren und Befehle von Anwendungen in der Cloud empfangen.

## Ereignisse publizieren
{: #event_publication}

Zusätzlich zum MQTT-Nachrichtenprotokoll können Sie Ihre Geräte auch so konfigurieren, dass Ereignisse in {{site.data.keyword.iot_short_notm}} über HTTP mithilfe von HTTP-Messaging-API-Befehlen publiziert werden.

Verwenden Sie eine der folgenden URLs, um eine ``POST``-Anforderung von einem Gerät zu übergeben, das mit {{site.data.keyword.iot_short_notm}} verbunden ist:

### Nicht sichere POST-Anforderung für das Publizieren von Ereignissen

<pre class="pre"><code class="hljs">http://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/events/<var class="keyword varname">Ereignis-ID</var></code></pre>

### Sichere POST-Anforderung zum Publizieren von Ereignissen

<pre class="pre"><code class="hljs">https://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/events/<var class="keyword varname">Ereignis-ID</var></code></pre>

**Hinweis:** Port 443, der SSL-Standardport, kann auch für sichere HTTP-API-Aufrufe angegeben werden.

## Befehle empfangen
{: #receive_commands}

Zusätzlich zur Verwendung des MQTT-Messaging-Protokolls können Sie Ihre Geräte auch so konfigurieren, dass Befehle von {{site.data.keyword.iot_short_notm}} über HTTP mithilfe von HTTP-Messaging-API-Befehlen empfangen werden. Ein Gerät kann Befehle empfangen, die an das Gerät selbst gerichtet sind.

Verwenden Sie eine der folgenden URLs, um eine ``POST``-Anforderung von einem Gerät zu übergeben, das mit {{site.data.keyword.iot_short_notm}} verbunden ist:

### Nicht sichere POST-Anforderung zum Empfangen von Befehlen

<pre class="pre"><code class="hljs">http://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/commands/<var class="keyword varname">Befehl</var>/request</code></pre>

### Sichere POST-Anforderung zum Empfangen von Befehlen

<pre class="pre"><code class="hljs">https://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/commands/<var class="keyword varname">Befehl</var>/request</code></pre>

**Hinweis:** Port 443, der SSL-Standardport, kann auch für sichere HTTP-API-Aufrufe angegeben werden.

Optional können Sie den Parameter *waitTimeSecs* in den Hauptteil der HTTP-Anforderung einschließen, um eine Ganzzahl für die maximale Anzahl Sekunden anzugeben, für die auf einen Befehl gewartet werden soll:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Wichtige Hinweise:**
- Der Wert von *waitTimeSecs* muss eine Ganzzahl zwischen 0 - 3600 Sekunden sein. Der Standardwert ist '0'.
- Um alle Befehle empfangen zu können, verwenden Sie für die Komponente {Befehl} das Platzhalterzeichen für "beliebig" (+). Wird das Platzhalterzeichen verwendet, so ist die Befehls-ID im Antwortheaderfeld *X-commandId* enthalten.
- Wenn der Statuscode der HTTP-Antwort 200 ist, dann sind die Befehlsdaten im Hauptteil der Antwort enthalten. Suchen Sie im Antwortheaderfeld *Inhaltstyp* nach dem Inhaltstyp.
- Wenn der Statuscode der HTTP-Antwort 204 ist, dann sind keine Befehlsdaten verfügbar.

Informationen zum Zugriff auf die {{site.data.keyword.iot_short_notm}}-HTTP-REST-APIs finden Sie in der Veröffentlichung zu [{{site.data.keyword.iot_short_notm}}-HTTP-REST-APIs ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

Informationen zum Zugriff auf die {{site.data.keyword.iot_short_notm}}-HTTP-Messaging-APIs finden Sie in der Veröffentlichung [{{site.data.keyword.iot_short_notm}}-HTTP-Messaging-APIs ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

Informationen von Entwickeln von Geräten finden Sie unter [In {{site.data.keyword.iot_short_notm}}Geräte entwickeln](../devices/device_dev_index.html).

## Beispiel für eine sichere POST-Anforderung

Im folgenden Beispiel wird eine Nachricht von einem Gerät über das HTTP-Protokoll an {{site.data.keyword.iot_short_notm}} gesendet. Eine Anwendung empfängt die Nachricht, indem sie das Topic subskribiert, in dem die Nachricht publiziert wird. Die folgende Tabelle enthält Informationen zu dem Gerät. 

|Parameter|Wert|
|:---|:---|
|Organisations-ID |myOrgID
|Gerätetyp| TestDevices
|Geräte-ID | TestPublishEvent
|Authentifizierungsmethode|use-token-auth
|Authentifizierungstoken|passw0rd


Im folgenden Beispiel wird 'curl' verwendet, um ein Ereignis mit dem Namen *TestMessage* vom Gerät *TestPublishEvent* an {{site.data.keyword.iot_short_notm}} über das HTTP-Protokoll zu senden: 

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

Die Nachricht ist in einer Textdatei namens *message.txt* enthalten. Der Inhalt der Datei *message.txt* wird im folgenden Beispiel dargestellt: 
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
Die Nachricht wird unter dem Topic *iot-2/evt/TestMessage/fmt/json* publiziert. Eine Anwendung mit dem Namen *testSubscriber* wird erstellt, die den Topic-Filter *iot-2/type/+/id/+/evt/+/fmt/+* subskribiert. Der Topic-Filter stimmt mit dem Topic überein, in dem die Nachricht publiziert wird.

Sie können das Python-Beispiel *simpleApp.py* verwenden, dass in der [Python-Bibliothek ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp) verfügbar ist, damit Sie Ihre Anwendung so konfigurieren können, dass sie den Topic-Filter *iot-2/type/+/id/+/evt/+/fmt/+* subskribiert. 

Das folgende Python-Script subskribiert einen Topic-Filter, der mit der Topic-Zeichenfolge übereinstimmt, für die die Nachricht publiziert wird:

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
Dabei gilt:

- **-o** ist der Organisationsname.
- **-i** ist der Name der Anwendung.
- **-k** ist der API-Schlüssel, der bei der Erstellung der Anwendung generiert wird.  
- **-t** ist das Authentifizierungstoken, das bei der Erstellung der Anwendung generiert wird.   


Das folgende Beispiel zeigt eine erfolgreiche Antwort auf den Empfang der publizierten Nachricht vom Gerät *TestPublishEvent*. 
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## Cache für zuletzt gemeldete Ereignisse
{: #last-event-cache}

Sie können den Cache für zuletzt gemeldete Ereignisse verwenden, um Informationen zu dem letzten gemeldeten Ereignis zu speichern, das von einem verbundenen Gerät an {{site.data.keyword.iot_short_notm}} gesendet wurde. Wenn Sie eine [API ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window} verwenden, können Sie den letzten aufgezeichneten Wert einer bestimmten Ereignis-ID für ein Gerät oder den letzten aufgezeichneten Wert für die einzelnen Ereignis-IDs abrufen, die von einem bestimmten Gerät gemeldet werden. 

### Cache für zuletzt gemeldete Ereignisse konfigurieren

Die Funktion des Cache für zuletzt gemeldete Ereignisse ist standardmäßig inaktiviert. Sie haben jedoch die folgenden Möglichkeiten, um diese Funktion zu aktivieren: 

-	Setzen Sie den Parameter *enabled* auf **true**, indem Sie eine API verwenden.
-	Legen Sie auf auf der Seite *Einstellungen* des {{site.data.keyword.iot_short_notm}}-Dashboards die Option **LEC aktivieren** auf **Ein** fest.

Die Zeitdauer, die Ereignisdaten im Cache gespeichert werden, wird durch den TTL-Wert (TTL, Time to Live) angegeben. Die letzten Ereignisdaten für ein bestimmtes Ereignis werden standardmäßig für sieben Tage gespeichert. Sie können die Dauer ändern, indem Sie eine API verwenden, um den Parameter **ttlDays** zu aktualisieren, oder indem Sie einen Wert für das Feld **Ereignisdaten-TTL** auf der Seite *Einstellungen* des {{site.data.keyword.iot_short_notm}}-Dashboards auswählen.

Für Lite-Pläne können Sie Informationen für ein Minimum von einem Tag und maximal sieben Tage speichern. Für andere Pläne können Sie Informationen für ein Minimum von einem Tag und maximal 45 Tage speichern.

Verwenden Sie die folgenden Informationen, um die Einstellungen für den Cache für zuletzt gemeldete Ereignisse mithilfe von APIs zu konfigurieren.

Verwenden Sie die folgende API, um die aktuelle Konfiguration des Cache für zuletzt gemeldete Ereignisse für eine Organisation abzurufen:

```
    GET /api/v0002/config/lec
```   

   
Das folgende Beispiel zeigt eine erfolgreiche Antwort auf die GET-Methode:
```
{
    "enabled": false,
    "ttlDays": 7
}
```
Jede Person oder Anwendung in einer Organisation ist berechtigt, auf diesen Endpunkt zuzugreifen. 

Wenn der Cache für zuletzt gemeldete Ereignisse für eine Organisation durch den {{site.data.keyword.iot_short_notm}}-Administrator inaktiviert wird, wird der folgende HTTP 403-Statuscode des Typs FORBIDDEN als Antwort auf die GET-Methode zurückgegeben:

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

Wenn der Cache für zuletzt gemeldete Ereignisse nicht aktiviert ist, wird als Antwort auf die GET-Methode ein HTTP 404-Statuscode zurückgegeben. Verwenden Sie die folgende API, um die Funktion des Cache für zuletzt gemeldete Ereignisse für eine Organisation zu aktivieren:

```
    PUT /api/v0002/config/lec
```

mit den folgenden Beispielnutzdaten: 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
Dabei gilt: 

- Der Wert für **enabled** ist erforderlich und gibt an, ob der Cache für zuletzt gemeldete Ereignisse für eine Organisation aktiviert ist. Der Wert muss ein boolescher Wert sein. 

- Der Wert für **ttlDays** ist optional und gibt die Anzahl der Tage an, an denen ein Ereignis im Cache für zuletzt gemeldete Ereignisse für eine Organisation aufbewahrt wird. Der Wert muss eine ganze Zahl zwischen 1 und 45 (einschließlich) sein.
   
 
Nur der Administrator, Benutzer mit Bedienerberechtigung oder Bedieneranwendungen sind berechtigt, auf diesen Endpunkt zuzugreifen. Wenn die Parameter oder das JSON-Format ungültig sind, wird als Antwort auf die PUT-Methode ein HTTP 401-Statuscode des Typs BAD REQUEST zurückgegeben. 

**Hinweis:** Es kann bis zu 30 Sekunden dauern, bis eine Konfigurationsänderung abgeschlossen ist.

### Informationen aus dem Cache für zuletzt gemeldete Ereignisse abrufen

Durch die Verwendung einer API können Sie das letzte Ereignis abrufen, das von einem Gerät gesendet wurde. Sie können den Gerätestatus unabhängig vom physischen Standort des Geräts oder vom Status seiner Verwendung abrufen. Sie können den zuletzt aufgezeichneten Wert einer Ereignis-ID für ein bestimmtes Gerät oder den zuletzt aufgezeichneten Wert für alle Ereignis-IDs abrufen, die von einem bestimmten Gerät gemeldet wurden. Daten zum letzten Ereignis eines Geräts können für jedes Ereignis abgerufen werden, das nicht später als 45 Tage zurückliegt.

Zum Abfragen des letzten aktuellen Werts für eine bestimmte Ereignis-ID verwenden Sie die folgende API-Anforderung, die den zuletzt aufgezeichneten Wert für die Ereignis-ID 'power' (Spannung) zurückgibt.

```
GET /api/v0002/device/types/<Gerätetyp>/devices/<Geräte-ID>/events/power
```

Die Antwort wird im folgenden JSON-Format zurückgegeben:

```
{
    "deviceId": "<Geräte-ID>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<Gerätetyp>"
}
```

**Hinweis:** Auch wenn die API-Antwort das Format JSON hat, können Ereignisnutzdaten in einem beliebigen anderen Format geschrieben werden. Nutzdaten, die von der API für zuletzt gemeldete Ereignisse zurückgegeben werden, haben die Base64-Codierung.

Zum Abfragen des letzten aktuellen Werts für die einzelnen Ereignis-IDs, die von einem Gerät gemeldet wurden, verwenden Sie folgende API-Anforderung:

```
GET /api/v0002/device/types/<Gerätetyp>/devices/<Geräte-ID>/events
```

Die Antwort enthält alle Ereignis-IDs, die vom Gerät gesendet wurden. Im folgenden Beispiel werden Werte für die Ereignisse 'power' (Spannung) und 'temperature' (Temperatur) zurückgegeben.

```
[
    {
        "deviceId": "<Geräte-ID>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<Gerätetyp>"
    },
    {
        "deviceId": "<Geräte-ID>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<Gerätetyp>"
    }
]
```

Informationen zum Zugriff auf die {{site.data.keyword.iot_short_notm}}-API für zuletzt gemeldete Ereignisse finden Sie in der Veröffentlichung zu den [HTTP-Information-Management-APIs von {{site.data.keyword.iot_short_notm}} ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
