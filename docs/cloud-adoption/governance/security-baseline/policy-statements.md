---
title: 'CAF: Beispiele für Richtlinienanweisungen der Sicherheitsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beispiele für Richtlinienanweisungen der Sicherheitsbaseline
author: BrianBlanchard
ms.openlocfilehash: ac40e022f8dff0c3021c04cd9d6ae42d28afaf1f
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900797"
---
# <a name="security-baseline-sample-policy-statements"></a><span data-ttu-id="3641c-103">Beispiele für Richtlinienanweisungen der Sicherheitsbaseline</span><span class="sxs-lookup"><span data-stu-id="3641c-103">Security Baseline sample policy statements</span></span>

<span data-ttu-id="3641c-104">Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden.</span><span class="sxs-lookup"><span data-stu-id="3641c-104">Individual cloud policy statements are guidelines for addressing specific risks identified during your risk assessment process.</span></span> <span data-ttu-id="3641c-105">Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="3641c-105">These statements should provide a concise summary of risks and plans to deal with them.</span></span> <span data-ttu-id="3641c-106">Jede Anweisungsdefinition sollte diese Informationen enthalten:</span><span class="sxs-lookup"><span data-stu-id="3641c-106">Each statement definition should include these pieces of information:</span></span>

- <span data-ttu-id="3641c-107">Technisches Risiko: Eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.</span><span class="sxs-lookup"><span data-stu-id="3641c-107">Technical risk - A summary of the risk this policy will address.</span></span>
- <span data-ttu-id="3641c-108">Richtlinienanweisung: Eine klare, zusammenfassende Erläuterung der Anforderungen der Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="3641c-108">Policy statement - A clear summary explanation of the policy requirements.</span></span>
- <span data-ttu-id="3641c-109">Technische Optionen: Direkt umsetzbare Empfehlungen, Spezifikationen oder anderen Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="3641c-109">Technical options - Actionable recommendations, specifications, or other guidance that IT teams and developers can use when implementing the policy.</span></span>

<span data-ttu-id="3641c-110">Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen sicherheitsbezogenen Geschäftsrisiken und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von eigentlichen Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="3641c-110">The following sample policy statements address a number of common security-related business risks, and are provided as examples for you to reference when drafting actual policy statements addressing your own organization's needs.</span></span> <span data-ttu-id="3641c-111">Diese Beispiele sind nicht restriktiv gemeint, und es gibt immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem einzelnen identifizierten Risiko.</span><span class="sxs-lookup"><span data-stu-id="3641c-111">These examples are not meant to be proscriptive, and there are potentially several policy options for dealing with any single identified risk.</span></span> <span data-ttu-id="3641c-112">Arbeiten Sie eng mit den Geschäfts-, Sicherheits- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre besonderen Sicherheitsrisiken zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="3641c-112">Work closely with business, security, and IT teams to identify the best policy solutions for your particular security risks.</span></span>  

## <a name="asset-classification"></a><span data-ttu-id="3641c-113">Ressourcenklassifizierung</span><span class="sxs-lookup"><span data-stu-id="3641c-113">Asset classification</span></span>

<span data-ttu-id="3641c-114">**Technisches Risiko**: Ressourcen, die nicht richtig als unternehmenskritisch oder im Zusammenhang mit sensiblen Daten stehend identifiziert werden, erhalten möglicherweise keinen ausreichenden Schutz, was zu potenziellen Datenverlusten oder Geschäftsunterbrechungen führt.</span><span class="sxs-lookup"><span data-stu-id="3641c-114">**Technical risk**: Assets that are not correctly identified as mission-critical or involving sensitive data may not receive sufficient protections, leading to potential data leaks or business disruptions.</span></span>

<span data-ttu-id="3641c-115">**Richtlinienanweisung**: Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-115">**Policy statement**: All deployed assets must be categorized by criticality and data classification.</span></span> <span data-ttu-id="3641c-116">Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloudgovernanceteam und die Besitzer der Anwendung überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-116">Classifications must be reviewed by the Cloud Governance team and the application owner before deployment to the cloud.</span></span>

<span data-ttu-id="3641c-117">**Potenzielle Entwurfsoption**: Legen Sie [Standards für Ressourcentags](../../decision-guides/resource-tagging/overview.md) fest, und stellen Sie sicher, dass IT-Mitarbeiter sie konsistent mithilfe von [Azure-Ressourcentags](/azure/azure-resource-manager/resource-group-using-tags) auf alle bereitgestellten Ressourcen anwenden.</span><span class="sxs-lookup"><span data-stu-id="3641c-117">**Potential design option**: Establish [resource tagging standards](../../decision-guides/resource-tagging/overview.md) and ensure IT staff apply them consistently to any deployed resources using [Azure resource tags](/azure/azure-resource-manager/resource-group-using-tags).</span></span>

## <a name="data-encryption"></a><span data-ttu-id="3641c-118">Datenverschlüsselung</span><span class="sxs-lookup"><span data-stu-id="3641c-118">Data encryption</span></span>

<span data-ttu-id="3641c-119">**Technisches Risiko**: Es besteht das Risiko, dass geschützte Daten während der Speicherung ungeschützt sind.</span><span class="sxs-lookup"><span data-stu-id="3641c-119">**Technical risk**: There is a risk of protected data being exposed during storage.</span></span>

<span data-ttu-id="3641c-120">**Richtlinienanweisung**: Alle geschützten Daten müssen im Ruhezustand verschlüsselt sein.</span><span class="sxs-lookup"><span data-stu-id="3641c-120">**Policy statement**: All protected data must be encrypted when at rest.</span></span>

<span data-ttu-id="3641c-121">**Potenzielle Entwurfsoption**: Im Artikel [Übersicht über die Azure-Verschlüsselung](/azure/security/security-azure-encryption-overview) finden Sie Informationen darüber, wie die Verschlüsselung für ruhende Daten auf der Azure-Plattform ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="3641c-121">**Potential design option**: See the [Azure encryption overview](/azure/security/security-azure-encryption-overview) article for a discussion of how data at rest encryption is performed on the Azure platform.</span></span>  

## <a name="network-isolation"></a><span data-ttu-id="3641c-122">Netzwerkisolation</span><span class="sxs-lookup"><span data-stu-id="3641c-122">Network isolation</span></span>

<span data-ttu-id="3641c-123">**Technisches Risiko**: Konnektivität zwischen Netzwerken und Subnetzen innerhalb von Netzwerken weist potenzielle Sicherheitslücken auf, die zu Datenverlusten oder einer Unterbrechung der unternehmenskritischen Dienste führen können.</span><span class="sxs-lookup"><span data-stu-id="3641c-123">**Technical risk**: Connectivity between networks and subnets within networks introduces potential vulnerabilities that can result in data leaks or disruption of mission-critical services.</span></span>

<span data-ttu-id="3641c-124">**Richtlinienanweisung**: Netzwerksubnetze mit geschützten Daten müssen von allen anderen Subnetzen isoliert werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-124">**Policy statement**: Network subnets containing protected data must be isolated from any other subnets.</span></span> <span data-ttu-id="3641c-125">Der Netzwerkdatenverkehr zwischen Subnetzen mit geschützten Daten muss regelmäßig überwacht werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-125">Network traffic between protected data subnets is to be audited regularly.</span></span>

<span data-ttu-id="3641c-126">**Potenzielle Entwurfsoption**: In Azure wird die Isolation von Netzwerk und Subnetz über [Azure Virtual Networks](/azure/virtual-network/virtual-networks-overview) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="3641c-126">**Potential design option**: In Azure, network and subnet isolation is managed through [Azure Virtual Networks](/azure/virtual-network/virtual-networks-overview).</span></span>

## <a name="secure-external-access"></a><span data-ttu-id="3641c-127">Sicherer externer Zugriff</span><span class="sxs-lookup"><span data-stu-id="3641c-127">Secure external access</span></span>

<span data-ttu-id="3641c-128">**Technisches Risiko**: Den Zugriff auf Workloads über das öffentliche Internet zuzulassen bringt das Risiko des unbefugten Eindringens mit sich, was nicht autorisierte Offenlegung von Daten oder Geschäftsunterbrechung zur Folge hat.</span><span class="sxs-lookup"><span data-stu-id="3641c-128">**Technical risk**: Allowing access to workloads from the public internet introduces a risk of intrusion resulting in unauthorized data exposure or business disruption.</span></span>

<span data-ttu-id="3641c-129">**Richtlinienanweisung**: Kein Subnetz mit geschützten Daten ist direkt über das öffentliche Internet oder rechenzentrumsübergreifend zugänglich.</span><span class="sxs-lookup"><span data-stu-id="3641c-129">**Policy statement**: No subnet containing protected data can be directly accessed over public internet or across datacenters.</span></span> <span data-ttu-id="3641c-130">Der Zugriff auf diese Subnetze muss über zwischengeschaltete Subnetze geroutet werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-130">Access to those subnets must be routed through intermediate subnet works.</span></span> <span data-ttu-id="3641c-131">Der gesamte Zugriff auf diese Subnetze muss über eine Firewalllösung erfolgen, die Funktionen zur Paketüberprüfung und Sperrfunktionen durchführen kann.</span><span class="sxs-lookup"><span data-stu-id="3641c-131">All access into those subnets must come through a firewall solution capable of performing packet scanning and blocking functions.</span></span>

<span data-ttu-id="3641c-132">**Potenzielle Entwurfsoption**: Sichern Sie in Azure öffentliche Endpunkte durch Bereitstellen einer [DMZ zwischen dem öffentlichen Internet und Ihrem cloudbasierten Netzwerk](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).</span><span class="sxs-lookup"><span data-stu-id="3641c-132">**Potential design option**: In Azure, secure public endpoints by deploying a [DMZ between the public internet and your cloud-based network](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).</span></span>

## <a name="ddos-protection"></a><span data-ttu-id="3641c-133">DDoS-Schutz</span><span class="sxs-lookup"><span data-stu-id="3641c-133">DDoS protection</span></span>

<span data-ttu-id="3641c-134">**Technisches Risiko**: Verteilte DoS-Angriffe (Distributed denial of service, DDoS) können zu einer Unterbrechung des Geschäftsbetriebs führen.</span><span class="sxs-lookup"><span data-stu-id="3641c-134">**Technical risk**: Distributed denial of service (DDoS) attacks can result in a business interruption.</span></span>

<span data-ttu-id="3641c-135">**Richtlinienanweisung**: Stellen Sie automatisierte DDoS-Entschärfungsmechanismen für alle öffentlich zugänglichen Netzwerkendpunkte bereit.</span><span class="sxs-lookup"><span data-stu-id="3641c-135">**Policy statement**: Deploy automated DDoS mitigation mechanisms to all publicly accessible network endpoints.</span></span>

<span data-ttu-id="3641c-136">**Potenzielle Entwurfsoption**: Minimieren Sie durch DDoS-Angriffe verursachte Unterbrechungen mit [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview).</span><span class="sxs-lookup"><span data-stu-id="3641c-136">**Potential design option**: Use [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview) to minimize disruptions caused by DDoS attacks.</span></span>

## <a name="secure-on-premises-connectivity"></a><span data-ttu-id="3641c-137">Absichern der lokalen Konnektivität</span><span class="sxs-lookup"><span data-stu-id="3641c-137">Secure on-premises connectivity</span></span>

<span data-ttu-id="3641c-138">**Technisches Risiko**: Unverschlüsselter Datenverkehr zwischen Ihrem Cloudnetzwerk und dem lokalen über das öffentliche Internet ist anfällig dafür, abgefangen zu werden – mit dem Risiko der Offenlegung von Daten.</span><span class="sxs-lookup"><span data-stu-id="3641c-138">**Technical risk**: Unencrypted traffic between your cloud network and on-premises over the public internet is vulnerable to interception, introducing the risk of data exposure.</span></span>

<span data-ttu-id="3641c-139">**Richtlinienanweisung**: Alle Verbindungen zwischen lokalen und Cloudnetzwerken müssen entweder über eine sichere, verschlüsselte VPN-Verbindung oder eine dedizierte private WAN-Verbindung erfolgen.</span><span class="sxs-lookup"><span data-stu-id="3641c-139">**Policy statement**: All connections between the on-premises and cloud networks must take place either through a secure encrypted VPN connection or a dedicated private WAN link.</span></span>

<span data-ttu-id="3641c-140">**Potenzielle Entwurfsoption**: Verwenden Sie in Azure ExpressRoute oder Azure VPN, um private Verbindungen zwischen Ihren lokalen und Cloudnetzwerken herzustellen.</span><span class="sxs-lookup"><span data-stu-id="3641c-140">**Potential design option**: In Azure, use ExpressRoute or Azure VPN to establish private connections between your on-premises and cloud networks.</span></span>

## <a name="network-monitoring-and-enforcement"></a><span data-ttu-id="3641c-141">Netzwerküberwachung und Durchsetzung</span><span class="sxs-lookup"><span data-stu-id="3641c-141">Network monitoring and enforcement</span></span>

<span data-ttu-id="3641c-142">**Technisches Risiko**: Änderungen an der Netzwerkkonfiguration können zu neuen Sicherheits- und Datenoffenlegungs-Risiken führen.</span><span class="sxs-lookup"><span data-stu-id="3641c-142">**Technical risk**: Changes to network configuration can lead to new vulnerabilities and data exposure risks.</span></span>

<span data-ttu-id="3641c-143">**Richtlinienanweisung**: Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsbaselineteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-143">**Policy statement**: Governance tooling must audit and enforce network configuration requirements defined by the Security Baseline team.</span></span>

<span data-ttu-id="3641c-144">**Potenzielle Entwurfsoption**: In Azure kann die Netzwerkaktivität mit [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview) überwacht werden, und [Azure Security Center](/azure/security-center/security-center-network-recommendations) kann Sicherheitsrisiken erkennen.</span><span class="sxs-lookup"><span data-stu-id="3641c-144">**Potential design option**: In Azure, network activity can be monitored using [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview), and [Azure Security Center](/azure/security-center/security-center-network-recommendations) can help identify security vulnerabilities.</span></span> <span data-ttu-id="3641c-145">Mit Azure Policy können Sie die Netzwerkressourcen- und Ressourcenkonfigurationsrichtlinie gemäß den vom Sicherheitsteam definierten Limits einschränken.</span><span class="sxs-lookup"><span data-stu-id="3641c-145">Azure Policy allows you to restrict network resources and resource configuration policy according to limits defined by the security team.</span></span>

## <a name="security-review"></a><span data-ttu-id="3641c-146">Sicherheitsüberprüfung</span><span class="sxs-lookup"><span data-stu-id="3641c-146">Security review</span></span>

<span data-ttu-id="3641c-147">**Technisches Risiko**: Im Laufe der Zeit treten neue Sicherheitsrisiken und Angriffstypen auf und erhöhen das Risiko der Offenlegung oder Unterbrechung Ihrer Cloudressourcen.</span><span class="sxs-lookup"><span data-stu-id="3641c-147">**Technical risk**: Over time, new security threats and attack types emerge, increasing the risk of exposure or disruption of your cloud resources.</span></span>

<span data-ttu-id="3641c-148">**Richtlinienanweisung**: Trends und potenzielle Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsbaselinetools bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="3641c-148">**Policy statement**: Trends and potential exploits that could affect cloud deployments should be reviewed regularly by the security team to provide updates to Security Baseline tooling used in the cloud.</span></span>

<span data-ttu-id="3641c-149">**Potenzielle Entwurfsoption**: Legen Sie ein regelmäßiges Meeting zur Sicherheitsüberprüfung fest, an dem relevante IT- und Governanceteammitglieder teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="3641c-149">**Potential design option**: Establish a regular security review meeting that includes relevant IT and governance team members.</span></span> <span data-ttu-id="3641c-150">Überprüfen Sie vorhandene Sicherheitsdaten und Metriken, um Lücken in den aktuellen Richtlinien- und Sicherheitsbaselinetools zu ermitteln, und aktualisieren Sie die Richtlinie, um alle neuen Risiken zu verringern.</span><span class="sxs-lookup"><span data-stu-id="3641c-150">Review existing security data and metrics to establish gaps in current policy and Security Baseline tooling, and update policy to mitigate any new risks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3641c-151">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="3641c-151">Next steps</span></span>

<span data-ttu-id="3641c-152">Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte Sicherheitsrisiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3641c-152">Use the samples mentioned in this article as a starting point to develop policies that address specific security risks that align with your cloud adoption plans.</span></span>

<span data-ttu-id="3641c-153">Um mit der Entwicklung Ihrer eigenen, benutzerdefinierten Richtlinienanweisungen mit Bezug auf die Sicherheitsbaseline zu beginnen, laden Sie die [Sicherheitsbaselinevorlage](template.md) herunter.</span><span class="sxs-lookup"><span data-stu-id="3641c-153">To begin developing your own custom policy statements related to Security Baseline, download the [Security Baseline template](template.md).</span></span>

<span data-ttu-id="3641c-154">Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie die [umsetzbare Governance Journey](../journeys/overview.md) aus, die Ihrer Umgebung am besten entspricht.</span><span class="sxs-lookup"><span data-stu-id="3641c-154">To accelerate adoption of this discipline, choose the [Actionable Governance Journey](../journeys/overview.md) that most closely aligns with your environment.</span></span> <span data-ttu-id="3641c-155">Ändern Sie dann das Design, um Ihre spezifischen Entscheidungen für Unternehmensrichtlinien zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="3641c-155">Then modify the design to incorporate your specific corporate policy decisions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3641c-156">Umsetzbare Governance Journeys</span><span class="sxs-lookup"><span data-stu-id="3641c-156">Actionable Governance Journeys</span></span>](../journeys/overview.md)