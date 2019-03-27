---
title: Entwurfs- und Implementierungsmuster
titleSuffix: Cloud Design Patterns
description: Ein guter Entwurf berücksichtigt Faktoren wie Konsistenz und Kohärenz im Komponentenentwurf und in der Bereitstellung, Wartbarkeit zur Vereinfachung der Verwaltung und Entwicklung sowie Wiederverwendbarkeit, um die Verwendung von Komponenten und Subsystemen in anderen Anwendungen und Szenarien zu ermöglichen. Während der Entwurfs- und Implementierungsphase getroffene Entscheidungen haben großen Einfluss auf die Qualität und die Gesamtkosten von in der Cloud gehosteten Anwendungen und Diensten.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: ff609679bed4ee4aa88daf95ff1cab4ab2d90d48
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248915"
---
# <a name="design-and-implementation-patterns"></a>Entwurfs- und Implementierungsmuster

Ein guter Entwurf berücksichtigt Faktoren wie Konsistenz und Kohärenz im Komponentenentwurf und in der Bereitstellung, Wartbarkeit zur Vereinfachung der Verwaltung und Entwicklung sowie Wiederverwendbarkeit, um die Verwendung von Komponenten und Subsystemen in anderen Anwendungen und Szenarien zu ermöglichen. Während der Entwurfs- und Implementierungsphase getroffene Entscheidungen haben großen Einfluss auf die Qualität und die Gesamtkosten von in der Cloud gehosteten Anwendungen und Diensten.

|                                Muster                                 |                                                                                                      Zusammenfassung                                                                                                       |
|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     [Ambassador](../ambassador.md)                     |                                                         Erstellen Sie Hilfsdienste, die im Auftrag von Consumerdiensten oder -anwendungen Netzwerkanforderungen senden.                                                          |
|          [Antikorruptionsschicht](../anti-corruption-layer.md)          |                                                               Eine „Fassade“ oder Adapterebene zwischen einer modernen Anwendung und einem älteren System implementieren                                                                |
|         [Back-Ends für Front-Ends](../backends-for-frontends.md)         |                                                          Separate Back-End-Dienste zur Nutzung durch bestimmte Front-End-Anwendungen oder -Schnittstellen erstellen                                                          |
|                           [CQRS](../cqrs.md)                           |                                                         Trennen Sie mithilfe separater Schnittstellen Datenlesevorgänge von Vorgängen zur Aktualisierung von Daten.                                                         |
| [Computeressourcenkonsolidierung](../compute-resource-consolidation.md) |                                                                     Konsolidieren mehrerer Tasks oder Vorgänge in einer einzelnen Compute-Einheit                                                                      |
|   [Externer Konfigurationsspeicher](../external-configuration-store.md)   |                                                        Konfigurationsinformationen aus dem Anwendungsbereitstellungspaket an einen zentralen Speicherort verschieben                                                         |
|            [Gatewayaggregation](../gateway-aggregation.md)            |                                                                   Aggregieren Sie mithilfe eines Gateways mehrere einzelne Anforderungen in einer einzigen Anforderung.                                                                   |
|             [Gatewayabladung](../gateway-offloading.md)             |                                                                      Lagern Sie gemeinsam genutzte oder spezielle Dienstfunktionen an einen Gatewayproxy aus.                                                                       |
|                [Gatewayrouting](../gateway-routing.md)                |                                                                            Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiterleiten                                                                            |
|                [Auswahl einer übergeordneten Instanz](../leader-election.md)                | Koordinieren Sie die Aktionen, die von einer Sammlung zusammenarbeitender Taskinstanzen ausgeführt werden, in einer verteilten Anwendung, indem eine Instanz als übergeordnete Instanz ausgewählt wird, die die Verantwortung für die Verwaltung der anderen Instanzen übernimmt. |
|              [Pipes und Filter](../pipes-and-filters.md)              |                                                     Eine Aufgabe, die komplexe Verarbeitungsvorgänge ausführt, in eine Reihe wiederverwendbarer separater Elemente unterteilen                                                      |
|                        [Sidecar](../sidecar.md)                        |                                                  Komponenten einer Anwendung zwecks Isolation und Kapselung in einem separaten Prozess oder Container bereitstellen                                                  |
|         [Hosten von statischen Inhalten](../static-content-hosting.md)         |                                                        Statische Inhalte in einem cloudbasierten Speicherdienst bereitstellen, der die Inhalte direkt an den Client übermitteln kann                                                        |
|                      [Einschnürung](../strangler.md)                      |                                         Migrieren Sie ein älteres System inkrementell, indem Sie bestimmte Teile der Funktionalität nach und nach durch neue Anwendungen und Dienste ersetzen.                                          |
