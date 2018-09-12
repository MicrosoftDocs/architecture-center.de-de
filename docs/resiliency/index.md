---
title: Entwerfen robuster Anwendungen für Azure
description: Es wird beschrieben, wie Sie in Azure robuste Anwendungen mit Hochverfügbarkeit und Notfallwiederherstellung erstellen.
author: MikeWasson
ms.date: 05/26/2017
ms.custom: resiliency
ms.openlocfilehash: b92a26323b4329f3dbe4f941b98da0080e730d65
ms.sourcegitcommit: c49aeef818d7dfe271bc4128b230cfc676f05230
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44389433"
---
# <a name="designing-resilient-applications-for-azure"></a>Entwerfen robuster Anwendungen für Azure

In einem verteilten System kommt es unweigerlich zu Fehlern. Hardware kann ausfallen. Im Netzwerk können vorübergehende Fehler auftreten. In seltenen Fällen kann ein gesamter Dienst oder eine Region ausfallen, aber auch hierfür muss eine Planung vorhanden sein. 

Das Erstellen einer zuverlässigen Anwendung in der Cloud unterscheidet sich vom Erstellen einer zuverlässigen Anwendung in einer Unternehmensumgebung. Bisher haben Sie höherwertige Hardware gekauft, um zentral hochzuskalieren, aber in einer Cloudumgebung müssen Sie stattdessen horizontal hochskalieren. Die Kosten für Cloudumgebungen werden niedrig gehalten, indem Standardhardware eingesetzt wird. In dieser neuen Umgebung liegt der Schwerpunkt auf der mittleren Zeit bis zur Wiederherstellung (Mean Time To Restore) und nicht mehr auf der mittleren Zeit zwischen Ausfällen (Mean Time Between Failures). Das Ziel besteht hierbei darin, die Auswirkungen eines Ausfalls gering zu halten.

Dieser Artikel enthält eine Übersicht über die Erstellung von robusten Anwendungen in Microsoft Azure. Zuerst werden der Begriff *Resilienz* und die dazugehörigen Konzepte definiert. Anschließend wird der Prozess zur Erreichung von Resilienz beschrieben. Hierzu wird ein strukturierter Ansatz für die Lebensdauer einer Anwendung verwendet – vom Entwurf und der Implementierung über die Bereitstellung bis zum Betrieb.

## <a name="what-is-resiliency"></a>Was ist Resilienz?
Bei der **Resilienz** (Robustheit) geht es um die Möglichkeit, nach Ausfällen für ein System eine Wiederherstellung durchzuführen und die Betriebsbereitschaft sicherzustellen. Es geht nicht um das *Vermeiden* von Ausfällen, sondern um das *Reagieren* auf Ausfälle, um Ausfallzeiten oder Datenverluste zu vermeiden. Das Ziel besteht bei der Resilienz darin, für die Anwendung nach einem Ausfall wieder die volle Funktionsfähigkeit herzustellen.

Zwei wichtige Aspekte der Resilienz sind Hochverfügbarkeit und Notfallwiederherstellung.

* **Hochverfügbarkeit** ist die Möglichkeit, die Anwendung ohne signifikante Ausfallzeit in einem fehlerfreien Zustand weiterzubetreiben. Mit „fehlerfreier Zustand“ ist gemeint, dass die Anwendung reagiert und Benutzer eine Verbindung mit der Anwendung herstellen und damit interagieren können.  
* Die **Notfallwiederherstellung** (Disaster Recovery, DR) ist die Möglichkeit, nach seltenen, schwerwiegenderen Vorfällen (dauerhafte Ausfälle größeren Ausmaßes, z.B. eine Dienstunterbrechung in einer gesamten Region) eine Wiederherstellung durchzuführen. Die Notfallwiederherstellung umfasst die Datensicherung und -archivierung und kann auch einen manuellen Eingriff beinhalten, z.B. das Wiederherstellen einer Datenbank aus einer Sicherung.

Sie können sich die Hochverfügbarkeit und Notfallwiederherstellung beispielsweise wie folgt vorstellen: Die Notfallwiederherstellung beginnt, wenn ein Fehler so große Auswirkungen hat, dass diese von der Einrichtung für Hochverfügbarkeit nicht mehr bewältigt werden können.  

Beim Ausführen der Entwurfsschritte für die Resilienz müssen Sie sich über Ihre Anforderungen an die Verfügbarkeit im Klaren sein. Wie viel Ausfallzeit ist akzeptabel? Dies ist teilweise eine Kostenfunktion. Welche Kosten fallen bei potenziellen Ausfallzeiten für Ihr Unternehmen an? Wie viel Geld sollten Sie investieren, um für die Anwendung die Hochverfügbarkeit sicherzustellen? Außerdem müssen Sie definieren, was die Verfügbarkeit der Anwendung bedeutet. Gilt die Anwendung beispielsweise als „ausgefallen“, wenn ein Kunde eine Bestellung senden kann, diese aber vom System nicht innerhalb des normalen Zeitrahmens verarbeitet werden kann? Berücksichtigen Sie außerdem die Wahrscheinlichkeit eines bestimmten Ausfalltyps und die Kosteneffektivität einer Lösungsstrategie.

Ein weiterer häufig verwendeter Begriff ist **Geschäftskontinuität** (Business Continuity, BC). Hierbei geht es um die Möglichkeit, auch bei bzw. nach widrigen Umständen, z.B. einer Naturkatastrophe oder einem Dienstausfall, wichtige Geschäftsfunktionen aufrechterhalten zu können. Die Geschäftskontinuität erstreckt sich auf den gesamten Betrieb des Unternehmens, z.B. physische Einrichtungen, Personal, Kommunikation, Transport und IT. In diesem Artikel geht es um Cloudanwendungen, aber die Resilienzplanung muss in Bezug auf alle Anforderungen der Geschäftskontinuität durchgeführt werden. 

**Datensicherung** ist ein wichtiger Teil der Notfallwiederherstellung. Wenn für die zustandslosen Komponenten einer Anwendung ein Fehler auftritt, können Sie sie immer wieder neu bereitstellen. Falls aber Daten verloren gehen, kann für das System kein stabiler Zustand mehr hergestellt werden. Daten müssen gesichert werden, und zwar idealerweise in einer anderen Region, falls ein Katastrophenfall eine gesamte Region betrifft. 

Die Sicherung unterscheidet sich von der **Datenreplikation**. Die Datenreplikation umfasst das Kopieren von Daten nahezu in Echtzeit, sodass das System schnell ein Failover zu einem Replikat durchführen kann. Viele Datenbankensysteme unterstützen die Replikation. Beispielsweise verfügt SQL Server über Unterstützung für SQL Server Always On-Verfügbarkeitsgruppen. Mit der Datenreplikation kann reduziert werden, wie lange die Wiederherstellung nach einem Ausfall dauert, indem sichergestellt wird, dass immer ein Replikat der Daten verfügbar ist. Die Datenreplikation bietet aber keinen Schutz vor menschlichen Fehlern. Falls Daten aufgrund eines menschlichen Fehlers beschädigt werden, werden sie einfach auf die Replikate kopiert. Aus diesem Grund benötigen Sie für Ihre Strategie für die Notfallwiederherstellung eine langfristige Sicherungslösung. 

## <a name="process-to-achieve-resiliency"></a>Prozess zur Erreichung von Resilienz
Resilienz ist kein Add-On. Sie muss in das System integriert und im Betrieb umgesetzt werden. Dieses allgemeine Modell dient hierbei als Grundlage:

1. **Definieren** Sie Ihre Verfügbarkeitsanforderungen basierend auf den geschäftlichen Anforderungen.
2. **Entwerfen** Sie die Anwendung mit Blick auf Resilienz. Beginnen Sie mit einer Architektur, die auf bewährten Methoden basiert, und identifizieren Sie dann die möglichen Fehlerpunkte dieser Architektur.
3. **Implementieren** Sie die Strategien für die Erkennung von Fehlern und die anschließende Wiederherstellung. 
4. **Testen** Sie die Implementierung, indem Sie Fehler simulieren und erzwungene Failover auslösen. 
5. **Stellen Sie die Anwendung in der Produktion bereit**, indem Sie einen zuverlässigen und wiederholbaren Prozess verwenden. 
6. **Überwachen** Sie die Anwendung, um Fehler zu erkennen. Indem Sie das System überwachen, können Sie die Integrität der Anwendung messen und bei Bedarf auf Vorfälle reagieren. 
7. **Reagieren** Sie, wenn es zu Fehlern kommt, für die manuelle Eingriffe erforderlich sind.

Im weiteren Verlauf dieses Artikels werden diese Schritte ausführlicher beschrieben.

## <a name="define-your-availability-requirements"></a>Definieren der Verfügbarkeitsanforderungen
Die Resilienzplanung beginnt mit den geschäftlichen Anforderungen. Hier sind einige Ansätze beschrieben, die für die Resilienz in diesem Zusammenhang geeignet sind.

### <a name="decompose-by-workload"></a>Aufteilen von Workloads
Viele Cloudlösungen bestehen aus mehreren Anwendungsworkloads. Der Begriff „Workload“ steht in diesem Kontext für eine diskrete Funktion oder Computingaufgabe, die von anderen Aufgaben in Bezug auf die Geschäftslogik und die Datenspeicheranforderungen logisch getrennt werden kann. Eine E-Commerce-App kann beispielsweise die folgenden Workloads enthalten:

* Suchen in einem Produktkatalog
* Erstellen und Nachverfolgen von Aufträgen
* Anzeigen von Empfehlungen

Für diese Workloads gelten unter Umständen unterschiedliche Anforderungen in Bezug auf die Verfügbarkeit, Skalierbarkeit, Datenkonsistenz, Notfallwiederherstellung usw. Auch dies sind geschäftliche Entscheidungen.

Berücksichtigen Sie außerdem die Nutzungsmuster. Gibt es bestimmte kritische Zeiträume, in denen das System verfügbar sein muss? Beispiele: Ein Dienst für Steuererklärungen darf kurz vor dem Einreichungsdatum nicht ausfallen, ein Dienst für Videostreams muss während eines großen Sportereignisses stabil bleiben usw. Während dieser kritischen Zeiträume verwenden Sie ggf. redundante Bereitstellungen in mehreren Regionen, damit für die Anwendung ein Failover durchgeführt werden kann, wenn eine Region ausfällt. Aber da die Bereitstellung in mehreren Regionen mit höheren Kosten verbunden ist, kann es ratsam sein, die Anwendung zu weniger kritischen Zeiten nur in einer Region auszuführen.

### <a name="rto-and-rpo"></a>RTO und RPO
Zwei wichtige Metriken, die berücksichtigt werden sollten, sind Recovery Time Objective (RTO) und Recovery Point Objective (RPO).

* **Recovery Time Objective** (RTO) ist der maximal zulässige Zeitraum, in dem eine Anwendung nach einem Vorfall nicht verfügbar sein darf. Wenn RTO bei Ihnen 90 Minuten beträgt, müssen Sie dazu in der Lage sein, die Anwendung innerhalb von 90 Minuten nach Beginn eines Notfalls wieder in den Ausführungszustand zu versetzen. Falls für RTO ein sehr niedriger Wert angesetzt ist, führen Sie ggf. eine zweite Bereitstellung ständig im Standby aus, um vor einem regionalen Ausfall geschützt zu sein.

* **Recovery Point Objective** (RPO) ist die maximale Dauer eines Datenverlusts, die während eines Notfalls zulässig ist. Wenn Sie Daten beispielsweise in einer einzelnen Datenbank ohne Replikation in anderen Datenbanken speichern und stündliche Sicherungen durchführen, können Daten für bis zu eine Stunde verloren gehen. 

RTO und RPO sind geschäftliche Anforderungen. Die Durchführung einer Risikobewertung kann hilfreich sein, um RTO und RPO für die Anwendung zu definieren. Eine weitere häufig genutzte Metrik ist die **mittlere Zeit bis zur Wiederherstellung** (Mean Time To Recover, MTTR). Dies ist die durchschnittliche Dauer der Wiederherstellung einer Anwendung nach einem Ausfall. MTTR ist ein empirischer Fakt eines Systems. Wenn der MTTR-Wert größer als der RTO-Wert ist, führt ein Ausfall des Systems zu einer nicht akzeptablen geschäftlichen Störung, weil es nicht möglich ist, das System innerhalb des definierten RTO-Zeitraums wiederherzustellen. 

### <a name="slas"></a>SLAs
In Azure wird in der [Vereinbarung zum Servicelevel (SLA)][sla] die garantierte Verfügbarkeit und Konnektivität beschrieben, die Microsoft zusichert. Wenn die Vereinbarung zum Servicelevel für einen bestimmten Dienst 99,9% beträgt, können Sie erwarten, dass der Dienst 99,9% der Zeit verfügbar ist.

> [!NOTE]
> Die Vereinbarung zum Servicelevel von Azure enthält auch Bestimmungen zum Erhalt einer Gutschrift, falls die Vereinbarung nicht erfüllt wird, sowie bestimmte Definitionen zur „Verfügbarkeit“ jedes Diensts. Dieser Aspekt der Vereinbarung zum Servicelevel fungiert als Durchsetzungsrichtlinie. 
> 
> 

Sie sollten eigene Ziel-SLAs für jede Workload Ihrer Lösung definieren. Anhand einer Vereinbarung zum Servicelevel kann ausgewertet werden, ob die Architektur die geschäftlichen Anforderungen erfüllt. Wenn für eine Workload beispielsweise eine Betriebszeit von 99,99% erforderlich ist, dafür aber eine Abhängigkeit von einem Dienst mit einer Vereinbarung zum Servicelevel mit 99,9% besteht, kann dieser Dienst im System kein Single Point of Failure sein. Eine Lösung hierfür ist die Verwendung eines Fallbackpfads für den Fall, dass der Dienst ausfällt, oder das Treffen von anderen Maßnahmen zur Wiederherstellung nach einem Ausfall des Diensts. 

In der folgenden Tabelle sind die potenziellen kumulativen Ausfallzeiten für verschiedene SLA-Ebenen angegeben. 

| SLA | Ausfallzeit pro Woche | Ausfallzeit pro Monat | Ausfallzeit pro Jahr |
| --- | --- | --- | --- |
| 99 % |1,68 Stunden |7,2 Stunden |3,65 Tage |
| 99,9 % |10,1 Minuten |43,2 Minuten |8,76 Stunden |
| 99,95 % |5 Minuten |21,6 Minuten |4,38 Stunden |
| 99,99 % |1,01 Minuten |4,32 Minuten |52,56 Minuten |
| 99,999% |6 Sekunden |25,9 Sekunden |5,26 Minuten |

Sofern alle anderen Faktoren identisch sind, ist eine höhere Verfügbarkeit natürlich immer vorzuziehen. Aber wenn eine noch höhere Verfügbarkeit angestrebt wird, erhöhen sich auch die Kosten und die Komplexität, um dies zu erreichen. Eine Betriebszeit von 99,99% entspricht einer Gesamtausfallzeit von ca. 5 Minuten pro Monat. Rechtfertigt die Erreichung von 99,999% die zusätzliche Komplexität und die höheren Kosten? Die Antwort hängt von Ihren geschäftlichen Anforderungen ab. 

Hier sind einige andere Aspekte aufgeführt, die für das Definieren einer Vereinbarung zum Servicelevel gelten:

* Wenn Sie 99,99% erreichen möchten, können Sie sich bei der Wiederherstellung nach Ausfällen wahrscheinlich nicht auf manuelle Eingriffe verlassen. Die Anwendung muss eine Selbstdiagnose und Selbstreparatur durchführen können. 
* Im Bereich über 99,99% stellt es eine große Herausforderung dar, Ausfälle schnell genug zu erkennen, um die SLA-Anforderungen zu erfüllen.
* Betrachten Sie das Zeitfenster, auf das sich Ihre Vereinbarung zum Servicelevel bezieht. Je kleiner das Fenster, desto enger die Toleranzen. Vermutlich ist es nicht sinnvoll, Ihre Vereinbarung zum Servicelevel in Bezug auf die stündliche oder tägliche Betriebszeit zu definieren. 

### <a name="composite-slas"></a>Zusammengesetzte SLAs
Angenommen, eine App Service-Web-App führt Schreibvorgänge in eine Azure SQL-Datenbank durch. Zum Zeitpunkt der Erstellung dieses Artikels verfügen diese Azure-Dienste über die folgenden SLAs:

* App Service-Web-Apps = 99,95%
* SQL-Datenbank = 99,99%

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/sla1.png)

Welche maximale Ausfallzeit erwarten Sie für diese Anwendung? Wenn einer der Dienste ausfällt, kommt es zu einem Ausfall der gesamten Anwendung. Im Allgemeinen ist die Wahrscheinlichkeit, dass jeder Dienst ausfällt, eine unabhängige Größe, und die zusammengesetzte Vereinbarung zum Servicelevel für diese Anwendung ist 99,95% &times; 99,99% = 99,94%. Dieser Wert ist niedriger als bei einzelnen SLAs. Dies ist nicht verwunderlich, da eine Anwendung, die von mehreren Diensten abhängig ist, über mehr potenzielle Fehlerpunkte verfügt. 

Andererseits können Sie die zusammengesetzte Vereinbarung zum Servicelevel verbessern, indem Sie unabhängige Fallbackpfade erstellen. Wenn SQL-Datenbank beispielsweise nicht verfügbar ist, können Transaktionen zur späteren Verarbeitung in eine Warteschlange eingereiht werden.

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/sla2.png)

Bei diesem Aufbau ist die Anwendung auch dann noch verfügbar, wenn keine Verbindung mit der Datenbank hergestellt werden kann. Dies ist aber nicht mehr der Fall, wenn die Datenbank und die Warteschlange gleichzeitig ausfallen. Der erwartete Zeitprozentwert für einen gleichzeitigen Ausfall ist 0,0001 &times; 0,001, sodass die zusammengesetzte Vereinbarung zum Servicelevel für diesen kombinierten Pfad wie folgt lautet:  

* Datenbank ODER Warteschlange = 1,0 &minus; (0,0001 &times; 0,001) = 99,99999%

Für die gesamte zusammengesetzte Vereinbarung zum Servicelevel gilt:

* Web-App UND (Datenbank ODER Warteschlange) = 99,95% &times; 99,99999% = 99,95% (ungefährer Wert)

Dieser Ansatz hat aber auch Nachteile. Die Anwendungslogik ist komplexer, es fallen Kosten für die Warteschlange an, und ggf. müssen Probleme mit der Datenkonsistenz gelöst werden.

**SLA für Bereitstellung in mehreren Regionen**. Ein weiteres Verfahren für Hochverfügbarkeit ist die Bereitstellung der Anwendung in mehr als einer Region und die Verwendung von Azure Traffic Manager zum Durchführen eines Failovers, wenn die Anwendung in einer Region ausfällt. Für eine Bereitstellung in zwei Regionen wird die zusammengesetzte Vereinbarung zum Servicelevel wie folgt berechnet: 

*N* steht für die zusammengesetzte Vereinbarung zum Servicelevel für die Anwendung, die in einer Region bereitgestellt wird. Die erwartete Wahrscheinlichkeit, dass die Anwendung in beiden Regionen gleichzeitig ausfällt, ist (1 &minus; N) &times; (1 &minus; N). Deshalb gilt Folgendes:

* Kombinierte Vereinbarung zum Servicelevel für beide Regionen = 1 &minus; (1 &minus; N)(1 &minus; N) = N + (1 &minus; N)N

Abschließend müssen Sie noch die [Vereinbarung zum Servicelevel für Traffic Manager][tm-sla] einbinden. Zum Zeitpunkt der Erstellung dieses Artikels verfügt die Vereinbarung zum Servicelevel für Traffic Manager über den Wert 99,99%.

* Zusammengesetzte Vereinbarung zum Servicelevel = 99,99% &times; (SLA kombiniert für beide Regionen)

Außerdem wird das Failover nicht sofort durchgeführt, und während eines Failovers kann es zu Ausfallzeit kommen. Informationen hierzu finden Sie unter [Traffic Manager-Endpunktüberwachung und -failover][tm-failover].

Der berechnete SLA-Wert ist ein nützlicher Anhaltspunkt, aber er hat in Bezug auf die Verfügbarkeit keine umfassende Aussagekraft. Häufig kann eine Anwendung korrekt herabgestuft werden, wenn ein nicht kritischer Pfad ausfällt. Stellen Sie sich eine Anwendung vor, in der ein Katalog mit Büchern angezeigt wird. Wenn die Anwendung das Miniaturbild für den Buchdeckel nicht abrufen kann, wird ggf. ein Platzhalterbild angezeigt. In diesem Fall wird die Betriebszeit der Anwendung durch das fehlende Abrufen des Bilds nicht reduziert, obwohl für Benutzer eine Beeinträchtigung besteht.  

## <a name="design-for-resiliency"></a>Entwurf mit Blick auf Resilienz

In der Entwurfsphase ist es ratsam, eine Fehlermodusanalyse (Failure Mode Analysis, FMA) durchzuführen. Das Ziel einer FMA ist die Identifizierung von möglichen Fehlerpunkten, und es wird definiert, wie die Anwendung auf diese Fehler reagiert.

* Wie erkennt die Anwendung diese Art von Fehler?
* Wie reagiert die Anwendung auf diese Art von Fehler?
* Wie protokollieren und überwachen Sie diese Art von Fehler? 

Weitere Informationen zum FMA-Prozess mit spezifischen Empfehlungen für Azure finden Sie unter [Azure resiliency guidance: Failure mode analysis][fma] (Azure-Leitfaden zur Resilienz: Fehlermodusanalyse).

### <a name="example-of-identifying-failure-modes-and-detection-strategy"></a>Beispiel für die Identifizierung von Fehlermodi und eine Erkennungsstrategie
**Fehlerpunkt:** Aufruf eines externen Webdiensts bzw. einer API.

| Fehlermodus | Erkennungsstrategie |
| --- | --- |
| Dienst nicht verfügbar |HTTP 5xx |
| Drosselung |HTTP 429 (Zu viele Anforderungen) |
| Authentifizierung |HTTP 401 (Nicht autorisiert) |
| Langsame Reaktion |Timeout der Anforderung |


### <a name="redundancy-and-designing-for-failure"></a>Redundanz und Ausrichten des Entwurfs auf Fehler

Fehler und Ausfälle können mit unterschiedlichen Auswirkungen verbunden sein. Einige Hardwarefehler, z.B. ein ausgefallener Datenträger, wirken sich ggf. nur auf einen einzelnen Hostcomputer aus. Ein fehlerhafter Netzwerkswitch kann sich auf ein gesamtes Serverrack auswirken. Weniger häufig treten Fehler auf, die zu Störungen für ein gesamtes Rechenzentrum führen, z.B. zu einem Stromausfall im Rechenzentrum. In selten Fällen kann es vorkommen, dass eine gesamte Region nicht mehr verfügbar ist.

Eines der wichtigsten Verfahren, mit dem für eine Anwendung die Resilienz sichergestellt werden kann, ist die Redundanz. Sie müssen diese Redundanz aber beim Entwerfen der Anwendung einplanen. Außerdem richtet sich der jeweils benötigte Redundanzgrad nach Ihren Geschäftsanforderungen – nicht für jede Anwendung ist eine regionsübergreifende Redundanz als Schutz vor einem regionalen Ausfall erforderlich. In der Regel muss ein Kompromiss zwischen höherer Redundanz und Zuverlässigkeit und einer höheren Kostensumme und Komplexität gefunden werden.  

Azure verfügt über eine Reihe von Features, mit denen für eine Anwendung für alle Fehlerebenen Redundanz erzielt werden kann – von einer einzelnen VM bis hin zu einer gesamten Region. 

![](./images/redundancy.svg)

**Einzelne VM**: Azure enthält eine Betriebszeit-SLA für einzelne VMs. Sie können zwar einen höheren SLA-Grad erzielen, indem Sie zwei oder mehr VMs ausführen, aber für einige Workloads kann eine einzelne VM zuverlässig genug sein. Für Produktionsworkloads empfehlen wir aus Redundanzgründen die Verwendung von zwei oder mehr VMs. 

**Verfügbarkeitsgruppen**: Stellen Sie als Schutz vor lokalen Hardwarefehlern, z.B. ein Ausfall eines Datenträgers oder Netzwerkswitchs, in einer Verfügbarkeitsgruppe zwei oder mehr VMs bereit. Eine Verfügbarkeitsgruppe besteht aus mindestens zwei *Fehlerdomänen*, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. VMs in einer Verfügbarkeitsgruppe sind auf die Fehlerdomänen verteilt. Wenn ein Hardwarefehler eine Fehlerdomäne betrifft, kann der Netzwerkdatenverkehr so weiterhin an die VMs in den anderen Fehlerdomänen weitergeleitet werden. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](/azure/virtual-machines/windows/manage-availability).

**Verfügbarkeitszonen**.  Eine Verfügbarkeitszone ist eine physisch getrennte Zone in einer Azure-Region. Jede Verfügbarkeitszone verfügt über eine eigene Stromquelle, ein Netzwerk und eine Kühlung. Die Bereitstellung von VMs über Verfügbarkeitszonen hinweg dient dem Schutz einer Anwendung vor Ausfällen, die ein gesamtes Rechenzentrum betreffen. 

**Regionspaare**: Um eine Anwendung vor einem regionalen Ausfall zu schützen, können Sie sie in mehreren Regionen bereitstellen, indem Sie Azure Traffic Manager zum Verteilen von Internetdatenverkehr auf die verschiedenen Regionen verwenden. Jede Azure-Region ist mit einer anderen Region gekoppelt. Zusammen bilden sie ein [Regionspaar](/azure/best-practices-availability-paired-regions). Mit Ausnahme von „Brasilien, Süden“ befinden sich die Regionen der Regionspaare immer innerhalb des gleichen geografischen Gebiets, um steuerliche und rechtliche Anforderungen an den Speicherort von Daten zu erfüllen.

Berücksichtigen Sie beim Entwerfen einer Anwendung für mehrere Regionen, dass die Netzwerklatenz für mehrere Regionen höher als innerhalb einer Region ist. Wenn Sie beispielsweise eine Datenbank replizieren, um das Failover zu ermöglichen, sollten Sie die synchrone Datenreplikation innerhalb einer Region und die asynchrone Datenreplikation für mehrere Regionen verwenden.

| &nbsp; | Verfügbarkeitsgruppe | Verfügbarkeitszone | Regionspaar |
|--------|------------------|-------------------|---------------|
| Fehlerumfang | Rack | Datacenter | Region |
| Routinganforderung | Load Balancer | Zonenübergreifender Lastenausgleich | Traffic Manager |
| Netzwerklatenz | Sehr niedrig | Niedrig | Mittel bis hoch |
| Virtuelles Netzwerk  | VNet | VNet | Regionsübergreifendes VNet-Peering |

## <a name="implement-resiliency-strategies"></a>Implementieren von Resilienzstrategien
Dieser Abschnitt enthält eine Übersicht über einige häufig verwendete Resilienzstrategien. Die meisten davon sind nicht auf eine bestimmte Technologie beschränkt. In den Beschreibungen in diesem Abschnitt sind jeweils die grundlegenden Ideen jedes Verfahrens mit Links zu weiteren Informationen zusammengefasst.

**Wiederholen des Vorgangs bei vorübergehenden Fehlern.** Vorübergehende Fehler können durch eine kurze Trennung der Netzwerkverbindung, einen Verlust der Verbindung zur Datenbank oder eine Zeitüberschreitung bei zu hoher Auslastung eines Diensts verursacht werden. Häufig kann ein vorübergehender Fehler einfach gelöst werden, indem versucht wird, die Anforderung erneut zu senden. Für viele Azure-Dienste werden vom Client-SDK automatische Wiederholungen so implementiert, dass dies für den Aufrufer transparent ist. Informationen hierzu finden Sie unter [Anleitung zu dienstspezifischen Wiederholungsmechanismen][retry-service-specific guidance].

Jeder Wiederholungsversuch trägt zur Gesamtwartezeit bei. Außerdem können zu viele fehlgeschlagene Anforderungsversuche zu einem Engpass führen, wenn sich ausstehende Anforderungen in der Warteschlange ansammeln. Diese blockierten Anforderungen können kritische Systemressourcen belegen, z.B. Arbeitsspeicher, Threads, Datenbankverbindungen usw., sodass es zu kaskadierenden Fehlern kommen kann. Verlängern Sie die Verzögerungszeit zwischen den einzelnen Wiederholungsversuchen, und begrenzen Sie die Gesamtzahl von fehlgeschlagenen Anforderungen, um dies zu verhindern. 

![](./images/retry.png)

**Übergreifender Lastenausgleich für Instanzen.** Aus Gründen der Skalierbarkeit sollte eine Cloudanwendung horizontal hochskaliert werden können, indem mehr Instanzen hinzugefügt werden. Mit diesem Ansatz wird auch die Resilienz verbessert, da fehlerhafte Instanzen aus der Rotation herausgenommen werden können. Beispiel: 

* Ordnen Sie mindestens zwei VMs hinter einem Lastenausgleich an. Mit dem Lastenausgleichsmodul wird Datenverkehr auf alle VMs verteilt. Informationen hierzu finden Sie unter [Run load-balanced VMs for scalability and availability][ra-multi-vm] (Ausführen von VMs mit Lastenausgleich, um Skalierbarkeit und Verfügbarkeit zu erzielen).
* Führen Sie für eine Azure App Service-App das zentrale Hochskalieren auf mehrere Instanzen durch. App Service verteilt die Last automatisch auf die Instanzen. Informationen hierzu finden Sie unter [Basic web application][ra-basic-web] (Einfache Webanwendung).
* Verwenden Sie [Azure Traffic Manager][tm], um Datenverkehr auf eine Gruppe von Endpunkten zu verteilen.

**Replizieren von Daten.** Das Replizieren von Daten ist eine allgemeine Strategie zum Verarbeiten von nicht vorübergehenden Fehlern in einem Datenspeicher. Viele Speichertechnologien verfügen über eine integrierte Replikation, z.B. Azure SQL-Datenbank, Cosmos DB und Apache Cassandra. Es ist wichtig, dass sowohl der Lese- als auch der Schreibpfad berücksichtigt wird. Je nach Speichertechnologie sind ggf. mehrere beschreibbare Replikate vorhanden (oder ein einzelnes beschreibbares Replikat und mehrere schreibgeschützte Replikate). 

Zur Maximierung der Verfügbarkeit können Replikate in mehreren Regionen angeordnet werden. Hiermit wird aber die Wartezeit beim Replizieren der Daten erhöht. Normalerweise wird das regionsübergreifende Replizieren asynchron durchgeführt. Dies ist mit einem Modell der letztlichen Konsistenz und potenziellem Datenverlust bei einem Ausfall eines Replikats verbunden. 

**Stufen Sie Funktionalität korrekt herab**. Wenn ein Dienst ausfällt und kein Failoverpfad vorhanden ist, kann die Anwendung unter Umständen korrekt herabgestuft werden und trotzdem noch eine akzeptable Benutzerfreundlichkeit bieten. Beispiel: 

* Reihen Sie ein Arbeitselement zur späteren Verarbeitung in eine Warteschlange ein. 
* Geben Sie einen geschätzten Wert zurück.
* Verwenden Sie lokal zwischengespeicherte Daten. 
* Zeigen Sie eine Fehlermeldung für den Benutzer an. (Diese Option ist besser, als wenn die Anwendung nicht mehr auf Anforderungen reagiert.)

**Drosseln von Benutzern mit hohem Volumen.** Es kann vorkommen, dass eine geringe Zahl von Benutzern für die Erzeugung sehr hoher Lasten verantwortlich ist. Dies kann negative Auswirkungen auf andere Benutzer haben und die Gesamtverfügbarkeit Ihrer Anwendung reduzieren.

Wenn von einem einzelnen Client übermäßig viele Anforderungen ausgehen, kann die Anwendung den Client ggf. für einen bestimmten Zeitraum drosseln. Während des Drosselungszeitraums verweigert die Anwendung einige oder alle Anforderungen dieses Clients (je nach Drosselungsstrategie). Der Schwellenwert für die Drosselung richtet sich unter Umständen nach der Dienstebene des Kunden. 

Die Drosselung muss nicht bedeuten, dass der Client in böser Absicht gehandelt hat, sondern nur, dass das Dienstkontingent überschritten wurde. In einigen Fällen kann es vorkommen, dass ein Consumer das Kontingent ständig überschreitet oder sich auf andere Weise unangemessen verhält. Sie können dann weitere Maßnahmen treffen und den Benutzer blockieren. Normalerweise wird hierzu ein API-Schlüssel oder ein IP-Adressbereich blockiert. Weitere Informationen finden Sie unter [Throttling Pattern][throttling-pattern] (Drosselungsmuster).

**Verwenden eines Schutzschalters.** Mit einem [Schutzschaltermuster][circuit-breaker-pattern] kann verhindert werden, dass von einer Anwendung wiederholt versucht wird, einen Vorgang auszuführen, der mit hoher Wahrscheinlichkeit fehlschlägt. Der Schutzschalter umschließt Aufrufe an einen Dienst und verfolgt die Anzahl kürzlich aufgetretener Fehler nach. Überschreitet die Fehleranzahl einen Schwellenwert, gibt der Schutzschalter einen Fehlercode zurück, ohne den Dienst aufzurufen. Dadurch hat der Dienst Zeit zur Wiederherstellung. 

**Verwenden eines Belastungsausgleichs zum Ausgleichen von Datenverkehrsspitzen.** Für Anwendungen kann es zu plötzlichen Datenverkehrsspitzen kommen, die von Diensten auf dem Back-End nicht mehr bewältigt werden können. Wenn ein Back-End-Dienst nicht schnell genug auf Anforderungen reagieren kann, kann dies dazu führen, dass Anforderungen in eine Warteschlange eingereiht werden (Rückstau) oder die Anwendung vom Dienst gedrosselt wird. Sie können eine Warteschlange als Puffer verwenden, um dies zu verhindern. Wenn ein neues Arbeitselement vorhanden ist, reiht die Anwendung ein Arbeitselement für die asynchrone Ausführung in die Warteschlange ein, anstatt sofort den Back-End-Dienst aufzurufen. Die Warteschlange fungiert als Puffer, um Lastspitzen zu glätten. Weitere Informationen finden Sie unter [Queue-Based Load Leveling Pattern][load-leveling-pattern] (Warteschlangenbasiertes Belastungsausgleichsmuster).

**Isolieren von kritischen Ressourcen.** Fehler in einem Subsystem können ggf. zu kaskadierenden Fehlern werden und Fehler in anderen Teilen der Anwendung verursachen. Dies kann passieren, wenn ein Fehler dazu führt, dass einige Ressourcen, z.B. Threads oder Sockets, nicht rechtzeitig freigegeben werden und die Ressourcen erschöpft sind. 

Um dies zu vermeiden, können Sie ein System in isolierte Gruppen partitionieren, damit ein Ausfall in einer Partition nicht zum Ausfall des gesamten Systems führt. Dieses Verfahren wird auch als „Bulkhead-Muster“ (Trennwandmuster) bezeichnet.

Beispiele:

* Partitionieren Sie eine Datenbank (z.B. nach Mandant), und weisen Sie einen separaten Pool mit Webserverinstanzen für jede Partition zu.  
* Verwenden Sie separate Threadpools, um Aufrufe von unterschiedlichen Diensten zu isolieren. Auf diese Weise werden kaskadierende Fehler verhindert, wenn einer der Dienste ausfällt. Ein Beispiel finden Sie in der [Hystrix-Bibliothek][hystrix] von Netflix.
* Verwenden Sie [Container][containers], um die Ressourcen zu begrenzen, die für ein bestimmtes Subsystem verfügbar sind. 

![](./images/bulkhead.png)

**Anwenden von ausgleichenden Transaktionen.** Mit einer [ausgleichenden Transaktion][compensating-transaction-pattern] werden die Auswirkungen einer anderen abgeschlossenen Transaktion rückgängig gemacht. In einem verteilten System kann es sehr schwierig sein, eine hohe Transaktionskonsistenz zu erzielen. Ausgleichende Transaktionen sind eine Möglichkeit, Konsistenz zu erreichen, indem eine Reihe von kleineren einzelnen Transaktionen verwendet wird, die in jedem Schritt rückgängig gemacht werden können.

Beim Buchen einer Reise kann ein Kunde beispielsweise einen Mietwagen, ein Hotelzimmer und einen Flug reservieren. Wenn einer dieser Schritte nicht erfolgreich ist, schlägt der gesamte Vorgang fehl. Anstatt eine einzelne verteilte Transaktion für den gesamten Vorgang zu verwenden, können Sie für jeden Schritt eine ausgleichende Transaktion definieren. Beispielsweise können Sie die Reservierung stornieren, um die Reservierung eines Mietwagens rückgängig zu machen. Ein Koordinator führt die einzelnen Schritte aus, um den gesamten Vorgang abzuschließen. Falls ein Schritt fehlschlägt, wendet der Koordinator ausgleichende Transaktionen an, um abgeschlossene Schritte rückgängig zu machen. 

## <a name="test-for-resiliency"></a>Testen der Resilienz
Im Allgemeinen können Sie die Resilienz nicht auf die gleiche Weise wie die Anwendungsfunktionalität testen (per Ausführung von Komponententests usw.). Stattdessen müssen Sie testen, wie sich die End-to-End-Workload unter Fehlerbedingungen verhält, die nur zeitweise auftreten.

Der Testvorgang ist ein iterativer Prozess. Testen Sie die Anwendung, messen Sie das Ergebnis, analysieren und beheben Sie alle aufgetretenen Fehler, und wiederholen Sie den Vorgang.

**Fault Injection-Tests**: Testen Sie die Resilienz des Systems bei Fehlern, indem Sie entweder echte Fehler auslösen oder diese simulieren. Hier sind einige häufige Fehlerszenarien aufgeführt, die getestet werden können:

* Herunterfahren von VM-Instanzen
* Absturz von Prozessen
* Ablauf von Zertifikaten
* Ändern von Zugriffsschlüsseln
* Herunterfahren des DNS-Diensts auf Domänencontrollern
* Begrenzen von verfügbaren Systemressourcen, z.B. RAM oder Anzahl von Threads
* Aufheben der Bereitstellung von Datenträgern
* Erneutes Bereitstellen eines virtuellen Computers

Messen Sie die Wiederherstellungszeiten, und stellen Sie sicher, dass Ihre geschäftlichen Anforderungen erfüllt werden. Testen Sie auch Kombinationen von Fehlermodi. Vergewissern Sie sich, dass es nicht zu kaskadierenden Fehlern kommt und dass diese isoliert behandelt werden.

Dies ist ein weiterer Grund, warum es wichtig ist, mögliche Fehlerpunkte während der Entwurfsphase zu analysieren. Die Ergebnisse dieser Analyse sollten in Ihren Testplan einfließen.

**Auslastungstests**: Auslastungstests sind sehr wichtig zur Identifizierung von Fehlern, die nur bei hoher Auslastung auftreten, z.B. eine Überlastung der Back-End-Datenbank oder eine Drosselung des Diensts. Führen Sie Tests für Spitzenlasten durch, indem Sie Produktionsdaten oder synthetische Daten verwenden, die den Produktionsdaten möglichst genau ähneln. Hierbei soll ermittelt werden, wie sich die Anwendung unter realen Bedingungen verhält.   

## <a name="deploy-using-reliable-processes"></a>Bereitstellen mithilfe zuverlässiger Prozesse
Nachdem eine Anwendung in der Produktion bereitgestellt wurde, sind Updates eine mögliche Fehlerquelle. Im schlimmsten Fall kann ein fehlerhaftes Update zu Ausfallzeiten führen. Der Bereitstellungsprozess muss vorhersagbar und wiederholbar sein, um dies zu vermeiden. Die Bereitstellung umfasst auch die Bereitstellung von Azure-Ressourcen und -Anwendungscode und das Anwenden von Konfigurationseinstellungen. Ein Update kann alle drei Vorgänge oder einen Teil davon abdecken. 

Der entscheidende Punkt ist, dass manuelle Bereitstellungen fehleranfällig sind. Daher wird empfohlen, einen automatisierten idempotenten Prozess zu verwenden, den Sie bedarfsabhängig und bei Fehlern auch erneut durchführen können. 

* Nutzen Sie Azure Resource Manager-Vorlagen, um die Bereitstellung von Azure-Ressourcen zu automatisieren.
* Verwenden Sie die [Azure Automation-Konfiguration für den gewünschten Zustand][dsc] (Desired State Configuration, DSC) zum Konfigurieren von VMs.
* Verwenden Sie einen automatisierten Bereitstellungsprozess für Anwendungscode.

Zwei Konzepte für die robuste Bereitstellung sind *Infrastruktur als Code* und *unveränderliche Infrastruktur*.

* Bei **Infrastruktur als Code** wird Code zum Bereitstellen und Konfigurieren der Infrastruktur verwendet. Für Infrastruktur als Code kann ein deklarativer Ansatz oder ein imperativer Ansatz genutzt werden (oder eine Kombination). Resource Manager-Vorlagen sind ein Beispiel für einen deklarativen Ansatz. PowerShell-Skripts sind ein Beispiel für einen imperativen Ansatz.
* Bei **unveränderlicher Infrastruktur** geht es darum, die Infrastruktur nicht mehr zu ändern, nachdem sie in der Produktion bereitgestellt wurde. Andernfalls kann es zu einem Zustand kommen, in dem Ad-hoc-Änderungen angewendet wurden. In diesem Fall ist es schwierig, die genauen Änderungen zu erkennen und Entscheidungen für das System zu treffen. 

Eine andere Frage ist, wie das Rollout für ein Anwendungsupdate durchgeführt werden soll. Wir empfehlen die Nutzung von Verfahren wie eine Blaugrün-Bereitstellung oder Canary-Releases, bei denen Updates unter starker Kontrolle übertragen werden, um mögliche negative Auswirkungen einer fehlerhaften Bereitstellung zu verringern.

* Die [Blaugrün-Bereitstellung][blue-green] ist ein Verfahren, bei dem ein Update in einer Produktionsumgebung separat von der Liveanwendung bereitgestellt wird. Wechseln Sie für das Routing von Datenverkehr nach der Überprüfung der Bereitstellung dann zur aktualisierten Version. Für Azure App Service-Web-Apps wird dies beispielsweise durch Stagingslots ermöglicht.
* [Canary-Releases][canary-release] ähneln Blaugrün-Bereitstellungen. Anstatt für den gesamten Datenverkehr auf die aktualisierte Version umzustellen, führen Sie für das Update den Rollout für einen kleinen Prozentsatz der Benutzer durch, indem Sie einen Teil des Datenverkehrs an die neue Bereitstellung weiterleiten. Falls ein Problem auftritt, können Sie den Vorgang stoppen und wieder auf die alte Bereitstellung zurückgreifen. Wenn alles in Ordnung ist, können Sie mehr Datenverkehr an die neue Version weiterleiten, bis die Umstellung für den gesamten Datenverkehr erfolgt ist.

Stellen Sie unabhängig vom gewählten Ansatz sicher, dass Sie einen Rollback auf die letzte als fehlerfrei bekannte Bereitstellung durchführen können, falls die neue Version nicht funktioniert. Wenn Fehler auftreten, muss in den Anwendungsprotokollen angegeben werden, welche Version den Fehler verursacht hat. 

## <a name="monitor-to-detect-failures"></a>Überwachen zur Fehlererkennung
Die Überwachung und die Diagnose sind für die Resilienz von entscheidender Bedeutung. Wenn ein Fehler auftritt, müssen Sie darüber informiert werden, und Sie müssen über Erkenntnisse zur Fehlerursache verfügen. 

Die Überwachung eines umfangreichen verteilten Systems stellt eine erhebliche Herausforderung dar. Angenommen, eine Anwendung wird auf einigen Dutzend VMs ausgeführt. Es ist nicht effizient, sich einzeln nacheinander an jeder VM anzumelden und sich die Protokolldateien anzusehen, um die Problembehandlung durchzuführen. Die Anzahl von VM-Instanzen ist außerdem wahrscheinlich nicht statisch. VMs werden hinzugefügt und entfernt, wenn die Anwendung horizontal herunter- und hochskaliert wird, und gelegentlich kann es zu einem Ausfall einer Instanz kommen, sodass eine erneute Bereitstellung erforderlich ist. Außerdem können für eine typische Cloudanwendung mehrere Datenspeicher verwendet werden (Azure Storage, SQL-Datenbank, Cosmos DB, Redis Cache), und eine einzelne Benutzeraktion kann auf mehrere Subsysteme verteilt sein. 

Sie können sich den Überwachungs- und Diagnoseprozess als Pipeline mit mehreren einzelnen Phasen vorstellen:

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/monitoring.png)

* **Instrumentierung**: Die Rohdaten für die Überwachung und Diagnose stammen aus vielen verschiedenen Quellen, z.B. Anwendungsprotokollen, Webserverprotokollen, Leistungsindikatoren des Betriebssystems, Datenbankprotokollen und Diagnosemodulen, die in die Azure Platform integriert sind. Die meisten Azure-Dienste verfügen über eine Diagnosefunktion, die Sie verwenden können, um die Ursache von Problemen zu ermitteln.
* **Sammlung und Speicherung**: Rohdaten der Instrumentierung können an verschiedenen Orten und in unterschiedlichen Formaten (z.B. Ablaufverfolgungsprotokolle von Anwendungen, IIS-Protokolle, Leistungsindikatoren) vorgehalten werden. Die Daten dieser unterschiedlichen Datenquellen werden gesammelt, zusammengefasst und zuverlässig gespeichert.
* **Analyse und Diagnose**: Nachdem die Daten zusammengefasst wurden, können sie analysiert werden, um die Problembehandlung durchzuführen und eine Gesamtansicht der Anwendungsintegrität bereitzustellen.
* **Visualisierung und Warnungen**: In dieser Phase werden Telemetriedaten so dargestellt, dass ein Bediener Probleme oder Trends schnell erkennen kann. Beispiele hierfür sind Dashboards oder E-Mail-Warnungen.  

Die Überwachung ist nicht dasselbe wie die Fehlererkennung. Es kann beispielsweise sein, dass Ihre Anwendung einen vorübergehenden Fehler erkennt und einen Wiederholungsversuch durchführt, ohne dass es zu Ausfallzeit kommt. Es sollte aber auch der Wiederholungsvorgang protokolliert werden, damit Sie die Fehlerrate überwachen können, um ein Gesamtbild der Anwendungsintegrität zu erhalten. 

Anwendungsprotokolle sind eine wichtige Quelle für Diagnosedaten. Bewährte Methoden für die Anwendungsprotokollierung sind:

* Protokollieren in der Produktion: Andernfalls fehlen Ihnen die Erkenntnisse in dem Bereich, für den Sie diese am dringendsten benötigen.
* Protokollieren von Ereignissen an den Dienstgrenzen: Binden Sie eine Korrelations-ID ein, die über Dienstgrenzen hinweg gilt. Wenn eine Transaktion mehrere Dienste durchläuft und einer davon ausfällt, können Sie anhand der Korrelations-ID ermitteln, warum es zum Transaktionsfehler gekommen ist.
* Verwenden Sie die semantische Protokollierung, die auch als strukturierte Protokollierung bezeichnet wird. Unstrukturierte Protokolle erschweren die Automatisierung des Verbrauchs und der Analyse von Protokolldaten, die auf Cloudebene erforderlich ist.
* Verwenden der asynchronen Protokollierung: Andernfalls kann das Protokollierungssystem selbst zu einem Fehler der Anwendung führen, wenn es zu einem Stau von Anforderungen und somit zu einer Blockade kommt, während auf das Schreiben eines Protokollierungsereignisses gewartet wird.
* Die Anwendungsprotokollierung ist nicht dasselbe wie die Überwachung per Auditing. Das Auditing muss unter Umständen aus Konformitätsgründen oder zur Einhaltung von Bestimmungen durchgeführt werden. Daher müssen die Überwachungsdatensätze vollständig sein, und es ist nicht zulässig, beim Verarbeiten von Transaktionen Datensätze zu verwerfen. Wenn für eine Anwendung das Auditing erforderlich ist, sollte dies von der Diagnoseprotokollierung getrennt werden. 

Weitere Informationen zur Überwachung und Diagnose finden Sie unter [Anleitung zur Überwachung und Diagnose][monitoring-guidance].

## <a name="respond-to-failures"></a>Reagieren auf Fehler
In den vorherigen Abschnitten wurden Strategien für die automatisierte Wiederherstellung beschrieben, die für die Hochverfügbarkeit wichtig sind. Aber in einigen Fällen sind manuelle Eingriffe erforderlich.

* **Warnungen**. Überwachen Sie Ihre Anwendung auf Warnsignale, die proaktive Eingriffe nötig machen. Wenn Sie beispielsweise sehen, dass Ihre Anwendung von SQL-Datenbank oder Cosmos DB ständig gedrosselt wird, müssen Sie ggf. Ihre Datenbankkapazität erhöhen oder Ihre Abfragen optimieren. In diesem Beispiel sollte Ihre Telemetrie auch dann eine Warnung auslösen, wenn die Anwendung Drosselungsfehler transparent behandelt, damit Sie Maßnahmen treffen können.  
* **Durchführen eines manuellen Failovers**: Für einige Systeme kann ein Failover nicht automatisch durchgeführt werden, sodass ein manuelles Failover erforderlich ist. 
* **Testen der Betriebsbereitschaft**: Wenn für Ihre Anwendung ein Failover in eine sekundäre Region durchgeführt wird, sollten Sie die Betriebsbereitschaft testen, bevor das Failback in die primäre Region erfolgt. Bei diesem Test sollte sichergestellt werden, dass die primäre Region betriebsbereit und fehlerfrei ist und wieder für den Empfang von Datenverkehr bereit ist.
* **Prüfen der Datenkonsistenz**: Wenn in einem Datenspeicher ein Fehler auftritt, kann es zu Inkonsistenzen bei den Daten kommen, sobald der Speicher wieder verfügbar ist. Dies gilt besonders, wenn die Daten repliziert wurden. 
* **Wiederherstellen einer Sicherung**: Wenn es beispielsweise für SQL-Datenbank zu einem regionalen Ausfall kommt, können Sie für die Datenbank basierend auf der letzten Sicherung die Geowiederherstellung durchführen.

Dokumentieren und testen Sie Ihren Plan für die Notfallwiederherstellung. Werten Sie die geschäftlichen Auswirkungen von Anwendungsausfällen aus. Automatisieren Sie den Prozess so weit wie möglich, und dokumentieren Sie alle manuellen Schritte, z.B. manuelles Failover oder die Datenwiederherstellung aus Sicherungen. Testen Sie Ihren Prozess für die Notfallwiederherstellung regelmäßig, um den Plan zu überprüfen und zu verbessern. 

## <a name="summary"></a>Zusammenfassung
In diesem Artikel wurde die Resilienz aus einer ganzheitlichen Perspektive beschrieben, und es wurden einige besondere Anforderungen der Cloud erläutert. Hierzu gehörten die verteilte Struktur des Cloud Computing, die Nutzung von Standardhardware und das Auftreten von vorübergehenden Netzwerkfehlern.

Hier sind noch einmal die wichtigsten Punkte dieses Artikels aufgeführt:

* Resilienz führt zu einer höheren Verfügbarkeit und geringeren mittleren Zeit bis zur Wiederherstellung nach Ausfällen. 
* Zur Erreichung von Resilienz in der Cloud sind andere Verfahren als bei herkömmlichen lokalen Lösungen erforderlich. 
* Resilienz ist kein Zufallsprodukt. Sie muss von Beginn an gezielt entworfen und integriert werden.
* Resilienz betrifft alle Bereiche des Anwendungslebenszyklus – von der Planung und Codierung bis zum Betrieb.
* Testen und überwachen Sie!


<!-- links -->

[blue-green]: http://martinfowler.com/bliki/BlueGreenDeployment.html
[canary-release]: http://martinfowler.com/bliki/CanaryRelease.html
[circuit-breaker-pattern]: https://msdn.microsoft.com/library/dn589784.aspx
[compensating-transaction-pattern]: https://msdn.microsoft.com/library/dn589804.aspx
[containers]: https://en.wikipedia.org/wiki/Operating-system-level_virtualization
[dsc]: /azure/automation/automation-dsc-overview
[contingency-planning-guide]: http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-34r1.pdf
[fma]: failure-mode-analysis.md
[hystrix]: http://techblog.netflix.com/2012/11/hystrix.html
[jmeter]: http://jmeter.apache.org/
[load-leveling-pattern]: ../patterns/queue-based-load-leveling.md
[monitoring-guidance]: ../best-practices/monitoring.md
[ra-basic-web]: ../reference-architectures/app-service-web-app/basic-web-app.md
[ra-multi-vm]: ../reference-architectures/virtual-machines-windows/multi-vm.md
[checklist]: ../checklist/resiliency.md
[retry-pattern]: ../patterns/retry.md
[retry-service-specific guidance]: ../best-practices/retry-service-specific.md
[sla]: https://azure.microsoft.com/support/legal/sla/
[throttling-pattern]: ../patterns/throttling.md
[tm]: https://azure.microsoft.com/services/traffic-manager/
[tm-failover]: /azure/traffic-manager/traffic-manager-monitoring
[tm-sla]: https://azure.microsoft.com/support/legal/sla/traffic-manager/v1_0/
