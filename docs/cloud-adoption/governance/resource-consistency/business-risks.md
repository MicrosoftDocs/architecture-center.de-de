---
title: 'Framework für die Cloudeinführung (CAF): Ressourcenkonsistenz: Motivationen und Geschäftsrisiken'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Ressourcenkonsistenz: Motivationen und Geschäftsrisiken'
author: alexbuckgit
ms.openlocfilehash: 19e0d761e4afa3473099bde2edc960c8b9eadb79
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901666"
---
# <a name="resource-consistency-motivations-and-business-risks"></a><span data-ttu-id="e3898-103">Ressourcenkonsistenz: Motivationen und Geschäftsrisiken</span><span class="sxs-lookup"><span data-stu-id="e3898-103">Resource Consistency motivations and business risks</span></span>

<span data-ttu-id="e3898-104">In diesem Artikel werden die Gründe beschrieben, warum Kunden typischerweise eine Disziplin „Ressourcenkonsistenz“ in ihre Cloud Governance-Strategie integrieren.</span><span class="sxs-lookup"><span data-stu-id="e3898-104">This article discusses the reasons that customers typically adopt a Resource Consistency discipline within a cloud governance strategy.</span></span> <span data-ttu-id="e3898-105">Darüber hinaus werden einige Beispiele für mögliche Geschäftsrisiken aufgeführt, die zu Richtlinienanweisungen führen können.</span><span class="sxs-lookup"><span data-stu-id="e3898-105">It also provides a few examples of potential business risks that can drive policy statements.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="is-resource-consistency-relevant"></a><span data-ttu-id="e3898-106">Ist Ressourcenkonsistenz relevant?</span><span class="sxs-lookup"><span data-stu-id="e3898-106">Is Resource Consistency relevant?</span></span>

<span data-ttu-id="e3898-107">Wenn es um die Bereitstellung von Ressourcen und Workloads geht, bietet die Cloud eine höhere Agilität und Flexibilität als die meisten herkömmlichen lokalen Rechenzentren.</span><span class="sxs-lookup"><span data-stu-id="e3898-107">When it comes to deploying resources and workloads, the cloud offers increased agility and flexibility over most traditional on-premises datacenters.</span></span> <span data-ttu-id="e3898-108">Allerdings sind diese potenziellen, cloudbasierten Vorteile auch an mögliche Verwaltungsnachteile gekoppelt, die den Erfolg Ihrer Cloudeinführung ernsthaft gefährden können.</span><span class="sxs-lookup"><span data-stu-id="e3898-108">However, these potential cloud-based advantages also come paired with potential management drawbacks that can seriously jeopardize the success of your cloud adoption.</span></span> <span data-ttu-id="e3898-109">Welche Ressourcen haben Sie bereitgestellt?</span><span class="sxs-lookup"><span data-stu-id="e3898-109">What assets have you deployed?</span></span> <span data-ttu-id="e3898-110">Welche Teams besitzen welche Ressourcen?</span><span class="sxs-lookup"><span data-stu-id="e3898-110">What teams own what assets?</span></span> <span data-ttu-id="e3898-111">Verfügen Sie über genügend Ressourcen, die eine Workload unterstützen?</span><span class="sxs-lookup"><span data-stu-id="e3898-111">Do you have enough resources supporting a workload?</span></span> <span data-ttu-id="e3898-112">Woher wissen Sie, ob Workloads fehlerfrei sind?</span><span class="sxs-lookup"><span data-stu-id="e3898-112">How do you know if workloads are healthy?</span></span>

<span data-ttu-id="e3898-113">Ressourcenkonsistenz ist entscheidend, um sicherzustellen, dass Ressourcen konsistent und reproduzierbar bereitgestellt, aktualisiert und konfiguriert werden, und dass Dienstunterbrechungen minimiert und in möglichst kurzer Zeit behoben werden.</span><span class="sxs-lookup"><span data-stu-id="e3898-113">Resource Consistency is crucial to ensure that resources are deployed, updated, and configured consistently and repeatably, and that service disruptions are minimized and remedied in as little time as possible.</span></span>

<span data-ttu-id="e3898-114">Die Disziplin „Ressourcenkonsistenz“ beschäftigt sich mit der Identifizierung und Korrektur von geschäftlichen Risiken im Zusammenhang mit den operationalen Aspekten Ihrer Cloudbereitstellung.</span><span class="sxs-lookup"><span data-stu-id="e3898-114">The Resource Consistency discipline is concerned with identifying and mitigating business risks related to the operational aspects of your cloud deployment.</span></span> <span data-ttu-id="e3898-115">Die Ressourcenkonsistenz umfasst die Überwachung der Anwendungs-, Workload- und Ressourcenleistung.</span><span class="sxs-lookup"><span data-stu-id="e3898-115">Resource Consistency includes monitoring of applications, workloads, and asset performance.</span></span> <span data-ttu-id="e3898-116">Darüber hinaus umfasst sie die Aufgaben, die zur Erfüllung von Skalierungsanforderungen erforderlich sind, sowie zur Korrektur von Verstößen gegen die Vereinbarung zum Leistungsservicelevel (Service Level Agreement, SLA) und zur proaktiven Vermeidung von SLA-Leistungsverstößen durch automatisierte Wartung.</span><span class="sxs-lookup"><span data-stu-id="e3898-116">It also includes the tasks required to meet scale demands, remediate performance Service Level Agreement (SLA) violations, and proactively avoid performance SLA violations through automated remediation.</span></span>

<span data-ttu-id="e3898-117">Anfängliche Testbereitstellungen erfordern möglicherweise nicht viel, das über die Einführung einiger oberflächlicher Benennungs- und Markierungsstandards hinausgeht, um Ihre Anforderungen an die Ressourcenkonsistenz zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e3898-117">Initial test deployments may not require much beyond adopting some cursory naming and tagging standards to support your Resource Consistency needs.</span></span> <span data-ttu-id="e3898-118">Mit zunehmender Reife Ihrer Cloudeinführung und der Bereitstellung komplizierterer und stärker unternehmenskritischer Ressourcen steigt die Notwendigkeit zur Investition in die Disziplin „Ressourcenkonsistenz“ schnell an.</span><span class="sxs-lookup"><span data-stu-id="e3898-118">As your cloud adoption matures and you deploy more complicated and mission-critical assets, the need to invest in the Resource Consistency discipline increases rapidly.</span></span>

## <a name="business-risk"></a><span data-ttu-id="e3898-119">Geschäftsrisiken</span><span class="sxs-lookup"><span data-stu-id="e3898-119">Business risk</span></span>

<span data-ttu-id="e3898-120">Die Disziplin „Ressourcenkonsistenz“ befasst sich mit den operationalen Kerngeschäftsrisiken.</span><span class="sxs-lookup"><span data-stu-id="e3898-120">The Resource Consistency discipline attempts to address core operational business risks.</span></span> <span data-ttu-id="e3898-121">Arbeiten Sie mit Ihren Geschäfts- und IT-Teams zusammen, um diese Risiken zu identifizieren und auf Relevanz zu überwachen, um sie bei der Planung und Implementierung Ihrer Cloudbereitstellungen zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="e3898-121">Work with your business and IT teams to identify these risks and monitor each of them for relevance as you plan for and implement your cloud deployments.</span></span>

<span data-ttu-id="e3898-122">Die Risiken werden je nach Organisation unterschiedlich sein, aber die folgenden allgemeinen Risiken können als Ausgangspunkt für Diskussionen innerhalb Ihres Cloud Governance-Teams verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="e3898-122">Risks will differ between organization, but the following serve as common risks that you can use as a starting point for discussions within your Cloud Governance team:</span></span>

- <span data-ttu-id="e3898-123">**Unnötige Betriebskosten**.</span><span class="sxs-lookup"><span data-stu-id="e3898-123">**Unnecessary operational cost**.</span></span> <span data-ttu-id="e3898-124">Veraltete oder nicht verwendete Ressourcen oder Ressourcen, die in Zeiten geringen Bedarfs überdimensioniert sind, verursachen unnötige zusätzliche Betriebskosten.</span><span class="sxs-lookup"><span data-stu-id="e3898-124">Obsolete or unused resources, or resources that are overprovisioned during times of low demand, add unnecessary operational costs.</span></span>
- <span data-ttu-id="e3898-125">**Unterdimensionierte Ressourcen**.</span><span class="sxs-lookup"><span data-stu-id="e3898-125">**Underprovisioned resources**.</span></span> <span data-ttu-id="e3898-126">Ressourcen, nach denen eine höhere Nachfrage herrscht als erwartet, können zu Unterbrechungen des Geschäftsbetriebs führen, weil Cloudressourcen von der Nachfrage überfordert werden.</span><span class="sxs-lookup"><span data-stu-id="e3898-126">Resources that experience higher than anticipated demand can result in business disruption as cloud resources are overwhelmed by demand.</span></span>
- <span data-ttu-id="e3898-127">**Verwaltungsineffizienzen**.</span><span class="sxs-lookup"><span data-stu-id="e3898-127">**Management inefficiencies**.</span></span> <span data-ttu-id="e3898-128">Das Fehlen konsistenter Benennungs- und Markierungsmetadaten, die Ressourcen zugeordnet sind, können dazu führen, dass IT-Mitarbeiter Schwierigkeiten haben, Ressourcen für Verwaltungsaufgaben zu finden, oder den Besitz und Abrechnungsinformationen im Zusammenhang mit den Ressourcen zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="e3898-128">Lack of consistent naming and tagging metadata associated with resources can lead to IT staff having difficulty finding resources for management tasks or identifying ownership and accounting information related to assets.</span></span> <span data-ttu-id="e3898-129">Dies führt zu Ineffizienzen bei der Verwaltung, die Kosten erhöhen und die IT-Reaktionsfähigkeit bei Dienstunterbrechungen oder anderen Betriebsproblemen verlangsamen können.</span><span class="sxs-lookup"><span data-stu-id="e3898-129">This results in management inefficiencies that can increase cost and slow IT responsiveness to service disruption or other operational issues.</span></span>
- <span data-ttu-id="e3898-130">**Unterbrechung des Geschäftsbetriebs**.</span><span class="sxs-lookup"><span data-stu-id="e3898-130">**Business Interruption**.</span></span> <span data-ttu-id="e3898-131">Dienstausfälle, die Verstöße gegen die vereinbarten Vereinbarungen zum Servicelevel (SLA) Ihrer Organisation nach sich ziehen, können zu Geschäftsverlusten oder anderen finanziellen Auswirkungen auf Ihr Unternehmen führen.</span><span class="sxs-lookup"><span data-stu-id="e3898-131">Service disruptions that result in violations of your organization's established Service Level Agreements (SLAs) can result in loss of business or other financial impacts to your company.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3898-132">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e3898-132">Next steps</span></span>

<span data-ttu-id="e3898-133">Dokumentieren Sie mit der [Cloudverwaltungsvorlage](./template.md) die Geschäftsrisiken, die wahrscheinlich durch den aktuellen Cloudeinführungsplan entstehen.</span><span class="sxs-lookup"><span data-stu-id="e3898-133">Using the [Cloud Management Template](./template.md), document business risks that are likely to be introduced by the current cloud adoption plan.</span></span>

<span data-ttu-id="e3898-134">Sobald ein Verständnis für realistische Geschäftsrisiken hergestellt ist, besteht der nächste Schritt darin, die Risikotoleranz des Unternehmens zu dokumentieren sowie die Indikatoren und Schlüsselmetrik zur Überwachung dieser Toleranz zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="e3898-134">Once an understanding of realistic business risks is established, the next step is to document the business's tolerance for risk and the indicators and key metrics to monitor that tolerance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e3898-135">Grundlegendes zu Indikatoren, Metriken und Risikotoleranz</span><span class="sxs-lookup"><span data-stu-id="e3898-135">Understand indicators, metrics, and risk tolerance</span></span>](./metrics-tolerance.md)