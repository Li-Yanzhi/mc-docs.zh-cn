---
title: 在 Privileged Identity Management 中分配 Azure 资源角色 - Azure Active Directory | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中分配 Azure 资源角色。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/09/2020
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 857b17cd6355689d3ef200433592d9824749e638
ms.sourcegitcommit: 59810f8eba5e430d85a595e346d3b7fb6e4a0102
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94501958"
---
# <a name="assign-azure-resource-roles-in-privileged-identity-management"></a>在 Privileged Identity Management 中分配 Azure 资源角色

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 可以管理内置的 Azure 资源角色以及自定义角色，包括但不限于：

- “所有者”
- 用户访问管理员
- 参与者
- 安全管理员
- 安全管理器

> [!NOTE]
> 默认情况下，分配到“所有者”或“用户访问管理员”订阅角色的用户或组成员以及在 Azure AD 中启用订阅管理的 Azure AD 全局管理员具有资源管理员权限。 这些管理员可以对 Azure 资源使用 Privileged Identity Management 来分配角色、配置角色设置，以及审查访问权限。 如果没有资源管理员权限，用户将无法管理资源的 Privileged Identity Management。 查看 [Azure 内置角色](../../role-based-access-control/built-in-roles.md)列表。

## <a name="assign-a-role"></a>分配角色

遵循以下步骤可使用户符合 Azure 资源角色的条件。

1. 使用“所有者”或“用户访问管理员”角色权限登录到 [Azure 门户](https://portal.azure.cn/)。

    有关如何授予其他管理员访问权限以管理 Privileged Identity Management 的信息，请参阅[授予其他管理员访问权限以管理 Privileged Identity Management](pim-how-to-give-access-to-pim.md)。

1. 打开“Azure AD Privileged Identity Management”。 

1. 选择“Azure 资源”  。

1. 使用资源筛选器可查找所需的受管理资源。

    ![要管理的 Azure 资源的列表](./media/pim-resource-roles-assign-roles/resources-list.png)

1. 选择要管理的资源以打开资源概览页。

1. 在“管理”下，选择“角色” 以查看 Azure 资源的角色列表。

    ![Azure 资源角色](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. 选择“添加分配”以打开“添加分配”窗格。 

1. 选择“选择角色”以打开“选择角色”页 。

    ![“新建分配”窗格](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. 选择要分配的角色，然后单击“选择”  。

    此时会打开“选择成员或组”窗格。

1. 选择要向角色分配的成员或组，然后单击“选择”  。

    ![“选择成员或组”窗格](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

1. 在“设置”选项卡的“分配类型”列表中，选择“合格”或“活动” 。

    ![“成员身份设置”窗格](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Azure 资源的 Privileged Identity Management 提供了两种不同的分配类型：

    - “合格”分配要求该角色的成员执行某个操作才能使用该角色。 操作可能包括执行多重身份验证 (MFA) 检查、提供业务理由或请求获得指定审批者的批准。

    - “活动”分配不要求成员执行任何操作便可使用该角色。 分配为“活动”的成员始终具有分配给该角色的权限。

1. 若要指定特定分配持续时间，请更改开始和结束日期与时间。

1. 完成后，选择“分配”。

1. 创建新的角色分配后，会显示状态通知。

    ![新建分配 - 通知](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>更新或删除现有的角色分配

按照以下步骤更新或删除现有的角色分配。

1. 打开“Azure AD Privileged Identity Management”。

1. 选择“Azure 资源” 。

1. 选择要管理的资源以打开其概览页。

1. 在“管理”下，选择“角色” 以查看 Azure 资源的角色列表。

    ![Azure 资源角色 - 选择角色](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. 选择要更新或删除的角色。

1. 在“合格角色”或“活动角色”选项卡上查找角色分配。

    ![更新或删除角色分配](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. 选择“更新”或“删除”以更新或删除角色分配。

    有关扩展角色分配的信息，请参阅[在 Privileged Identity Management 中扩展或续订 Azure 资源角色](pim-resource-roles-renew-extend.md)。

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中扩展或续订 Azure 资源角色](pim-resource-roles-renew-extend.md)
- [在 Privileged Identity Management 中配置 Azure 资源角色设置](pim-resource-roles-configure-role-settings.md)
- [在 Privileged Identity Management 中分配 Azure AD 角色](pim-how-to-add-role-to-user.md)

