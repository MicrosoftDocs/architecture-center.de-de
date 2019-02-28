---
title: 'CAF: Governance-Entwurf für eine einfache Workload'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Leitfaden zur Konfiguration von Kontrollen in Bezug auf Azure-Governance, um für Benutzer die Bereitstellung einer einfachen Workload zu ermöglichen.
author: petertaylor9999
ms.date: 2/11/2019
ms.openlocfilehash: ce090562bf256a34078cfe1a9e9f678dbdf10ffe
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900796"
---
# <a name="governance-design-for-a-simple-workload"></a><span data-ttu-id="bd684-103">Governance-Entwurf für eine einfache Workload</span><span class="sxs-lookup"><span data-stu-id="bd684-103">Governance design for a simple workload</span></span>

<span data-ttu-id="bd684-104">Anhand dieser Anleitung können Sie sich mit dem Prozess für das Entwerfen eines Modells zur Ressourcenkontrolle (Governance) in Azure vertraut machen, um ein einzelnes Team und eine einzelne Workload zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="bd684-104">The goal of this guidance is to help you learn the process for designing a resource governance model in Azure to support a single team and a simple workload.</span></span> <span data-ttu-id="bd684-105">Sie lernen eine Reihe von hypothetischen Governanceanforderungen kennen und gehen dann mehrere Beispielimplementierungen durch, die diese Anforderungen erfüllen.</span><span class="sxs-lookup"><span data-stu-id="bd684-105">You'll look at a set of hypothetical governance requirements, then go through several example implementations that satisfy those requirements.</span></span>

<span data-ttu-id="bd684-106">In der grundlegenden Einführungsphase besteht das Ziel darin, eine einfache Workload in Azure bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="bd684-106">In the foundational adoption stage, our goal is to deploy a simple workload to Azure.</span></span> <span data-ttu-id="bd684-107">Dies führt zu den folgenden Anforderungen:</span><span class="sxs-lookup"><span data-stu-id="bd684-107">This results in the following requirements:</span></span>

* <span data-ttu-id="bd684-108">Identitätsverwaltung für einen einzelnen **Workloadbesitzer**, der für das Bereitstellen und Warten der einfachen Workload verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="bd684-108">Identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="bd684-109">Der Workloadbesitzer benötigt die Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Ressourcen sowie die Berechtigung zum Delegieren dieser Rechte an andere Benutzer im Identitätsverwaltungssystem.</span><span class="sxs-lookup"><span data-stu-id="bd684-109">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>
* <span data-ttu-id="bd684-110">Verwaltung aller Ressourcen für die einfache Workload zusammen in einer Verwaltungseinheit.</span><span class="sxs-lookup"><span data-stu-id="bd684-110">Manage all resources for the simple workload as a single management unit.</span></span>

## <a name="licensing-azure"></a><span data-ttu-id="bd684-111">Lizenzieren von Azure</span><span class="sxs-lookup"><span data-stu-id="bd684-111">Licensing Azure</span></span>

<span data-ttu-id="bd684-112">Bevor Sie damit beginnen, das Governancemodell zu entwerfen, ist es wichtig, dass Sie sich mit der Azure-Lizenzierung vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="bd684-112">Before you begin designing our governance model, it's important to understand how Azure is licensed.</span></span> <span data-ttu-id="bd684-113">Der Grund ist, dass die Administratorkonten, die Ihrer Azure-Lizenz zugeordnet sind, über die höchste Zugriffsebene auf Ihre gesamten Azure-Ressourcen verfügen.</span><span class="sxs-lookup"><span data-stu-id="bd684-113">This is because the administrative accounts associated with your Azure license have the highest level of access to all of your Azure resources.</span></span> <span data-ttu-id="bd684-114">Diese Administratorkonten bilden die Grundlage Ihres Governance-Modells.</span><span class="sxs-lookup"><span data-stu-id="bd684-114">These administrative accounts form the basis of your governance model.</span></span>  

> [!NOTE]
> <span data-ttu-id="bd684-115">Wenn Ihre Organisation über ein vorhandenes [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) verfügt, in dem Azure nicht enthalten ist, kann Azure hinzugefügt werden, indem Sie vorab eine Zahlungsverpflichtung eingehen.</span><span class="sxs-lookup"><span data-stu-id="bd684-115">If your organization has an existing [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) that does not include Azure, Azure can be added by making an upfront monetary commitment.</span></span> <span data-ttu-id="bd684-116">Weitere Informationen finden Sie unter [Lizenzierung von Azure für das Unternehmen](https://azure.microsoft.com/pricing/enterprise-agreement/).</span><span class="sxs-lookup"><span data-stu-id="bd684-116">See [licensing Azure for the enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) for more information.</span></span>

<span data-ttu-id="bd684-117">Als Azure dem Enterprise Agreement Ihrer Organisation hinzugefügt wurde, wurde Ihre Organisation aufgefordert, ein **Azure-Konto** zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bd684-117">When Azure was added to your organization's Enterprise Agreement, your organization was prompted to create an **Azure account**.</span></span> <span data-ttu-id="bd684-118">Bei der Erstellung des Kontos wurden ein **Azure-Kontobesitzer** und ein Azure Active Directory-Mandant (Azure AD) mit einem Konto vom Typ **Globaler Administrator** erstellt.</span><span class="sxs-lookup"><span data-stu-id="bd684-118">During the account creation process, an **Azure account owner** was created, as well as an Azure Active Directory (Azure AD) tenant with a **global administrator** account.</span></span> <span data-ttu-id="bd684-119">Ein Azure AD-Mandant ist ein logisches Konstrukt, das eine sichere, dedizierte Instanz von Azure AD darstellt.</span><span class="sxs-lookup"><span data-stu-id="bd684-119">An Azure AD tenant is a logical construct that represents a secure, dedicated instance of Azure AD.</span></span>

<span data-ttu-id="bd684-120">![Azure-Konto mit Azure-Konto-Manager und globalem Azure AD-Administrator](../../_images/governance-3-0.png)
*Abbildung 1: Azure-Konto mit einem Azure-Konto-Manager und globalem Azure AD-Administrator*</span><span class="sxs-lookup"><span data-stu-id="bd684-120">![Azure account with Azure Account Manager and Azure AD global administrator](../../_images/governance-3-0.png)
*Figure 1. An Azure account with an Account Manager and Azure AD Global Administrator.*</span></span>

## <a name="identity-management"></a><span data-ttu-id="bd684-121">Identitätsverwaltung</span><span class="sxs-lookup"><span data-stu-id="bd684-121">Identity management</span></span>

<span data-ttu-id="bd684-122">Da für Azure nur [Azure AD](/azure/active-directory) vertrauenswürdig ist, was die Authentifizierung von Benutzern und die Autorisierung des Benutzerzugriffs auf Ressourcen betrifft, ist Azure AD unser Identitätsverwaltungssystem.</span><span class="sxs-lookup"><span data-stu-id="bd684-122">Azure only trusts [Azure AD](/azure/active-directory) to authenticate users and authorize user access to resources, so Azure AD is our identity management system.</span></span> <span data-ttu-id="bd684-123">Der globale Azure AD-Administrator verfügt über Berechtigungen der höchsten Ebene und kann identitätsbezogene Aktionen durchführen, z.B. das Erstellen von Benutzern und das Zuweisen von Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="bd684-123">The Azure AD global administrator has the highest level of permissions and can perform all actions related to identity, including creating users and assigning permissions.</span></span>

<span data-ttu-id="bd684-124">Wir benötigen eine Identitätsverwaltung für einen einzelnen **Workloadbesitzer**, der für das Bereitstellen und Warten der einfachen Workload verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="bd684-124">Our requirement is identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="bd684-125">Der Workloadbesitzer benötigt die Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Ressourcen sowie die Berechtigung zum Delegieren dieser Rechte an andere Benutzer im Identitätsverwaltungssystem.</span><span class="sxs-lookup"><span data-stu-id="bd684-125">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>

<span data-ttu-id="bd684-126">Unser globaler Azure AD-Administrator erstellt das **Workloadbesitzer**-Konto für den **Workloadbesitzer**:</span><span class="sxs-lookup"><span data-stu-id="bd684-126">Our Azure AD global administrator will create the **workload owner** account for the **workload owner**:</span></span>

<span data-ttu-id="bd684-127">![Globaler Azure AD-Administrator erstellt das Workloadbesitzer-Konto](../../_images/governance-1-2.png)
*Abbildung 2: Globaler Azure AD-Administrator erstellt das Workloadbesitzer-Konto.*</span><span class="sxs-lookup"><span data-stu-id="bd684-127">![The Azure AD global administrator creates the workload owner account](../../_images/governance-1-2.png)
*Figure 2. The Azure AD global administrator creates the workload owner user account.*</span></span>

<span data-ttu-id="bd684-128">Sie können die Zugriffsberechtigung für Ressourcen erst zuweisen, nachdem dieser Benutzer einem **Abonnement** hinzugefügt wurde. Dies führen Sie in den nächsten beiden Abschnitten durch.</span><span class="sxs-lookup"><span data-stu-id="bd684-128">You aren't able to assign resource access permission until this user is added to a **subscription**, so you'll do that in the next two sections.</span></span>

## <a name="resource-management-scope"></a><span data-ttu-id="bd684-129">Ressourcenverwaltungsbereich</span><span class="sxs-lookup"><span data-stu-id="bd684-129">Resource management scope</span></span>

<span data-ttu-id="bd684-130">Wenn die Anzahl der von Ihrer Organisation bereitgestellten Ressourcen zunimmt, erhöht sich auch die Komplexität in Bezug auf die Steuerung dieser Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="bd684-130">As the number of resources deployed by your organization grows, the complexity of governing those resources grows as well.</span></span> <span data-ttu-id="bd684-131">Azure implementiert eine logische Containerhierarchie, damit Ihre Organisation die Ressourcen in Gruppen mit unterschiedlicher Granularität verwalten kann. Dies wird auch als **Bereich** (Scope) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="bd684-131">Azure implements a logical container hierarchy to enable your organization to manage your resources in groups at various levels of granularity, also known as **scope**.</span></span>

<span data-ttu-id="bd684-132">Die oberste Ebene des Ressourcenverwaltungsbereichs ist die Ebene **Abonnement**.</span><span class="sxs-lookup"><span data-stu-id="bd684-132">The top level of resource management scope is the **subscription** level.</span></span> <span data-ttu-id="bd684-133">Ein Abonnement wird vom Azure-**Kontobesitzer** erstellt, der die Zahlungsverpflichtung einrichtet und für die Bezahlung aller Azure-Ressourcen verantwortlich ist, die dem Abonnement zugeordnet sind:</span><span class="sxs-lookup"><span data-stu-id="bd684-133">A subscription is created by the Azure **account owner**, who establishes the financial commitment and is responsible for paying for all Azure resources associated with the subscription:</span></span>

<span data-ttu-id="bd684-134">![Azure-Kontobesitzer erstellt ein Abonnement](../../_images/governance-1-3.png)
*Abbildung 3: Der Azure-Kontobesitzer erstellt ein Abonnement.*</span><span class="sxs-lookup"><span data-stu-id="bd684-134">![The Azure account owner creates a subscription](../../_images/governance-1-3.png)
*Figure 3. The Azure account owner creates a subscription.*</span></span>

<span data-ttu-id="bd684-135">Bei der Erstellung des Abonnements ordnet der Azure-**Kontobesitzer** dem Abonnement einen Azure AD-Mandanten zu, und dieser Azure AD-Mandant wird verwendet, um Benutzer zu authentifizieren und zu autorisieren:</span><span class="sxs-lookup"><span data-stu-id="bd684-135">When the subscription is created, the Azure **account owner** associates an Azure AD tenant with the subscription, and this Azure AD tenant is used for authenticating and authorizing users:</span></span>

<span data-ttu-id="bd684-136">![Azure-Kontobesitzer ordnet den Azure AD-Mandanten dem Abonnement zu](../../_images/governance-1-4.png)
*Abbildung 4: Der Azure-Kontobesitzer ordnet den Azure AD-Mandanten dem Abonnement zu.*</span><span class="sxs-lookup"><span data-stu-id="bd684-136">![The Azure account owner associates the Azure AD tenant with the subscription](../../_images/governance-1-4.png)
*Figure 4. The Azure account owner associates the Azure AD tenant with the subscription.*</span></span>

<span data-ttu-id="bd684-137">Unter Umständen haben Sie bemerkt, dass dem Abonnement derzeit kein Benutzer zugeordnet ist. Dies bedeutet, dass keine Person über die Berechtigung zum Verwalten von Ressourcen verfügt.</span><span class="sxs-lookup"><span data-stu-id="bd684-137">You may have noticed that there is currently no user associated with the subscription, which means that no one has permission to manage resources.</span></span> <span data-ttu-id="bd684-138">In Wirklichkeit ist der **Kontobesitzer** der Besitzer des Abonnements und verfügt über die Berechtigung zur Durchführung aller Aktionen für eine Ressource des Abonnements.</span><span class="sxs-lookup"><span data-stu-id="bd684-138">In reality, the **account owner** is the owner of the subscription and has permission to take any action on a resource in the subscription.</span></span> <span data-ttu-id="bd684-139">Aber in der Praxis ist der **Kontobesitzer** in Ihrer Organisation wahrscheinlich eher ein Mitarbeiter der Finanzabteilung und nicht dafür verantwortlich, Ressourcen zu erstellen, zu lesen, zu aktualisieren und zu löschen. Diese Aufgaben werden vom **Workloadbesitzer** übernommen.</span><span class="sxs-lookup"><span data-stu-id="bd684-139">However, in practical terms the **account owner** is more than likely a finance person in your organization and is not responsible for creating, reading, updating, and deleting resources - those tasks will be performed by the **workload owner**.</span></span> <span data-ttu-id="bd684-140">Daher müssen Sie den **Workloadbesitzer** dem Abonnement hinzufügen und Berechtigungen zuweisen.</span><span class="sxs-lookup"><span data-stu-id="bd684-140">Therefore, you need to add the **workload owner** to the subscription and assign permissions.</span></span>

<span data-ttu-id="bd684-141">Da der **Kontobesitzer** derzeit der einzige Benutzer mit der Berechtigung zum Hinzufügen des **Workloadbesitzers** zum Abonnement ist, übernimmt er die Aufgabe, den **Workloadbesitzer** dem Abonnement hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="bd684-141">Since the **account owner** is currently the only user with permission to add the **workload owner** to the subscription, they add the **workload owner** to the subscription:</span></span>

<span data-ttu-id="bd684-142">![Azure-Kontobesitzer fügt den **Workloadbesitzer** dem Abonnement hinzu](../../_images/governance-1-5.png)
*Abbildung 5: Azure-Kontobesitzer fügt den **Workloadbesitzer** dem Abonnement hinzu.*</span><span class="sxs-lookup"><span data-stu-id="bd684-142">![The Azure account owner adds the **workload owner** to the subscription](../../_images/governance-1-5.png)
*Figure 5. The Azure account owner adds the workload owner to the subscription.*</span></span>

<span data-ttu-id="bd684-143">Der Azure-**Kontobesitzer** erteilt dem **Workloadbesitzer** Berechtigungen, indem eine RBAC-Rolle ([Role-Based Access Control, rollenbasierte Zugriffssteuerung](/azure/role-based-access-control/)) zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="bd684-143">The Azure **account owner** grants permissions to the **workload owner** by assigning a [role-based access control (RBAC)](/azure/role-based-access-control/) role.</span></span> <span data-ttu-id="bd684-144">Mit der RBAC-Rolle wird ein Satz mit Berechtigungen angegeben, über die der **Workloadbesitzer** für einen oder mehrere Ressourcentypen verfügt.</span><span class="sxs-lookup"><span data-stu-id="bd684-144">The RBAC role specifies a set of permissions that the **workload owner** has for an individual resource type or a set of resource types.</span></span>

<span data-ttu-id="bd684-145">Beachten Sie, dass der **Kontobesitzer** in diesem Beispiel die [integrierte Rolle **Besitzer**](/azure/role-based-access-control/built-in-roles#owner) zugewiesen hat:</span><span class="sxs-lookup"><span data-stu-id="bd684-145">Notice that in this example, the **account owner** has assigned the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner):</span></span>

<span data-ttu-id="bd684-146">![Dem **Workloadbesitzer** wurde die integrierte Rolle „Besitzer“ zugewiesen](../../_images/governance-1-6.png)
*Abbildung 6: Dem Workloadbesitzer wurde die integrierte Rolle „Besitzer“ zugewiesen.*</span><span class="sxs-lookup"><span data-stu-id="bd684-146">![The **workload owner** was assigned the built-in owner role](../../_images/governance-1-6.png)
*Figure 6. The workload owner was assigned the built-in owner role.*</span></span>

<span data-ttu-id="bd684-147">Mit der integrierten Rolle **Besitzer** werden für den **Workloadbesitzer** alle Berechtigungen für den Abonnementbereich gewährt.</span><span class="sxs-lookup"><span data-stu-id="bd684-147">The built-in **owner** role grants all permissions to the **workload owner** at the subscription scope.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bd684-148">Der Azure-**Kontobesitzer** ist für die Zahlungsverpflichtung verantwortlich, die dem Abonnement zugeordnet ist, aber der **Workloadbesitzer** verfügt über die gleichen Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="bd684-148">The Azure **acount owner** is responsible for the financial committment associated with the subscription, but the **workload owner** has the same permissions.</span></span> <span data-ttu-id="bd684-149">Der **Kontobesitzer** muss darauf vertrauen, dass der **Workloadbesitzer** Ressourcen bereitstellt, die sich innerhalb des Abonnementbudgets bewegen.</span><span class="sxs-lookup"><span data-stu-id="bd684-149">The **account owner** must trust the **workload owner** to deploy resources that are within the subscription budget.</span></span>

<span data-ttu-id="bd684-150">Die nächste Ebene des Verwaltungsbereichs ist die Ebene **Ressourcengruppe**.</span><span class="sxs-lookup"><span data-stu-id="bd684-150">The next level of management scope is the **resource group** level.</span></span> <span data-ttu-id="bd684-151">Eine Ressourcengruppe ist ein logischer Container für Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="bd684-151">A resource group is a logical container for resources.</span></span> <span data-ttu-id="bd684-152">Vorgänge, die auf der Ressourcengruppenebene angewendet werden, gelten für alle Ressourcen einer Gruppe.</span><span class="sxs-lookup"><span data-stu-id="bd684-152">Operations applied at the resource group level apply to all resources in a group.</span></span> <span data-ttu-id="bd684-153">Es ist auch wichtig zu beachten, dass die Berechtigungen für die einzelnen Benutzer von der nächsthöheren Ebene geerbt werden, sofern sie in diesem Bereich nicht explizit geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bd684-153">Also, it's important to note that permissions for each user are inherited from the next level up unless they are explicitly changed at that scope.</span></span>

<span data-ttu-id="bd684-154">Zur Verdeutlichung sehen wir uns an, was passiert, wenn der **Workloadbesitzer** eine Ressourcengruppe erstellt:</span><span class="sxs-lookup"><span data-stu-id="bd684-154">To illustrate this, let's look at what happens when the **workload owner** creates a resource group:</span></span>

<span data-ttu-id="bd684-155">![**Workloadbesitzer** erstellt eine Ressourcengruppe](../../_images/governance-1-7.png)
*Abbildung 7: Der Workloadbesitzer erstellt eine Ressourcengruppe und erbt die integrierte Rolle „Besitzer“ für den Ressourcengruppenbereich.*</span><span class="sxs-lookup"><span data-stu-id="bd684-155">![The **workload owner** creates a resource group](../../_images/governance-1-7.png)
*Figure 7. The workload owner creates a resource group and inherits the built-in owner role at the resource group scope.*</span></span>

<span data-ttu-id="bd684-156">Mit der integrierten Rolle **Besitzer** werden für den **Workloadbesitzer** wiederum alle Berechtigungen für den Ressourcengruppenbereich gewährt.</span><span class="sxs-lookup"><span data-stu-id="bd684-156">Again, the built-in **owner** role grants all permissions to the **workload owner** at the resource group scope.</span></span> <span data-ttu-id="bd684-157">Wie bereits beschrieben, wird diese Rolle von der Abonnementebene geerbt.</span><span class="sxs-lookup"><span data-stu-id="bd684-157">As discussed earlier, this role is inherited from the subscription level.</span></span> <span data-ttu-id="bd684-158">Wenn diesem Benutzer für diesen Bereich eine andere Rolle zugewiesen wird, gilt dies nur für den Bereich.</span><span class="sxs-lookup"><span data-stu-id="bd684-158">If a different role is assigned to this user at this scope, it applies to this scope only.</span></span>

<span data-ttu-id="bd684-159">Die niedrigste Ebene des Verwaltungsbereichs ist die Ebene **Ressource**.</span><span class="sxs-lookup"><span data-stu-id="bd684-159">The lowest level of management scope is at the **resource** level.</span></span> <span data-ttu-id="bd684-160">Vorgänge, die auf Ressourcenebene durchgeführt werden, gelten nur für die Ressource selbst.</span><span class="sxs-lookup"><span data-stu-id="bd684-160">Operations applied at the resource level apply only to the resource itself.</span></span> <span data-ttu-id="bd684-161">Auch hier werden die Berechtigungen der Ressourcenebene vom Ressourcengruppenbereich geerbt.</span><span class="sxs-lookup"><span data-stu-id="bd684-161">And once again, permissions at the resource level are inherited from resource group scope.</span></span> <span data-ttu-id="bd684-162">Wir können uns beispielsweise ansehen, was passiert, wenn der **Workloadbesitzer** in der Ressourcengruppe ein [virtuelles Netzwerk](/azure/virtual-network/virtual-networks-overview) bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="bd684-162">For example, let's look at what happens if the **workload owner** deploys a [virtual network](/azure/virtual-network/virtual-networks-overview) into the resource group:</span></span>

<span data-ttu-id="bd684-163">![**Workloadbesitzer** erstellt eine Ressource](../../_images/governance-1-8.png)
*Abbildung 8: Der Workloadbesitzer erstellt eine Ressource und erbt die integrierte Rolle „Besitzer“ für den Ressourcenbereich.*</span><span class="sxs-lookup"><span data-stu-id="bd684-163">![The **workload owner** creates a resource](../../_images/governance-1-8.png)
*Figure 8. The workload owner creates a resource and inherits the built-in owner role at the resource scope.*</span></span>

<span data-ttu-id="bd684-164">Der **Workloadbesitzer** erbt die Rolle „Besitzer“ für den Ressourcenbereich. Dies bedeutet, dass der Workloadbesitzer über alle Berechtigungen für das virtuelle Netzwerk verfügt.</span><span class="sxs-lookup"><span data-stu-id="bd684-164">The **workload owner** inherits the owner role at the resource scope, which means the workload owner has all permissions for the virtual network.</span></span>

## <a name="implementing-the-basic-resource-access-management-model"></a><span data-ttu-id="bd684-165">Implementieren des grundlegenden Modells für die Ressourcenzugriffsverwaltung</span><span class="sxs-lookup"><span data-stu-id="bd684-165">Implementing the basic resource access management model</span></span>

<span data-ttu-id="bd684-166">Kommen wir zur Implementierung des zuvor entworfenen Governance-Modells.</span><span class="sxs-lookup"><span data-stu-id="bd684-166">Let's move on to learn how to implement the governance model designed earlier.</span></span>

<span data-ttu-id="bd684-167">Ihre Organisation benötigt ein Azure-Konto, um beginnen zu können.</span><span class="sxs-lookup"><span data-stu-id="bd684-167">To begin, your organization requires an Azure account.</span></span> <span data-ttu-id="bd684-168">Wenn Ihre Organisation über ein vorhandenes [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) verfügt, in dem Azure nicht enthalten ist, kann Azure hinzugefügt werden, indem Sie vorab eine Zahlungsverpflichtung eingehen.</span><span class="sxs-lookup"><span data-stu-id="bd684-168">If your organization has an existing [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) that does not include Azure, Azure can be added by making an upfront monetary commitment.</span></span> <span data-ttu-id="bd684-169">Weitere Informationen finden Sie unter [Lizenzierung von Azure für das Unternehmen](https://azure.microsoft.com/pricing/enterprise-agreement/).</span><span class="sxs-lookup"><span data-stu-id="bd684-169">See [licensing Azure for the enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) for more information.</span></span>

<span data-ttu-id="bd684-170">Beim Erstellen Ihres Azure-Kontos geben Sie eine Person in Ihrer Organisation als Azure-**Kontobesitzer** an.</span><span class="sxs-lookup"><span data-stu-id="bd684-170">When your Azure account is created, you specify a person in your organization to be the Azure **account owner**.</span></span> <span data-ttu-id="bd684-171">Anschließend wird standardmäßig ein Azure AD-Mandant (Azure Active Directory) erstellt.</span><span class="sxs-lookup"><span data-stu-id="bd684-171">An Azure Active Directory (Azure AD) tenant is then created by default.</span></span> <span data-ttu-id="bd684-172">Ihr Azure-**Kontobesitzer** muss die [Erstellung des Benutzerkontos](/azure/active-directory/add-users-azure-active-directory) für die Person in Ihrer Organisation durchführen, die als **Workloadbesitzer** fungiert.</span><span class="sxs-lookup"><span data-stu-id="bd684-172">Your Azure **account owner** must [create the user account](/azure/active-directory/add-users-azure-active-directory) for the person in your organization who is the **workload owner**.</span></span>

<span data-ttu-id="bd684-173">Als Nächstes muss Ihr Azure-**Kontobesitzer** ein [Abonnement erstellen](/partner-center/create-a-new-subscription) und diesem [den Azure AD-Mandanten zuordnen](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="bd684-173">Next, your Azure **account owner** must [create a subscription](/partner-center/create-a-new-subscription) and [associate the Azure AD tenant](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory) with it.</span></span>

<span data-ttu-id="bd684-174">Nachdem das Abonnement erstellt und Ihr Azure AD-Mandant zugeordnet wurde, können Sie [den **Workloadbesitzer** dem Abonnement mit der integrierten Rolle **Besitzer** hinzufügen](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="bd684-174">Finally, now that the subscription is created and your Azure AD tenant is associated with it, you can [add the **workload owner** to the subscription with the built-in **owner** role](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd684-175">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="bd684-175">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bd684-176">Bereitstellen einer grundlegenden Workload in Azure</span><span class="sxs-lookup"><span data-stu-id="bd684-176">Deploy a basic workload to Azure</span></span>](../../infrastructure/basic-workload.md)
> [!div class="nextstepaction"]
> [<span data-ttu-id="bd684-177">Informationen zum Ressourcenzugriff für mehrere Teams</span><span class="sxs-lookup"><span data-stu-id="bd684-177">Learn about resource access for multiple teams</span></span>](governance-multiple-teams.md)