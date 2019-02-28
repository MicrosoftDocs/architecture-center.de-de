<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

## <a name="policy-statements"></a><span data-ttu-id="4a454-101">Richtlinienanweisungen</span><span class="sxs-lookup"><span data-stu-id="4a454-101">Policy statements</span></span>

<span data-ttu-id="4a454-102">Die folgenden Richtlinienanweisungen legen die Anforderungen zum Verringern der definierten Risiken fest.</span><span class="sxs-lookup"><span data-stu-id="4a454-102">The following policy statements establish the requirements needed to mitigate the defined risks.</span></span> <span data-ttu-id="4a454-103">Diese Richtlinien definieren die Funktionsanforderungen für das Governance-MVP.</span><span class="sxs-lookup"><span data-stu-id="4a454-103">These policies define the functional requirements for the governance MVP.</span></span> <span data-ttu-id="4a454-104">Jede wird in der Implementierung der Governance-MVP dargestellt.</span><span class="sxs-lookup"><span data-stu-id="4a454-104">Each will be represented in the implementation of the governance MVP.</span></span>

<span data-ttu-id="4a454-105">Beschleunigung der Bereitstellung:</span><span class="sxs-lookup"><span data-stu-id="4a454-105">Deployment Acceleration:</span></span>

- <span data-ttu-id="4a454-106">Alle Ressourcen müssen gruppiert und gemäß der definierten Gruppierungs- und Markierungsstrategien markiert werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-106">All assets must be grouped and tagged according to defined grouping and tagging strategies.</span></span>
- <span data-ttu-id="4a454-107">Alle Ressourcen müssen ein genehmigtes Bereitstellungsmodell verwenden.</span><span class="sxs-lookup"><span data-stu-id="4a454-107">All assets must use an approved deployment model.</span></span>
- <span data-ttu-id="4a454-108">Sobald eine Governancegrundlage für einen Cloudanbieter festgelegt ist, müssen alle Tools für die Bereitstellung mit den Tools kompatibel sein, die vom Governanceteam definiert werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-108">Once a governance foundation has been established for a cloud provider, any deployment tooling must be compatible with the tools defined by the governance team.</span></span>

<span data-ttu-id="4a454-109">Identitätsbaseline:</span><span class="sxs-lookup"><span data-stu-id="4a454-109">Identity Baseline:</span></span>

- <span data-ttu-id="4a454-110">Alle in der Cloud bereitgestellten Ressourcen sollten mit Identitäten und Rollen kontrolliert werden, die von aktuellen Governancerichtlinien genehmigt werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-110">All assets deployed to the cloud should be controlled using identities and roles approved by current governance policies.</span></span>
- <span data-ttu-id="4a454-111">Alle Gruppen in der lokalen Active Directory-Infrastruktur mit erhöhten Rechten sollten einer genehmigten RBAC-Rolle zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-111">All groups in the on-premises Active Directory infrastructure that have elevated privileges should be mapped to an approved RBAC role.</span></span>

<span data-ttu-id="4a454-112">Sicherheitsbaseline:</span><span class="sxs-lookup"><span data-stu-id="4a454-112">Security Baseline:</span></span>

- <span data-ttu-id="4a454-113">Jede in der Cloud bereitgestellte Ressource muss eine genehmigte Datenklassifizierung haben.</span><span class="sxs-lookup"><span data-stu-id="4a454-113">Any asset deployed to the cloud must have an approved data classification.</span></span>
- <span data-ttu-id="4a454-114">Keine Ressourcen, die mit einer geschützten Datenebene identifiziert werden, können in der Cloud bereitgestellt werden, solange nicht genügend Anforderungen für Sicherheit und Governance genehmigt und implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="4a454-114">No assets identified with a protected level of data may be deployed to the cloud, until sufficient requirements for security and governance can be approved and implemented.</span></span>
- <span data-ttu-id="4a454-115">Solange keine minimalen Netzwerksicherheitsanforderungen überprüft und gesteuert werden können, werden Cloudumgebungen als demilitarisierte Zone betrachtet und sollten ähnliche Anforderungen an die Verbindung mit anderen Rechenzentren oder internen Netzwerken erfüllen.</span><span class="sxs-lookup"><span data-stu-id="4a454-115">Until minimum network security requirements can be validated and governed, cloud environments are seen as a demilitarized zone and should meet similar connection requirements to other data centers or internal networks.</span></span>

<span data-ttu-id="4a454-116">Kostenmanagement:</span><span class="sxs-lookup"><span data-stu-id="4a454-116">Cost Management:</span></span>

- <span data-ttu-id="4a454-117">Für die Nachverfolgung müssen alle Ressourcen innerhalb einer der zentralen Geschäftsfunktionen einem Anwendungsbesitzer zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-117">For tracking purposes, all assets must be assigned to an application owner within one of the core business functions.</span></span>
- <span data-ttu-id="4a454-118">Wenn Kostenprobleme auftreten, werden zusätzliche Governanceanforderungen mit dem Finanzteam festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4a454-118">When cost concerns arise, additional governance requirements will be established with the Finance team.</span></span>

<span data-ttu-id="4a454-119">Ressourcenkonsistenz:</span><span class="sxs-lookup"><span data-stu-id="4a454-119">Resource Consistency:</span></span>

- <span data-ttu-id="4a454-120">Da in dieser Phase keine unternehmenskritischen Workloads bereitgestellt werden, müssen keine SLA-, Leistungs- oder BCDR-Anforderungen verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="4a454-120">Because no mission-critical workloads are deployed at this stage, there are no SLA, performance, or BCDR requirements to be governed.</span></span>
- <span data-ttu-id="4a454-121">Wenn unternehmenskritische Workloads bereitgestellt werden, werden zusätzliche Governanceanforderungen mit der IT-Abteilung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4a454-121">When mission-critical workloads are deployed, additional governance requirements will be established with IT operations.</span></span>

## <a name="processes"></a><span data-ttu-id="4a454-122">Prozesse</span><span class="sxs-lookup"><span data-stu-id="4a454-122">Processes</span></span>

<span data-ttu-id="4a454-123">Für die kontinuierliche Überwachung und Durchsetzung dieser Governancerichtlinien wurde kein Budget zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="4a454-123">No budget has been allocated for ongoing monitoring and enforcement of these governance policies.</span></span> <span data-ttu-id="4a454-124">Aus diesem Grund stehen dem Cloudgovernanceteam einige Ad-hoc-Methoden zur Verfügung, um die Einhaltung von Richtlinienanweisungen zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="4a454-124">Because of that, the Cloud Governance team has some ad hoc ways to monitor adherence to policy statements.</span></span>

- <span data-ttu-id="4a454-125">**Bildung**: Das Cloudgovernanceteam investiert Zeit in die Schulung der Cloudeinführungsteams zu den Governance Journeys, die diese Richtlinien unterstützen.</span><span class="sxs-lookup"><span data-stu-id="4a454-125">**Education**: The Cloud Governance team is investing time to educate the cloud adoption teams on the governance journeys that support these policies.</span></span>
- <span data-ttu-id="4a454-126">**Bereitstellungsprüfungen**: Vor der Bereitstellung einer Ressource überprüft das Cloudgovernanceteam die Governance Journey mit den Cloudeinführungsteams.</span><span class="sxs-lookup"><span data-stu-id="4a454-126">**Deployment** reviews: Before deploying any asset, the Cloud Governance team will review the governance journey with the cloud adoption teams.</span></span>