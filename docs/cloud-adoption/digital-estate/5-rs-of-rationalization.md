---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Die fünf R der Rationalisierung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Beschreibt die Optionen, bei der Rationalisierung eines digitales Umfelds zur Verfügung stehen.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 25987e790df5c82ad2c8296f1be1dabc949f1629
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243301"
---
# <a name="the-5-rs-of-rationalization"></a>Die fünf R der Rationalisierung

Cloudrationalisierung bezeichnet die Untersuchung von Ressourcen, um die bestmögliche Migrations- oder Modernisierungsmethode für die jeweilige Ressource in der Cloud zu ermitteln. Weitere Informationen zum Rationalisierungsprozess finden Sie unter [Worum handelt es sich bei digitalen Ressourcen?](overview.md).

Die hier aufgeführten fünf R der Rationalisierung beschreiben die gängigsten Optionen für die Rationalisierung.

## <a name="rehost"></a>Rehosten

Auch „Lift & Shift“ genannt. Beim Rehosten wird die Ressource im aktuellen Zustand zum gewählten Cloudanbieter migriert. Die Architektur bleibt dabei größtenteils unverändert.

Mögliche Gründe:

* Senkung der Investitionskosten
* Freigabe von Platz im Datencenter
* Schnelle Amortisierung der Cloud

Faktoren für die quantitative Analyse:

* VM-Größe (CPU, Arbeitsspeicher, Speicherplatz)
* Abhängigkeiten (Netzwerkdatenverkehr)
* Ressourcenkompatibilität

Faktoren für die qualitative Analyse:

* Änderungstoleranz
* Geschäftliche Prioritäten
* Kritische Unternehmensereignisse
* Prozessabhängigkeiten

## <a name="refactor"></a>Refactoring

PaaS-Optionen (Platform-as-a-Service) können zur Senkung der Betriebskosten zahlreicher Anwendungen beitragen. Unter Umständen empfiehlt es sich, eine Anwendung geringfügig umzugestalten, um sie an ein PaaS-basiertes Modell anzupassen.

„Umgestalten“ bezieht sich auch auf den Anwendungsentwicklungsprozess der Codeumgestaltung, um eine Anwendung für neue Geschäftschancen fit zu machen.

Mögliche Gründe:

* Schnellere und kürzere Updates
* Codeportabilität
* Höhere Cloudeffizienz (Ressourcen, Geschwindigkeit, Kosten)

Faktoren für die quantitative Analyse:

* Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)
* Abhängigkeiten (Netzwerkdatenverkehr)
* Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)
* Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)

Faktoren für die qualitative Analyse:

* Laufende Unternehmensinvestitionen
* Burstoptionen/-zeitpläne
* Geschäftsprozessabhängigkeiten

## <a name="rearchitect"></a>Rearchitect (Überarbeiten)

Einige ältere Anwendungen sind aufgrund von architekturbezogenen Entscheidungen, die bei der Entwicklung der Anwendung getroffen wurden, nicht mit Cloudanbietern kompatibel. In diesem Fall muss die Anwendung vor der Transformation ggf. überarbeitet werden.

Anwendungen, die zwar mit der Cloud kompatibel, aber keine nativen Cloudanwendungen sind, können unter Umständen kostengünstiger und effizienter betrieben werden, wenn die Lösung überarbeitet und in eine native Cloudanwendung umgewandelt wird.

Mögliche Gründe:

* Skalierbarkeit und Flexibilität der Anwendung
* Einfachere Einführung neuer Cloudfunktionen
* Verwendung verschiedener Technologiestapel

Faktoren für die quantitative Analyse:

* Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)
* Abhängigkeiten (Netzwerkdatenverkehr)
* Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)
* Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)

Faktoren für die qualitative Analyse:

* Steigende Unternehmensinvestitionen
* Betriebskosten
* Potenzielle Feedbackschleifen und DevOps-Investitionen

## <a name="rebuild"></a>Neu erstellen

In manchen Szenarien ist die Kluft, die für eine Weiterverwendung einer Anwendung überwunden werden muss, zu groß, um weitere Investitionen zu rechtfertigen. Das gilt insbesondere für Anwendungen, die inzwischen nicht mehr unterstützt werden und nicht mehr die Anforderungen der aktuellen Geschäftsprozesse erfüllen. In diesem Fall wird eine neue Codebasis erstellt, die auf einen [cloudnativen](https://azure.microsoft.com/overview/cloudnative/) Ansatz ausgerichtet ist.

Mögliche Gründe:

* Beschleunigung von Innovationen
* Schnellere App-Erstellung
* Senkung der Betriebskosten

Faktoren für die quantitative Analyse:

* Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)
* Abhängigkeiten (Netzwerkdatenverkehr)
* Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)
* Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)

Faktoren für die qualitative Analyse:

* Sinkende Endbenutzerzufriedenheit
* Einschränkung von Geschäftsprozessen aufgrund des Funktionsumfangs
* Potenzielle Verbesserungen bei Kosten, Erfahrung oder Umsatz

## <a name="replace"></a>Replace

Lösungen werden in der Regel mit der besten Technologie und Methode implementiert, die zum Zeitpunkt der Implementierung zur Verfügung steht. Manchmal erfüllen SaaS-Anwendungen (Software-as-a-Service) sämtliche Funktionsanforderungen der gehosteten Anwendung. In diesem Fall kann eine Workload zukünftig ersetzt werden, wodurch sie bei der Transformation praktisch nicht weiter berücksichtigt werden muss.

Mögliche Gründe:

* Standardisierung auf der Grundlage bewährter Branchenmethoden
* Beschleunigung der Einführung geschäftsprozessbasierter Ansätze
* Umverteilung von Entwicklungsinvestitionen für Anwendungen, die Alleinstellungsmerkmale oder Wettbewerbsvorteile generieren

Faktoren für die quantitative Analyse:

* Senkung der allgemeinen Betriebskosten
* VM-Größe (CPU, Arbeitsspeicher, Speicherplatz)
* Abhängigkeiten (Netzwerkdatenverkehr)
* Auszumusternde Ressourcen

Faktoren für die qualitative Analyse:

* Kosten-Nutzen-Analyse der aktuellen Architektur im Vergleich zu einer SaaS-Lösung
* Geschäftsprozesszuordnungen
* Datenschemas
* Benutzerdefinierte oder automatisierte Prozesse

## <a name="next-steps"></a>Nächste Schritte

Die fünf R der Rationalisierung können auf digitale Ressourcen angewendet werden, um Rationalisierungsentscheidungen hinsichtlich des zukünftigen Zustands der einzelnen Anwendungen zu treffen.

> [!div class="nextstepaction"]
> [Worum handelt es sich bei digitalen Ressourcen?](overview.md)
