---
title: Azure 网络体系结构
description: 本文提供有关 Azure 基础结构网络的一般说明。
services: security
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2020
ms.author: v-johya
origin.date: 02/20/2019
ms.openlocfilehash: 1674469ccdbbb844fe4e482b2db8309d48d286d0
ms.sourcegitcommit: 6f66215d61c6c4ee3f2713a796e074f69934ba98
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92127910"
---
# <a name="azure-network-architecture"></a>Azure 网络体系结构
Azure 网络体系结构提供从 Internet 到 Azure 数据中心的连接。 在 Azure 上部署的工作负载（IaaS、PaaS 和 SaaS）都在使用 Azure 数据中心网络。

## <a name="network-topology"></a>网络拓扑
Azure 数据中心的网络体系结构包含以下组件：

- 边缘网络
- 广域网
- 区域网关网络
- 数据中心网络

![Azure 网络示意图](./media/infrastructure-network/network-arch.png)

## <a name="network-components"></a>网络组件
网络组件的简短说明。

- 边缘网络

   - Microsoft 网络和其他网络（例如 Internet、企业网络）之间的分界点
   - 向 Azure 提供 Internet 和 [ExpressRoute](../../expressroute/expressroute-introduction.md) 对等互连

- 广域网

   - 覆盖全球的 Microsoft 智能主干网络
   - 在 [Azure 区域](https://azure.microsoft.com/global-infrastructure/geographies/)之间提供连接

- 区域网关

   - Azure 区域中所有数据中心的聚合点
   - 在 Azure 区域内的数据中心之间提供大规模连接（例如，每个数据中心数百 TB）

- 数据中心网络

   - 提供数据中心内服务器之间的连接，确保较低的超额订阅带宽

上述网络组件旨在提供最大的可用性，以支持永远在线、始终可用的云业务。 从物理方面一直到控制协议，网络中都设计并内置了冗余。

## <a name="datacenter-network-resiliency"></a>数据中心网络复原能力
下面讲解使用数据中心网络的复原能力设计原则。

数据中心网络是 [CLOS 网络](https://en.wikipedia.org/wiki/Clos_network)的修订版本，可为云规模流量提供较高的双向带宽。 网络使用大量的商用设备构造，以减少单个硬件故障造成的影响。 这些设备战略性地分处不同的物理位置，具有独立的电源和冷却域，可减少环境事件的影响。  在控制平面上，所有网络设备都以 OSI 模型第 3 层路由模式运行，从而消除流量循环的历史问题。 不同层之间的所有路径都处于活动状态，以便使用相等成本多路径 (ECMP) 路由提供高冗余和带宽。

下图演示了数据中心网络由不同的网络设备层进行构造的情况。 图中的条形表示提供冗余和高带宽连接性的网络设备组。

![数据中心网络](./media/infrastructure-network/datacenter-network.png)

## <a name="next-steps"></a>后续步骤
若要详细了解 Microsoft 如何帮助保护 Azure 基础结构，请参阅：

- [Azure 设施、场地和物理安全性](physical-security.md)
- [Azure 基础结构可用性](infrastructure-availability.md)
- [Azure 信息系统的组件和边界](infrastructure-components.md)
- [Azure 生产网络](production-network.md)
- [Azure SQL 数据库安全功能](infrastructure-sql.md)
- [Azure 生产运营和管理](infrastructure-operations.md)
- [Azure 基础结构监视](infrastructure-monitoring.md)
- [Azure 基础结构完整性](infrastructure-integrity.md)
- [Azure 客户数据保护](protection-customer-data.md)

