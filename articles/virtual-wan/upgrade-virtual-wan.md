---
title: 将 Azure 虚拟 WAN 从基本层级升级到标准层级 - Azure 门户 | Azure
description: 你可以升级虚拟 WAN 类型以获得更多功能。
services: virtual-wan
ms.service: virtual-wan
ms.topic: how-to
origin.date: 11/04/2019
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: yes
ms.testdate: 11/02/2020
ms.author: v-yeche
ms.openlocfilehash: c28b7f76d231236007379992c7f644b295019b86
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93104300"
---
<!--Verified successfully-->
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>将虚拟 WAN 从基本层级升级到标准层级

本文可帮助你将基本 WAN 升级到标准 WAN。 “基本”WAN 类型将它中的所有中心都创建为基本 SKU 中心。 在基本中心内，只能使用站点到站点 VPN 功能。 “标准”WAN 类型将它中的所有中心都创建为标准 SKU 中心。 使用标准中心时，可通过 Azure 中心启用 ExpressRoute、用户（点到站点）VPN、完整网格中心和 VNet 到 VNet 传输。

下表显示了每种 WAN 类型的可用配置：

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>更改虚拟 WAN 类型

1. 在虚拟 WAN 的页面上，选择“配置”  以打开“配置”页面。

    ![此屏幕截图显示了“配置”页，在底部突出显示了有关如何升级到标准类型虚拟 WAN 的信息文本框。](./media/upgrade-virtual-wan/1.png)
2. 对于“虚拟 WAN 类型”，请从下拉列表中选择“标准”。

    ![此屏幕截图显示了“虚拟 WAN 类型”下拉菜单。](./media/upgrade-virtual-wan/2.png)
3. 请注意，如果升级到标准虚拟 WAN，将无法还原到基本虚拟 WAN。 如果要升级，请选择“确认”  。

    ![显示“升级确认”对话框的屏幕截图。](./media/upgrade-virtual-wan/4.png)
4. 保存更改后，你的虚拟 WAN 页面将与此示例类似。

    :::image type="content" source="./media/upgrade-virtual-wan/5.png" alt-text="虚拟 WAN 示意图":::

## <a name="next-steps"></a>后续步骤

若要详细了解虚拟 WAN，请参阅[虚拟 WAN 概述](virtual-wan-about.md)页。

<!-- Update_Description: update meta properties, wording update, update link -->