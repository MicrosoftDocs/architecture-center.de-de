---
title: Ausführen der Surveys-Anwendung
description: Hier erfahren Sie, wie Sie die Surveys-Beispielanwendung lokal ausführen.
author: MikeWasson
ms.date: 07/21/2017
ms.openlocfilehash: b73eeb04755b3dc8443b215bb034c82e3095681d
ms.sourcegitcommit: 7d9efe716e8c9e99f3fafa9d0213d48c23d9713d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54160756"
---
# <a name="run-the-surveys-application"></a><span data-ttu-id="b0062-103">Ausführen der Surveys-Anwendung</span><span class="sxs-lookup"><span data-stu-id="b0062-103">Run the Surveys application</span></span>

<span data-ttu-id="b0062-104">In diesem Artikel wird beschrieben, wie Sie die Anwendung [Tailspin Surveys](./tailspin.md) über Visual Studio lokal ausführen.</span><span class="sxs-lookup"><span data-stu-id="b0062-104">This article describes how to run the [Tailspin Surveys](./tailspin.md) application locally, from Visual Studio.</span></span> <span data-ttu-id="b0062-105">In diesen Schritten wird die Anwendung nicht in Azure bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b0062-105">In these steps, you won't deploy the application to Azure.</span></span> <span data-ttu-id="b0062-106">Sie müssen jedoch einige Azure-Ressourcen erstellen: ein Azure AD-Verzeichnis (Azure Active Directory) und einen Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="b0062-106">However, you will need to create some Azure resources &mdash; an Azure Active Directory (Azure AD) directory and a Redis cache.</span></span>

<span data-ttu-id="b0062-107">Zusammenfassung der Schritte:</span><span class="sxs-lookup"><span data-stu-id="b0062-107">Here is a summary of the steps:</span></span>

1. <span data-ttu-id="b0062-108">Erstellen Sie ein Azure AD-Verzeichnis (Mandant) für das fiktive Unternehmen Tailspin.</span><span class="sxs-lookup"><span data-stu-id="b0062-108">Create an Azure AD directory (tenant) for the fictitious Tailspin company.</span></span>
2. <span data-ttu-id="b0062-109">Registrieren Sie die Surveys-Anwendung und die Back-End-Web-API bei Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0062-109">Register the Surveys application and the backend web API with Azure AD.</span></span>
3. <span data-ttu-id="b0062-110">Erstellen Sie eine Instanz von Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="b0062-110">Create an Azure Redis Cache instance.</span></span>
4. <span data-ttu-id="b0062-111">Konfigurieren Sie die Anwendungseinstellungen, und erstellen Sie eine lokale Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b0062-111">Configure application settings and create a local database.</span></span>
5. <span data-ttu-id="b0062-112">Führen Sie die Anwendung aus, und registrieren Sie einen neuen Mandanten.</span><span class="sxs-lookup"><span data-stu-id="b0062-112">Run the application and sign up a new tenant.</span></span>
6. <span data-ttu-id="b0062-113">Fügen Sie Benutzern Anwendungsrollen hinzu.</span><span class="sxs-lookup"><span data-stu-id="b0062-113">Add application roles to users.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0062-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b0062-114">Prerequisites</span></span>

- <span data-ttu-id="b0062-115">[Visual Studio 2017][VS2017] mit installierter Workload [ASP.NET und Webentwicklung](https://visualstudio.microsoft.com/vs/support/selecting-workloads-visual-studio-2017)</span><span class="sxs-lookup"><span data-stu-id="b0062-115">[Visual Studio 2017][VS2017] with the [ASP.NET and web development workload](https://visualstudio.microsoft.com/vs/support/selecting-workloads-visual-studio-2017) installed</span></span>
- <span data-ttu-id="b0062-116">[Microsoft Azure](https://azure.microsoft.com)-Konto</span><span class="sxs-lookup"><span data-stu-id="b0062-116">[Microsoft Azure](https://azure.microsoft.com) account</span></span>

## <a name="create-the-tailspin-tenant"></a><span data-ttu-id="b0062-117">Erstellen des Tailspin-Mandanten</span><span class="sxs-lookup"><span data-stu-id="b0062-117">Create the Tailspin tenant</span></span>

<span data-ttu-id="b0062-118">Tailspin ist das fiktive Unternehmen, das die Surveys-Anwendung hostet.</span><span class="sxs-lookup"><span data-stu-id="b0062-118">Tailspin is the fictitious company that hosts the Surveys application.</span></span> <span data-ttu-id="b0062-119">Tailspin verwendet Azure AD, um anderen Mandanten das Registrieren bei der App zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b0062-119">Tailspin uses Azure AD to enable other tenants to register with the app.</span></span> <span data-ttu-id="b0062-120">Diese Kunden können sich dann mit ihren Azure AD-Anmeldeinformationen bei der App registrieren.</span><span class="sxs-lookup"><span data-stu-id="b0062-120">Those customers can then use their Azure AD credentials to sign into the app.</span></span>

<span data-ttu-id="b0062-121">In diesem Schritt erstellen Sie ein Azure AD-Verzeichnis für Tailspin.</span><span class="sxs-lookup"><span data-stu-id="b0062-121">In this step, you'll create an Azure AD directory for Tailspin.</span></span>

1. <span data-ttu-id="b0062-122">Melden Sie sich beim [Azure-Portal][portal] an.</span><span class="sxs-lookup"><span data-stu-id="b0062-122">Sign into the [Azure portal][portal].</span></span>

2. <span data-ttu-id="b0062-123">Klicken Sie auf **+ Ressource erstellen** > **Identität** > **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b0062-123">Click **+ Create a Resource** > **Identity** > **Azure Active Directory**.</span></span>

3. <span data-ttu-id="b0062-124">Geben Sie für den Organisationsnamen `Tailspin` ein, und geben Sie einen Domänennamen ein.</span><span class="sxs-lookup"><span data-stu-id="b0062-124">Enter `Tailspin` for the organization name, and enter a domain name.</span></span> <span data-ttu-id="b0062-125">Der Domänenname hat das Format `xxxx.onmicrosoft.com` und muss global eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="b0062-125">The domain name will have the form `xxxx.onmicrosoft.com` and must be globally unique.</span></span>

    ![Dialogfeld „Verzeichnis erstellen“](./images/running-the-app/new-tenant.png)

4. <span data-ttu-id="b0062-127">Klicken Sie auf **Create**.</span><span class="sxs-lookup"><span data-stu-id="b0062-127">Click **Create**.</span></span> <span data-ttu-id="b0062-128">Die Erstellung des neuen Verzeichnisses kann einige Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="b0062-128">It may take a few minutes to create the new directory.</span></span>

<span data-ttu-id="b0062-129">Zum Abschließen des End-to-End-Szenarios benötigen Sie ein zweites Azure AD-Verzeichnis, das einen Kunden darstellt, der sich für die Anwendung registriert.</span><span class="sxs-lookup"><span data-stu-id="b0062-129">To complete the end-to-end scenario, you'll need a second Azure AD directory to represent a customer that signs up for the application.</span></span> <span data-ttu-id="b0062-130">Sie können Ihr Azure AD-Standardverzeichnis (nicht Tailspin) verwenden oder für diesen Zweck ein neues Verzeichnis erstellen.</span><span class="sxs-lookup"><span data-stu-id="b0062-130">You can use your default Azure AD directory (not Tailspin), or create a new directory for this purpose.</span></span> <span data-ttu-id="b0062-131">In den Beispielen verwenden wir als fiktiven Kunden Contoso.</span><span class="sxs-lookup"><span data-stu-id="b0062-131">In the examples, we use Contoso as the fictitious customer.</span></span>

## <a name="register-the-surveys-web-api"></a><span data-ttu-id="b0062-132">Registrieren der Surveys-Web-API</span><span class="sxs-lookup"><span data-stu-id="b0062-132">Register the Surveys web API</span></span>

1. <span data-ttu-id="b0062-133">Wechseln Sie im [Azure-Portal][portal] zu dem neuen Tailspin-Verzeichnis, indem Sie in der rechten oberen Ecke des Portals Ihr Konto auswählen.</span><span class="sxs-lookup"><span data-stu-id="b0062-133">In the [Azure portal][portal], switch to the new Tailspin directory by selecting your account in the top right corner of the portal.</span></span>

2. <span data-ttu-id="b0062-134">Wählen Sie im linken Navigationsbereich **Azure Active Directory** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-134">In the left-hand navigation pane, choose **Azure Active Directory**.</span></span>

3. <span data-ttu-id="b0062-135">Klicken Sie auf **App-Registrierungen** > **Registrierung einer neuen Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="b0062-135">Click **App registrations** > **New application registration**.</span></span>

4. <span data-ttu-id="b0062-136">Geben Sie auf dem Blatt **Erstellen** die folgenden Informationen ein:</span><span class="sxs-lookup"><span data-stu-id="b0062-136">In the **Create** blade, enter the following information:</span></span>

   - <span data-ttu-id="b0062-137">**Name**: `Surveys.WebAPI`</span><span class="sxs-lookup"><span data-stu-id="b0062-137">**Name**: `Surveys.WebAPI`</span></span>

   - <span data-ttu-id="b0062-138">**Anwendungstyp:**: `Web app / API`</span><span class="sxs-lookup"><span data-stu-id="b0062-138">**Application type**: `Web app / API`</span></span>

   - <span data-ttu-id="b0062-139">**Anmelde-URL**: `https://localhost:44301/`</span><span class="sxs-lookup"><span data-stu-id="b0062-139">**Sign-on URL**: `https://localhost:44301/`</span></span>

   ![Screenshot: Registrieren der Web-API](./images/running-the-app/register-web-api.png)

5. <span data-ttu-id="b0062-141">Klicken Sie auf **Create**.</span><span class="sxs-lookup"><span data-stu-id="b0062-141">Click **Create**.</span></span>

6. <span data-ttu-id="b0062-142">Wählen Sie im Blatt **App-Registrierungen** die neue Anwendung **Surveys.WebAPI** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-142">In the **App registrations** blade, select the new **Surveys.WebAPI** application.</span></span>

7. <span data-ttu-id="b0062-143">Klicken Sie auf **Einstellungen** > **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="b0062-143">Click **Settings** > **Properties**.</span></span>

8. <span data-ttu-id="b0062-144">Geben Sie im Bearbeitungsfeld **App-ID-URI** `https://<domain>/surveys.webapi` ein, wobei `<domain>` der Domänenname des Verzeichnisses ist.</span><span class="sxs-lookup"><span data-stu-id="b0062-144">In the **App ID URI** edit box, enter `https://<domain>/surveys.webapi`, where `<domain>` is the domain name of the directory.</span></span> <span data-ttu-id="b0062-145">Beispiel: `https://tailspin.onmicrosoft.com/surveys.webapi`</span><span class="sxs-lookup"><span data-stu-id="b0062-145">For example: `https://tailspin.onmicrosoft.com/surveys.webapi`</span></span>

    ![Einstellungen](./images/running-the-app/settings.png)

9. <span data-ttu-id="b0062-147">Legen Sie **Mehrinstanzenfähig** auf **JA** fest.</span><span class="sxs-lookup"><span data-stu-id="b0062-147">Set **Multi-tenanted** to **YES**.</span></span>

10. <span data-ttu-id="b0062-148">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="b0062-148">Click **Save**.</span></span>

## <a name="register-the-surveys-web-app"></a><span data-ttu-id="b0062-149">Registrieren der Surveys-Web-App</span><span class="sxs-lookup"><span data-stu-id="b0062-149">Register the Surveys web app</span></span>

1. <span data-ttu-id="b0062-150">Kehren Sie zum Blatt **App-Registrierungen** zurück, und klicken Sie auf **Registrierung einer neuen Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="b0062-150">Navigate back to the **App registrations** blade, and click **New application registration**.</span></span>

2. <span data-ttu-id="b0062-151">Geben Sie auf dem Blatt **Erstellen** die folgenden Informationen ein:</span><span class="sxs-lookup"><span data-stu-id="b0062-151">In the **Create** blade, enter the following information:</span></span>

    - <span data-ttu-id="b0062-152">**Name**: `Surveys`</span><span class="sxs-lookup"><span data-stu-id="b0062-152">**Name**: `Surveys`</span></span>
    - <span data-ttu-id="b0062-153">**Anwendungstyp:**: `Web app / API`</span><span class="sxs-lookup"><span data-stu-id="b0062-153">**Application type**: `Web app / API`</span></span>
    - <span data-ttu-id="b0062-154">**Anmelde-URL**: `https://localhost:44300/`</span><span class="sxs-lookup"><span data-stu-id="b0062-154">**Sign-on URL**: `https://localhost:44300/`</span></span>

    <span data-ttu-id="b0062-155">Beachten Sie, dass die Anmelde-URL eine andere Portnummer als die `Surveys.WebAPI`-App im vorherigen Schritt enthält.</span><span class="sxs-lookup"><span data-stu-id="b0062-155">Notice that the sign-on URL has a different port number from the `Surveys.WebAPI` app in the previous step.</span></span>

3. <span data-ttu-id="b0062-156">Klicken Sie auf **Create**.</span><span class="sxs-lookup"><span data-stu-id="b0062-156">Click **Create**.</span></span>

4. <span data-ttu-id="b0062-157">Wählen Sie im Blatt **App-Registrierungen** die neue Anwendung **Surveys** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-157">In the **App registrations** blade, select the new **Surveys** application.</span></span>

5. <span data-ttu-id="b0062-158">Kopieren Sie die Anwendungs-ID.</span><span class="sxs-lookup"><span data-stu-id="b0062-158">Copy the application ID.</span></span> <span data-ttu-id="b0062-159">Sie benötigen sie später.</span><span class="sxs-lookup"><span data-stu-id="b0062-159">You will need this later.</span></span>

    ![Screenshot: Kopieren der Anwendungs-ID](./images/running-the-app/application-id.png)

6. <span data-ttu-id="b0062-161">Klicken Sie auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="b0062-161">Click **Properties**.</span></span>

7. <span data-ttu-id="b0062-162">Geben Sie im Bearbeitungsfeld **App-ID-URI** `https://<domain>/surveys` ein, wobei `<domain>` der Domänenname des Verzeichnisses ist.</span><span class="sxs-lookup"><span data-stu-id="b0062-162">In the **App ID URI** edit box, enter `https://<domain>/surveys`, where `<domain>` is the domain name of the directory.</span></span>

    ![Einstellungen](./images/running-the-app/settings.png)

8. <span data-ttu-id="b0062-164">Legen Sie **Mehrinstanzenfähig** auf **JA** fest.</span><span class="sxs-lookup"><span data-stu-id="b0062-164">Set **Multi-tenanted** to **YES**.</span></span>

9. <span data-ttu-id="b0062-165">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="b0062-165">Click **Save**.</span></span>

10. <span data-ttu-id="b0062-166">Klicken Sie auf dem Blatt **Einstellungen** auf **Antwort-URLs**.</span><span class="sxs-lookup"><span data-stu-id="b0062-166">In the **Settings** blade, click **Reply URLs**.</span></span>

11. <span data-ttu-id="b0062-167">Fügen Sie die folgende Antwort-URL hinzu: `https://localhost:44300/signin-oidc`.</span><span class="sxs-lookup"><span data-stu-id="b0062-167">Add the following reply URL: `https://localhost:44300/signin-oidc`.</span></span>

12. <span data-ttu-id="b0062-168">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="b0062-168">Click **Save**.</span></span>

13. <span data-ttu-id="b0062-169">Klicken Sie unter **API-ZUGRIFF** auf **Schlüssel**.</span><span class="sxs-lookup"><span data-stu-id="b0062-169">Under **API ACCESS**, click **Keys**.</span></span>

14. <span data-ttu-id="b0062-170">Geben Sie eine Beschreibung ein, z.B. `client secret`.</span><span class="sxs-lookup"><span data-stu-id="b0062-170">Enter a description, such as `client secret`.</span></span>

15. <span data-ttu-id="b0062-171">Wählen Sie in der Dropdownliste **Dauer auswählen** den Eintrag **1 Jahr** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-171">In the **Select Duration** dropdown, select **1 year**.</span></span>

16. <span data-ttu-id="b0062-172">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="b0062-172">Click **Save**.</span></span> <span data-ttu-id="b0062-173">Beim Speichern wird der Schlüssel generiert.</span><span class="sxs-lookup"><span data-stu-id="b0062-173">The key will be generated when you save.</span></span>

17. <span data-ttu-id="b0062-174">Bevor Sie dieses Blatt verlassen, kopieren Sie den Wert des Schlüssels.</span><span class="sxs-lookup"><span data-stu-id="b0062-174">Before you navigate away from this blade, copy the value of the key.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0062-175">Nachdem Sie das Blatt verlassen haben, wird der Schlüssel nicht mehr angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b0062-175">The key won't be visible again after you navigate away from the blade.</span></span>

18. <span data-ttu-id="b0062-176">Klicken Sie unter **API-ZUGRIFF** auf **Erforderliche Berechtigungen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-176">Under **API ACCESS**, click **Required permissions**.</span></span>

19. <span data-ttu-id="b0062-177">Klicken Sie auf **Hinzufügen** > **Hiermit wählen Sie eine API aus**.</span><span class="sxs-lookup"><span data-stu-id="b0062-177">Click **Add** > **Select an API**.</span></span>

20. <span data-ttu-id="b0062-178">Suchen Sie im Suchfeld nach `Surveys.WebAPI`.</span><span class="sxs-lookup"><span data-stu-id="b0062-178">In the search box, search for `Surveys.WebAPI`.</span></span>

    ![Berechtigungen](./images/running-the-app/permissions.png)

21. <span data-ttu-id="b0062-180">Wählen Sie `Surveys.WebAPI` aus, und klicken Sie auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-180">Select `Surveys.WebAPI` and click **Select**.</span></span>

22. <span data-ttu-id="b0062-181">Aktivieren Sie unter **Delegierte Berechtigungen** das Kontrollkästchen **Auf Surveys.WebAPI zugreifen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-181">Under **Delegated Permissions**, check **Access Surveys.WebAPI**.</span></span>

    ![Festlegen der delegierten Berechtigungen](./images/running-the-app/delegated-permissions.png)

23. <span data-ttu-id="b0062-183">Klicken Sie auf **Auswählen** > **Fertig**.</span><span class="sxs-lookup"><span data-stu-id="b0062-183">Click **Select** > **Done**.</span></span>

## <a name="update-the-application-manifests"></a><span data-ttu-id="b0062-184">Aktualisieren des Anwendungsmanifests</span><span class="sxs-lookup"><span data-stu-id="b0062-184">Update the application manifests</span></span>

1. <span data-ttu-id="b0062-185">Kehren Sie zum Blatt **Einstellungen** für die `Surveys.WebAPI`-App zurück.</span><span class="sxs-lookup"><span data-stu-id="b0062-185">Navigate back to the **Settings** blade for the `Surveys.WebAPI` app.</span></span>

2. <span data-ttu-id="b0062-186">Klicken Sie auf **Manifest** > **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="b0062-186">Click **Manifest** > **Edit**.</span></span>

    ![Screenshot: Bearbeiten des Anwendungsmanifests](./images/running-the-app/manifest.png)

3. <span data-ttu-id="b0062-188">Fügen Sie dem `appRoles`-Element den folgenden JSON-Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="b0062-188">Add the following JSON to the `appRoles` element.</span></span> <span data-ttu-id="b0062-189">Generieren Sie für die `id`-Eigenschaften neue GUIDs.</span><span class="sxs-lookup"><span data-stu-id="b0062-189">Generate new GUIDs for the `id` properties.</span></span>

   ```json
   {
     "allowedMemberTypes": ["User"],
     "description": "Creators can create surveys",
     "displayName": "SurveyCreator",
     "id": "<Generate a new GUID. Example: 1b4f816e-5eaf-48b9-8613-7923830595ad>",
     "isEnabled": true,
     "value": "SurveyCreator"
   },
   {
     "allowedMemberTypes": ["User"],
     "description": "Administrators can manage the surveys in their tenant",
     "displayName": "SurveyAdmin",
     "id": "<Generate a new GUID>",  
     "isEnabled": true,
     "value": "SurveyAdmin"
   }
   ```

4. <span data-ttu-id="b0062-190">Fügen Sie in der `knownClientApplications`-Eigenschaft die Anwendungs-ID für die Surveys-Webanwendung hinzu, die Sie zuvor beim Registrieren der Surveys-Anwendung erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="b0062-190">In the `knownClientApplications` property, add the application ID for the Surveys web application, which you got when you registered the Surveys application earlier.</span></span> <span data-ttu-id="b0062-191">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="b0062-191">For example:</span></span>

   ```json
   "knownClientApplications": ["be2cea23-aa0e-4e98-8b21-2963d494912e"],
   ```

   <span data-ttu-id="b0062-192">Durch diese Einstellung wird die Surveys-App der Liste der Clients hinzugefügt, die zum Aufrufen der Web-API berechtigt sind.</span><span class="sxs-lookup"><span data-stu-id="b0062-192">This setting adds the Surveys app to the list of clients authorized to call the web API.</span></span>

5. <span data-ttu-id="b0062-193">Klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="b0062-193">Click **Save**.</span></span>

<span data-ttu-id="b0062-194">Wiederholen Sie jetzt die gleichen Schritte für die Surveys-App, fügen Sie jedoch keinen Eintrag für `knownClientApplications` hinzu.</span><span class="sxs-lookup"><span data-stu-id="b0062-194">Now repeat the same steps for the Surveys app, except do not add an entry for `knownClientApplications`.</span></span> <span data-ttu-id="b0062-195">Verwenden Sie die gleichen Rollendefinitionen, generieren Sie jedoch für die IDs neue GUIDs.</span><span class="sxs-lookup"><span data-stu-id="b0062-195">Use the same role definitions, but generate new GUIDs for the IDs.</span></span>

## <a name="create-a-new-redis-cache-instance"></a><span data-ttu-id="b0062-196">Erstellen einer neuen Instanz von Redis Cache</span><span class="sxs-lookup"><span data-stu-id="b0062-196">Create a new Redis Cache instance</span></span>

<span data-ttu-id="b0062-197">Die Surveys-Anwendung verwendet zum Zwischenspeichern von OAuth 2-Zugriffstoken Redis.</span><span class="sxs-lookup"><span data-stu-id="b0062-197">The Surveys application uses Redis to cache OAuth 2 access tokens.</span></span> <span data-ttu-id="b0062-198">So erstellen Sie den Cache:</span><span class="sxs-lookup"><span data-stu-id="b0062-198">To create the cache:</span></span>

1. <span data-ttu-id="b0062-199">Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und klicken Sie auf **+ Ressource erstellen** > **Datenbanken** > **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="b0062-199">Go to [Azure Portal](https://portal.azure.com) and click **+ Create a Resource** > **Databases** > **Redis Cache**.</span></span>

2. <span data-ttu-id="b0062-200">Geben Sie die erforderlichen Informationen, einschließlich DNS-Name, Ressourcengruppe, Standort und Tarif, ein.</span><span class="sxs-lookup"><span data-stu-id="b0062-200">Fill in the required information, including DNS name, resource group, location, and pricing tier.</span></span> <span data-ttu-id="b0062-201">Sie können eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden.</span><span class="sxs-lookup"><span data-stu-id="b0062-201">You can create a new resource group or use an existing resource group.</span></span>

3. <span data-ttu-id="b0062-202">Klicken Sie auf **Create**.</span><span class="sxs-lookup"><span data-stu-id="b0062-202">Click **Create**.</span></span>

4. <span data-ttu-id="b0062-203">Navigieren Sie im Portal zur Ressource, nachdem die Redis Cache-Instanz erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b0062-203">After the Redis cache is created, navigate to the resource in the portal.</span></span>

5. <span data-ttu-id="b0062-204">Klicken Sie auf **Zugriffsschlüssel**, und kopieren Sie den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="b0062-204">Click **Access keys** and copy the primary key.</span></span>

<span data-ttu-id="b0062-205">Weitere Informationen über das Erstellen eines Redis Cache finden Sie unter [Verwenden von Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache).</span><span class="sxs-lookup"><span data-stu-id="b0062-205">For more information about creating a Redis cache, see [How to Use Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache).</span></span>

## <a name="set-application-secrets"></a><span data-ttu-id="b0062-206">Festlegen von Anwendungsgeheimnissen</span><span class="sxs-lookup"><span data-stu-id="b0062-206">Set application secrets</span></span>

1. <span data-ttu-id="b0062-207">Öffnen Sie in Visual Studio die Projektmappe „Tailspin.Surveys“.</span><span class="sxs-lookup"><span data-stu-id="b0062-207">Open the Tailspin.Surveys solution in Visual Studio.</span></span>

2. <span data-ttu-id="b0062-208">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Tailspin.Surveys.Web-Projekt, und wählen Sie **Benutzerschlüssel verwalten**.</span><span class="sxs-lookup"><span data-stu-id="b0062-208">In Solution Explorer, right-click the Tailspin.Surveys.Web project and select **Manage User Secrets**.</span></span>

3. <span data-ttu-id="b0062-209">Fügen Sie in der Datei „secrets.json“ Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="b0062-209">In the secrets.json file, paste in the following:</span></span>

    ```json
    {
      "AzureAd": {
        "ClientId": "<Surveys application ID>",
        "ClientSecret": "<Surveys app client secret>",
        "PostLogoutRedirectUri": "https://localhost:44300/",
        "WebApiResourceId": "<Surveys.WebAPI app ID URI>"
      },
      "Redis": {
        "Configuration": "<Redis DNS name>.redis.cache.windows.net,password=<Redis primary key>,ssl=true"
      }
    }
    ```

    <span data-ttu-id="b0062-210">Ersetzen Sie die Elemente in spitzen Klammern wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b0062-210">Replace the items shown in angle brackets, as follows:</span></span>

    - <span data-ttu-id="b0062-211">`AzureAd:ClientId`: Die Anwendungs-ID der Surveys-App.</span><span class="sxs-lookup"><span data-stu-id="b0062-211">`AzureAd:ClientId`: The application ID of the Surveys app.</span></span>
    - <span data-ttu-id="b0062-212">`AzureAd:ClientSecret`: Der Schlüssel, der generiert wurde, als Sie die Surveys-Anwendung in Azure AD registriert haben.</span><span class="sxs-lookup"><span data-stu-id="b0062-212">`AzureAd:ClientSecret`: The key that you generated when you registered the Surveys application in Azure AD.</span></span>
    - <span data-ttu-id="b0062-213">`AzureAd:WebApiResourceId`: Der App-ID-URI, den Sie angegeben haben, als Sie die Anwendung „Surveys.WebAPI“ in Azure AD erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b0062-213">`AzureAd:WebApiResourceId`: The App ID URI that you specified when you created the Surveys.WebAPI application in Azure AD.</span></span> <span data-ttu-id="b0062-214">Er muss das Format `https://<directory>.onmicrosoft.com/surveys.webapi` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0062-214">It should have the form `https://<directory>.onmicrosoft.com/surveys.webapi`</span></span>
    - <span data-ttu-id="b0062-215">`Redis:Configuration`: Erstellen Sie diese Zeichenfolge aus dem DNS-Namen des Redis Cache und dem primären Zugriffsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="b0062-215">`Redis:Configuration`: Build this string from the DNS name of the Redis cache and the primary access key.</span></span> <span data-ttu-id="b0062-216">Beispiel: „tailspin.redis.cache.windows.net,password=2h5tBxxx,ssl=true“.</span><span class="sxs-lookup"><span data-stu-id="b0062-216">For example, "tailspin.redis.cache.windows.net,password=2h5tBxxx,ssl=true".</span></span>

4. <span data-ttu-id="b0062-217">Speichern Sie die aktualisierte secrets.json-Datei.</span><span class="sxs-lookup"><span data-stu-id="b0062-217">Save the updated secrets.json file.</span></span>

5. <span data-ttu-id="b0062-218">Wiederholen Sie diese Schritte für das Projekt „Tailspin.Surveys.WebAPI“, fügen Sie jedoch in „secrets.json“ den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="b0062-218">Repeat these steps for the Tailspin.Surveys.WebAPI project, but paste the following into secrets.json.</span></span> <span data-ttu-id="b0062-219">Ersetzen Sie wie zuvor die Elemente in spitzen Klammern.</span><span class="sxs-lookup"><span data-stu-id="b0062-219">Replace the items in angle brackets, as before.</span></span>

    ```json
    {
      "AzureAd": {
        "WebApiResourceId": "<Surveys.WebAPI app ID URI>"
      },
      "Redis": {
        "Configuration": "<Redis DNS name>.redis.cache.windows.net,password=<Redis primary key>,ssl=true"
      }
    }
    ```

## <a name="initialize-the-database"></a><span data-ttu-id="b0062-220">Initialisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="b0062-220">Initialize the database</span></span>

<span data-ttu-id="b0062-221">In diesem Schritt verwenden Sie Entity Framework 7, um mithilfe von LocalDB eine lokale SQL-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b0062-221">In this step, you will use Entity Framework 7 to create a local SQL database, using LocalDB.</span></span>

1. <span data-ttu-id="b0062-222">Öffnen Sie ein Befehlsfenster.</span><span class="sxs-lookup"><span data-stu-id="b0062-222">Open a command window</span></span>

2. <span data-ttu-id="b0062-223">Navigieren Sie zum Projekt „Tailspin.Surveys.Data“.</span><span class="sxs-lookup"><span data-stu-id="b0062-223">Navigate to the Tailspin.Surveys.Data project.</span></span>

3. <span data-ttu-id="b0062-224">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b0062-224">Run the following command:</span></span>

    ```bat
    dotnet ef database update --startup-project ..\Tailspin.Surveys.Web
    ```

## <a name="run-the-application"></a><span data-ttu-id="b0062-225">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b0062-225">Run the application</span></span>

<span data-ttu-id="b0062-226">Starten Sie zum Ausführen der Anwendung die Projekte „Tailspin.Surveys.Web“ und „Tailspin.Surveys.WebAPI“.</span><span class="sxs-lookup"><span data-stu-id="b0062-226">To run the application, start both the Tailspin.Surveys.Web and Tailspin.Surveys.WebAPI projects.</span></span>

<span data-ttu-id="b0062-227">Sie können in Visual Studio wie folgt festlegen, dass durch Drücken von F5 beide Projekte automatisch ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="b0062-227">You can set Visual Studio to run both projects automatically on F5, as follows:</span></span>

1. <span data-ttu-id="b0062-228">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **Startprojekte festlegen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-228">In Solution Explorer, right-click the solution and click **Set Startup Projects**.</span></span>
2. <span data-ttu-id="b0062-229">Wählen Sie **Mehrere Startprojekte** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-229">Select **Multiple startup projects**.</span></span>
3. <span data-ttu-id="b0062-230">Legen Sie für die Projekte „Tailspin.Surveys.Web“ und „Tailspin.Surveys.WebAPI“ **Aktion** = **Starten** fest.</span><span class="sxs-lookup"><span data-stu-id="b0062-230">Set **Action** = **Start** for the Tailspin.Surveys.Web and Tailspin.Surveys.WebAPI projects.</span></span>

## <a name="sign-up-a-new-tenant"></a><span data-ttu-id="b0062-231">Registrieren eines neuen Mandanten</span><span class="sxs-lookup"><span data-stu-id="b0062-231">Sign up a new tenant</span></span>

<span data-ttu-id="b0062-232">Da Sie beim Starten der Anwendung nicht angemeldet sind, wird die Homepage angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b0062-232">When the application starts, you are not signed in, so you see the welcome page:</span></span>

![Homepage](./images/running-the-app/screenshot1.png)

<span data-ttu-id="b0062-234">So registrieren Sie eine Organisation:</span><span class="sxs-lookup"><span data-stu-id="b0062-234">To sign up an organization:</span></span>

1. <span data-ttu-id="b0062-235">Klicken Sie auf **Enroll your company in Tailspin** (Registrieren Sie Ihr Unternehmen in Tailspin).</span><span class="sxs-lookup"><span data-stu-id="b0062-235">Click **Enroll your company in Tailspin**.</span></span>
2. <span data-ttu-id="b0062-236">Melden Sie sich mit der Surveys-App bei dem Azure AD-Verzeichnis an, das die Organisation darstellt.</span><span class="sxs-lookup"><span data-stu-id="b0062-236">Sign in to the Azure AD directory that represents the organization using the Surveys app.</span></span> <span data-ttu-id="b0062-237">Sie müssen sich als Administratorbenutzer anmelden.</span><span class="sxs-lookup"><span data-stu-id="b0062-237">You must sign in as an admin user.</span></span>
3. <span data-ttu-id="b0062-238">Stimmen Sie der Zustimmungsaufforderung zu.</span><span class="sxs-lookup"><span data-stu-id="b0062-238">Accept the consent prompt.</span></span>

<span data-ttu-id="b0062-239">Die Anwendung registriert den Mandanten, und anschließend werden Sie abgemeldet. Sie werden von der App abgemeldet, da Sie vor dem Verwenden der Anwendung die Anwendungsrollen in Azure AD einrichten müssen.</span><span class="sxs-lookup"><span data-stu-id="b0062-239">The application registers the tenant, and then signs you out. The app signs you out because you need to set up the application roles in Azure AD, before using the application.</span></span>

![Nach der Registrierung](./images/running-the-app/screenshot2.png)

## <a name="assign-application-roles"></a><span data-ttu-id="b0062-241">Zuweisen von Anwendungsrollen</span><span class="sxs-lookup"><span data-stu-id="b0062-241">Assign application roles</span></span>

<span data-ttu-id="b0062-242">Wenn sich ein Mandant registriert, muss ein AD-Administrator für den Mandanten Benutzern Anwendungsrollen zuweisen.</span><span class="sxs-lookup"><span data-stu-id="b0062-242">When a tenant signs up, an AD admin for the tenant must assign application roles to users.</span></span>

1. <span data-ttu-id="b0062-243">Wechseln Sie im [Azure-Portal][portal] zu dem Azure AD-Verzeichnis, das Sie zum Registrieren für die Surveys-App verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="b0062-243">In the [Azure portal][portal], switch to the Azure AD directory that you used to sign up for the Surveys app.</span></span>

2. <span data-ttu-id="b0062-244">Wählen Sie im linken Navigationsbereich **Azure Active Directory** aus.</span><span class="sxs-lookup"><span data-stu-id="b0062-244">In the left-hand navigation pane, choose **Azure Active Directory**.</span></span>

3. <span data-ttu-id="b0062-245">Klicken Sie auf **Unternehmensanwendungen** > **Alle Anwendungen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-245">Click **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="b0062-246">Im Portal werden `Survey` und `Survey.WebAPI` aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="b0062-246">The portal will list `Survey` and `Survey.WebAPI`.</span></span> <span data-ttu-id="b0062-247">Wenn dies nicht der Fall ist, stellen Sie sicher, dass Sie den Registrierungsvorgang abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="b0062-247">If not, make sure that you completed the sign up process.</span></span>

4. <span data-ttu-id="b0062-248">Klicken Sie auf die Surveys-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b0062-248">Click on the Surveys application.</span></span>

5. <span data-ttu-id="b0062-249">Klicken Sie auf **Benutzer und Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-249">Click **Users and Groups**.</span></span>

6. <span data-ttu-id="b0062-250">Klicken Sie auf **Benutzer hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-250">Click **Add user**.</span></span>

7. <span data-ttu-id="b0062-251">Wenn Sie über Azure AD Premium verfügen, klicken Sie auf **Benutzer und Gruppen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-251">If you have Azure AD Premium, click **Users and groups**.</span></span> <span data-ttu-id="b0062-252">Klicken Sie andernfalls auf **Benutzer**.</span><span class="sxs-lookup"><span data-stu-id="b0062-252">Otherwise, click **Users**.</span></span> <span data-ttu-id="b0062-253">(Zum Zuweisen einer Rolle zu einer Gruppe ist Azure AD Premium erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="b0062-253">(Assigning a role to a group requires Azure AD Premium.)</span></span>

8. <span data-ttu-id="b0062-254">Wählen Sie einen oder mehrere Benutzer aus, und klicken Sie auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-254">Select one or more users and click **Select**.</span></span>

    ![Benutzer oder Gruppe auswählen](./images/running-the-app/select-user-or-group.png)

9. <span data-ttu-id="b0062-256">Wählen Sie die Rolle aus, und klicken Sie auf **Auswählen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-256">Select the role and click **Select**.</span></span>

    ![Benutzer oder Gruppe auswählen](./images/running-the-app/select-role.png)

10. <span data-ttu-id="b0062-258">Klicken Sie auf **Zuweisen**.</span><span class="sxs-lookup"><span data-stu-id="b0062-258">Click **Assign**.</span></span>

<span data-ttu-id="b0062-259">Wiederholen Sie die gleichen Schritte, um Rollen für die Survey.WebAPI-Anwendung zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="b0062-259">Repeat the same steps to assign roles for the Survey.WebAPI application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0062-260">Ein Benutzer muss in der Survey-Anwendung und der Survey.WebAPI-Anwendung immer über die gleichen Rollen verfügen.</span><span class="sxs-lookup"><span data-stu-id="b0062-260">A user should always have the same roles in both Survey and Survey.WebAPI.</span></span> <span data-ttu-id="b0062-261">Andernfalls verfügt der Benutzer über nicht übereinstimmende Berechtigungen, was zur Ausgabe des Fehlers 403 (Verboten) durch die Web-API führen kann.</span><span class="sxs-lookup"><span data-stu-id="b0062-261">Otherwise, the user will have inconsistent permissions, which may lead to 403 (Forbidden) errors from the Web API.</span></span>

<span data-ttu-id="b0062-262">Kehren Sie jetzt zur App zurück, und melden Sie sich erneut an.</span><span class="sxs-lookup"><span data-stu-id="b0062-262">Now go back to the app and sign in again.</span></span> <span data-ttu-id="b0062-263">Klicken Sie auf **My Surveys** (Meine Umfragen).</span><span class="sxs-lookup"><span data-stu-id="b0062-263">Click **My Surveys**.</span></span> <span data-ttu-id="b0062-264">Wenn der Benutzer der Rolle SurveyAdmin oder SurveyCreator zugewiesen ist, wird die Schaltfläche **Create Survey** (Umfrage erstellen) angezeigt. Dies bedeutet, dass der Benutzer über Berechtigungen zum Erstellen einer neuen Umfrage verfügt.</span><span class="sxs-lookup"><span data-stu-id="b0062-264">If the user is assigned to the SurveyAdmin or SurveyCreator role, you will see a **Create Survey** button, indicating that the user has permissions to create a new survey.</span></span>

![My Surveys (Meine Umfragen)](./images/running-the-app/screenshot3.png)

<!-- links -->

[portal]: https://portal.azure.com
[VS2017]: https://www.visualstudio.com/vs/
