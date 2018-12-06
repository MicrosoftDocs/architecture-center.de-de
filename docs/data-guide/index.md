---
title: Azure-Datenarchitekturleitfaden
description: ''
author: zoinerTejada
ms.date: 02/12/2018
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: ace8b7d87d764287dced6863fd80e8c71c5e026a
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52902018"
---
# <a name="azure-data-architecture-guide"></a>Azure-Datenarchitekturleitfaden

In diesem Leitfaden wird ein strukturierter Ansatz für den Entwurf von datenorientierten Lösungen in Microsoft Azure vorgestellt. Er basiert auf bewährten Methoden, die aus Kundeninteraktionen abgeleitet wurden.

## <a name="introduction"></a>Einführung

Die Cloud verändert die Art und Weise, wie Anwendungen entwickelt werden, und auch die Verarbeitung und Speicherung von Daten. Anstelle einer einzelnen allgemeinen Datenbank, die alle Daten einer Lösung enthält, nutzen _mehrsprachige Persistenzlösungen_ mehrere spezielle Datenspeicher, die jeweils zur Bereitstellung bestimmter Funktionen optimiert sind. Dadurch ändert sich die Perspektive auf die Daten in der Lösung. Mehrere Ebenen von Geschäftslogik, die aus und in eine(r) einzelne(n) Datenschicht lesen und schreiben, gehören der Vergangenheit an. Stattdessen werden Lösungen rund um eine *Datenpipeline* konzipiert, die beschreibt, wie Daten durch eine Lösung fließen, wo sie verarbeitet, wo sie gespeichert und wie sie von der nächsten Komponente in der Pipeline genutzt werden. 

## <a name="how-this-guide-is-structured"></a>Aufbau dieses Leitfadens

Dieser Leitfaden basiert auf zwei allgemeinen Kategorien für Datenlösungen: *herkömmliche RDBMS-Workloads* und *Big Data-Lösungen*. 

**[Herkömmliche RDMBS-Workloads:](./relational-data/index.md)** Hierzu zählen OLTP (Online Transaction Processing, Onlinetransaktionsverarbeitung) und OLAP (Online Analytical Processing, analytische Onlineverarbeitung). Bei den Daten in OLTP-Systemen handelt es sich in der Regel um relationale Daten mit einem vordefinierten Schema und einer Reihe von Einschränkungen zur Wahrung der referenziellen Integrität. Häufig werden Daten aus mehreren Quellen in der Organisation in einem Data Warehouse konsolidiert. Dabei werden die Quelldaten mithilfe eines ETL-Prozesses verschoben und transformiert.

![](./images/guide-rdbms.svg)

**[Big Data-Lösungen:](./big-data/index.md)** Eine Big Data-Architektur ist für die Erfassung, Verarbeitung und Analyse von Daten konzipiert, die für herkömmliche Datenbanksysteme zu groß oder zu komplex sind. Die Verarbeitung der Daten kann in Batches oder in Echtzeit erfolgen. Big Data-Lösungen umfassen in der Regel große Mengen nicht-relationaler Daten. Hierzu zählen etwa Schlüssel-Wert-Daten, JSON-Dokumente oder Zeitreihendaten. Herkömmliche RDBMS-Systeme eignen sich üblicherweise nicht besonders für die Speicherung solcher Daten. Der Begriff *NoSQL* bezieht sich auf eine Datenbankfamilie, die für die Speicherung nicht relationaler Daten konzipiert ist. (Der Begriff ist nicht ganz zutreffend, da viele nicht relationale Datenspeicher SQL-kompatible Abfragen unterstützen.)

![](./images/guide-big-data.svg)

Diese beiden Kategorien schließen sich nicht gegenseitig aus und überschneiden sich teilweise. Wir sind jedoch der Ansicht, dass sie eine gute Diskussionsgrundlage bilden. Der Leitfaden geht auf **gängige Szenarien** für die jeweilige Kategorie sowie auf die relevanten Azure-Dienste und die geeignete Architektur für das jeweilige Szenario ein. Darüber hinaus werden die **Technologieoptionen** für Datenlösungen in Azure (einschließlich Open-Source-Optionen) verglichen. In jeder Kategorie finden Sie eine Beschreibung der wichtigsten Auswahlkriterien und eine Funktionsmatrix, die Ihnen die Auswahl der passenden Technologie für Ihr Szenario erleichtert. 

In diesem Leitfaden geht es nicht um Data Science oder Datenbanktheorie. Zu diesen Themen wurden bereits ganze Bücher verfasst. Stattdessen möchten wir Ihnen dabei helfen, die passende Datenarchitektur oder Datenpipeline für Ihr Szenario zu finden und anschließend die Azure-Dienste und -Technologien auszuwählen, die am besten für Ihre Anforderungen geeignet sind. Wenn Sie bereits eine bestimmte Architektur geplant haben, können Sie direkt mit dem Abschnitt zur Auswahl der Technologie fortfahren.
