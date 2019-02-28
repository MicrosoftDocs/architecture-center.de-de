---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Verbesserung der Disziplin „Beschleunigung der Bereitstellung“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Beschleunigung der Bereitstellung“
author: alexbuckgit
ms.openlocfilehash: 98192c777d8866efb01544737e8cabea6354c4d7
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902194"
---
# <a name="deployment-acceleration-discipline-improvement"></a><span data-ttu-id="20599-103">Verbesserung der Disziplin „Beschleunigung der Bereitstellung“</span><span class="sxs-lookup"><span data-stu-id="20599-103">Deployment Acceleration discipline improvement</span></span>

<span data-ttu-id="20599-104">Bei der Disziplin „Beschleunigung der Bereitstellung“ geht es darum, Richtlinien einzurichten, die sicherstellen, dass Ressourcen konsistent und wiederholbar bereitgestellt und konfiguriert werden und während des gesamten Lebenszyklus richtlinienkonform sind.</span><span class="sxs-lookup"><span data-stu-id="20599-104">The Deployment Acceleration discipline focuses on establishing policies that ensure that resources are deployed and configured consistently and repeatably, and remain in compliance throughout their lifecycle.</span></span> <span data-ttu-id="20599-105">Innerhalb der fünf Disziplinen der Cloudgovernance umfasst die Beschleunigung der Bereitstellung Entscheidungen zu Folgendem: Automatisierung von Bereitstellungen, Quellsteuerung von Bereitstellungsartefakten, Überwachung bereitgestellter Ressourcen zur Aufrechterhaltung des gewünschten Zustands und Überwachung von Complianceproblemen.</span><span class="sxs-lookup"><span data-stu-id="20599-105">Within the Five Disciplines of Cloud Governance, Deployment Acceleration includes decisions regarding automating deployments, source-controlling deployment artifacts, monitoring deployed resources to maintain desired state, and auditing any compliance issues.</span></span>

<span data-ttu-id="20599-106">Dieser Artikel beschreibt einige potenzielle Aufgaben, die Ihr Unternehmen ausführen kann, um die Disziplin „Beschleunigung der Bereitstellung“ besser erstellen und weiterentwickeln zu können.</span><span class="sxs-lookup"><span data-stu-id="20599-106">This article outlines some potential tasks your company can engage in to better develop and mature the Deployment Acceleration discipline.</span></span> <span data-ttu-id="20599-107">Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Einführung und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für die Cloudgovernance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span><span class="sxs-lookup"><span data-stu-id="20599-107">These tasks can be broken down into planning, building, adopting, and operating phases of implementing a cloud solution, which are then iterated on allowing the development of an [incremental approach to cloud governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).</span></span>

![Vier Phasen der Einführung](../../_images/adoption-phases.png)

<span data-ttu-id="20599-109">*Abbildung 1: Einführungsphasen des inkrementellen Ansatzes für die Cloudgovernance.*</span><span class="sxs-lookup"><span data-stu-id="20599-109">*Figure 1. Adoption phases of the incremental approach to cloud governance.*</span></span>

<span data-ttu-id="20599-110">Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="20599-110">It's impossible for any one document to account for the requirements of all businesses.</span></span> <span data-ttu-id="20599-111">Daher werden im vorliegenden Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20599-111">As such, this article outlines suggested minimum and potential example activities for each phase of the governance maturation process.</span></span> <span data-ttu-id="20599-112">Ziel dieser Aktivitäten ist es, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="20599-112">The initial objective of these activities is to help you build a [Policy MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) and establish a framework for incremental policy evolution.</span></span> <span data-ttu-id="20599-113">Ihr Cloudgovernance-Team muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance der Identitätsbaseline zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="20599-113">Your Cloud Governance team will need to decide how much to invest in these activities to improve your Identity Baseline governance capabilities.</span></span>

> [!CAUTION]
> <span data-ttu-id="20599-114">Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="20599-114">Neither the minimum or potential activities outlined in this article are aligned to specific corporate policies or third party compliance requirements.</span></span> <span data-ttu-id="20599-115">Dieser Leitfaden soll bei Gesprächen über die Ausrichtung beider Anforderungen auf ein Cloudgovernancemodell helfen.</span><span class="sxs-lookup"><span data-stu-id="20599-115">This guidance is designed to help facilitate the conversations that will lead to alignment of both requirements with a cloud governance model.</span></span>

## <a name="planning-and-readiness"></a><span data-ttu-id="20599-116">Planung und Bereitschaft</span><span class="sxs-lookup"><span data-stu-id="20599-116">Planning and readiness</span></span>

<span data-ttu-id="20599-117">Diese Entwicklungsphase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien.</span><span class="sxs-lookup"><span data-stu-id="20599-117">This phase of governance maturity bridges the divide between business outcomes and actionable strategies.</span></span> <span data-ttu-id="20599-118">Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.</span><span class="sxs-lookup"><span data-stu-id="20599-118">During this process, the leadership team defines specific metrics, maps those metrics to the digital estate, and begins planning the overall migration effort.</span></span>

<span data-ttu-id="20599-119">**Mindestens empfohlene Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-119">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="20599-120">Bewerten Sie die Optionen Ihrer [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md), und implementieren Sie eine Hybridstrategie, die für Ihre Organisation geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="20599-120">Evaluate your [Deployment Acceleration toolchain](toolchain.md) options and implement a hybrid strategy that is appropriate to your organization.</span></span>
- <span data-ttu-id="20599-121">Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.</span><span class="sxs-lookup"><span data-stu-id="20599-121">Develop a draft Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="20599-122">Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="20599-122">Educate and involve the people and teams affected by the development of Architecture Guidelines.</span></span>
- <span data-ttu-id="20599-123">Schulen Sie Entwicklungsteams und IT-Mitarbeiter, damit diese die DevSecOps-Prinzipien und -Strategien und die Bedeutung vollständig automatisierter Bereitstellungen in der Disziplin „Beschleunigung der Bereitstellung“ genau verstehen.</span><span class="sxs-lookup"><span data-stu-id="20599-123">Train development teams and IT staff to understand DevSecOps principles and strategies and the importance of fully automated deployments in the Deployment Acceleration Discipline.</span></span>

<span data-ttu-id="20599-124">**Potenzielle Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-124">**Potential activities:**</span></span>

- <span data-ttu-id="20599-125">Definieren Sie Rollen und Zuweisungen, die die Beschleunigung der Bereitstellung in der Cloud regeln.</span><span class="sxs-lookup"><span data-stu-id="20599-125">Define roles and assignments that will govern Deployment Acceleration in the cloud.</span></span>

## <a name="build-and-pre-deployment"></a><span data-ttu-id="20599-126">Aufbau und Aufgaben vor der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="20599-126">Build and pre-deployment</span></span>

<span data-ttu-id="20599-127">**Mindestens empfohlene Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-127">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="20599-128">Führen Sie vollständig automatisierte Bereitstellungen bei neuen cloudbasierten Anwendungen frühzeitig im Entwicklungsprozess ein.</span><span class="sxs-lookup"><span data-stu-id="20599-128">For new cloud-based applications, introduce fully automated deployments early in the development process.</span></span> <span data-ttu-id="20599-129">Diese Investition verbessert die Zuverlässigkeit für Ihre Testprozesse und stellt die Konsistenz über all Ihre Entwicklungs-, Qualitätssicherungs- und Produktionsumgebungen hinweg sicher.</span><span class="sxs-lookup"><span data-stu-id="20599-129">This investment will improve the reliability of your testing processes and ensure consistency across your development, QA, and production environments.</span></span>
- <span data-ttu-id="20599-130">Speichern Sie alle Bereitstellungsartefakte, z.B. Bereitstellungsvorlagen oder Konfigurationsskripts, auf einer Plattform für die Quellcodeverwaltung wie GitHub oder Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="20599-130">Store all deployment artifacts such as deployment templates or configuration scripts using a source-control platform such as GitHub or Azure DevOps.</span></span>
- <span data-ttu-id="20599-131">Ziehen Sie die Durchführung eines Pilottests in Betracht, bevor Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) implementieren, um sicherzustellen, dass diese Ihre Bereitstellungen so weit wie möglich optimiert.</span><span class="sxs-lookup"><span data-stu-id="20599-131">Consider a pilot test before implementing your [Deployment Acceleration toolchain](toolchain.md), making sure it streamlines your deployments as much as possible.</span></span> <span data-ttu-id="20599-132">Wenden Sie Feedback aus den Pilottests während der Phase vor der Bereitstellung an, und wiederholen Sie die Tests ggf.</span><span class="sxs-lookup"><span data-stu-id="20599-132">Apply feedback from pilot tests during the pre-deployment phase, repeating as needed.</span></span>
- <span data-ttu-id="20599-133">Bewerten Sie die logische und physische Architektur Ihrer Anwendungen, und identifizieren Sie Möglichkeiten, um die Bereitstellung von Anwendungsressourcen zu automatisieren oder Teile der Architektur mithilfe anderer cloudbasierter Ressourcen zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="20599-133">Evaluate the logical and physical architecture of your applications, and identify opportunities to automate the deployment of application resources or improve portions of the architecture using other cloud-based resources.</span></span>
- <span data-ttu-id="20599-134">Aktualisieren Sie das Dokument mit Architekturrichtlinien mit den Plänen für die Bereitstellung und die Akzeptanz durch Benutzer, und geben Sie dieses an die wichtigsten Beteiligten weiter.</span><span class="sxs-lookup"><span data-stu-id="20599-134">Update the Architecture Guidelines document to include deployment and user adoption plans, and distribute to key stakeholders.</span></span>
- <span data-ttu-id="20599-135">Setzen Sie die Schulung der Personen und Teams fort, die von den Architekturrichtlinien am meisten betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="20599-135">Continue to educate the people and teams most affected by the architecture guidelines.</span></span>

<span data-ttu-id="20599-136">**Potenzielle Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-136">**Potential activities:**</span></span>

- <span data-ttu-id="20599-137">Definieren Sie eine CI/CD-Pipeline (Continuous Integration/Continuous Deployment), um die Freigabe von Updates für Ihre Anwendungen in Ihren Entwicklungs-, Qualitätssicherungs- und Produktionsumgebungen vollständig zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="20599-137">Define a continuous integration and continuous deployment (CI/CD) pipeline to fully manage releasing updates to your application through your development, QA, and production environments.</span></span>

## <a name="adopt-and-migrate"></a><span data-ttu-id="20599-138">Einführen und Migrieren</span><span class="sxs-lookup"><span data-stu-id="20599-138">Adopt and migrate</span></span>

<span data-ttu-id="20599-139">Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.</span><span class="sxs-lookup"><span data-stu-id="20599-139">Migration is an incremental process that focuses on the movement, testing, and adoption of applications or workloads in an existing digital estate.</span></span>

<span data-ttu-id="20599-140">**Mindestens empfohlene Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-140">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="20599-141">Migrieren Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) aus der Entwicklung in die Produktion.</span><span class="sxs-lookup"><span data-stu-id="20599-141">Migrate your [Deployment Acceleration toolchain](toolchain.md) from development to production.</span></span>
- <span data-ttu-id="20599-142">Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.</span><span class="sxs-lookup"><span data-stu-id="20599-142">Update the Architecture Guidelines document and distribute to key stakeholders.</span></span>
- <span data-ttu-id="20599-143">Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Entwickler und die IT-Abteilung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="20599-143">Develop educational materials and documentation, awareness communications, incentives, and other programs to help drive developer and IT adoption.</span></span>

<span data-ttu-id="20599-144">**Potenzielle Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-144">**Potential activities:**</span></span>

- <span data-ttu-id="20599-145">Überprüfen Sie, ob die in der Erstellungsphase und der Phase vor der Bereitstellung definierten Best Practices ordnungsgemäß ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="20599-145">Validate that the best practices defined during the build and pre-deployment phases are properly executed.</span></span>
- <span data-ttu-id="20599-146">Stellen Sie vor der Freigabe sicher, dass jede Anwendung oder Workload der Strategie für die Beschleunigung der Bereitstellung entspricht.</span><span class="sxs-lookup"><span data-stu-id="20599-146">Ensure that each application or workload aligns with the Deployment Acceleration strategy before release.</span></span>

## <a name="operate-and-post-implementation"></a><span data-ttu-id="20599-147">Betrieb und Aufgaben nach der Implementierung</span><span class="sxs-lookup"><span data-stu-id="20599-147">Operate and post-implementation</span></span>

<span data-ttu-id="20599-148">Sobald die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben.</span><span class="sxs-lookup"><span data-stu-id="20599-148">Once the transformation is complete, governance and operations must live on for the natural lifecycle of an application or workload.</span></span> <span data-ttu-id="20599-149">In dieser Phase der Ausgereiftheit der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus beginnt, sich zu stabilisieren.</span><span class="sxs-lookup"><span data-stu-id="20599-149">This phase of governance maturity focuses on the activities that commonly come after the solution is implemented and the transformation cycle begins to stabilize.</span></span>

<span data-ttu-id="20599-150">**Mindestens empfohlene Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-150">**Minimum suggested activities:**</span></span>

- <span data-ttu-id="20599-151">Passen Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) an die Änderungen der Identitätsanforderungen Ihrer Organisation an.</span><span class="sxs-lookup"><span data-stu-id="20599-151">Customize your [Deployment Acceleration toolchain](toolchain.md) based on changes to your organization’s changing identity needs.</span></span>
- <span data-ttu-id="20599-152">Automatisieren Sie Benachrichtigungen und Berichte, damit Sie bei potenziellen Konfigurationsproblemen oder Bedrohungen gewarnt werden.</span><span class="sxs-lookup"><span data-stu-id="20599-152">Automate notifications and reports to alert you of potential configuration issues or malicious threats.</span></span>
- <span data-ttu-id="20599-153">Überwachen Sie die Anwendungs- und Ressourcennutzung, und erstellen Sie entsprechende Berichte.</span><span class="sxs-lookup"><span data-stu-id="20599-153">Monitor and report on application and resource usage.</span></span>
- <span data-ttu-id="20599-154">Erstellen Sie Berichte zu Metriken nach der Bereitstellung, und geben Sie diese an die Beteiligten weiter.</span><span class="sxs-lookup"><span data-stu-id="20599-154">Report on post-deployment metrics and distribute to stakeholders.</span></span>
- <span data-ttu-id="20599-155">Überarbeiten Sie die Architekturrichtlinien, um zukünftige Einführungsprozesse zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="20599-155">Revise the Architecture Guidelines to guide future adoption processes.</span></span>
- <span data-ttu-id="20599-156">Sorgen Sie für die regelmäßige Schulung und Kommunikation mit den betroffenen Personen und Teams, um die ununterbrochene Einhaltung der Architekturrichtlinien sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="20599-156">Continue to communicate with and train the affected people and teams on a regular basis to ensure ongoing adherence to Architecture Guidelines.</span></span>

<span data-ttu-id="20599-157">**Potenzielle Aktivitäten**:</span><span class="sxs-lookup"><span data-stu-id="20599-157">**Potential activities:**</span></span>

- <span data-ttu-id="20599-158">Konfigurieren Sie ein Überwachungs- und Berichterstellungstool für die Konfiguration des gewünschten Zustands.</span><span class="sxs-lookup"><span data-stu-id="20599-158">Configure a desired state configuration monitoring and reporting tool.</span></span>
- <span data-ttu-id="20599-159">Überprüfen Sie die Konfigurationstools und -skripts regelmäßig, um Prozesse zu verbessern und allgemeine Probleme zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="20599-159">Regularly review configuration tools and scripts to improve processes and identify common issues.</span></span>
- <span data-ttu-id="20599-160">Arbeiten Sie mit den Entwicklungs-, Betriebs- und Sicherheitsteams zusammen, um DevSecOps-Methoden weiterzuentwickeln und ineffiziente Organisationssilos aufzubrechen.</span><span class="sxs-lookup"><span data-stu-id="20599-160">Work with development, operations, and security teams to help mature DevSecOps practices and break down organizational silos that lead to inefficiencies.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20599-161">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="20599-161">Next steps</span></span>

<span data-ttu-id="20599-162">Sie kennen jetzt das Konzept der Identitätsgovernance in der Cloud. Als Nächstes sollten Sie die [Toolkette für die Identitätsbaseline](toolchain.md) untersuchen, um die Azure-Tools und -Features zu identifizieren, die Sie bei der Entwicklung der Governancedisziplin „Identitätsbaseline“ auf der Azure-Plattform benötigen.</span><span class="sxs-lookup"><span data-stu-id="20599-162">Now that you understand the concept of cloud identity governance, examine the [Identity Baseline toolchain](toolchain.md) to identify Azure tools and features that you'll need when developing the Identity Baseline governance discipline on the Azure platform.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="20599-163">Identitätsbaseline-Toolkette für Azure</span><span class="sxs-lookup"><span data-stu-id="20599-163">Identity Baseline toolchain for Azure</span></span>](toolchain.md)