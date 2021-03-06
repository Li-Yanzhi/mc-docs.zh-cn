---
title: 使用访问评审管理用户访问权限 - Azure AD
description: 了解如何使用 Azure Active Directory 访问评审管理（以组成员身份）用户访问权限或对应用程序的分配
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/11/2020
ms.author: v-junlch
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27432f5dc2a07426b96bcf9383e778962b78ffc6
ms.sourcegitcommit: 753c74533aca0310dc7acb621cfff5b8993c1d20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92211451"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>使用 Azure AD 访问评审管理用户访问权限

使用 Azure Active Directory (Azure AD) 可以轻松确保用户有适当的访问权限。 为此，可让用户本人或决策人参与访问评审，鉴定（或“证明”）用户的访问权限。 审阅者可基于 Azure AD 的建议，针对每个用户就继续访问的需求提供意见。 访问评审完成后，即可进行更改，并删除不再需要访问权限的用户的访问权限。

> [!NOTE]
> 如果希望评审具有管理角色（如全局管理员）的用户成员身份，请参阅[在 Azure AD Privileged Identity Management 中启动访问评审](../privileged-identity-management/pim-how-to-start-security-review.md)。

## <a name="prerequisites"></a>必备条件

- Azure AD Premium P2

有关详细信息，请参阅[许可证要求](access-reviews-overview.md#license-requirements)。

## <a name="create-and-perform-an-access-review"></a>创建和执行访问评审

访问评审中可有一个或多个用户作为审阅者。  

1. 在具有一个或多个成员的 Azure AD 中选择一个组。 或者选择连接到 Azure AD（已为其分配一个或多个用户）的应用程序。 

2. 决定是由每个用户评审自己的访问权限，还是由一个或多个用户评审每个人的访问权限。

3. 以全局管理员或用户管理员身份，转到[“标识监管”页](https://portal.azure.cn/#blade/Microsoft_AAD_ERM/DashboardBlade/)。

4. 创建访问评审。 有关详细信息，请参阅[创建组或应用程序的访问评审](create-access-review.md)。

5. 访问评审开始时，要求审阅者提供输入。 默认情况下，他们每个人都会收到来自 Azure AD 的电子邮件，其中包含指向访问面板的链接，他们将在访问面板中[评审组或应用程序的访问权限](perform-access-review.md)。

6. 如果审阅者尚未提供输入，可以要求 Azure AD 向他们发送提醒。 默认情况下，Azure AD 自动在中途向还未作出回复的审阅者发送结束日期提醒。

7. 审阅者提供输入后，将停止访问评审并应用更改。 有关详细信息，请参阅[完成组或应用程序的访问评审](complete-access-review.md)。


## <a name="next-steps"></a>后续步骤

[创建组或应用程序的访问评审](create-access-review.md)





