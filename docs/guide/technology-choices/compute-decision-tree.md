---
title: Entscheidungsstruktur für Azure-Computedienste
description: Ein Flussdiagramm zur Auswahl eines Computediensts
author: MikeWasson
ms.date: 04/21/2018
ms.openlocfilehash: 3dcfbd156d4fced863a56bcc8bb74483aa665f9f
ms.sourcegitcommit: 7ced70ebc11aa0df0dc0104092d3cc6ad5c28bd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="decision-tree-for-azure-compute-services"></a><span data-ttu-id="2478d-103">Entscheidungsstruktur für Azure-Computedienste</span><span class="sxs-lookup"><span data-stu-id="2478d-103">Decision tree for Azure compute services</span></span>

<span data-ttu-id="2478d-104">Azure bietet eine Vielzahl von Möglichkeiten zum Hosten Ihres Anwendungscodes.</span><span class="sxs-lookup"><span data-stu-id="2478d-104">Azure offers a number of ways to host your application code.</span></span> <span data-ttu-id="2478d-105">Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2478d-105">The term *compute* refers to the hosting model for the computing resources that your application runs on.</span></span> <span data-ttu-id="2478d-106">Das folgende Flussdiagramm unterstützt Sie bei der Auswahl eines Computediensts für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2478d-106">The following flowchart will help you to choose a compute service for your application.</span></span> <span data-ttu-id="2478d-107">Das Flussdiagramm führt Sie durch eine Reihe wichtiger Entscheidungskriterien, um eine Empfehlung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="2478d-107">The flowchart guides you through a set of key decision criteria to reach a recommendation.</span></span> 

<span data-ttu-id="2478d-108">**Betrachten Sie dieses Flussdiagramm als Ausgangspunkt.**</span><span class="sxs-lookup"><span data-stu-id="2478d-108">**Treat this flowchart as a starting point.**</span></span> <span data-ttu-id="2478d-109">Da jede Anwendung besondere Anforderungen aufweist, betrachten Sie die Empfehlung als Ausgangspunkt.</span><span class="sxs-lookup"><span data-stu-id="2478d-109">Every application has unique requirements, so use the recommendation as a starting point.</span></span> <span data-ttu-id="2478d-110">Führen Sie dann eine ausführlichere Auswertung durch, bei der Sie z.B. folgende Aspekte betrachten:</span><span class="sxs-lookup"><span data-stu-id="2478d-110">Then perform a more detailed evaluation, looking at aspects such as:</span></span>
 
- <span data-ttu-id="2478d-111">Funktionsumfang</span><span class="sxs-lookup"><span data-stu-id="2478d-111">Feature set</span></span>
- [<span data-ttu-id="2478d-112">Diensteinschränkungen</span><span class="sxs-lookup"><span data-stu-id="2478d-112">Service limits</span></span>](/azure/azure-subscription-service-limits)
- [<span data-ttu-id="2478d-113">Kosten</span><span class="sxs-lookup"><span data-stu-id="2478d-113">Cost</span></span>](https://azure.microsoft.com/pricing/)
- [<span data-ttu-id="2478d-114">SLA</span><span class="sxs-lookup"><span data-stu-id="2478d-114">SLA</span></span>](https://azure.microsoft.com/support/legal/sla/)
- [<span data-ttu-id="2478d-115">Regionale Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="2478d-115">Regional availability</span></span>](https://azure.microsoft.com/global-infrastructure/services/)
- <span data-ttu-id="2478d-116">Entwicklerökosystem und Teamkenntnisse</span><span class="sxs-lookup"><span data-stu-id="2478d-116">Developer ecosystem and team skills</span></span>
- [<span data-ttu-id="2478d-117">Kriterien für die Auswahl einer Azure-Compute-Option</span><span class="sxs-lookup"><span data-stu-id="2478d-117">Compute comparison tables</span></span>](./compute-comparison.md)

<span data-ttu-id="2478d-118">Wenn Ihre Anwendung aus mehreren Workloads besteht, bewerten Sie jede Workload getrennt.</span><span class="sxs-lookup"><span data-stu-id="2478d-118">If your application consists of multiple workloads, evaluate each workload separately.</span></span> <span data-ttu-id="2478d-119">Eine vollständige Lösung kann zwei oder mehr Computedienste umfassen.</span><span class="sxs-lookup"><span data-stu-id="2478d-119">A complete solution may incorporate two or more compute services.</span></span>

![](../images/compute-decision-tree.svg)

