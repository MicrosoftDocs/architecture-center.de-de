---
title: Leistungs- und Skalierbarkeitsmuster
titleSuffix: Cloud Design Patterns
description: Die Leistung gibt Aufschluss über die Reaktionsfähigkeit eines Systems, d.h. seine Fähigkeit, alle Aktionen innerhalb eines bestimmten Zeitintervalls auszuführen. Die Skalierbarkeit ist dagegen die Fähigkeit eines Systems, einen Anstieg der Last ohne Auswirkungen auf die Leistung zu bewältigen oder die Anzahl verfügbarer Ressourcen schnell und problemlos zu erhöhen. Bei Cloudanwendungen treten in der Regel variable Arbeitsauslastungen und Aktivitätsspitzen auf. Diese vorherzusagen, ist insbesondere in einem Szenario mit mehreren Mandanten nahezu unmöglich. Stattdessen sollten sich Anwendungen innerhalb der jeweiligen Grenzen je nach Bedarf horizontal hoch- und herunterskalieren lassen. Die Skalierbarkeit betrifft nicht nur Compute-Instanzen, sondern auch andere Elemente wie den Datenspeicher, die Messaginginfrastruktur usw.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: a57194f70218a8294507bd389ea9526660e3041b
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248905"
---
# <a name="performance-and-scalability-patterns"></a>Leistungs- und Skalierbarkeitsmuster

[!INCLUDE [header](../../_includes/header.md)]

Die Leistung gibt Aufschluss über die Reaktionsfähigkeit eines Systems, d.h. seine Fähigkeit, alle Aktionen innerhalb eines bestimmten Zeitintervalls auszuführen. Die Skalierbarkeit ist dagegen die Fähigkeit eines Systems, einen Anstieg der Last ohne Auswirkungen auf die Leistung zu bewältigen oder die Anzahl verfügbarer Ressourcen schnell und problemlos zu erhöhen. Bei Cloudanwendungen treten in der Regel variable Arbeitsauslastungen und Aktivitätsspitzen auf. Diese vorherzusagen, ist insbesondere in einem Szenario mit mehreren Mandanten nahezu unmöglich. Stattdessen sollten sich Anwendungen innerhalb der jeweiligen Grenzen je nach Bedarf horizontal hoch- und herunterskalieren lassen. Die Skalierbarkeit betrifft nicht nur Compute-Instanzen, sondern auch andere Elemente wie den Datenspeicher, die Messaginginfrastruktur usw.

|                           Muster                            |                                                                        Zusammenfassung                                                                         |
|--------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
|               [Cache-Aside](../cache-aside.md)               |                                                   Daten bei Bedarf aus einem Datenspeicher in einen Cache laden                                                   |
|                      [CQRS](../cqrs.md)                      |                           Mithilfe separater Schnittstellen Vorgänge trennen, die Daten von Vorgängen zur Aktualisierung von Daten lesen                           |
|            [Ereignisherkunftsermittlung](../event-sourcing.md)            |                     Einen nur zum Anfügen vorgesehenen Speicher verwenden, um die vollständige Serie von Ereignissen aufzuzeichnen, die an Daten in einer Domäne ausgeführte Aktionen beschreiben                      |
|               [Indextabelle](../index-table.md)               |                                Erstellen Sie Indizes für die Felder im Datenspeicher, auf die häufig von Abfragen verwiesen wird.                                |
|         [Materialisierte Sicht](../materialized-view.md)         |       Voraufgefüllte Sichten für die Daten in einem oder mehreren Datenspeichern generieren, wenn die Daten für erforderliche Abfragevorgänge nicht ideal formatiert sind        |
|            [Prioritätswarteschlange](../priority-queue.md)            | An Dienste gesendete Anforderungen priorisieren, sodass Anforderungen mit einer höheren Priorität schneller empfangen und verarbeitet werden als Anforderungen mit einer niedrigeren Priorität |
| [Warteschlangenbasierter Lastenausgleich](../queue-based-load-leveling.md) |              Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen.               |
|                  [Sharding](../sharding.md)                  |                                           Einen Datenspeicher in einen Satz horizontaler Partitionen oder Shards unterteilen                                           |
|    [Hosten von statischen Inhalten](../static-content-hosting.md)    |                          Stellen Sie statische Inhalte in einem cloudbasierten Speicherdienst bereit, der die Inhalte direkt an den Client übermitteln kann.                          |
|                [Drosselung](../throttling.md)                |                Steuern Sie den Verbrauch der von einer Anwendungsinstanz, einem einzelnen Mandanten oder einem gesamten Dienst verwendeten Ressourcen.                 |
