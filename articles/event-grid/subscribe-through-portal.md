---
title: 通过门户创建 Azure 事件网格订阅
description: 本文介绍如何使用 Azure 门户为支持的源（如 Azure Blob 存储）创建事件网格订阅。
author: Johnnytechn
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: v-johya
ms.openlocfilehash: e7c90b16453956bd642b51c8d97e07fd9d354e37
ms.sourcegitcommit: 6f66215d61c6c4ee3f2713a796e074f69934ba98
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92128161"
---
# <a name="subscribe-to-events-through-portal"></a>通过门户订阅事件

本文介绍了如何通过门户创建事件网格订阅。

## <a name="create-event-subscriptions"></a>创建事件订阅

若要针对任何受支持的[事件源](overview.md#event-sources)创建事件网格订阅，请使用以下步骤。 本文展示了如何为 Azure 订阅创建事件网格订阅。

1. 选择“所有服务”  。

   ![选择所有服务](./media/subscribe-through-portal/select-all-services.png)

1. 搜索“事件网格订阅”  ，并从可用选项中选择它。

   ![屏幕截图显示了 Azure 门户中的“搜索”，其中已选择“事件网格订阅”。](./media/subscribe-through-portal/search.png)

1. 选择“+ 事件订阅”。 

   ![添加订阅](./media/subscribe-through-portal/add-subscription.png)

1. 选择要创建的订阅的类型。 例如，若要订阅你的 Azure 订阅的事件，请选择“Azure 订阅”  ，然后选择目标订阅。

   ![选择 Azure 订阅](./media/subscribe-through-portal/azure-subscription.png)

1. 为此订阅选择事件类型。

   ![选择事件类型](./media/subscribe-through-portal/select-event-types.png)
<!--Not available in MC: **Subscribe to all event types** option checkbox-->

1. 提供关于事件订阅的其他详细信息，例如，用于处理事件的终结点以及订阅名称。

   ![此屏幕截图显示了“终结点详细信息”和“事件订阅详细信息”部分，其中已输入订阅名称值。](./media/subscribe-through-portal/provide-subscription-details.png)

1. 若要启用死信和自定义重试策略，请选择“其他功能”  。

   ![选择其他功能](./media/subscribe-through-portal/select-additional-features.png)

1. 选择用于存储未传递事件的容器，并设置重试的发送方式。

   ![启用死信并重试](./media/subscribe-through-portal/set-deadletter-retry.png)

1. 完成后，选择“创建”。 

## <a name="create-subscription-on-resource"></a>创建关于资源的订阅

某些事件源支持通过该资源的门户界面创建事件订阅。 选择事件源，并在左窗格中查找“事件”  。

![提供订阅详细信息](./media/subscribe-through-portal/resource-events.png)

门户提供了用于创建与该源相关的事件订阅的选项。

## <a name="next-steps"></a>后续步骤

* 有关事件传送和重试的信息，请参阅[事件网格消息传送和重试](delivery-and-retry.md)。
* 有关事件网格的介绍，请参阅[关于事件网格](overview.md)。
* 若要快速开始使用事件网格，请参阅[使用 Azure 事件网格创建和路由自定义事件](custom-event-quickstart.md)。

