---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-18"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# リソース・レベル・アクセス制御の概要
{: #RLAC_overview}

リソース・レベル・アクセス制御を使用すれば、デバイスを管理するためにユーザーと API キーのアクセスを制御できます。 それぞれのユーザーや API キーで管理できる組織内のデバイスを指定するために、リソース・グループを使用します。 リソース・レベル・アクセス制御の構成方法については、 [リソース・レベル・アクセス制御の構成](rlac.html#configure_RLAC)を参照してください。

## リソース・レベル・アクセス制御の概念 
{: #RLAC_concepts}

{{site.data.keyword.iot_short_notm}} のリソース・レベル・アクセス制御では、デバイス・グループという単位を使用します。 デバイス・グループ機能を使用すれば、ユーザーが自分の役割に基づいてアクセスできるデバイスの数を管理できます。 デバイス・グループを実装する前に、ソリューションの効率を高めるために、デバイス・グループの意図と範囲を明確にして記録しておいてください。 

IoT ソリューションの一部としてデバイス・グループを実装すれば、ユーザーやアプリケーションからアクセスできるデバイスを制限し、組織に関連する全デバイスではなく、デバイスのサブセットだけに限定できるようになります。 デバイス・グループの効率を高めるために、1 人のユーザーが多数のデバイスにアクセスしなければならない場合に限って、デバイス・グループを使用するようにしてください。

例えば、多数の技術員とオペレーション担当者を抱える会社が、英国全体を対象にして IoT ソリューションを実装したいと思っています。 その会社では、69 都市のそれぞれに対して 1 つのデバイス・グループを構成し、地域ごとに 9 つのデバイス・グループを構成し、英国全体を対象にした 1 つのデバイス・グループを構成しています。 そうしたグループにデバイスを割り振り、そのようなグループを 15 人の技術員とオペレーション担当者に割り当てます。中には、複数のグループにアクセスする人もいます。 1 つか 2 つのデバイスだけを担当するユーザーは、{{site.data.keyword.iot_short_notm}} の外部のモバイル・アプリケーションやエンタープライズ・アーキテクチャーを引き続き使用しますが、この IoT ソリューション全体に賛同しています。

デバイス・グループについての質問は、[IoT に関する質問のスペース ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window} にお送りください。

## アクセス制御のリソース・グループ制限値

リソース・グループ制限値が適用されています。目的は、リソース・レベル・アクセス制御機能の効率的な活用です。 1 つのサブジェクトに割り当てられるリソース・グループの数、グループ内のデバイスの数、1 つのリソースが所属できるグループの数を限定する制限値があります。

以下の制限値が適用されています。

 - 1 つの API キー、ユーザー、ゲートウェイに対して割り当てられるリソース・グループの最大数は 10 です。
 - 1 つのリソース・グループに組み込めるリソースの最大数は 300 です。
 - 1 つのリソースが所属できるグループの最大数は 10 です。

## リソース・レベル・アクセス制御の用語

リソース・レベル・アクセス制御機能の用語を以下のセクションにまとめています。 

### サブジェクト
{: #subjects}

サブジェクトとは、プラットフォーム・アクセスを要求する認証済みのエンティティーであり、そのような要求は許可されます。 以下のタイプのサブジェクトが有効です。

- *ユーザー*: アプリケーションを使用するエンド・ユーザー。 有効期限のないアカウントを持っているユーザーをメンバーといい、有効期限のあるアカウントを持っているユーザーをゲストといいます。
- *デバイス*: {{site.data.keyword.iot_short_notm}} プラットフォームにアクセスする物理デバイスであり、デバイスというタイプを使用します。
- *ゲートウェイ*: {{site.data.keyword.iot_short_notm}} プラットフォームにアクセスする物理デバイスであり、ゲートウェイというタイプを使用します。
- *アプリケーション*: 許可とアクセス制御のために API キーを使用して {{site.data.keyword.iot_short_notm}} プラットフォームにアクセスするアプリケーションやサービス。

### リソース
{: #resources}

リソースは、サブジェクトがアクションを実行する対象のエンティティーです。 サブジェクトは、リソースに対する許可プラットフォーム・アクセスを要求します。 

### アクション
{: #actions}

アクションは、サブジェクトが実行する処理であり、作成、読み取り、更新、削除、実行などがあります。 操作を使用して、リソースに対するアクションをグループ化します。

### アプリケーション
{: #applications}

アプリケーションは、プラットフォームにとって不明なサブジェクトの代わりにアクションを実行できます。例えば、家庭用の電化製品を制御するモバイル・アプリケーションなどがあります。実際のデバイスやシミュレートしたデバイスの代わりにアクションを実行するアプリケーションもあります。その場合のデバイスは、プラットフォームに認識されている場合もあれば、プラットフォームにとって不明な場合もあります。 他の種類のデバイスの代わりにアクションを実行しないサーバー・サイドの実際のアプリケーションもあります。

認証には API キーやトークンを使用します。{{site.data.keyword.iot_short_notm}} プラットフォームへのアクセスも API キーやトークンに基づいています。

### 操作
{: #operations}

1 つの操作に、特定のリソース・タイプに対して実行する 1 つ以上のアクションを組み込みます。実行には、ユーザー・インターフェースやプログラマチック・インターフェース (REST サービスや MQTT インターフェースなど) を使用します。 さまざまなサブジェクト (メンバー、アプリケーション、デバイスなど) が操作を使用します。 操作 ID (OID) で操作を識別できます。

### 役割
{: #roles}

役割は、一連の操作を定義したものであり、役割によって、そうした操作の使用許可を与えます。 1 つのサブジェクトに複数の役割を設定できます。役割はサブジェクトの属性です。

**役割の形式**

0 個から n 個の役割の配列

    { "roles": [ "PD_ADMIN_APP" ] }

**「役割とグループ」のペアの形式**

0 個から n 個の <string, list<string>> のペアのマップ

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### アクセス権
{: #permissions}

アクセス権によって、サブジェクトはリソースやリソース・グループに対して操作を実行できるようになります。

### リソース・グループ
{: #resource_groups}

リソース・グループは、リソースであるデバイスの集合です。 デバイスをリソース・グループに追加し、「役割とグループ」のペアでリソース・グループをサブジェクトに割り当てます。 サブジェクトは、そうした関係に基づいて、特定のグループのデバイスに対して特定の役割の操作を実行できます。

### ユーザー
{: #users}

ユーザーは、組織のメンバーであることを示す ID を使用して、{{site.data.keyword.iot_short_notm}} プラットフォームのダッシュボードにログインします。 ユーザーに役割を割り当てることによって、特定セットの HTTP API を呼び出す許可をユーザーに与えます。 ユーザー役割の詳細については、[ユーザー役割のアクセス・レベル](roles_access.html#levels-of-access-for-user-roles)を参照してください。

### API キー
{: #API_keys}

API キーは、{{site.data.keyword.iot_short_notm}} プラットフォームの HTTP API を呼び出す時に使用するトークンです。 API キーに役割を割り当てることによって、特定セットの HTTP API を呼び出す許可を API キーに与えます。 API キー役割の詳細については、[アプリケーション役割のアクセス・レベル](app_roles_access.html#levels-of-access-for-application-roles)を参照してください。

## リソース・レベル・アクセス制御が適用される API
{: #RLAC_enforced_APIs}
リソース・レベル・アクセス制御が有効になっていると、このセクションで取り上げるデバイス関連 API を実行できるのは、呼び出し元から指定のデバイスにアクセスできる場合に限定されるようになります。

### デバイス API (個別)

**特定タイプのデバイスのリスト**

    GET /api/v0002/device/types/${typeId}

該当するグループに所属するデバイスのサブセットだけが返されます。

**デバイスの取得**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス管理詳細情報の取得**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**ゲートウェイで登録されているデバイスの取得**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

呼び出し元からゲートウェイにアクセスできない場合は、404 エラーが返されます。 該当するグループに所属するデバイスのサブセットだけが返されます。 デバイスもゲートウェイも、呼び出し元のグループに入っていなければなりません。

**デバイスの更新**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイスの削除**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

### デバイス API (一括)

**デバイスのリスト**

    GET /api/v0002/bulk/devices

該当するグループに所属するデバイスのサブセットだけが返されます。

**デバイスの削除**

    DELETE /api/v0002/bulk/devices/remove

該当するグループに所属するデバイスのサブセットだけが削除されます。 アクセスできないデバイスについても、success = true が返されます。

**デバイスの更新:**

    PUT /api/v0002/bulk/devices/update

### デバイス管理 API

**要求の開始**

    POST /api/v0002/mgmt/requests

呼び出し元からデバイスにアクセスできない場合は、失敗します。

**要求の削除**

    DELETE /api/v0002/mgmt/requests/${requestId}

呼び出し元からデバイスにアクセスできない場合は、失敗します。

**要求の表示**

    GET /api/v0002/mgmt/requests/${requestId}

**要求のリスト**

    GET /api/v0002/mgmt/requests

**1 つの要求の取得**

    GET /api/v0002/mgmt/requests/${requestId}

**要求デバイス詳細情報の取得**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**1 つのデバイスの要求デバイス詳細情報の取得**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### 問題判別 API

**1 つのデバイスの接続ログ**

    GET /api/v0002/logs/connection

デバイスにアクセスできない場合は、空の配列が返されます。

**デバイス・エラー・コードの追加**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス・エラー・コードのリスト**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス・エラー・コードの消去**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス診断ログの追加**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス診断ログのリスト**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**1 つのデバイス診断ログの取得**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイス診断ログの消去**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**1 つのデバイス診断ログの削除**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

###最新イベント・キャッシュ API

**デバイスのイベントの取得**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイスのイベントの取得**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

呼び出し元からデバイスにアクセスできない場合は、404 エラーが返されます。

**デバイスの論理インターフェースの取得**

    GET /api/v0002/device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}

デバイスが呼び出し元のグループに含まれていない場合は、404 エラーが返されます。
