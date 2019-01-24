---
title: Bereitstellen hochverfügbarer virtueller Netzwerkgeräte
titleSuffix: Azure Reference Architectures
description: Stellen Sie virtuelle Netzwerkgeräte mit hoher Verfügbarkeit bereit.
author: telmosampaio
ms.date: 12/08/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, networking
ms.openlocfilehash: a0973fad14bd9b4e81ec9940c83b8ebb31e9599b
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486786"
---
# <a name="deploy-highly-available-network-virtual-appliances"></a>Bereitstellen hochverfügbarer virtueller Netzwerkgeräte

In diesem Artikel erfahren Sie, wie eine Reihe virtueller Netzwerkgeräte (NVAs) für hohe Verfügbarkeit in Azure bereitgestellt wird. Ein NVA wird normalerweise verwendet, um den Netzwerkdatenverkehrsfluss von einem Umkreisnetzwerk (auch als DMZ bekannt) zu anderen Netzwerken oder Subnetzen zu steuern. Informationen zum Implementieren einer DMZ in Azure finden Sie unter [Microsoft-Clouddienste und Netzwerksicherheit][cloud-security]. Der Artikel enthält Beispielarchitekturen für nur eingehenden, nur ausgehenden sowie ein- und ausgehenden Datenverkehr.

**Voraussetzungen:** In diesem Artikel werden grundlegende Kenntnisse über Azure-Netzwerke, [Azure-Lastenausgleichsmodule][lb-overview] und [benutzerdefinierte Routen][udr-overview] (User-Defined Routes, UDRs) vorausgesetzt.

## <a name="architecture-diagrams"></a>Architekturdiagramme

Ein NVA kann in einer DMZ in vielen verschiedenen Architekturen bereitgestellt werden. Die folgende Abbildung veranschaulicht beispielsweise die Verwendung eines [einzelnen NVA][nva-scenario] für eingehenden Datenverkehr.

![[0]][0]

In dieser Architektur stellt das NVA eine sichere Netzwerkgrenze dar, indem der gesamte eingehende und ausgehende Netzwerkdatenverkehr überprüft und nur der Datenverkehr weitergeleitet wird, der die Netzwerksicherheitsregeln erfüllt. Aufgrund der Tatsache, dass der gesamte Netzwerkdatenverkehr über das NVA erfolgen muss, stellt das NVA eine einzelne Fehlerquelle (Single Point of Failure) im Netzwerk dar. Tritt beim NVA ein Fehler auf, steht kein anderer Pfad für den Netzwerkdatenverkehr bereit und keines der Back-End-Subnetze ist verfügbar.

Um Hochverfügbarkeit für ein NVA zu gewährleisten, stellen Sie mehrere NCAs in einer Verfügbarkeitsgruppe bereit.

In den folgenden Architekturen sind die für hochverfügbare NVAs erforderlichen Ressourcen und die Konfiguration angegeben:

<!-- markdownlint-disable MD033 -->

| Lösung | Vorteile | Überlegungen |
| --- | --- | --- |
| [Eingehender Datenverkehr mit Layer-7-NVAs][ingress-with-layer-7] |Alle NVA-Knoten sind aktiv. |Erfordert ein NVA, das Verbindungen beenden und SNAT verwenden kann.<br/> Erfordert eine separate Gruppe von NVAs für den Datenverkehr aus dem Internet und von Azure. <br/> Kann nur für Datenverkehr verwendet werden, dessen Ursprung außerhalb von Azure liegt. |
| [Ausgehender Datenverkehr mit Layer-7-NVAs][egress-with-layer-7] |Alle NVA-Knoten sind aktiv. | Erfordert ein NVA, das Verbindungen beenden kann und die Übersetzung der Quellnetzwerkadresse (SNAT) implementiert.
| [Eingehender/ausgehender Datenverkehr mit Layer-7-NVAs][ingress-egress-with-layer-7] |Alle Knoten sind aktiv.<br/>Kann den aus Azure stammenden Datenverkehr abwickeln. |Erfordert ein NVA, das Verbindungen beenden und SNAT verwenden kann.<br/>Erfordert eine separate Gruppe von NVAs für den Datenverkehr aus dem Internet und von Azure. |
| [PIP/UDR-Switch][pip-udr-switch] |Einzelne Gruppe von NVAs für den gesamten Datenverkehr.<br/>Kann den gesamten Datenverkehr abwickeln (keine Beschränkung für Portregeln). |Aktiv/Passiv<br/>Erfordert einen Failoverprozess. |
| [PIP/UDR ohne SNAT](#pip-udr-nvas-without-snat) | Einzelne Gruppe von NVAs für den gesamten Datenverkehr.<br/>Kann den gesamten Datenverkehr abwickeln (keine Beschränkung für Portregeln).<br/>Erfordert keine SNAT-Konfiguration für eingehende Anforderungen. |Aktiv/Passiv<br/>Erfordert einen Failoverprozess.<br/>Sondierung und Failoverlogik werden außerhalb des virtuellen Netzwerks ausgeführt. |

<!-- markdown-enable MD033 -->

## <a name="ingress-with-layer-7-nvas"></a>Eingehender Datenverkehr mit Layer-7-NVAs

Die folgende Abbildung zeigt eine Hochverfügbarkeitsarchitektur, die eine DMZ für eingehenden Datenverkehr hinter einem Lastenausgleich mit Internetzugriff implementiert. Diese Architektur bietet Konnektivität für Azure-Workloads für den Layer-7-Datenverkehr, z. B. HTTP oder HTTPS:

![[1]][1]

Der Vorteil dieser Architektur ist, dass alle NVAs aktiv sind, und bei Ausfall eines NVA wird der Netzwerkdatenverkehr durch den Lastenausgleich an das andere NVA weitergeleitet. Beide NVAs leiten den Datenverkehr an den internen Lastenausgleich, sodass der Datenverkehr weiter fließt, solange ein NVA aktiv ist. Die NVAs sind erforderlich, um den für virtuelle Computer der Webebene vorgesehenen SSL-Datenverkehr zu beenden. Diese NVAs können nicht auf die Handhabung des lokalen Datenverkehrs ausgeweitet werden, da für den lokalen Datenverkehr eine andere dedizierte Gruppe von NVAs mit eigenen Netzwerkrouten erforderlich ist.

> [!NOTE]
> Diese Architektur wird in der Referenzarchitektur für die [DMZ zwischen Azure und Ihrem lokalen Rechenzentrum][dmz-on-prem] und der Referenzarchitektur für die [DMZ zwischen Azure und dem Internet][dmz-internet] verwendet. Jede dieser Referenzarchitekturen umfasst eine Bereitstellungslösung, die Sie verwenden können. Weitere Informationen stehen über die Links bereit.

## <a name="egress-with-layer-7-nvas"></a>Ausgehender Datenverkehr mit Layer-7-NVAs

Die vorherige Architektur kann so erweitert werden, dass sie eine ausgehende DMZ für Anforderungen umfasst, die aus der Azure-Workload stammen. Die folgende Architektur bietet Hochverfügbarkeit der NVAs in der DMZ für den Layer-7-Datenverkehr, z. B. HTTP oder HTTPS:

![[2]][2]

In dieser Architektur wird der gesamte aus Azure stammende Datenverkehr an einen internen Lastenausgleich weitergeleitet. Der Lastenausgleich verteilt ausgehende Anforderungen auf eine Gruppe von NVAs. Diese NVAs leiten den Datenverkehr über ihre jeweiligen öffentlichen IP-Adressen an das Internet weiter.

> [!NOTE]
> Diese Architektur wird in der Referenzarchitektur für die [DMZ zwischen Azure und Ihrem lokalen Rechenzentrum][dmz-on-prem] und der Referenzarchitektur für die [DMZ zwischen Azure und dem Internet][dmz-internet] verwendet. Jede dieser Referenzarchitekturen umfasst eine Bereitstellungslösung, die Sie verwenden können. Weitere Informationen stehen über die Links bereit.

## <a name="ingress-egress-with-layer-7-nvas"></a>Eingehender/ausgehender Datenverkehr mit Layer-7-NVAs

Bei den beiden vorherigen Architekturen gab es eine separate DMZ für eingehenden und ausgehenden Datenverkehr. Die folgende Architektur veranschaulicht das Erstellen einer DMZ, die sowohl für eingehenden als auch ausgehenden Layer-7-Datenverkehr, z. B. HTTP oder HTTPS, verwendet werden kann:

![[4]][4]

In dieser Architektur verarbeiten die NVAs eingehende Anforderungen vom Anwendungsgateway. Die NVAs verarbeiten auch ausgehende Anforderungen von den Workload-VMs im Back-End-Pool des Lastenausgleichs. Da eingehender Datenverkehr mit einem Anwendungsgateway und ausgehender Datenverkehr mit einem Lastenausgleich weitergeleitet wird, sind die NVAs für die Bewahrung der Sitzungsaffinität zuständig. Das heißt, dass das Anwendungsgateway eine Zuordnung von eingehenden und ausgehenden Anforderungen verwaltet, damit es die richtige Antwort an den ursprünglichen Anforderer weiterleiten kann. Der interne Lastenausgleich hat jedoch keinen Zugriff auf die Zuordnungen des Anwendungsgateways und verwendet eine eigene Logik zum Senden von Antworten an die NVAs. Es ist möglich, dass der Lastenausgleich eine Antwort an ein NVA sendet, das die Anforderung nicht ursprünglich vom Anwendungsgateway empfangen hat. In diesem Fall müssen die NVAs kommunizieren und die Antwort untereinander übertragen, damit das richtige NVA die Antwort an das Anwendungsgateway weiterleiten kann.

> [!NOTE]
> Sie können das Problem des asymmetrischen Routings auch beheben, indem Sie sicherstellen, dass die NVAs eine eingehende Übersetzung der Quellnetzwerkadresse (SNAT) ausführen. Dadurch wird die ursprüngliche Quell-IP des Anforderers durch eine der IP-Adressen des NVA ersetzt, der für den eingehenden Datenfluss verwendet wird. Auf diese Weise ist sichergestellt, dass Sie mehrere NVAs gleichzeitig verwenden und dabei die Routensymmetrie beibehalten können.

## <a name="pip-udr-switch-with-layer-4-nvas"></a>PIP/UDR-Switch mit Layer-4-NVAs

Die folgende Architektur weist ein aktives und ein passives NVA auf. Diese Architektur verarbeitet sowohl eingehenden als auch ausgehenden Layer-4-Datenverkehr:

![[3]][3]

> [!TIP]
> Eine vollständige Lösung für diese Architektur ist auf [GitHub][pnp-ha-nva] verfügbar.

Diese Architektur ähnelt der ersten in diesem Artikel erläuterten Architektur. Die erste Architektur enthielt ein einzelnes NVA zum Akzeptieren und Filtern eingehender Layer-4-Anforderungen. Bei dieser Architektur wird nun ein zweites passives NVA zur Bereitstellung von hoher Verfügbarkeit hinzugefügt. Wenn beim aktiven NVA ein Fehler auftritt, wird das passive NVA aktiviert, und UDR und PIP werden so geändert, dass sie auf die NICs auf dem jetzt aktiven NVA verweisen. Diese Änderungen von UDR und PIP können entweder manuell erfolgen oder mithilfe eines automatisierten Prozesses. Bei dem automatisierten Prozess handelt es sich in der Regel um einen Daemon oder einen anderen Überwachungsdienst, der in Azure ausgeführt wird. Er fragt einen Integritätstest für das aktive NVA ab und führt den UDR- und PIP-Switch aus, wenn er einen Fehler beim NVA entdeckt.

Die Abbildung oben zeigt einen [ZooKeeper][zookeeper]-Beispielcluster mit einem Daemon für hohe Verfügbarkeit. Innerhalb des ZooKeeper-Clusters wählt ein Quorum von Knoten einen übergeordneten Knoten. Wenn bei diesem ein Fehler auftritt, wählen die übrigen Knoten einen neuen übergeordneten Knoten. Bei dieser Architektur führt der übergeordnete Knoten den Daemon aus, der den Integritätsendpunkt auf dem NVA abfragt. Wenn das NVA nicht auf den Integritätstest reagiert, aktiviert der Daemon das passive NVA. Der Daemon ruft dann die Azure-REST-API auf, um den PIP vom fehlerhaften NVA zu entfernen, und fügt diesen an das neu aktivierte NVA an. Dann ändert der Daemon die UDR so, dass sie auf die interne IP-Adresse des neu aktivierten NVA verweist.

Schließen Sie die ZooKeeper-Knoten nicht in ein Subnetz ein, auf das nur über eine Route zugegriffen werden kann, die das NVA enthält. Andernfalls kann bei einem Fehler des NVA nicht auf die ZooKeeper-Knoten zugegriffen werden. Sollte aus irgendeinem Grund ein Fehler beim Daemon auftreten, können Sie auf keinen der ZooKeeper-Knoten zugreifen, um das Problem zu diagnostizieren.

Die vollständige Lösung einschließlich Beispielcode finden Sie in den Dateien im [GitHub-Repository][pnp-ha-nva].

## <a name="pip-udr-nvas-without-snat"></a>PIP/UDR-NVAs ohne SNAT

Bei dieser Architektur werden zwei virtuelle Azure-Computer verwendet, um die NVA-Firewall in einer Aktiv/Passiv-Konfiguration zu hosten, die automatische Failovervorgänge unterstützt, aber keine Übersetzung der Quellnetzwerkadresse (Source Network Address Translation, SNAT) benötigt.

![Architektur mit PIP/UDR-NVAs ohne SNAT](./images/nva-ha/pip-udr-without-snat.png)

> [!TIP]
> Eine vollständige Lösung für diese Architektur ist auf [GitHub][ha-nva-fo] verfügbar.

Diese Lösung ist für Azure-Kunden konzipiert, die SNAT nicht für eingehende Anforderungen in Ihrer NVA-Firewall konfigurieren können. SNAT blendet die ursprüngliche Quellclient-IP-Adresse aus. Wenn Sie die ursprünglichen IP-Adressen protokollieren oder innerhalb anderer mehrstufiger Sicherheitskomponenten hinter Ihren NVAs verwenden möchten, ist diese Lösung grundsätzlich geeignet.

Das Failover von UDR-Tabelleneinträgen wird mittels einer Adresse für den nächsten Hop automatisiert, die auf die IP-Adresse einer Schnittstelle des virtuellen Computers mit der aktiven NVA-Firewall festgelegt ist. Die Logik für das automatische Failover wird in einer Funktions-App gehostet, die Sie mithilfe von [Azure Functions](/azure/azure-functions/) erstellen. Der Failovercode wird als serverlose Funktion in Azure Functions ausgeführt. Die Bereitstellung ist unkompliziert und kostengünstig und lässt sich einfach verwalten und anpassen. Und da die Funktions-App in Azure Functions gehostet wird, ist sie nicht vom virtuellen Netzwerk abhängig. Wenn sich Änderungen am virtuellen Netzwerk auf die NVA-Firewalls auswirken, wird die Funktions-App weiterhin unabhängig ausgeführt. Auch Tests sind genauer, da sie außerhalb des virtuellen Netzwerks und unter Verwendung der gleichen Route ausgeführt werden, die auch für eingehende Clientanforderungen verwendet wird.

Die Verfügbarkeit der NVA-Firewall wird vom Code der Funktions-App auf zwei Arten überprüft:

- Überwachen des Zustands der virtuellen Azure-Computer, die die NVA-Firewall hosten

- Testen, ob in der Firewall ein offener Port zum Back-End-Webserver vorhanden ist. Für diese Option muss das virtuelle Netzwerkgerät über PIP einen Socket verfügbar machen, den der Code der Funktions-App testen kann.

Der gewünschte Test wird beim Konfigurieren der Funktions-App ausgewählt. Die vollständige Lösung einschließlich Beispielcode finden Sie in den Dateien im [GitHub-Repository][ha-nva-fo].

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie mithilfe von Layer-7-NVAs [eine DMZ zwischen Azure und Ihrem lokalen Rechenzentrum implementieren][dmz-on-prem].
- Erfahren Sie, wie Sie mithilfe von Layer-7-NVAs [eine DMZ zwischen Azure und dem Internet implementieren][dmz-internet].
- [Behandeln von Problemen mit virtuellen Netzwerkappliances in Azure](/azure/virtual-network/virtual-network-troubleshoot-nva)

<!-- links -->

[cloud-security]: /azure/best-practices-network-security
[dmz-on-prem]: ./secure-vnet-hybrid.md
[dmz-internet]: ./secure-vnet-dmz.md
[egress-with-layer-7]: #egress-with-layer-7-nvas
[ingress-with-layer-7]: #ingress-with-layer-7-nvas
[ingress-egress-with-layer-7]: #ingress-egress-with-layer-7-nvas
[lb-overview]: /azure/load-balancer/load-balancer-overview/
[nva-scenario]: /azure/virtual-network/virtual-network-scenario-udr-gw-nva/
[pip-udr-switch]: #pip-udr-switch-with-layer-4-nvas
[udr-overview]: /azure/virtual-network/virtual-networks-udr-overview/
[zookeeper]: https://zookeeper.apache.org/
[pnp-ha-nva]: https://github.com/mspnp/ha-nva
[ha-nva-fo]: https://aka.ms/ha-nva-fo

<!-- images -->

[0]: ./images/nva-ha/single-nva.png "Architektur mit einzelnem NVA"
[1]: ./images/nva-ha/l7-ingress.png "Eingehender Layer-7-Datenverkehr"
[2]: ./images/nva-ha/l7-ingress-egress.png "Ausgehender Layer-7-Datenverkehr"
[3]: ./images/nva-ha/active-passive.png "Aktiv/Passiv-Cluster"
[4]: ./images/nva-ha/l7-ingress-egress-ag.png
