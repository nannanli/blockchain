---

copyright:
  years: 2018
lastupdated: "2018-06-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Starter Plan-Netz steuern
{: #getting-started-with-starter-plan}

Der {{site.data.keyword.blockchainfull}} Platform Starter Plan stellt durch einen Klick ein vorkonfiguriertes Blockchain-Netz bereit. Das Angebot <!--offers you a free trial of 30 days and -->stellt standardmäßig ein genehmigtes Netz mit Konfiguration von zwei [Organisationen](glossary.html#organization), einem [Peer](glossary.html#peer) pro Organisation und einem [Kanal](glossary.html#channel) bereit. Wenn das Netz erstellt ist, können Sie Ihr Netz skalieren und ihm weitere Organisationen und Peers hinzufügen. <!--Note that it might cause extra cost if you exceed the default resource limits of two organizations and two peers.-->
{:shortdesc}

**Hinweis**: Beim {{site.data.keyword.blockchainfull}} Platform Starter Plan handelt es sich um eine Entwicklungs- und Testumgebung, die für Produktionsworkloads nicht geeignet ist. Wenn Sie eine Produktionsumgebung benötigen, lesen Sie die [Informationen zum Enterprise Plan](enterprise_plan.html).

Mit dem Starter Plan können Sie sich mit {{site.data.keyword.blockchainfull_notm}} Platform vertraut machen und sich Kenntnisse aneignen, Beispielanwendungen ausführen, eigene Anwendungen testen und ein Szenario mit mehreren Organisationen simulieren.  Dieses Einführungslernprogramm erläutert die Voraussetzungen und beschreibt die Schritte, die Sie ausführen müssen um ein Starter Plan-Netz zu erstellen und zu verwenden.

Wenn Sie {{site.data.keyword.blockchainfull_notm}} Platform und Blockchain noch nicht kennen, können Sie das [Glossar](glossary.html) lesen, um sich mit den in dieser Dokumentation verwendeten Begriffen vertraut zu machen. Weitere Informationen zu Blockchain finden Sie in der [Hyperledger Fabric-Dokumentation ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](http://hyperledger-fabric.readthedocs.io/en/latest/blockchain.html).


## Netz erstellen
Sie können ein Starter Plan-[Netz](glossary.html#network) mit der Standardkonfiguration gleich im Anschluss an Ihre Erstellung einer {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz abrufen.

1. Lokalisieren Sie den [Blockchain-Service ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/blockchain) im {{site.data.keyword.cloud_notm}}-Katalog.   
    **Hinweis**: Sie müssen sich mit Ihrem gebührenpflichtigen {{site.data.keyword.cloud_notm}}-Konto anmelden. Wenn Sie kein Konto haben, klicken Sie auf die Schaltfläche **Für Erstellung registrieren**. Nach der Erstellung eines kostenfreien Testkontos führen Sie ein Upgrade auf ein **nutzungsabhängig Konto** durch, indem Sie **Verwalten** > **Abrechnung und Nutzung** > **Abrechnung** in der {{site.data.keyword.cloud_notm}}-Konsole aufrufen und auf **Kreditkarte hinzufügen** klicken.
2. Wählen Sie die Region in {{site.data.keyword.cloud_notm}} zum Erstellen des Netzes aus. 
3. Wählen Sie Ihre Cloud Foundry-Organisation und den Bereich zum Erstellen des Netzes aus. 
4. Wählen Sie **Starter-Mitgliedschaftsplan** in der Tabelle mit den Preisstrukturplänen aus.
5. Klicken Sie auf die Schaltfläche **Erstellen**. Beachten Sie, dass ein Popup-Eingangsfenster angezeigt wird, wenn Sie zur Teilnahme an einem Netz eingeladen wurden. Zum Erstellen eines Netzes wählen Sie **Mit Ihrem Netz fortfahren** aus und klicken auf **Weiter**. Informationen zum Teilnehmen an einem Netz finden Sie in Schritt 5 unter [Am Netz teilnehmen](#joining-a-network).
  Jetzt sind Sie bereit, Ihr Starter Plan-Netz mit der Standardkonfiguration zu verwenden. Das Netz wird mit einem Anordnungsknoten ("orderer"), der als "SOLO"-Anordnungsservice bezeichnet wird, zwei Organisationen, einer Zertifizierungsstelle (CA) und einem Peer pro Organisation ausgeführt. Darüber hinaus wird ein Standardkanal erstellt.
6. Klicken Sie auf die Schaltfläche **Starten**.

Sie finden Ihre Blockchain-Serviceinstanz in Ihrem [{{site.data.keyword.Bluemix_notm}}-Service-Dashboard ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://console.bluemix.net/dashboard/services "{{site.data.keyword.Bluemix_notm}}-Service-Dashboard").


## Mitglieder einladen
Sie können andere [Organisationen](glossary.html#organization) zur Teilnahme an Ihrem Starter Plan-Netz als [Mitglieder](glossary.html#member) einladen, sodass Sie [Transaktionen](glossary.html#transaction) miteinander ausführen können. Wenn Sie den Starter Plan zum Erwerb von Kenntnissen und zu Testzwecken verwenden möchten, können Sie ein Netz mit mehreren Organisationen simulieren, indem Sie dem Netz selbst weitere Mitglieder hinzufügen.

1. Klicken Sie in der Anzeige "Mitglieder" Ihres Network Monitor auf die Schaltfläche **Mitglieder einladen**.
2. Das Fenster "Mitglied einladen" wird geöffnet.
    - Wenn Sie eine andere Organisation einladen möchten, wählen Sie "Mitglied einladen" aus.  Geben Sie den Namen und die E-Mail-Adresse des Operators der Organisation an, die Sie einladen wollen.  Sie können darüber hinaus zusätzliche Informationen, die Ihrer Einladung beigefügt werden sollen, in das Feld "Hinweis hinzufügen" eingeben.  Klicken Sie auf die Schaltfläche **Einladung senden**.  Die eingeladene Organisation empfängt eine Einladungs-E-Mail und kann anschließend die Anweisungen in der E-Mail ausführen, um an Ihrem Netz teilzunehmen.
    - Wenn Sie weitere Organisationen hinzufügen wollen, die einem Kanal hinzugefügt werden können, wählen Sie "Mitglied hinzufügen" aus.  Geben Sie einen Namen für Ihre neue Organisation an. Sie können Ihrer neuen Organisation optional Peers hinzufügen oder dies später im Network Monitor tun.  Klicken Sie auf die Schaltfläche **Erstellen**. Beachten Sie, dass Sie nach dem Hinzufügen von Peers für Ihre neue Organisation zu dieser neuen Organisation wechseln müssen, um Ihre Peers anzuzeigen. Weitere Informationen zum Wechseln von Organisationen finden Sie unter [Organisationen wechseln](dashboard.html#switch-organizations).


## Am Netz teilnehmen
Wenn Sie von einem Netzinitiator eingeladen werden, empfangen Sie eine E-Mail-Benachrichtigung mit Anweisungen, wie Sie an dem Netz teilnehmen. Führen Sie die Anweisungen in der E-Mail aus, sodass Sie zu einem der Mitglieder in dem Netz werden.

Sie müssen eine [{{site.data.keyword.blockchain}} Platform-Serviceinstanz ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/blockchain) mit der Option der Starter Plan-Mitgliedschaft in {{site.data.keyword.cloud_notm}} erstellen.

1. Melden Sie sich mit Ihrem {{site.data.keyword.cloud_notm}}-Konto an. Wenn Sie kein Konto haben, klicken Sie auf die Schaltfläche **Für Erstellung registrieren**.
2. Wählen Sie die Cloud Foundry-Organisation aus, in der Sie Ihr {{site.data.keyword.blockchain}}-Netz speichern wollen.
3. Wählen Sie **Starter-Mitgliedschaftsplan** in der Tabelle mit den Preisstrukturplänen aus. 
4. Klicken Sie auf die Schaltfläche **Erstellen**. Die Seite der Serviceinstanz wird mit einer Popup-Eingangsanzeige geöffnet. Beachten Sie, dass Sie auswählen können, an einem Netz teilzunehmen oder mit dem Erstellen eines eigenen Netzes fortzufahren. Informationen zum Erstellen eines Netzes finden Sie in Schritt 4 unter [Netz erstellen](#creating-a-network).
5. Wählen Sie in der Eingangsanzeige die Option **An vorhandenem Netz teilnehmen** aus, wählen Sie das Netz, an dem Sie teilnehmen wollen, in der Dropdown-Liste aus, und klicken Sie auf **Weiter**.

Sie finden Ihre Blockchain-Serviceinstanz im [{{site.data.keyword.cloud_notm}}-Service-Dashboard ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://console.bluemix.net/dashboard/services "{{site.data.keyword.cloud_notm}}-Service-Dashboard").

<!--
## Creating channels
You can create a [channel](glossary.html#channel) in your network and invite other organizations to your channel.  For more information on creating channels, see [Creating a channel](howto/create_channel.html#creating-a-channel).
-->
<!--
## Installing and instantiating your chaincode
You can run [chaincode](glossary.html#chaincode) on your peers in the network.  For more information about deploying pre-built samples, see [Installing, instantiating, and updating a chaincode](howto/install_instantiate_chaincode.html).
-->


## Netzberechtigungsnachweise und Verbindungsprofil abrufen
Wenn Sie ein Starter Plan-Netz in {{site.data.keyword.cloud_notm}} erstellt haben, können Sie die Netzberechtigungsnachweise und das Verbindungsprofil auf der Serviceinstanzseite oder im Network Monitor abrufen.

### Von der Serviceinstanzseite abrufen
Sie befinden sich auf der Serviceinstanzseite gleich, nachdem Sie eine Serviceinstanz erstellt haben. Sie können auch auf Ihren Service im [{{site.data.keyword.cloud_notm}}-Service-Dashboard ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://console.bluemix.net/dashboard/services "{{site.data.keyword.cloud_notm}}-Service-Dashboard") klicken, um Ihre Serviceinstanzseite zu öffnen.

Führen Sie die folgenden Schritte aus, um Ihre Serviceberechtigungsnachweise abzurufen:
1. Klicken Sie auf der Serviceinstanzseite auf **Serviceberechtigungsnachweise** im Navigator auf der linken Seite, um die Anzeige "Serviceberechtigungsnachweise" anzuzeigen.
2. Klicken Sie auf **Neuer Berechtigungsnachweis** in der Anzeige "Serviceberechtigungsnachweise".
3. Geben Sie in der Anzeige "Neuen Berechtigungsnachweis hinzufügen" dem Berechtigungsnachweis einen Namen und geben Sie im Feld "Inline-Konfigurationsparameter hinzufügen' **{"type": "service_instance_token"}** ein. Klicken Sie auf **Hinzufügen**. Der neue Berechtigungsnachweis wird in der Tabelle hinzugefügt. Sie können auf **Berechtigungsnachweise anzeigen** in der Spalte "AKTIONEN" klicken, um die Berechtigungsnachweisdetails anzuzeigen. Dieser Berechtigungsnachweis enthält den API-Schlüssel und den geheimen Schlüssel ('secret'), mit denen Sie APIs berechtigen können.  

![Netzberechtigungsnachweise abrufen](images/service_credentials.gif "Netzberechtigungsnachweise abrufen")

### Im Network Monitor abrufen
Sie finden die Netzberechtigungsnachweise in der Anzeige "APIs" in Ihrem Network Monitor. Weitere Informationen zur Verwendung von APIs finden Sie in [Swagger-APIs verwenden, um mit dem Netz zu interagieren](howto/swagger_apis.html).

Sie können das Verbindungsprofil in der Anzeige "Übersicht" im Network Monitor abrufen. Klicken Sie auf die Schaltfläche **Verbindungsprofil** in der Anzeige "Übersicht". Das Verbindungsprofil wird auf einer neuen Seite angezeigt.


## Beispielanwendungen bereitstellen
Der Starter Plan bietet Ihnen die Möglichkeit, Beispielanwendungen mit wenigen Klicks in Ihrem Netz bereitzustellen. Mithilfe der Beispielanwendungen können Sie sich auf einfache Weise mit der Funktionsweise von Chaincode und Anwendungen in einem Netz vertraut machen.

Weitere Informationen finden Sie unter [Beispielanwendungen bereitstellen](howto/prebuilt_samples.html).


## Angepasste Unternehmensnetze entwickeln und bereitstellen
Der Starter Plan integriert die Entwicklerumgebung von IBM Blockchain Platform: Develop und das Hyperledger Composer-Entwicklertoolset. Sie können Ihr Blockchain-Netz Ihren Geschäftsanforderungen entsprechend entwickeln.  Wenn Sie ein Netz für Ihr Unternehmen entwickelt haben, können Sie Ihr Unternehmensnetz im Starter Plan-Netz bereitstellen.

Weitere Informationen finden Sie unter [Netz entwickeln](develop.html) und [Unternehmensnetze mit dem Starter Plan bereitstellen](develop_starter.html).


## Netzressourcen überwachen
Wenn Ihre Anwendung eine Transaktion anfordert, können Sie Informationen zum Transaktionsstatus im Network Monitor anzeigen. Weitere Informationen zur Netzüberwachung finden Sie unter [Netz überwachen](howto/monitor_network.html).


## Netz zurücksetzen
Wenn Sie angepasste Konfigurationen, aktiven Chaincode oder bereitgestellte Anwendungen bereinigen wollen, können Sie Ihr Netz auf die Standarderstkonfiguration zurücksetzen.  Weitere Informationen finden Sie unter [Netz zurücksetzen](dashboard.html#reset-network).


## Von Starter Plan auf Enterprise Plan migrieren
{: #migrate}

Sie können eine BNA-Datei (`.bna`), Chaincode und Anwendungen, die Sie in einem Starter Plan-Netz testen, in einem Enterprise Plan-Netz bereitstellen.

Wenn Sie Ihre Unternehmensnetzarchivdatei (`.bna`) zur Hand haben, befolgen Sie die Anweisungen zur [Bereitstellung eines Unternehmensnetzes im Enterprise Plan](./develop_enterprise.html). Wenn Ihre `.bna`-Datei noch nicht vorliegt, rufen Sie sie aus der Starter Plan-Instanz mit dem Befehl `composer network download` ab. Weitere Informationen zum Befehl `composer network download` finden Sie in der [Hyperledger Composer-Befehlszeilendokumentation ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://hyperledger.github.io/composer/latest/reference/commands){:new_window}.

Chaincode, der `.bna`-Dateien ähnlich ist, wird extern entwickelt. Wenn Sie Chaincode, den Sie in einem Starter Plan-Netz testen, im Enterprise Plan bereitstellen wollen, führen Sie die Anweisungen unter [Chaincode installieren, instanziieren und aktualisieren](howto/install_instantiate_chaincode.html#installchaincode) aus.

<!--
As you can see in [Deploying sample applications](howto/prebuilt_samples.html), Starter Plan makes it easy to get a sample application integrated with your network by using Toolchain. This setup also allows for continuous integration by automatically updating your sample application whenever your forked application repo is changed. If you want to deploy this application into an Enterprise Plan network, you can copy your forked application repo into a new repo and then follow the instructions in [Deploying sample applications manually](howto/prebuilt_samples.html#deploy_sample_applications_manually).
-->

Wenn Sie eine Beispielanwendung in Ihrem Starter Plan-Netz bereitstellen und diese Anwendung in einem Enterprise Plan-Netz bereitgestellt werden soll, können Sie Ihr Anwendungsrepository mit Verzweigungen in ein neues Repository kopieren und anschließend die Anweisungen ausführen, die unter [Beispielanwendungen manuell bereitstellen](howto/prebuilt_samples.html#deploy_sample_applications_manually) beschrieben sind. 


## Netz löschen oder verlassen
Wenn Sie ein Netz löschen oder verlassen möchten, können Sie die Blockchain-Serviceinstanz aus Ihrem {{site.data.keyword.cloud_notm}}-Dashboard löschen.

**Hinweis**: Bevor Sie ein Netz verlassen, müssen Sie sicherstellen, dass Sie kein Mitglied eines Kanals des Netzes sind. Andernfalls treten Fehler auf, wenn Sie das Netz verlassen. Für das Entfernen eines Kanalmitglieds ist das Ausführen des Kanalaktualisierungsprozesses erforderlich. Weitere Informationen zum Kanalaktualisierungsprozess finden Sie in [Kanal aktualisieren](howto/create_channel.html#updating-a-channel).


<!--
## References
* For more information about {{site.data.keyword.blockchainfull_notm}} offerings, see [Blockchain offerings](index.html).
* For more information about Hyperledger Fabric V1.1, see [Hyperledger Fabric documentation ![External link icon](images/external_link.svg "External link icon")](http://hyperledger-fabric.readthedocs.io/en/latest/){:new_window}.
-->
