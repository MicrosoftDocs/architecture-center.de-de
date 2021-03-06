---
title: Ausführen einer hoch verfügbaren SharePoint Server 2016-Farm in Azure
titleSuffix: Azure Reference Architectures
description: Empfohlene Architektur zum Bereitstellen einer SharePoint Server 2016-Hochverfügbarkeitsfarm in Azure
author: njray
ms.date: 07/26/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18
ms.openlocfilehash: 0cce207dedd0eb42e29a152b3bb84acc2dca323a
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58344987"
---
# <a name="run-a-highly-available-sharepoint-server-2016-farm-in-azure"></a>Ausführen einer hoch verfügbaren SharePoint Server 2016-Farm in Azure

In dieser Referenzarchitektur werden bewährte Methoden zum Bereitstellen einer SharePoint Server 2016-Hochverfügbarkeitsfarm in Azure mit der MinRole-Topologie und SQL Server AlwaysOn-Verfügbarkeitsgruppen veranschaulicht. Die SharePoint-Farm wird in einem geschützten virtuellen Netzwerk ohne Internetendpunkt bzw. -präsenz bereitgestellt. [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

![Referenzarchitektur für eine hoch verfügbare SharePoint Server 2016-Farm in Azure](./images/sharepoint-ha.png)

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

## <a name="architecture"></a>Architecture

Diese Architektur baut auf der Architektur auf, die unter [Run Windows VMs for an N-tier application][windows-n-tier] (Ausführen von Windows-VMs für eine n-schichtige Anwendung) dargestellt ist. Eine SharePoint Server 2016-Hochverfügbarkeitsfarm wird in einem virtuellen Azure-Netzwerk (VNet) bereitgestellt. Diese Architektur ist für eine Test- oder Produktionsumgebung, eine SharePoint-Hybridinfrastruktur mit Office 365 oder als Basis für ein Szenario für die Notfallwiederherstellung geeignet.

Die Architektur umfasst die folgenden Komponenten:

- **Ressourcengruppen**: Eine [Ressourcengruppe][resource-group] ist ein Container, der verwandte Azure-Ressourcen enthält. Eine Ressourcengruppe wird für die SharePoint-Server verwendet, und eine andere Ressourcengruppe wird für Infrastrukturkomponenten genutzt, die unabhängig von VMs sind, z.B. das virtuelle Netzwerk und Lastenausgleichsmodule.

- **Virtuelles Netzwerk (VNET)**. Die VMs werden in einem VNet mit einem eindeutigen Intranetadressraum bereitgestellt. Das VNet ist in Subnetze unterteilt.

- **Virtuelle Computer (Virtual Machines, VMs)**: Die VMs werden im VNet bereitgestellt, und private statische IP-Adressen werden allen VMs zugewiesen. Statische IP-Adressen sind für die VMs zu empfehlen, auf denen SQL Server und SharePoint Server 2016 ausgeführt werden, um Probleme mit der Zwischenspeicherung von IP-Adressen und Adressänderungen nach einem Neustart zu vermeiden.

- **Verfügbarkeitsgruppen**: Ordnen Sie die VMs für jede SharePoint-Rolle in separaten [Verfügbarkeitsgruppen][availability-set] an, und stellen Sie mindestens zwei virtuelle Computer (VMs) für jede Rolle bereit. Auf diese Weise können die VMs für eine höhere Vereinbarung zum Servicelevel (SLA) genutzt werden.

- **Interner Load Balancer**. Über den [Lastenausgleich][load-balancer] wird Datenverkehr in Form von SharePoint-Anforderungen aus dem lokalen Netzwerk auf die Front-End-Webserver der SharePoint-Farm verteilt.

- **Netzwerksicherheitsgruppen (NSGs)**: Für jedes Subnetz, das virtuelle Computer enthält, wird eine [Netzwerksicherheitsgruppe][nsg] erstellt. Verwenden Sie NSGs zum Beschränken von Netzwerkdatenverkehr im VNet, um Subnetze zu isolieren.

- **Gateway**: Das Gateway stellt eine Verbindung zwischen Ihrem lokalen Netzwerk und dem virtuellen Azure-Netzwerk her. Hierfür kann eine ExpressRoute- oder eine Site-to-Site-VPN-Verbindung verwendet werden. Weitere Informationen finden Sie unter [Connect an on-premises network to Azure][hybrid-ra] (Verbinden eines lokalen Netzwerks mit Azure).

- **Windows Server Active Directory-Domänencontroller**: Anhand dieser Referenzarchitektur werden Windows Server AD-Domänencontroller bereitgestellt. Diese Domänencontroller werden im Azure VNet ausgeführt und verfügen über eine Vertrauensstellung mit der lokalen Windows Server AD-Gesamtstruktur. Clientwebanforderungen für Ressourcen von SharePoint-Farmen werden im VNet authentifiziert, anstatt den Datenverkehr des Authentifizierungsvorgangs über die Gatewayverbindung an das lokale Netzwerk zu senden. In DNS werden A- oder CNAME-Intraneteinträge erstellt, damit Intranetbenutzer den Namen der SharePoint-Farm in die private IP-Adresse des internen Lastenausgleichs auflösen können.

  SharePoint Server 2016 unterstützt auch die Verwendung [Azure Active Directory Domain Services](/azure/active-directory-domain-services/). Azure AD Domain Services stellt verwaltete Domänendienste bereit, sodass Sie keine Domänencontroller in Azure bereitstellen und verwalten müssen.

- **SQL Server Always On-Verfügbarkeitsgruppe**. Wir empfehlen die Verwendung von [SQL Server Always On-Verfügbarkeitsgruppen][sql-always-on], um für die SQL Server-Datenbank Hochverfügbarkeit zu erzielen. Zwei virtuelle Computer werden für SQL Server verwendet. Einer enthält das primäre Datenbankreplikat, und der andere enthält das sekundäre Replikat.

- **Hauptknoten-VM**: Mit dieser VM kann der Failovercluster ein Quorum festlegen. Weitere Informationen finden Sie unter [Grundlegendes zu Quorumkonfigurationen in einem Failovercluster][sql-quorum].

- **SharePoint-Server**: Die SharePoint-Server übernehmen die Rollen für Web-Front-End, Caching, Anwendung und Suche.

- **Jumpbox**: Wird auch als [geschützter Host][bastion-host] bezeichnet. Dies ist eine geschützte VM im Netzwerk, die von Administratoren zum Herstellen der Verbindung mit anderen VMs verwendet wird. Die Jumpbox verfügt über eine NSG, bei der Remotedatenverkehr nur von öffentlichen IP-Adressen zugelassen wird, die in einer Liste mit sicheren Adressen enthalten sind. Die NSG sollte Remotedesktop-Datenverkehr (RDP) zulassen.

## <a name="recommendations"></a>Empfehlungen

Ihre Anforderungen können von der hier beschriebenen Architektur abweichen. Verwenden Sie diese Empfehlungen als Startpunkt.

### <a name="resource-group-recommendations"></a>Empfehlungen für Ressourcengruppen

Wir empfehlen Ihnen, Ressourcengruppen nach der Serverrolle zu trennen und eine separate Ressourcengruppe für Infrastrukturkomponenten zu verwenden, bei denen es sich um globale Ressourcen handelt. Bei dieser Architektur bilden die SharePoint-Ressourcen eine Gruppe und SQL Server und andere Hilfsprogrammobjekte die andere Gruppe.

### <a name="virtual-network-and-subnet-recommendations"></a>Empfehlungen für virtuelle Netzwerke und Subnetze

Verwenden Sie ein Subnetz für jede SharePoint-Rolle sowie zusätzlich jeweils ein Subnetz für das Gateway und für die Jumpbox.

Das Gatewaysubnetz muss den Namen *GatewaySubnet* haben. Weisen Sie den Gatewaysubnetz-Adressraum basierend auf dem letzten Teil des VNet-Adressraums zu. Weitere Informationen finden Sie unter [Connect an on-premises network to Azure using a VPN gateway][hybrid-vpn-ra] (Verbinden eines lokalen Netzwerks mit Azure per VPN Gateway).

### <a name="vm-recommendations"></a>Empfehlungen für virtuelle Computer

Für diese Architektur sind mindestens 44 Kerne erforderlich:

- 8 SharePoint-Server mit Standard_DS3_v2 (jeweils vier Kerne) = 32 Kerne
- 2 Active Directory-Domänencontroller mit Standard_DS1_v2 (jeweils ein Kern) = 2 Kerne
- 2 SQL Server-VMs mit Standard_DS3_v2 = 8 Kerne
- 1 Hauptknoten mit Standard_DS1_v2 = 1 Kern
- 1 Verwaltungsserver mit Standard_DS1_v2 = 1 Kern

Stellen Sie sicher, dass Ihr Azure-Abonnement ein ausreichendes Kontingent von VM-Kernen für die Bereitstellung aufweist, da bei der Bereitstellung sonst ein Fehler auftritt. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen][quotas].

Für alle SharePoint-Rollen mit Ausnahme des Suchindexers empfehlen wir die Verwendung der VM-Größe [Standard_DS3_v2][vm-sizes-general]. Die Indexerstellung für die Suche sollte mindestens die Größe [Standard_DS13_v2][vm-sizes-memory] haben. In einer Testbereitstellung ist in den Parameterdateien für diese Referenzarchitektur die kleinere Größe DS3_v2 für die Suchindexerrolle angegeben. Aktualisieren Sie für eine Produktionsbereitstellung die Parameterdateien, um mindestens die Größe DS13 zu verwenden. Weitere Informationen finden Sie unter [Hardware- und Softwareanforderungen für SharePoint Server 2016][sharepoint-reqs].

Für die virtuellen SQL Server-Computer werden mindestens vier Kerne und acht GB RAM empfohlen. In den Parameterdateien für diese Referenzarchitektur ist die Größe DS3_v2 angegeben. Für eine Produktionsbereitstellung müssen Sie unter Umständen eine größere VM-Größe angeben. Weitere Informationen finden Sie unter [Speicher- und SQL Server-Kapazitätsplanung und -Konfiguration (SharePoint Server)](/sharepoint/administration/storage-and-sql-server-capacity-planning-and-configuration#estimate-memory-requirements).

### <a name="nsg-recommendations"></a>NSG-Empfehlungen

Wir empfehlen Ihnen die Verwendung von einer NSG pro Subnetz, in dem VMs enthalten sind, um die Subnetzisolation zu ermöglichen. Gehen Sie wie folgt vor, wenn Sie die Subnetzisolation konfigurieren möchten: Fügen Sie NSG-Regeln hinzu, mit denen der zulässige bzw. unzulässige ein- und ausgehende Datenverkehr für jedes Subnetz definiert wird. Weitere Informationen finden Sie unter [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen][virtual-networks-nsg].

Weisen Sie dem Gatewaysubnetz keine NSG zu, da das Gateway sonst nicht mehr funktioniert.

### <a name="storage-recommendations"></a>Empfehlungen für Speicher

Die Speicherkonfiguration für die VMs der Farm sollte den jeweiligen bewährten Methoden entsprechen, die für lokale Bereitstellungen verwendet werden. SharePoint-Server sollten über einen separaten Datenträger für Protokolle verfügen. Für SharePoint-Server, die Suchindexrollen hosten, wird zusätzlicher Speicherplatz zum Speichern des Suchindex benötigt. Für SQL Server besteht das Standardverfahren darin, Daten und Protokolle zu trennen. Fügen Sie weitere Datenträger für die Speicherung von Datenbanksicherungen hinzu, und verwenden Sie einen separaten Datenträger für [tempdb][tempdb].

Wir empfehlen die Verwendung von [Azure Managed Disks][managed-disks], um die beste Zuverlässigkeit zu erzielen. Mit Managed Disks (verwalteten Datenträgern) stellen Sie sicher, dass die Datenträger für VMs in einer Verfügbarkeitsgruppe isoliert sind, um Single Points of Failure zu vermeiden.

> [!NOTE]
> Derzeit werden für die Resource Manager-Vorlage für diese Referenzarchitektur keine verwalteten Datenträger verwendet. Es ist geplant, die Vorlage zu aktualisieren, um die Verwendung von verwalteten Datenträgern zu ermöglichen.

Verwenden Sie verwaltete Premium-Datenträger für alle SharePoint- und SQL Server-VMs. Sie können verwaltete Standard-Datenträger für den Hauptknotenserver, die Domänencontroller und den Verwaltungsserver nutzen.

### <a name="sharepoint-server-recommendations"></a>SharePoint Server-Empfehlungen

Stellen Sie vor dem Konfigurieren der SharePoint-Farm sicher, dass Sie über ein Windows Server Active Directory-Dienstkonto pro Dienst verfügen. Für diese Architektur benötigen Sie mindestens die folgenden Konten auf Domänenebene, um die Berechtigungen pro Rolle zu isolieren:

- SQL Server-Dienstkonto
- Konto für Setupbenutzer
- Konto für Serverfarm
- Konto für Suchdienst
- Konto für Inhaltszugriff
- Web-App-Pool-Konten
- Service App-Pool-Konten
- Administratorkonto für Cache
- Lese-Administratorkonto für Cache

Achten Sie darauf, die Search-Architektur richtig zu planen, um die Supportanforderung für den Datenträgerdurchsatz von mindestens 200 MB pro Sekunde zu erfüllen. Informationen hierzu finden Sie unter [Planen der Architektur der Unternehmenssuche in SharePoint Server 2013][sharepoint-search]. Halten Sie sich auch an die Richtlinien unter [Best practices for crawling in SharePoint Server 2016][sharepoint-crawling] (Bewährte Methoden zum Durchsuchen per Crawler in SharePoint Server 2016).

Speichern Sie außerdem die Suchkomponentendaten auf einem separaten Speichervolume oder einer Partition mit hoher Leistung. Konfigurieren Sie die Objektcache-Benutzerkonten, die in dieser Architektur erforderlich sind, um die Last zu reduzieren und den Durchsatz zu verbessern. Teilen Sie die Dateien des Windows Server-Betriebssystems, die SharePoint Server 2016-Programmdateien und die Diagnoseprotokolle auf drei separate Speichervolumes oder Partitionen mit normaler Leistung auf.

Weitere Informationen zu diesen Empfehlungen finden Sie unter [Erstmalige Bereitstellung von Administrator- und Dienstkonten in SharePoint Server][sharepoint-accounts].

### <a name="hybrid-workloads"></a>Hybrid-Workloads

Mit dieser Referenzarchitektur wird eine SharePoint Server 2016-Farm bereitgestellt, die als [SharePoint-Hybridumgebung][sharepoint-hybrid] zur Erweiterung von SharePoint Server 2016 auf Office 365 SharePoint Online verwendet werden kann. Wenn Sie über Office Online Server verfügen, helfen Ihnen die Informationen unter [Office Web Apps und Office Online Server-Unterstützung in Azure][office-web-apps] weiter.

Die Standarddienstanwendungen dieser Bereitstellung sind für die Unterstützung von Hybrid-Workloads ausgelegt. Alle SharePoint Server 2016- und Office 365-Hybrid-Workloads können bis auf eine Ausnahme ohne Änderungen der SharePoint-Infrastruktur für diese Farm bereitgestellt werden: Die Hybrid-Suchdienstanwendung für die Cloud darf nicht auf Servern bereitgestellt werden, die eine vorhandene Suchtopologie hosten. Aus diesem Grund muss der Farm mindestens eine VM hinzugefügt werden, die auf Suchrollen basiert, um dieses Hybridszenario zu unterstützen.

### <a name="sql-server-always-on-availability-groups"></a>SQL Server Always On-Verfügbarkeitsgruppen

Für diese Architektur werden virtuelle SQL Server-Computer genutzt, da Azure SQL-Datenbank für SharePoint Server 2016 nicht verwendet werden kann. Zur Unterstützung der Hochverfügbarkeit in SQL Server empfehlen wir die Verwendung von Always On-Verfügbarkeitsgruppen, mit denen eine Gruppe von Datenbanken angegeben wird, für die das Failover zusammen durchgeführt wird. Dies führt dazu, dass sie hoch verfügbar und wiederherstellbar sind. In dieser Referenzarchitektur werden die Datenbanken während der Bereitstellung erstellt, aber Sie müssen Always On-Verfügbarkeitsgruppen manuell aktivieren und die SharePoint-Datenbanken einer Verfügbarkeitsgruppe hinzufügen. Weitere Informationen finden Sie unter [Erstellen der Verfügbarkeitsgruppe und Hinzufügen der SharePoint-Datenbanken][create-availability-group].

Außerdem wird empfohlen, dem Cluster eine Listener-IP-Adresse hinzuzufügen, bei der es sich um die private IP-Adresse des internen Lastenausgleichs für die virtuellen SQL Server-Computer handelt.

Die empfohlenen VM-Größen und andere Leistungsempfehlungen für SQL Server in Azure finden Sie unter [Bewährte Methoden zur Leistung für SQL Server auf virtuellen Azure-Computern][sql-performance]. Halten Sie sich auch an die Empfehlungen unter [Bewährte Methoden für SQL Server in einer SharePoint Server-Farm][sql-sharepoint-best-practices].

Es wird empfohlen, den Hauptknotenserver auf einem andern Computer als die Replikationspartner anzuordnen. Mit dem Server kann der sekundäre Replikationspartnerserver in einer Hochsicherheitsmodus-Sitzung erkennen, ob ein automatisches Failover initiiert werden soll. Im Gegensatz zu den beiden Partnern dient der Hauptknotenserver nicht als Server für die Datenbank, sondern unterstützt das automatische Failover.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Ändern Sie einfach die VM-Größe, um die vorhandenen Server zentral hochzuskalieren.

Mit der Funktion [MinRoles][minroles] in SharePoint Server 2016 können Sie Server basierend auf der Rolle des Servers horizontal hochskalieren und außerdem Server aus einer Rolle entfernen. Wenn Sie die Server einer Rolle hinzufügen, können Sie beliebige einzelne Rollen oder eine kombinierte Rolle angeben. Sie müssen die Suchtopologie aber mit PowerShell neu konfigurieren, wenn Sie der Search-Rolle Server hinzufügen. Sie können Rollen auch mit MinRoles konvertieren. Weitere Informationen finden Sie unter [Verwalten einer MinRole-Serverfarm in SharePoint Server 2016][sharepoint-minrole].

Beachten Sie, dass SharePoint Server 2016 die Verwendung von VM-Skalierungsgruppen für die automatische Skalierung nicht unterstützt.

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Diese Referenzarchitektur unterstützt Hochverfügbarkeit in einer Azure-Region, da für jede Rolle in einer Verfügbarkeitsgruppe mindestens zwei VMs bereitgestellt werden.

Erstellen Sie als Schutz vor einem regionalen Ausfall eine separate Farm für die Notfallwiederherstellung in einer anderen Azure-Region. Ihre Recovery Time Objectives (RTOs) und Recovery Point Objectives (RPOs) bestimmen die Anforderungen für die Einrichtung. Ausführliche Informationen finden Sie unter [Wählen einer Notfallwiederherstellungsstrategie für SharePoint Server][sharepoint-dr]. Die sekundäre Region muss mit der primären Region *gekoppelt* sein und ein Regionspaar bilden. Im Falle eines umfassenden Ausfalls hat bei jedem Regionspaar die Wiederherstellung einer der Regionen Priorität. Weitere Informationen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare][paired-regions].

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

Befolgen Sie die empfohlenen Methoden für SharePoint-Vorgänge zum Betreiben und Verwalten von Servern, Serverfarmen und Sites. Weitere Informationen finden Sie unter [Vorgänge für SharePoint Server][sharepoint-ops].

Die erforderlichen Aufgaben beim Verwalten von SQL Server in einer SharePoint-Umgebung können sich von den Aufgaben unterscheiden, die normalerweise für eine Datenbankanwendung anfallen. Eine bewährte Methode ist das vollständige Sichern aller SQL-Datenbanken einmal pro Woche mit inkrementellen Sicherungen jede Nacht. Sichern Sie Transaktionsprotokolle alle 15 Minuten. Eine andere Methode ist das Implementieren von SQL Server-Wartungsaufgaben in den Datenbanken, während die integrierten SharePoint-Aufgaben deaktiviert werden. Weitere Informationen finden Sie unter [Speicher- und SQL Server-Kapazitätsplanung und -Konfiguration (SharePoint Server)][sql-server-capacity-planning].

## <a name="security-considerations"></a>Sicherheitshinweise

Für die Dienstkonten der Domänenebene zum Ausführen von SharePoint Server 2016 sind Windows Server AD-Domänencontroller für den Domänenbeitritt und die Authentifizierung erforderlich. Azure Active Directory Domain Services kann für diesen Zweck nicht verwendet werden. Zum Erweitern der Windows Server AD-Identitätsinfrastruktur, die im Intranet bereits vorhanden ist, nutzt diese Architektur zwei Windows Server AD-Replikatdomänencontroller einer vorhandenen lokalen Windows Server AD-Gesamtstruktur.

Außerdem ist es immer ratsam, mit Blick auf die Sicherheitshärtung zu planen. Weitere Empfehlungen sind:

- Fügen Sie NSGs Regeln hinzu, um Subnetze und Rollen zu isolieren.
- Vermeiden Sie es, VMs öffentliche IP-Adressen zuzuweisen.
- Erwägen Sie für die Angriffserkennung und die Analyse von Nutzlasten die Verwendung eines virtuellen Netzwerkgeräts vor den Front-End-Webservern anstelle eines internen Azure Load Balancers.
- Sie können optional IPsec-Richtlinien für die Verschlüsselung von Klartext-Datenverkehr zwischen Servern verwenden. Wenn Sie auch die Subnetzisolation durchführen, sollten Sie Ihre Netzwerksicherheitsgruppen-Regeln aktualisieren, um IPsec-Datenverkehr zuzulassen.
- Installieren Sie Antischadsoftware-Agents für die VMs.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][github] verfügbar. Der gesamte Bereitstellungsvorgang kann mehrere Stunden in Anspruch nehmen.

Die Bereitstellung erstellt die folgenden Ressourcengruppen in Ihrem Abonnement:

- ra-onprem-sp2016-rg
- ra-sp2016-network-rg

Die Vorlagenparameterdateien beziehen sich auf diese Namen, d.h. wenn Sie sie ändern, müssen Sie die Parameterdateien entsprechend aktualisieren.

Die Parameterdateien enthalten an verschiedenen Stellen ein hartcodiertes Kennwort. Ändern Sie diese Werte vor der Bereitstellung.

### <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deployment-steps"></a>Bereitstellungsschritte

1. Führen Sie den folgenden Befehl aus, um ein simuliertes lokales Netzwerk bereitzustellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p onprem.json --deploy
    ```

2. Führen Sie den folgenden Befehl aus, um das Azure-VNET und das VPN-Gateway bereitzustellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p connections.json --deploy
    ```

3. Führen Sie den folgenden Befehl aus, um die Jumpbox, AD-Domänencontroller und virtuelle SQL Server-Computer bereitzustellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure1.json --deploy
    ```

4. Führen Sie den folgenden Befehl aus, um den Failovercluster und die Verfügbarkeitsgruppe zu erstellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure2-cluster.json --deploy
    ```

5. Führen Sie den folgenden Befehl aus, um die übrigen virtuellen Computer bereitzustellen:

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure3.json --deploy
    ```

An diesem Punkt stellen Sie sicher, dass Sie für die SQL Server Always On-Verfügbarkeitsgruppe eine TCP-Verbindung vom Web-Front-End zum Lastenausgleich herstellen können. Führen Sie dazu die folgenden Schritte aus:

1. Suchen Sie im Azure-Portal die VM namens `ra-sp-jb-vm1` in der `ra-sp2016-network-rg`-Ressourcengruppe. Dies ist der virtuelle Jumpbox-Computer.

2. Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen. Verwenden Sie das Kennwort, das Sie in der `azure1.json`-Parameterdatei angegeben haben.

3. Melden Sie sich in der Remotedesktopsitzung bei 10.0.5.4 an. Dies ist die IP-Adresse des virtuellen Computers mit dem Namen `ra-sp-app-vm1`.

4. Öffnen Sie auf dem virtuellen Computer eine PowerShell-Konsole, und verwenden Sie das Cmdlet `Test-NetConnection`, um sicherzustellen, dass Sie eine Verbindung mit dem Lastenausgleich herstellen können.

    ```powershell
    Test-NetConnection 10.0.3.100 -Port 1433
    ```

Die Ausgabe sollte in etwa wie folgt aussehen:

```console
ComputerName     : 10.0.3.100
RemoteAddress    : 10.0.3.100
RemotePort       : 1433
InterfaceAlias   : Ethernet 3
SourceAddress    : 10.0.0.132
TcpTestSucceeded : True
```

Falls ein Fehler auftritt, starten Sie mithilfe des Azure-Portals den virtuellen Computer namens `ra-sp-sql-vm2` neu. Nach dem Neustart des virtuellen Computers führen Sie den Befehl `Test-NetConnection` erneut aus. Möglicherweise müssen Sie nach dem Neustart des virtuellen Computers etwa eine Minute warten, bis die Verbindung erfolgreich ist.

Schließen Sie jetzt die Bereitstellung wie folgt ab.

1. Führen Sie den folgenden Befehl aus, um den primären Knoten der SharePoint-Farm bereitzustellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure4-sharepoint-server.json --deploy
    ```

2. Führen Sie den folgenden Befehl aus, um SharePoint-Cache, -Suche und -Web bereitzustellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure5-sharepoint-farm.json --deploy
    ```

3. Führen Sie den folgenden Befehl aus, um die NSG-Regeln zu erstellen.

    ```bash
    azbb -s <subscription_id> -g ra-onprem-sp2016-rg -l <location> -p azure6-security.json --deploy
    ```

### <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

1. Navigieren Sie im [Azure-Portal][azure-portal] zur Ressourcengruppe `ra-onprem-sp2016-rg`.

2. Wählen Sie in der Liste mit den Ressourcen die VM-Ressource mit dem Namen `ra-onpr-u-vm1`.

3. Stellen Sie eine Verbindung mit der VM her, wie unter [Herstellen der Verbindung mit dem virtuellen Computer][connect-to-vm] beschrieben. Der Benutzername lautet `\onpremuser`.

4. Nachdem die Remoteverbindung mit der VM eingerichtet wurde, können Sie auf der VM einen Browser öffnen und zu `http://portal.contoso.local` navigieren.

5. Melden Sie sich im Feld **Windows-Sicherheit** am SharePoint-Portal an, indem Sie `contoso.local\testuser` als Benutzername verwenden.

Bei dieser Anmeldung wird ein Tunnel von der Domäne „Fabrikam.com“, die vom lokalen Netzwerk genutzt wird, zur Domäne „contoso.local“ des SharePoint-Portals erstellt. Wenn die SharePoint-Website geöffnet wird, wird die Demo-Stammwebsite angezeigt.

**_Mitwirkende dieser Referenzarchitektur_**: Joe Davies, Bob Fox, Neil Hodgkinson, Paul Stork

<!-- links -->

[availability-set]: /azure/virtual-machines/windows/manage-availability
[azure-portal]: https://portal.azure.com
[azure-ps]: /powershell/azure/overview
[azure-pricing]: https://azure.microsoft.com/pricing/calculator/
[bastion-host]: https://en.wikipedia.org/wiki/Bastion_host
[create-availability-group]: /SharePoint/administration/sharepoint-intranet-farm-in-azure-phase-5-create-the-availability-group-and-add
[connect-to-vm]: /azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine
[github]: https://github.com/mspnp/reference-architectures
[hybrid-ra]: ../hybrid-networking/index.md
[hybrid-vpn-ra]: ../hybrid-networking/vpn.md
[load-balancer]: /azure/load-balancer/load-balancer-internal-overview
[managed-disks]: /azure/storage/storage-managed-disks-overview
[minroles]: https://technet.microsoft.com/library/mt346114(v=office.16).aspx
[nsg]: /azure/virtual-network/virtual-networks-nsg
[office-web-apps]: https://support.microsoft.com/help/3199955/office-web-apps-and-office-online-server-supportability-in-azure
[paired-regions]: /azure/best-practices-availability-paired-regions
[readme]: https://github.com/mspnp/reference-architectures/tree/master/sharepoint/sharepoint-2016
[resource-group]: /azure/azure-resource-manager/resource-group-overview
[quotas]: /azure/azure-subscription-service-limits
[sharepoint-accounts]: https://technet.microsoft.com/library/ee662513(v=office.16).aspx
[sharepoint-crawling]: https://technet.microsoft.com/library/dn535606(v=office.16).aspx
[sharepoint-dr]: https://technet.microsoft.com/library/ff628971(v=office.16).aspx
[sharepoint-hybrid]: https://aka.ms/sphybrid
[sharepoint-minrole]: https://technet.microsoft.com/library/mt743705(v=office.16).aspx
[sharepoint-ops]: https://technet.microsoft.com/library/cc262289(v=office.16).aspx
[sharepoint-reqs]: https://technet.microsoft.com/library/cc262485(v=office.16).aspx
[sharepoint-search]: https://technet.microsoft.com/library/dn342836.aspx
[sql-always-on]: /sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server
[sql-performance]: /azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance
[sql-server-capacity-planning]: https://technet.microsoft.com/library/cc298801(v=office.16).aspx
[sql-quorum]: https://technet.microsoft.com/library/cc731739(v=ws.11).aspx
[sql-sharepoint-best-practices]: https://technet.microsoft.com/library/hh292622(v=office.16).aspx
[tempdb]: /sql/relational-databases/databases/tempdb-database
[virtual-networks-nsg]: /azure/virtual-network/virtual-networks-nsg
[visio-download]: https://archcenter.blob.core.windows.net/cdn/Sharepoint-2016.vsdx
[vm-sizes-general]: /azure/virtual-machines/windows/sizes-general
[vm-sizes-memory]: /azure/virtual-machines/windows/sizes-memory
[windows-n-tier]: ../virtual-machines-windows/n-tier.md
