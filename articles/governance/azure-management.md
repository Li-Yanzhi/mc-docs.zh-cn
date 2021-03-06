---
title: Azure 管理概述 - Azure 治理
description: Azure 应用程序和资源管理领域概述及 Azure 管理工具上内容的链接。
origin.date: 09/09/2020
ms.date: 11/06/2020
ms.author: v-tawe
ms.topic: overview
ms.openlocfilehash: 988c04460109975698ce6075779c5bc2c6db0315
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328764"
---
# <a name="what-are-the-azure-management-areas"></a>什么是 Azure 管理区域？

Azure 中的监管是 Azure 管理的一个方面。 本文介绍了在 Azure 中部署和维护资源的不同管理领域。

管理指的是维护业务应用程序以及为其提供支持的资源所需的任务和流程。 Azure 有许多服务和工具可以协同工作以提供完整的管理。 这些服务不仅适用于 Azure 中的资源，还适用于其他云和本地服务。 了解不同的工具以及它们如何协同工作是设计完整管理环境的第一步。

下图说明了维护任何应用程序或资源所需的不同管理方面。 可将这些不同的区域视为一个生命周期。 每个区域都需要在资源的整个生存期内保持连续。 此资源生命周期始于其初始部署，贯穿其持续操作，在其停用时结束。

:::image type="complex" source="../monitoring/media/management-overview/management-capabilities.png" alt-text="“Azure 中的管理原则”示意图。" border="false":::
   此图显示了在 Azure 中支持管理和治理的服务轮的“迁移”、“安全”、“保护”、“监视”、“配置”和“治理”元素。 “安全”有“安全管理”和“威胁防护”作为子项。 “保护”有“备份”和“灾难恢复”作为子项。 “监视”有“应用、基础结构和网络监视”以及“日志分析和诊断”作为子项。 “配置”有“配置”、“更新管理”、“自动化”和“脚本编写”作为子项。 “治理”有“策略管理”和“成本管理”作为子项。
:::image-end:::

没有一个 Azure 服务完全满足特定管理区域的要求。 但搭配多个服务就能实现这一点。 某些服务（如 Application Insight）可为 Web 应用程序提供有针对性的监视功能。 其他服务（例如 Azure Monitor 日志）可为其他服务存储管理数据。 可使用此功能分析由不同服务收集的不同类型的数据。

下列部分简要介绍了不同的管理领域，并提供了用于处理这些领域的主要 Azure 服务的详细内容链接。

## <a name="monitor"></a>监视

监视是一种数据收集和分析操作，用于审核资源的性能、运行状况及可用性。 有效的监视策略有助于了解组件的运行情况，并通过通知延长正常运行时间。 请阅读监视概述，了解[监视 Azure 应用程序和资源](../azure-monitor/overview.md)中使用的不同服务。

## <a name="configure"></a>配置

配置是指资源的初始部署和配置以及持续维护。
自动执行这些任务，可以消除冗余，最大限度地节省时间和工作量，以及提高准确性和效率。 [Azure 自动化](../automation/automation-intro.md)提供了大量用于自动执行配置任务的服务。 而 Runbook 可处理流程自动化、配置和更新管理，帮助管理配置。

## <a name="govern"></a>治理

<!-- /cost-management/ not available -->

“治理”提供了机制和流程来保持对 Azure 中的应用程序和资源的控制。 它涉及规划计划和设置战略优先级。
Azure 中的治理主要是通过两个服务实现的。 [Azure Policy ](./policy/overview.md)允许你创建、分配和管理策略定义，以强制执行资源规则。
此功能可使这些资源符合企业标准。

## <a name="secure"></a>安全

管理资源和数据的安全性。 安全计划涉及评估威胁、收集和分析数据以及应用程序和资源的符合性。 安全监视和威胁分析由 [Azure 安全中心](../security-center/security-center-intro.md)提供，该中心包括跨混合云工作负荷的统一安全管理和高级威胁防护。 请参阅 [Azure 安全性简介](../security/fundamentals/overview.md)的全面信息，以及有关保护 Azure 资源的指南。

## <a name="protect"></a>保护

保护是指保持应用程序和数据可用，即使是超出控制范围的中断也是如此。 Azure 中的保护由两个服务提供。 [Azure 备份](../backup/backup-overview.md)提供数据备份和恢复（在云中或本地）。 [Azure Site Recovery](../site-recovery/site-recovery-overview.md) 可在发生灾难期间提供业务连续性和即时恢复。

<!-- Migrate Service is not available in mc -->
<!-- >
## Migrate

Migration refers to transitioning workloads currently running on-premises to the Azure cloud.
[Azure Migrate](../migrate/migrate-services-overview.md) is a service that helps you assess the migration
suitability of on-premises virtual machines to Azure. Azure Site Recovery migrates virtual machines
[from on-premises](../site-recovery/migrate-tutorial-on-premises-azure.md) or [from Amazon Web
Services](../site-recovery/migrate-tutorial-aws-azure.md). [Azure Database
Migration](../dms/dms-overview.md) assists you in migrating database sources to Azure Data
platforms.
-->

<!--
## Next Steps

To learn more about Azure Governance, see these articles:

- See the [Azure Governance hub](./index.yml).
- See [Governance in the Cloud Adoption Framework for Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/)
-->
