---
title: Semantische Modellierung
description: 
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: 343d17af0d933d515c724a062237c8d5df3a9e31
ms.sourcegitcommit: 29fbcb1eec44802d2c01b6d3bcf7d7bd0bae65fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="semantic-modeling"></a><span data-ttu-id="8bc50-102">Semantische Modellierung</span><span class="sxs-lookup"><span data-stu-id="8bc50-102">Semantic modeling</span></span>

<span data-ttu-id="8bc50-103">Ein semantisches Datenmodell ist ein konzeptionelles Modell, mit dem die Bedeutung der darin enthaltenen Datenelemente beschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="8bc50-103">A semantic data model is a conceptual model that describes the meaning of the data elements it contains.</span></span> <span data-ttu-id="8bc50-104">In Organisationen werden häufig eigene Begriffe verwendet, bei denen es sich um Synonyme handeln kann oder die für denselben Begriff unterschiedliche Bedeutungen haben.</span><span class="sxs-lookup"><span data-stu-id="8bc50-104">Organizations often have their own terms for things, sometimes with synonyms, or even different meanings for the same term.</span></span> <span data-ttu-id="8bc50-105">Beispiel: In einer Bestandsdatenbank wird ein Ausrüstungsteil anhand einer Asset-ID und einer Seriennummer nachverfolgt, während in einer Vertriebsdatenbank die Seriennummer als Asset-ID bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="8bc50-105">For example, an inventory database might track a piece of equipment with an asset ID and a serial number, but a sales database might refer to the serial number as the asset ID.</span></span> <span data-ttu-id="8bc50-106">Es gibt keine einfache Möglichkeit, diese Werte miteinander in Beziehung zu setzen, ohne dass ein Modell verwendet wird, mit dem die Beziehung beschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="8bc50-106">There is no simple way to relate these values without a model that describes the relationship.</span></span> 

<span data-ttu-id="8bc50-107">Die semantische Modellierung ermöglicht ein bestimmtes Maß an Abstraktion für das Datenbankschema, damit Benutzer nicht über die zugrunde liegenden Datenstrukturen informiert sein müssen.</span><span class="sxs-lookup"><span data-stu-id="8bc50-107">Semantic modeling provides a level of abstraction over the database schema, so that users don't need to know the underlying data structures.</span></span> <span data-ttu-id="8bc50-108">Dies erleichtert Endbenutzern das Abfragen von Daten ohne Durchführung von Aggregierungs- und Verknüpfungsvorgängen für das zugrunde liegende Schema.</span><span class="sxs-lookup"><span data-stu-id="8bc50-108">This makes it easier for end users to query data without performing aggregates and joins over the underlying schema.</span></span> <span data-ttu-id="8bc50-109">Außerdem erhalten Spalten normalerweise benutzerfreundlichere Namen, damit der Kontext und die Bedeutung der Daten besser erkennbar ist.</span><span class="sxs-lookup"><span data-stu-id="8bc50-109">Also, usually columns are renamed to more user-friendly names, so that the context and meaning of the data are more obvious.</span></span>

<span data-ttu-id="8bc50-110">Die semantische Modellierung wird hauptsächlich für Szenarien mit hohem Leseaufwand verwendet, z.B. Analytics und Business Intelligence (OLAP), und nicht für die transaktionale Datenverarbeitung (OLTP) mit hohem Schreibaufwand.</span><span class="sxs-lookup"><span data-stu-id="8bc50-110">Semantic modeling is predominately used for read-heavy scenarios, such as analytics and business intelligence (OLAP), as opposed to more write-heavy transactional data processing (OLTP).</span></span> <span data-ttu-id="8bc50-111">Dies liegt vor allem an der Art einer typischen semantischen Ebene:</span><span class="sxs-lookup"><span data-stu-id="8bc50-111">This is mostly due to the nature of a typical semantic layer:</span></span>

- <span data-ttu-id="8bc50-112">Das Aggregierungsverhalten ist so festgelegt, dass es von Tools für die Berichterstellung richtig angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="8bc50-112">Aggregation behaviors are set so that reporting tools display them properly.</span></span>
- <span data-ttu-id="8bc50-113">Die Geschäftslogik und die Berechnungen sind definiert.</span><span class="sxs-lookup"><span data-stu-id="8bc50-113">Business logic and calculations are defined.</span></span>
- <span data-ttu-id="8bc50-114">Zeitabhängige Berechnungen sind enthalten.</span><span class="sxs-lookup"><span data-stu-id="8bc50-114">Time-oriented calculations are included.</span></span>
- <span data-ttu-id="8bc50-115">Daten werden häufig aus mehreren Quellen integriert.</span><span class="sxs-lookup"><span data-stu-id="8bc50-115">Data is often integrated from multiple sources.</span></span> 

<span data-ttu-id="8bc50-116">Aus diesen Gründen wird die semantische Ebene meist über einem Data Warehouse angeordnet.</span><span class="sxs-lookup"><span data-stu-id="8bc50-116">Traditionally, the semantic layer is placed over a data warehouse for these reasons.</span></span>

![Beispieldiagramm mit einer semantischen Ebene zwischen einem Data Warehouse und einem Berichterstellungstool](./images/semantic-modeling.png)

<span data-ttu-id="8bc50-118">Es gibt zwei Hauptarten von Semantikmodellen:</span><span class="sxs-lookup"><span data-stu-id="8bc50-118">There are two primary types of semantic models:</span></span>

* <span data-ttu-id="8bc50-119">**Tabellarisch**:</span><span class="sxs-lookup"><span data-stu-id="8bc50-119">**Tabular**.</span></span> <span data-ttu-id="8bc50-120">Hierfür werden Konstrukte der relationalen Modellierung verwendet (Modell, Tabellen, Spalten).</span><span class="sxs-lookup"><span data-stu-id="8bc50-120">Uses relational modeling constructs (model, tables, columns).</span></span> <span data-ttu-id="8bc50-121">Intern werden Metadaten von Konstrukten der OLAP-Modellierung geerbt (Cubes, Dimensionen, Measures).</span><span class="sxs-lookup"><span data-stu-id="8bc50-121">Internally, metadata is inherited from OLAP modeling constructs (cubes, dimensions, measures).</span></span> <span data-ttu-id="8bc50-122">Für den Code und Skripts werden OLAP-Metadaten genutzt.</span><span class="sxs-lookup"><span data-stu-id="8bc50-122">Code and script use OLAP metadata.</span></span>
* <span data-ttu-id="8bc50-123">**Mehrdimensional**:</span><span class="sxs-lookup"><span data-stu-id="8bc50-123">**Multidimensional**.</span></span> <span data-ttu-id="8bc50-124">Hierfür werden herkömmliche Konstrukte der OLAP-Modellierung verwendet (Cubes, Dimensionen, Measures).</span><span class="sxs-lookup"><span data-stu-id="8bc50-124">Uses traditional OLAP modeling constructs (cubes, dimensions, measures).</span></span>

<span data-ttu-id="8bc50-125">In Frage kommender Azure-Dienst:</span><span class="sxs-lookup"><span data-stu-id="8bc50-125">Relevant Azure service:</span></span>
- [<span data-ttu-id="8bc50-126">Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="8bc50-126">Azure Analysis Services</span></span>](https://azure.microsoft.com/services/analysis-services/)

## <a name="example-use-case"></a><span data-ttu-id="8bc50-127">Beispiel eines Anwendungsfalls</span><span class="sxs-lookup"><span data-stu-id="8bc50-127">Example use case</span></span>

<span data-ttu-id="8bc50-128">In einer Organisation werden Daten in einer großen Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8bc50-128">An organization has data stored in a large database.</span></span> <span data-ttu-id="8bc50-129">Diese Daten sollen für geschäftliche Benutzer und Kunden verfügbar gemacht werden, damit diese eigene Berichte erstellen und Analysen durchführen können.</span><span class="sxs-lookup"><span data-stu-id="8bc50-129">It wants to make this data available to business users and customers to create their own reports and do some analysis.</span></span> <span data-ttu-id="8bc50-130">Eine Option besteht darin, diesen Benutzern direkten Zugriff auf die Datenbank zu gewähren.</span><span class="sxs-lookup"><span data-stu-id="8bc50-130">One option is just to give those users direct access to the database.</span></span> <span data-ttu-id="8bc50-131">Dieser Ansatz ist aber mit mehreren Nachteilen verbunden, z.B. dem Aufwand für die Verwaltung der Sicherheit und Steuerung des Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="8bc50-131">However, there are several drawbacks to doing this, including managing security and controlling access.</span></span> <span data-ttu-id="8bc50-132">Außerdem kann das Design der Datenbank, z.B. Namen von Tabellen und Spalten, für Benutzer schwer verständlich sein.</span><span class="sxs-lookup"><span data-stu-id="8bc50-132">Also, the design of the database, including the names of tables and columns, may be hard for a user to understand.</span></span> <span data-ttu-id="8bc50-133">Benutzer müssen wissen, welche Tabellen abgefragt werden sollen, wie diese Tabellen verknüpft werden müssen und wie es sich mit der weiteren Geschäftslogik verhält, die angewendet werden muss, um die richtigen Ergebnisse zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="8bc50-133">Users would need to know which tables to query, how those tables should be joined, and other business logic that must be applied to get the correct results.</span></span> <span data-ttu-id="8bc50-134">Benutzer müssen sich zudem mit einer Abfragesprache, z.B. SQL, auskennen, um beginnen zu können.</span><span class="sxs-lookup"><span data-stu-id="8bc50-134">Users would also need to know a query language like SQL even to get started.</span></span> <span data-ttu-id="8bc50-135">Normalerweise führt dies dazu, dass mehrere Benutzer die gleichen Metriken melden, aber mit unterschiedlichen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="8bc50-135">Typically this leads to multiple users reporting the same metrics but with different results.</span></span>

<span data-ttu-id="8bc50-136">Eine andere Option besteht darin, alle Informationen, die Benutzer benötigen, in einem Semantikmodell zusammenzufassen.</span><span class="sxs-lookup"><span data-stu-id="8bc50-136">Another option is to encapsulate all of the information that users need into a semantic model.</span></span> <span data-ttu-id="8bc50-137">Das Semantikmodell kann von Benutzern mit einem Berichterstellungstool ihrer Wahl einfacher abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="8bc50-137">The semantic model can be more easily queried by users with a reporting tool of their choice.</span></span> <span data-ttu-id="8bc50-138">Die vom Semantikmodell bereitgestellten Daten werden per Pullvorgang aus einem Data Warehouse abgerufen, um sicherzustellen, dass alle Benutzer die einzige und richtige Version angezeigt bekommen.</span><span class="sxs-lookup"><span data-stu-id="8bc50-138">The data provided by the semantic model is pulled from a data warehouse, ensuring that all users see a single version of the truth.</span></span> <span data-ttu-id="8bc50-139">Darüber hinaus werden über das Semantikmodell auch Anzeigenamen für Tabellen und Spalten, Beziehungen zwischen Tabellen, Beschreibungen, Berechnungen und die Sicherheit auf Zeilenebene bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8bc50-139">The semantic model also provides friendly table and column names, relationships between tables, descriptions, calculations, and row-level security.</span></span>

## <a name="typical-traits-of-semantic-modeling"></a><span data-ttu-id="8bc50-140">Typische Merkmale der semantischen Modellierung</span><span class="sxs-lookup"><span data-stu-id="8bc50-140">Typical traits of semantic modeling</span></span>

<span data-ttu-id="8bc50-141">Für die semantische Modellierung und analytische Verarbeitung gelten in der Regel die folgenden Merkmale:</span><span class="sxs-lookup"><span data-stu-id="8bc50-141">Semantic modeling and analytical processing tends to have the following traits:</span></span>

| <span data-ttu-id="8bc50-142">Anforderung</span><span class="sxs-lookup"><span data-stu-id="8bc50-142">Requirement</span></span> | <span data-ttu-id="8bc50-143">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="8bc50-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8bc50-144">Schema</span><span class="sxs-lookup"><span data-stu-id="8bc50-144">Schema</span></span> | <span data-ttu-id="8bc50-145">Schema beim Schreiben, strikte Erzwingung</span><span class="sxs-lookup"><span data-stu-id="8bc50-145">Schema on write, strongly enforced</span></span>|
| <span data-ttu-id="8bc50-146">Nutzung von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="8bc50-146">Uses Transactions</span></span> | <span data-ttu-id="8bc50-147">Nein </span><span class="sxs-lookup"><span data-stu-id="8bc50-147">No</span></span> |
| <span data-ttu-id="8bc50-148">Sperrstrategie</span><span class="sxs-lookup"><span data-stu-id="8bc50-148">Locking Strategy</span></span> | <span data-ttu-id="8bc50-149">Keine</span><span class="sxs-lookup"><span data-stu-id="8bc50-149">None</span></span> |
| <span data-ttu-id="8bc50-150">Aktualisierbar</span><span class="sxs-lookup"><span data-stu-id="8bc50-150">Updateable</span></span> | <span data-ttu-id="8bc50-151">Nein (normalerweise Neuberechnung des Cubes erforderlich)</span><span class="sxs-lookup"><span data-stu-id="8bc50-151">No (typically requires recomputing cube)</span></span> |
| <span data-ttu-id="8bc50-152">Erweiterbar</span><span class="sxs-lookup"><span data-stu-id="8bc50-152">Appendable</span></span> | <span data-ttu-id="8bc50-153">Nein (normalerweise Neuberechnung des Cubes erforderlich)</span><span class="sxs-lookup"><span data-stu-id="8bc50-153">No (typically requires recomputing cube)</span></span> |
| <span data-ttu-id="8bc50-154">Workload</span><span class="sxs-lookup"><span data-stu-id="8bc50-154">Workload</span></span> | <span data-ttu-id="8bc50-155">Hoher Leseaufwand, schreibgeschützt</span><span class="sxs-lookup"><span data-stu-id="8bc50-155">Heavy reads, read-only</span></span> |
| <span data-ttu-id="8bc50-156">Indizierung</span><span class="sxs-lookup"><span data-stu-id="8bc50-156">Indexing</span></span> | <span data-ttu-id="8bc50-157">Mehrdimensionale Indizierung</span><span class="sxs-lookup"><span data-stu-id="8bc50-157">Multidimensional indexing</span></span> |
| <span data-ttu-id="8bc50-158">Bezugsgröße</span><span class="sxs-lookup"><span data-stu-id="8bc50-158">Datum size</span></span> | <span data-ttu-id="8bc50-159">Kleine bis mittlere Größe</span><span class="sxs-lookup"><span data-stu-id="8bc50-159">Small to medium sized</span></span> |
| <span data-ttu-id="8bc50-160">Modell</span><span class="sxs-lookup"><span data-stu-id="8bc50-160">Model</span></span> | <span data-ttu-id="8bc50-161">Mehrdimensional</span><span class="sxs-lookup"><span data-stu-id="8bc50-161">Multidimensional</span></span> |
| <span data-ttu-id="8bc50-162">Datenform:</span><span class="sxs-lookup"><span data-stu-id="8bc50-162">Data shape:</span></span>| <span data-ttu-id="8bc50-163">Cube oder Stern/Schneeflockenschema</span><span class="sxs-lookup"><span data-stu-id="8bc50-163">Cube or star/snowflake schema</span></span> |
| <span data-ttu-id="8bc50-164">Abfrageflexibilität</span><span class="sxs-lookup"><span data-stu-id="8bc50-164">Query flexibility</span></span> | <span data-ttu-id="8bc50-165">Sehr flexibel</span><span class="sxs-lookup"><span data-stu-id="8bc50-165">Highly flexible</span></span> |
| <span data-ttu-id="8bc50-166">Skalierung:</span><span class="sxs-lookup"><span data-stu-id="8bc50-166">Scale:</span></span> | <span data-ttu-id="8bc50-167">Groß (Dutzende bis mehrere Hundert GB)</span><span class="sxs-lookup"><span data-stu-id="8bc50-167">Large (10s-100s GBs)</span></span> |

## <a name="see-also"></a><span data-ttu-id="8bc50-168">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8bc50-168">See also</span></span>

- [<span data-ttu-id="8bc50-169">Data Warehousing</span><span class="sxs-lookup"><span data-stu-id="8bc50-169">Data warehousing</span></span>](../scenarios/data-warehousing.md)
- [<span data-ttu-id="8bc50-170">Analytische Onlineverarbeitung (OLAP)</span><span class="sxs-lookup"><span data-stu-id="8bc50-170">Online analytical processing (OLAP)</span></span>](../scenarios/online-analytical-processing.md)