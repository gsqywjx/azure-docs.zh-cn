---
title: "创建面向 Internet 的负载均衡器 - Azure 门户（经典）| Microsoft 文档"
description: "了解如何使用 Azure 经典门户在经典部署模型中创建面向 Internet 的负载均衡器"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e07b6808f2401ac7b2b21e5f8816bac5a15b50b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>开始在 Azure 经典门户中创建面向 Internet 的负载均衡器（经典）

> [!div class="op_single_selector"]
> * [Azure 经典门户](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure 云服务](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> 在使用 Azure 资源之前，请务必了解 Azure 当前使用两种部署模型：Azure Resource Manager 部署模型和经典部署模型。 在使用任何 Azure 资源之前，请确保了解 [部署模型和工具](../azure-classic-rm.md) 。 可以通过单击本文顶部的选项卡来查看不同工具的文档。 本文介绍经典部署模型。 还可以[了解如何使用 Azure Resource Manager 创建面向 Internet 的负载均衡器](load-balancer-get-started-internet-arm-ps.md)。

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>为虚拟机设置面向 Internet 的负载均衡器

若要在云服务的虚拟机之间负载均衡来自 Internet 的网络流量，必须创建负载均衡集。 此过程假定已创建虚拟机，并且这些虚拟机全部位于同一云服务中。

**为虚拟机配置负载均衡集**

1. 在 Azure 经典门户中，单击“虚拟机”，并单击负载均衡集中虚拟机的名称。
2. 单击“终结点”，并单击“添加”。
3. 在“将终结点添加到虚拟机”页上，单击右箭头。
4. 在“指定终结点的详细信息”页上执行以下操作：

   * 在“名称”中，键入终结点的名称，或者从常用协议的预定义终结点列表中选择名称。
   * 在“协议”中，根据需要选择终结点类型需要的协议，例如 TCP 或 UDP。
   * 在“公用端口”和“专用端口”中，根据需要键入希望虚拟机使用的端口号。 可以使用虚拟机上的专用端口和防火墙规则，从而以适合应用程序的方式重定向流量。 专用端口可以与公用端口一样。 例如，对于 Web (HTTP) 流量的终结点，可将端口 80 指定为公用端口和专用端口。

5. 选择“创建负载均衡集”，并单击右箭头。
6. 在“配置负载均衡集”页上，键入负载均衡集的名称，并分配用于 Azure 负载均衡器的探测行为的值。 负载均衡器使用探测来确定负载均衡集中的虚拟机是否可用于接收传入流量。
7. 单击复选标记以创建负载均衡的终结点。 将在虚拟机的“终结点”页的“负载均衡集名称”列中看到“是”。
8. 在门户中，依次单击“虚拟机”、负载均衡集中其他虚拟机的名称、“终结点”、“添加”。
9. 在“将终结点添加到虚拟机”页上，单击“将终结点添加到现有负载均衡集”，选择负载均衡集的名称，然后单击右箭头。
10. 在“指定终结点详细信息”页上，键入终结点的名称，并单击复选标记。

对于负载均衡集中的其他虚拟机，重复执行步骤 8 到步骤 10。

## <a name="next-steps"></a>后续步骤

[开始配置内部负载均衡器](load-balancer-get-started-ilb-arm-ps.md)

[配置负载均衡器分发模式](load-balancer-distribution-mode.md)

[配置负载均衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
