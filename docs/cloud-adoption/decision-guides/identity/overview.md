---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur Entscheidungsfindung für die Identitätsverwaltung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr über die Identitätsverwaltung als wichtigen Aspekt bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: addc11928a0bc72917a28b468a04880720a1fdf4
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901640"
---
# <a name="identity-decision-guide"></a><span data-ttu-id="ad788-103">Leitfaden zur Entscheidungsfindung für die Identitätsverwaltung</span><span class="sxs-lookup"><span data-stu-id="ad788-103">Identity decision guide</span></span>

<span data-ttu-id="ad788-104">In allen Umgebungen, seien es lokale, Hybrid- oder reine Cloudumgebungen, muss die IT-Abteilung genau bestimmen, welche Administratoren, Benutzer und Gruppen Zugriff auf Ressourcen haben.</span><span class="sxs-lookup"><span data-stu-id="ad788-104">In any environment, whether on-premises, hybrid, or cloud-only, IT needs to control which administrators, users, and groups have access to resources.</span></span> <span data-ttu-id="ad788-105">Dienste für die Identitäts- und Zugriffsverwaltung (Identity and Access Management, IAM) ermöglichen Ihnen das Verwalten der Zugriffssteuerung in der Cloud.</span><span class="sxs-lookup"><span data-stu-id="ad788-105">Identity and access management (IAM) services enable you to manage access control in the cloud.</span></span>

![Abbildung der Identitätsoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-identity.png)

<span data-ttu-id="ad788-107">Wechseln Sie zu: [Ermitteln der Anforderung an die Identitätsintegration](#determine-identity-integration-requirements) | [Cloudnativ](#cloud-baseline) | [Verzeichnissynchronisierung](#directory-synchronization) | [In der Cloud gehostete Domänendienste](#cloud-hosted-domain-services) | [Active Directory-Verbunddienste (AD FS)](#active-directory-federation-services) | [Sich weiterentwickelnde Identitätsintegration](#evolving-identity-integration) | [Weitere Informationen](#learn-more)</span><span class="sxs-lookup"><span data-stu-id="ad788-107">Jump to: [Determine Identity Integration Requirements](#determine-identity-integration-requirements) | [Cloud native](#cloud-baseline) | [Directory Synchronization](#directory-synchronization) | [Cloud hosted domain services](#cloud-hosted-domain-services) | [Active Directory Federation Services](#active-directory-federation-services) | [Evolving identity integration](#evolving-identity-integration) | [Learn more](#learn-more)</span></span>

<span data-ttu-id="ad788-108">Es gibt in einer Cloudumgebung mehrere Möglichkeiten der Identitätsverwaltung, die sich hinsichtlich Kosten und Komplexität unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="ad788-108">There are several ways to manage identity in a cloud environment, which vary in cost and complexity.</span></span> <span data-ttu-id="ad788-109">Ein wesentlicher Faktor bei der Strukturierung Ihrer cloudbasierten Identitätsverwaltungsdienste ist der Grad der Integration in Ihre bestehende lokale Identitätsverwaltungsinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="ad788-109">A key factor in structuring your cloud-based identity services is the level of integration required with your existing on-premises identity infrastructure.</span></span>

<span data-ttu-id="ad788-110">Cloudbasierte SaaS-Identitätslösungen (Software as a Service) bieten für Cloudressourcen eine grundlegende Ebene der Zugriffssteuerung und Identitätsverwaltung.</span><span class="sxs-lookup"><span data-stu-id="ad788-110">Cloud-based software as a service (SaaS) identity solutions provide a base level of access control and identity management for cloud resources.</span></span> <span data-ttu-id="ad788-111">Wenn die Active Directory-Infrastruktur (AD) Ihrer Organisation jedoch eine komplexe Gesamtstruktur oder angepasste Organisationseinheiten aufweist, können Ihre cloudbasierten Workloads eine Verzeichnisreplikation in die Cloud erfordern, um einen einheitlichen Satz von Identitäten, Gruppen und Rollen zwischen der lokalen und der Cloudumgebung zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="ad788-111">However, if your organization's Active Directory (AD) infrastructure has a complex forest structure or customized organizational units (OUs), your cloud-based workloads may require directory replication to the cloud for a consistent set of identities, groups, and roles between your on-premises and cloud environments.</span></span> <span data-ttu-id="ad788-112">Falls für eine globale Lösung eine Verzeichnisreplikation erforderlich ist, kann die Komplexität deutlich zunehmen.</span><span class="sxs-lookup"><span data-stu-id="ad788-112">If directory replication is required for a global solution, complexity can increase significantly.</span></span> <span data-ttu-id="ad788-113">Darüber hinaus kann die Unterstützung von Anwendungen, die von veralteten Authentifizierungsmechanismen abhängen, die Bereitstellung von Domänendiensten in der Cloud erfordern.</span><span class="sxs-lookup"><span data-stu-id="ad788-113">Additionally, support for applications dependent on legacy authentication mechanisms may require the deployment of domain services in the cloud.</span></span>

## <a name="determine-identity-integration-requirements"></a><span data-ttu-id="ad788-114">Bestimmen der Anforderungen an die Identitätsintegration</span><span class="sxs-lookup"><span data-stu-id="ad788-114">Determine identity integration requirements</span></span>

| <span data-ttu-id="ad788-115">Frage</span><span class="sxs-lookup"><span data-stu-id="ad788-115">Question</span></span> | <span data-ttu-id="ad788-116">Reine Cloudlösung</span><span class="sxs-lookup"><span data-stu-id="ad788-116">Cloud baseline</span></span> | <span data-ttu-id="ad788-117">Verzeichnissynchronisierung</span><span class="sxs-lookup"><span data-stu-id="ad788-117">Directory synchronization</span></span> | <span data-ttu-id="ad788-118">In der Cloud gehostete Domänendienste</span><span class="sxs-lookup"><span data-stu-id="ad788-118">Cloud-hosted Domain Services</span></span> | <span data-ttu-id="ad788-119">Active Directory-Verbunddienste (AD FS)</span><span class="sxs-lookup"><span data-stu-id="ad788-119">AD Federation Services</span></span> |
|------|------|------|------|------|
| <span data-ttu-id="ad788-120">Fehlt Ihnen derzeit ein lokaler Verzeichnisdienst?</span><span class="sxs-lookup"><span data-stu-id="ad788-120">Do you currently lack an on-premises directory service?</span></span> | <span data-ttu-id="ad788-121">Ja</span><span class="sxs-lookup"><span data-stu-id="ad788-121">Yes</span></span> | <span data-ttu-id="ad788-122">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-122">No</span></span> | <span data-ttu-id="ad788-123">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-123">No</span></span> | <span data-ttu-id="ad788-124">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-124">No</span></span> |
| <span data-ttu-id="ad788-125">Müssen sich Ihre Workloads bei lokalen Identitätsdiensten authentifizieren?</span><span class="sxs-lookup"><span data-stu-id="ad788-125">Do your workloads need to authenticate against on-premises identity services?</span></span> | <span data-ttu-id="ad788-126">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-126">No</span></span> | <span data-ttu-id="ad788-127">Ja</span><span class="sxs-lookup"><span data-stu-id="ad788-127">Yes</span></span> | <span data-ttu-id="ad788-128">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-128">No</span></span> | <span data-ttu-id="ad788-129">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-129">No</span></span> |
| <span data-ttu-id="ad788-130">Sind Ihre Workloads von veralteten Authentifizierungsmechanismen wie Kerberos oder NTLM abhängig?</span><span class="sxs-lookup"><span data-stu-id="ad788-130">Do your workloads depend on legacy authentication mechanisms, such as Kerberos or NTLM?</span></span> | <span data-ttu-id="ad788-131">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-131">No</span></span> | <span data-ttu-id="ad788-132">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-132">No</span></span> | <span data-ttu-id="ad788-133">Ja</span><span class="sxs-lookup"><span data-stu-id="ad788-133">Yes</span></span> | <span data-ttu-id="ad788-134">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-134">No</span></span> |
| <span data-ttu-id="ad788-135">Ist eine Integration zwischen Cloud- und lokalen Identitätsdiensten unmöglich?</span><span class="sxs-lookup"><span data-stu-id="ad788-135">Is integration between cloud and on-premises identity services impossible?</span></span> | <span data-ttu-id="ad788-136">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-136">No</span></span> | <span data-ttu-id="ad788-137">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-137">No</span></span> | <span data-ttu-id="ad788-138">Ja</span><span class="sxs-lookup"><span data-stu-id="ad788-138">Yes</span></span> | <span data-ttu-id="ad788-139">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-139">No</span></span> |
| <span data-ttu-id="ad788-140">Wird einmaliges Anmelden über mehrere Identitätsanbieter benötigt?</span><span class="sxs-lookup"><span data-stu-id="ad788-140">Do you require single sign-on across multiple identity providers?</span></span> | <span data-ttu-id="ad788-141">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-141">No</span></span> | <span data-ttu-id="ad788-142">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-142">No</span></span> | <span data-ttu-id="ad788-143">Nein </span><span class="sxs-lookup"><span data-stu-id="ad788-143">No</span></span> | <span data-ttu-id="ad788-144">Ja</span><span class="sxs-lookup"><span data-stu-id="ad788-144">Yes</span></span> |

<span data-ttu-id="ad788-145">Im Rahmen der Planung Ihrer Migration zu Azure müssen Sie entscheiden, wie Sie Ihre bestehenden Identitätsverwaltungs- und Cloudidentitätsdienste am besten integrieren.</span><span class="sxs-lookup"><span data-stu-id="ad788-145">As part of planning your migration to Azure, you will need to determine how best to integrate your existing identity management and cloud identity services.</span></span> <span data-ttu-id="ad788-146">Es folgen gängige Integrationsszenarien.</span><span class="sxs-lookup"><span data-stu-id="ad788-146">The following are common integration scenarios.</span></span>

### <a name="cloud-baseline"></a><span data-ttu-id="ad788-147">Reine Cloudlösung</span><span class="sxs-lookup"><span data-stu-id="ad788-147">Cloud baseline</span></span>

<span data-ttu-id="ad788-148">Öffentliche Cloudplattformen bieten ein natives System zur Identitäts- und Zugriffsverwaltung (IAM), über das Benutzer und Gruppen Zugriff auf Verwaltungsfunktionen erhalten.</span><span class="sxs-lookup"><span data-stu-id="ad788-148">Public cloud platforms provide a native IAM system for granting users and groups access to management features.</span></span> <span data-ttu-id="ad788-149">Wenn es in Ihrem Unternehmen keine nennenswerte lokale Identitätsverwaltungslösung gibt und Sie planen, Workloads so zu migrieren, dass sie mit cloudbasierten Authentifizierungsmechanismen kompatibel sind, sollten Sie Ihre Identitätsinfrastruktur mit einem cloudnativen Identitätsdienst erstellen.</span><span class="sxs-lookup"><span data-stu-id="ad788-149">If your organization lacks a significant on-premises identity solution, and you plan on migrating workloads to be compatible with cloud-based authentication mechanisms, you should build your identity infrastructure using a cloud-native identity service.</span></span>

<span data-ttu-id="ad788-150">**Annahmen für eine reine Cloudlösung**.</span><span class="sxs-lookup"><span data-stu-id="ad788-150">**Cloud baseline assumptions**.</span></span> <span data-ttu-id="ad788-151">Für den Einsatz einer rein cloudbasierten Identitätsinfrastruktur wird Folgendes angenommen:</span><span class="sxs-lookup"><span data-stu-id="ad788-151">Using a purely cloud-native identity infrastructure assumes the following:</span></span>

- <span data-ttu-id="ad788-152">Ihre cloudbasierten Ressourcen weisen keine Abhängigkeiten von lokalen Verzeichnisdiensten oder Active Directory-Servern auf, oder Workloads können so geändert werden, dass diese Abhängigkeiten entfallen.</span><span class="sxs-lookup"><span data-stu-id="ad788-152">Your cloud-based resources will not have dependencies on on-premises directory services or Active Directory servers, or workloads can be modified to remove those dependencies your.</span></span>
- <span data-ttu-id="ad788-153">Die zu migrierenden Anwendungs- oder Dienstworkloads unterstützen entweder Authentifizierungsmechanismen, die mit Cloudidentitätsanbietern kompatibel sind, oder können problemlos so geändert werden, dass diese unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="ad788-153">The application or service workloads being migrated either support authentication mechanisms compatible with cloud identity providers or can be modified easily to support them.</span></span> <span data-ttu-id="ad788-154">Cloudnative Identitätsanbieter greifen auf internetfähige Authentifizierungsmechanismen wie SAML, OAuth und OpenID Connect zurück.</span><span class="sxs-lookup"><span data-stu-id="ad788-154">Cloud native identity providers rely on internet-ready authentication mechanisms such as SAML, OAuth, and OpenID Connect.</span></span> <span data-ttu-id="ad788-155">Bestehende Workloads, die von veralteten Authentifizierungsmethoden mit Protokollen wie Kerberos oder NTLM abhängen, müssen möglicherweise vor der Migration in die Cloud angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="ad788-155">Existing workloads that depend on legacy authentication methods using protocols such as Kerberos or NTLM may need to be refactored before migrating to the cloud.</span></span>

> [!TIP]
> <span data-ttu-id="ad788-156">Die meisten cloudnativen Identitätsdienste sind kein vollständiger Ersatz für herkömmliche lokale Verzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="ad788-156">Most cloud-native identity services are not full replacements for traditional on-premises directories.</span></span> <span data-ttu-id="ad788-157">Verzeichnisfunktionen wie Computerverwaltung oder Gruppenrichtlinien sind möglicherweise nicht ohne zusätzliche Tools oder Dienste verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ad788-157">Directory features such as computer management or group policy may not be available without using additional tools or services.</span></span>

<span data-ttu-id="ad788-158">Durch die vollständige Migration Ihrer Identitätsdienste zu einem cloudbasierten Anbieter entfällt die Notwendigkeit, Ihre eigene Identitätsinfrastruktur zu pflegen, wodurch Ihre IT-Verwaltung erheblich vereinfacht wird.</span><span class="sxs-lookup"><span data-stu-id="ad788-158">Completely migrating your identity services to a cloud-based provider eliminates the need to maintain your own identity infrastructure, significantly simplifying your IT management.</span></span>

### <a name="directory-synchronization"></a><span data-ttu-id="ad788-159">Verzeichnissynchronisierung</span><span class="sxs-lookup"><span data-stu-id="ad788-159">Directory synchronization</span></span>

<span data-ttu-id="ad788-160">Für Organisationen mit einer bestehenden Identitätsinfrastruktur ist die Verzeichnissynchronisierung oft die beste Lösung, um die bestehende Benutzer- und Zugriffsverwaltung aufrechtzuerhalten und gleichzeitig die erforderlichen IAM-Funktionen für die Verwaltung von Cloudressourcen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ad788-160">For organizations with an existing identity infrastructure, directory synchronization is often the best solution for preserving existing user and access management while providing the required IAM capabilities for managing cloud resources.</span></span> <span data-ttu-id="ad788-161">Bei diesem Prozess werden Verzeichnisinformationen kontinuierlich zwischen der Cloud- und der lokalen Umgebung repliziert, wodurch einmaliges Anmelden (SSO) für Benutzer und ein einheitliches Identitäts-, Rollen- und Berechtigungssystem in der gesamten Organisation ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="ad788-161">This process continuously replicates directory information between the cloud and on-premises environments, allowing single sign-on (SSO) for users and a consistent identity, role, and permission system across your entire organization.</span></span>

<span data-ttu-id="ad788-162">Hinweis: Organisationen, die Office 365 eingeführt haben, haben möglicherweise bereits die [Verzeichnissynchronisierung](/office365/enterprise/set-up-directory-synchronization) zwischen ihrer lokalen Active Directory-Infrastruktur und Azure Active Directory eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="ad788-162">Note: Organizations that have adopted Office 365 may have already implemented [directory synchronization](/office365/enterprise/set-up-directory-synchronization) between their on-premises Active Directory infrastructure and Azure Active Directory.</span></span>

<span data-ttu-id="ad788-163">**Annahmen zur Verzeichnissynchronisierung**.</span><span class="sxs-lookup"><span data-stu-id="ad788-163">**Directory synchronization assumptions**.</span></span> <span data-ttu-id="ad788-164">Für den Einsatz einer synchronisierten Identitätslösung wird Folgendes angenommen:</span><span class="sxs-lookup"><span data-stu-id="ad788-164">Using a synchronized identity solution assumes the following:</span></span>

- <span data-ttu-id="ad788-165">Sie benötigen in Ihrer Cloud- und lokalen IT-Infrastruktur einen gemeinsamen Satz von Benutzerkonten und -gruppen.</span><span class="sxs-lookup"><span data-stu-id="ad788-165">You need to maintain a common set of user accounts and groups across your cloud and on-premises IT infrastructure.</span></span>
- <span data-ttu-id="ad788-166">Ihre lokalen Identitätsdienste unterstützen die Replikation mit Ihrem Cloudidentitätsanbieter.</span><span class="sxs-lookup"><span data-stu-id="ad788-166">Your on-premises identity services support replication with your cloud identity provider.</span></span>
- <span data-ttu-id="ad788-167">Sie benötigen Mechanismen für einmaliges Anmelden für Benutzer, die auf Cloud- und lokale Identitätsanbieter zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ad788-167">You require SSO mechanisms for users accessing cloud and on-premises identity providers.</span></span>

> [!TIP]
> <span data-ttu-id="ad788-168">Alle cloudbasierten Workloads, die von veralteten Authentifizierungsmechanismen abhängen, welche nicht von cloudbasierten Identitätsdiensten wie Azure AD unterstützt werden, erfordern weiterhin entweder eine Verbindung mit lokalen Domänendiensten oder virtuellen Servern in der Cloudumgebung, die diese Dienste bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ad788-168">Any cloud-based workloads that depend on legacy authentication mechanisms that are not supported by cloud-based identity services like Azure AD will still require either connectivity to on-premises domain services or virtual servers in the cloud environment providing these services.</span></span> <span data-ttu-id="ad788-169">Bei Nutzung lokaler Identitätsdienste entstehen auch Abhängigkeiten von der Konnektivität zwischen der Cloud und lokalen Netzwerken.</span><span class="sxs-lookup"><span data-stu-id="ad788-169">Using on-premises identity services also introduces dependencies on connectivity between the cloud and on-premises networks.</span></span>

### <a name="cloud-hosted-domain-services"></a><span data-ttu-id="ad788-170">In der Cloud gehostete Domänendienste</span><span class="sxs-lookup"><span data-stu-id="ad788-170">Cloud-hosted domain services</span></span>

<span data-ttu-id="ad788-171">Angenommen, Sie haben Workloads, die von einer anspruchsbasierten Authentifizierung mit älteren Protokollen wie Kerberos oder NTLM abhängen, und diese Workloads nicht so angepasst werden können, dass sie moderne Authentifizierungsprotokolle wie SAML oder OAuth und OpenID Connect unterstützen. Dann müssen Sie möglicherweise einige Ihrer Domänendienste als Teil Ihrer Cloudbereitstellung in die Cloud migrieren.</span><span class="sxs-lookup"><span data-stu-id="ad788-171">If you have workloads that depend on claims-based authentication using legacy protocols such as Kerberos or NTLM, and those workloads cannot be refactored to accept modern authentication protocols such as SAML or OAuth and OpenID Connect, you may need to migrate some of your domain services to the cloud as part of your cloud deployment.</span></span>

<span data-ttu-id="ad788-172">Diese Art der Bereitstellung erfordert die Bereitstellung virtueller Computer mit ausgeführtem Active Directory in Ihren cloudbasierten virtuellen Netzwerken, um Domänendienste für Ressourcen in der Cloud bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ad788-172">This type of deployment involves deploying virtual machines running Active Directory in your cloud-based virtual networks to provide domain services for resources in the cloud.</span></span> <span data-ttu-id="ad788-173">Alle bestehenden Anwendungen und Dienste, die in Ihr Cloudnetzwerk migrieren, sollten in der Lage sein, nach geringfügigen Änderungen diese von der Cloud gehosteten Verzeichnisserver zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="ad788-173">Any existing applications and services migrating to your cloud network should be able to use of these cloud-hosted directory servers with minor modifications.</span></span>

<span data-ttu-id="ad788-174">Es ist wahrscheinlich, dass Ihre bestehenden Verzeichnisse und Domänendienste weiterhin in Ihrer lokalen Umgebung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ad788-174">It's likely that your existing directories and domain services will continue to be used in your on-premises environment.</span></span> <span data-ttu-id="ad788-175">In diesem Szenario wird empfohlen, dass Sie auch die Verzeichnissynchronisierung verwenden, um einen gemeinsamen Satz von Benutzern und Rollen sowohl in der Cloud- als auch in der lokalen Umgebung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ad788-175">In this scenario, it's recommended that you also use directory synchronization to provide a common set of users and roles in both the cloud and on-premises environments.</span></span>

<span data-ttu-id="ad788-176">**Annahmen für in der Cloud gehostete Domänendienste**.</span><span class="sxs-lookup"><span data-stu-id="ad788-176">**Cloud hosted domain services assumptions**.</span></span> <span data-ttu-id="ad788-177">Für eine Verzeichnismigration wird Folgendes angenommen:</span><span class="sxs-lookup"><span data-stu-id="ad788-177">Performing a directory migration assumes the following:</span></span>

- <span data-ttu-id="ad788-178">Ihre Workloads hängen von der anspruchsbasierten Authentifizierung mit Protokollen wie Kerberos oder NTLM ab.</span><span class="sxs-lookup"><span data-stu-id="ad788-178">Your workloads depend on claims-based authentication using protocols like Kerberos or NTLM.</span></span>
- <span data-ttu-id="ad788-179">Die virtuellen Computer Ihrer Workloads müssen für die Verwaltung oder Anwendung von Active Directory-Gruppenrichtlinien der Domäne beitreten.</span><span class="sxs-lookup"><span data-stu-id="ad788-179">Your workload virtual machines need to be domain-joined for management or application of Active Directory group policy purposes.</span></span>

> [!TIP]
> <span data-ttu-id="ad788-180">Während eine Verzeichnismigration in Verbindung mit in der Cloud gehosteten Domänendiensten eine große Flexibilität bei der Migration bestehender Workloads bietet, erhöht das Hosting virtueller Computer in Ihrem virtuellen Cloudnetzwerk zur Bereitstellung dieser Dienste die Komplexität Ihrer IT-Verwaltungsaufgaben.</span><span class="sxs-lookup"><span data-stu-id="ad788-180">While a directory migration coupled with cloud-hosted domain services provides great flexibility when migrating existing workloads, hosting virtual machines within your cloud virtual network to provide these services does increase the complexity of your IT management tasks.</span></span> <span data-ttu-id="ad788-181">Mit zunehmender Erfahrung mit der Migration in die Cloud sollten Sie den langfristigen Wartungsbedarf für das Hosting dieser Server untersuchen.</span><span class="sxs-lookup"><span data-stu-id="ad788-181">As your cloud migration experience matures, examine the long-term maintenance requirements of hosting these servers.</span></span> <span data-ttu-id="ad788-182">Prüfen Sie, ob die Anpassung bestehender Workloads aus Gründen der Kompatibilität mit Cloudidentitätsanbietern wie Azure Active Directory den Bedarf an diesen in der Cloud gehosteten Servern verringern kann.</span><span class="sxs-lookup"><span data-stu-id="ad788-182">Consider whether refactoring existing workloads for compatibility with cloud identity providers such as Azure Active Directory can reduce the need for these cloud-hosted servers.</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="ad788-183">Active Directory-Verbunddienste (AD FS)</span><span class="sxs-lookup"><span data-stu-id="ad788-183">Active Directory Federation Services</span></span>

<span data-ttu-id="ad788-184">Über den Identitätsverbund werden Vertrauensbeziehungen zwischen mehreren Identitätsverwaltungssystemen aufgebaut, um gemeinsame Authentifizierungs- und Autorisierungsfunktionen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="ad788-184">Identity federation establishes trust relationships across multiple identity management systems to allow common authentication and authorization capabilities.</span></span> <span data-ttu-id="ad788-185">Sie können dann Funktionen für einmaliges Anmelden in mehreren Domänen innerhalb Ihrer Organisation oder Identitätssysteme unterstützen, die von Ihren Kunden oder Geschäftspartnern verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="ad788-185">You can then support single sign-on capabilities across multiple domains within your organization or identity systems managed by your customers or business partners.</span></span>

<span data-ttu-id="ad788-186">Azure AD unterstützt den Verbund lokaler Active Directory-Domänen mithilfe von [Active Directory-Verbunddienste (AD FS)](/azure/active-directory/hybrid/how-to-connect-fed-whatis).</span><span class="sxs-lookup"><span data-stu-id="ad788-186">Azure AD supports federation of on-premises Active Directory domains using [Active Directory Federation Services](/azure/active-directory/hybrid/how-to-connect-fed-whatis) (AD FS).</span></span> <span data-ttu-id="ad788-187">In der Referenzarchitektur [Erweiterung von AD FS auf Azure](../../../reference-architectures/identity/adfs.md) sehen Sie, wie dies in Azure erfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="ad788-187">See the reference architecture [Extend AD FS to Azure](../../../reference-architectures/identity/adfs.md) to see how this can be implemented in Azure.</span></span>

## <a name="evolving-identity-integration"></a><span data-ttu-id="ad788-188">Weiterentwicklung der Identitätsintegration</span><span class="sxs-lookup"><span data-stu-id="ad788-188">Evolving identity integration</span></span>

<span data-ttu-id="ad788-189">Die Identitätsintegration ist ein iterativer Vorgang.</span><span class="sxs-lookup"><span data-stu-id="ad788-189">Identity integration is an iterative process.</span></span> <span data-ttu-id="ad788-190">Möglicherweise möchten Sie für eine erste Bereitstellung mit einer cloudnativen Lösung mit einer kleinen Anzahl von Benutzern und entsprechenden Rollen beginnen.</span><span class="sxs-lookup"><span data-stu-id="ad788-190">You may want to start with a cloud native solution with a small set of users and corresponding roles for an initial deployment.</span></span> <span data-ttu-id="ad788-191">Wenn Sie mehr Erfahrung mit der Migration haben, erwägen Sie den Einsatz eines Verbundmodells oder eine vollständige Verzeichnismigration Ihrer lokalen Identitätsdienste in die Cloud.</span><span class="sxs-lookup"><span data-stu-id="ad788-191">As your migration matures, consider adopting a federated model or performing a full directory migration of your on-premises identity services to the cloud.</span></span> <span data-ttu-id="ad788-192">Überprüfen Sie Ihre Identitätsstrategie bei jeder Iteration Ihres Migrationsprozesses.</span><span class="sxs-lookup"><span data-stu-id="ad788-192">Revisit your identity strategy in every iteration of your migration process.</span></span>

## <a name="learn-more"></a><span data-ttu-id="ad788-193">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="ad788-193">Learn more</span></span>

<span data-ttu-id="ad788-194">Nachstehend finden Sie weitere Informationen zu Identitätsdiensten auf der Azure-Plattform.</span><span class="sxs-lookup"><span data-stu-id="ad788-194">See the following for more information about identity services on the Azure platform.</span></span>

- <span data-ttu-id="ad788-195">[Azure AD](https://azure.microsoft.com/services/active-directory).</span><span class="sxs-lookup"><span data-stu-id="ad788-195">[Azure AD](https://azure.microsoft.com/services/active-directory).</span></span> <span data-ttu-id="ad788-196">Azure AD bietet cloudbasierte Identitätsdienste.</span><span class="sxs-lookup"><span data-stu-id="ad788-196">Azure AD provides cloud-based identity services.</span></span> <span data-ttu-id="ad788-197">Es ermöglicht Ihnen die Verwaltung des Zugriffs auf Ihre Azure-Ressourcen und das Steuern der Identitätsverwaltung, Geräteregistrierung, Benutzerbereitstellung, Anwendungszugriffssteuerung und Datensicherung.</span><span class="sxs-lookup"><span data-stu-id="ad788-197">It allows you to manage access to your Azure resources and control identity management, device registration, user provisioning, application access control, and data protection.</span></span>
- <span data-ttu-id="ad788-198">[Azure AD Connect](/azure/active-directory/hybrid/whatis-hybrid-identity):</span><span class="sxs-lookup"><span data-stu-id="ad788-198">[Azure AD Connect](/azure/active-directory/hybrid/whatis-hybrid-identity).</span></span> <span data-ttu-id="ad788-199">Mit dem Tool Azure AD Connect können Sie Azure AD-Instanzen mit Ihren bestehenden Identitätsverwaltungslösungen verbinden und so Ihr bestehendes Verzeichnis in der Cloud synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="ad788-199">The Azure AD Connect tool allows you to connect Azure AD instances with your existing identity management solutions, allowing synchronization of your existing directory in the cloud.</span></span>
- <span data-ttu-id="ad788-200">[Rollenbasierte Zugriffssteuerung](/azure/role-based-access-control/overview) (RBAC).</span><span class="sxs-lookup"><span data-stu-id="ad788-200">[Role-based access control](/azure/role-based-access-control/overview) (RBAC).</span></span> <span data-ttu-id="ad788-201">Azure AD bietet RBAC für die effiziente und sichere Verwaltung des Zugriffs auf Ressourcen auf Verwaltungsebene.</span><span class="sxs-lookup"><span data-stu-id="ad788-201">Azure AD provides RBAC to efficiently and securely manage access to resources in the management plane.</span></span> <span data-ttu-id="ad788-202">Aufträge und Aufgaben sind in Rollen organisiert und Benutzer diesen Rollen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="ad788-202">Jobs and responsibilities are organized into roles, and users are assigned to these roles.</span></span> <span data-ttu-id="ad788-203">Mit RBAC können Sie steuern, wer Zugriff auf eine Ressource hat und welche Aktionen ein Benutzer auf diese Ressource anwenden kann.</span><span class="sxs-lookup"><span data-stu-id="ad788-203">RBAC allows you to control who has access to a resource along with which actions a user can perform on that resource.</span></span>
- <span data-ttu-id="ad788-204">[Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure) (PIM).</span><span class="sxs-lookup"><span data-stu-id="ad788-204">[Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure) (PIM).</span></span> <span data-ttu-id="ad788-205">PIM reduziert die Offenlegungszeit von Ressourcenzugriffsrechten und verschafft Ihnen durch Berichte und Warnungen einen besseren Überblick über deren Nutzung.</span><span class="sxs-lookup"><span data-stu-id="ad788-205">PIM lowers the exposure time of resource access privileges and increases your visibility into their use through reports and alerts.</span></span> <span data-ttu-id="ad788-206">Hierfür werden Benutzern Beschränkungen auferlegt, indem ihnen Berechtigungen nur gemäß des Just-in-Time-Prinzips (JIT) zugeteilt oder nur für eine kurze Dauer zugewiesen und danach automatisch widerrufen werden.</span><span class="sxs-lookup"><span data-stu-id="ad788-206">It limits users to taking on their privileges "just in time" (JIT), or by assigning privileges for a shorter duration, after which privileges are revoked automatically.</span></span>
- <span data-ttu-id="ad788-207">[Integrieren lokaler Active Directory-Domänen in Azure Active Directory](../../../reference-architectures/identity/azure-ad.md).</span><span class="sxs-lookup"><span data-stu-id="ad788-207">[Integrate on-premises Active Directory domains with Azure Active Directory](../../../reference-architectures/identity/azure-ad.md).</span></span> <span data-ttu-id="ad788-208">Diese Referenzarchitektur bietet ein Beispiel für die Verzeichnissynchronisierung zwischen lokalen Active Directory-Domänen und Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad788-208">This reference architecture provides an example of directory synchronization between on-premises Active Directory domains and Azure AD.</span></span>
- <span data-ttu-id="ad788-209">[Erweitern von Active Directory Domain Services (AD DS) auf Azure](../../../reference-architectures/identity/adds-extend-domain.md).</span><span class="sxs-lookup"><span data-stu-id="ad788-209">[Extend Active Directory Domain Services (AD DS) to Azure.](../../../reference-architectures/identity/adds-extend-domain.md)</span></span> <span data-ttu-id="ad788-210">Diese Referenzarchitektur bietet ein Beispiel für den Einsatz von AD DS-Servern zur Erweiterung von Domänendiensten auf cloudbasierte Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="ad788-210">This reference architecture provides an example of deploying AD DS servers to extend domain services to cloud-based resources.</span></span>
- <span data-ttu-id="ad788-211">[Erweitern von Active Directory Federation Services (AD FS) auf Azure](../../../reference-architectures/identity/adfs.md).</span><span class="sxs-lookup"><span data-stu-id="ad788-211">[Extend Active Directory Federation Services (AD FS) to Azure](../../../reference-architectures/identity/adfs.md).</span></span> <span data-ttu-id="ad788-212">In dieser Referenzarchitektur wird Active Directory-Verbunddienste (AD FS) konfiguriert, um Authentifizierung und Autorisierung im Verbund mit Ihrem Azure AD-Verzeichnis durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="ad788-212">This reference architecture configures Active Directory Federation Services (AD FS) to perform federated authentication and authorization with your Azure AD directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad788-213">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ad788-213">Next steps</span></span>

<span data-ttu-id="ad788-214">Erfahren Sie, wie Sie die Erzwingung von Richtlinien in der Cloud realisieren.</span><span class="sxs-lookup"><span data-stu-id="ad788-214">Learn how to implement policy enforcement in the cloud.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ad788-215">Richtlinienerzwingung</span><span class="sxs-lookup"><span data-stu-id="ad788-215">Policy enforcement</span></span>](../policy-enforcement/overview.md)