---
title: Verbinden eines lokalen Netzwerks mit Azure
titleSuffix: Azure Reference Architectures
description: Implementieren Sie eine hochverfügbare und sichere Site-to-Site-Netzwerkarchitektur, die ein virtuelles Azure-Netzwerk und ein lokales Netzwerk mit ExpressRoute-Verbindung und VPN-Gatewayfailover umfasst.
author: telmosampaio
ms.date: 10/22/2017
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, networking
ms.openlocfilehash: 503320c2ed429c97c6581cd7b48ce328996df6c6
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241361"
---
# <a name="connect-an-on-premises-network-to-azure-using-expressroute-with-vpn-failover"></a>Verbinden eines lokalen Netzwerks mit Azure unter Verwendung von ExpressRoute mit VPN-Failover

Diese Referenzarchitektur zeigt, wie ein lokales Netzwerk unter Verwendung von ExpressRoute mit einem virtuellen Azure-Netzwerk (VNet) verbunden wird, wobei ein Site-to-Site-VPN (Virtual Private Network) als Failoververbindung dient. Der Datenverkehr zwischen dem lokalen Netzwerk und dem Azure-VNet wird über eine ExpressRoute-Verbindung übertragen. Wenn die ExpressRoute-Verbindung ausfällt, wird der Datenverkehr über einen IPSec-VPN-Tunnel weitergeleitet. [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

Beachten Sie, dass die VPN-Route nur Private Peering-Verbindungen behandelt, wenn die ExpressRoute-Verbindung nicht verfügbar ist. Public Peering- und Microsoft-Peering-Verbindungen werden über das Internet geleitet.

![Referenzarchitektur für eine hochverfügbare Hybrid-Netzwerkarchitektur mit ExpressRoute- und VPN-Gateway](./images/expressroute-vpn-failover.png)

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

## <a name="architecture"></a>Architecture

Die Architektur umfasst die folgenden Komponenten.

- **Lokales Netzwerk**. Ein in einer Organisation betriebenes privates lokales Netzwerk.

- **VPN-Gerät**. Ein Gerät oder ein Dienst, das bzw. der externe Konnektivität mit dem lokalen Netzwerk bereitstellt. Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung, z.B. den Routing- und RAS-Dienst unter Windows Server 2012, handeln. Eine Liste der unterstützten VPN-Geräte und Informationen zur Konfiguration ausgewählter VPN-Geräte für die Verbindung mit Azure finden Sie unter [Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].

- **ExpressRoute-Verbindung**. Eine vom Konnektivitätsanbieter bereitgestellte Layer 2- oder Layer 3-Verbindung, die das lokale Netzwerk über die Edgerouter mit Azure verbindet. Für die Verbindung wird die vom Konnektivitätsanbieter verwaltete Hardwareinfrastruktur verwendet.

- **ExpressRoute-Gateway für virtuelle Netzwerke**. Das ExpressRoute-Gateway für virtuelle Netzwerke ermöglicht es dem VNet, mit der ExpressRoute-Leitung eine Verbindung herzustellen, die für die Konnektivität mit Ihrem lokalen Netzwerk verwendet wird.

- **VPN-Gateway für virtuelle Netzwerke**. Die VPN-Gateway für virtuelle Netzwerke ermöglicht es dem VNet, eine Verbindung mit dem VPN-Gerät im lokalen Netzwerk herzustellen. Die VPN-Gateway für virtuelle Netzwerke ist so konfiguriert, dass es Anforderungen aus dem lokalen Netzwerk nur über das VPN-Gerät akzeptiert. Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem Microsoft Azure Virtual Network][connect-to-an-Azure-vnet].

- **VPN-Verbindung**. Die Verbindung verfügt über Eigenschaften, die den Verbindungstyp (IPSec) und den Schlüssel angeben, der für das lokale VPN-Gerät freigegeben wird, um Datenverkehr zu verschlüsseln.

- **Azure Virtual Network (VNet)**. Jedes VNet befindet sich in einer einzelnen Azure-Region und kann mehrere Logikschichten hosten. Logikschichten können mithilfe von Subnetzen in jedem VNet segmentiert werden.

- **Gatewaysubnetz**. Die Gateways für virtuelle Netzwerke befinden sich in demselben Subnetz.

- **Cloudanwendung**. Die in Azure gehostete Anwendung. Sie kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind. Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].

## <a name="recommendations"></a>Empfehlungen

Die folgenden Empfehlungen gelten für die meisten Szenarios. Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.

### <a name="vnet-and-gatewaysubnet"></a>VNet und GatewaySubnet

Erstellen Sie das ExpressRoute-Gateway für virtuelle Netzwerke und das VPN-Gateway für virtuelle Netzwerke in demselben VNet. Dies bedeutet, dass sie das Subnetz mit dem Namen *GatewaySubnet* gemeinsam nutzen sollten.

Wenn das VNet bereits ein Subnetz mit dem Namen *GatewaySubnet* enthält, stellen Sie sicher, dass es einen /27-Adressraum oder einen größeren Adressraum aufweist. Wenn das vorhandene Subnetz zu klein ist, entfernen Sie es mit dem folgenden PowerShell-Befehl:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Wenn das VNET kein Subnetz mit dem Namen **GatewaySubnet** enthält, erstellen Sie mit dem folgenden PowerShell-Befehl ein neues Subnetz:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN- und ExpressRoute-Gateways

Stellen Sie sicher, dass Ihre Organisation die [ExpressRoute-Voraussetzungen][expressroute-prereq] zum Herstellen der Verbindung mit Azure erfüllt.

Falls Sie im Azure-VNET bereits über ein VPN-Gateway für virtuelle Netzwerke verfügen, entfernen Sie es mithilfe des folgenden PowerShell-Befehls:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Befolgen Sie die Anweisungen in [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][implementing-expressroute], um eine sichere ExpressRoute-Verbindung herzustellen.

Befolgen Sie die Anweisungen in [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure und lokalem VPN][implementing-vpn], um eine Verbindung mit dem VPN-Gateway für virtuelle Netzwerke herzustellen.

Nachdem Sie die Verbindung mit dem Gateway für virtuelle Netzwerke hergestellt haben, testen Sie die Umgebung wie folgt:

1. Stellen Sie sicher, dass Sie vom lokalen Netzwerk eine Verbindung mit dem Azure-VNet herstellen können.
2. Wenden Sie sich an Ihren Anbieter, um die ExpressRoute-Konnektivität zu Testzwecken zu beenden.
3. Stellen Sie sicher, dass Sie weiterhin über das VPN-Gateway für virtuelle Verbindungen eine Verbindung vom lokalen Netzwerk mit dem Azure-VNet herstellen können.
4. Wenden Sie sich an Ihren Anbieter, um die ExpressRoute-Konnektivität wiederherzustellen.

## <a name="considerations"></a>Überlegungen

Überlegungen zu ExpressRoute finden Sie im Leitfaden [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][guidance-expressroute].

Überlegungen zu Site-to-Site-VPNs finden Sie im Leitfaden [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Azure und lokalem VPN][guidance-vpn].

Informationen zu allgemeinen Sicherheitsaspekten für Azure finden Sie unter [Microsoft-Clouddienste und Netzwerksicherheit][best-practices-security].

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

**Voraussetzungen** Sie müssen über eine lokale Infrastruktur verfügen, die bereits mit einer geeigneten Netzwerkappliance konfiguriert ist.

Führen Sie die folgenden Schritte aus, um die Lösung bereitzustellen.

<!-- markdownlint-disable MD033 -->

1. Klicken Sie auf diese Schaltfläche:<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fhybrid-networking%2Fexpressroute-vpn-failover%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

2. Warten Sie, bis der Link im Azure-Portal geöffnet wird, und gehen Sie dann folgendermaßen vor:
   - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert. Wählen Sie also **Neu erstellen**, und geben Sie im Textfeld `ra-hybrid-vpn-er-rg` ein.
   - Wählen Sie im Dropdownfeld **Standort** die Region aus.
   - Lassen Sie die Textfelder für den **Vorlagenstamm-URI** bzw. **Parameterstamm-URI** unverändert.
   - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   - Klicken Sie auf die Schaltfläche **Kaufen**.

3. Warten Sie, bis die Bereitstellung abgeschlossen ist.

4. Klicken Sie auf diese Schaltfläche:<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fhybrid-networking%2Fexpressroute-vpn-failover%2Fazuredeploy-expressRouteCircuit.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

5. Warten Sie, bis der Link im Azure-Portal geöffnet wird, und gehen Sie dann folgendermaßen vor:
   - Wählen Sie im Abschnitt **Ressourcengruppe** die Option **Vorhandene verwenden** aus, und geben Sie im Textfeld `ra-hybrid-vpn-er-rg` ein.
   - Wählen Sie im Dropdownfeld **Standort** die Region aus.
   - Lassen Sie die Textfelder für den **Vorlagenstamm-URI** bzw. **Parameterstamm-URI** unverändert.
   - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   - Klicken Sie auf die Schaltfläche **Kaufen**.

<!-- markdownlint-enable MD033 -->

<!-- links -->

[windows-vm-ra]: ../virtual-machines-windows/index.md
[linux-vm-ra]: ../virtual-machines-linux/index.md
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[expressroute-prereq]: /azure/expressroute/expressroute-prerequisites
[implementing-expressroute]: ./expressroute.md
[implementing-vpn]: ./vpn.md
[guidance-expressroute]: ./expressroute.md
[guidance-vpn]: ./vpn.md
[best-practices-security]: /azure/best-practices-network-security
[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-architectures.vsdx
