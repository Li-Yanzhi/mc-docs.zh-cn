---
title: 使用 Azure 门户将逻辑应用导入为 API | Microsoft Docs
description: 本文演示如何使用 API 管理 (APIM) 将逻辑应用导入为 API。
services: api-management
documentationcenter: ''
author: Johnnytechn
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
origin.date: 08/01/2019
ms.topic: article
ms.date: 11/18/2020
ms.author: v-johya
ms.openlocfilehash: 3b96d79c5f89e1ae980907fa10bfbe717a523e9f
ms.sourcegitcommit: c2c9dc65b886542d220ae17afcb1d1ab0a941932
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94977092"
---
# <a name="import-a-logic-app-as-an-api"></a>将逻辑应用导入为 API

本文介绍如何将逻辑应用导入为 API 并测试导入的 API。

在本文中，学习如何：

> [!div class="checklist"]
>
> -   将逻辑应用导入为 API
> -   在 Azure 门户中测试 API
> -   在开发人员门户中测试 API

## <a name="prerequisites"></a>先决条件

-   请完成以下快速入门：[创建一个 Azure API 管理实例](get-started-create-service-instance.md)
-   确保订阅中已有一个公开 HTTP 终结点的逻辑应用。 有关详细信息，请参阅[使用 HTTP 终结点触发工作流](../logic-apps/logic-apps-http-endpoint.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>导入和发布后端 API

1. 在 Azure 门户中导航到 API 管理服务，然后从菜单中选择“API”  。
2. 从“添加新的 API”列表中选择“逻辑应用”   。

    ![逻辑应用](./media/import-logic-app-as-api/logic-app-api.png)

3. 按“浏览”查看订阅中使用 HTTP 触发器的逻辑应用列表  。 （请注意，不使用 HTTP 触发器的逻辑应用不会出现在此列表中。）
4. 选择应用。 API 管理找到与所选应用关联的 swagger 后，将其提取并导入。
5. 添加 API URL 后缀。 后缀是用于在该 API 管理实例中标识此特定 API 的名称。 在该 API 管理实例中，后缀必须唯一。
6. 通过关联 API 与产品来发布 API。 本例中使用了“无限制”产品  。 如果想要发布 API 并使其对开发人员可用，请将其添加到产品中。 可在 API 创建期间执行此操作，或稍后进行设置。

    产品是一个或多个 API 的关联。 可以包含多个 API，并通过开发人员门户将其提供给开发人员。 开发人员必须先订阅产品才能访问 API。 订阅时，他们会得到一个订阅密钥，此密钥对该产品中的任何 API 都有效。 如果你创建了 API 管理实例，那么你已是管理员，因此默认情况下订阅了每个产品。

    默认情况下，每个 API 管理实例附带两个示例产品：

    - **入门**
    - **不受限制**

7. 输入其他 API 设置。 可以在创建过程中设置这些值，也可以稍后转到“设置”  选项卡来配置这些值。在[导入和发布第一个 API](import-and-publish.md) 教程中对这些设置进行了说明。
8. 选择“创建”  。

## <a name="test-the-api-in-the-azure-portal"></a>在 Azure 门户中测试 API

可直接从 Azure 门户调用操作，这样可以方便地查看和测试 API 的操作。

1. 选择上一步中创建的 API。
2. 按“测试”选项卡  。
3. 选择某个操作。

    该页将显示查询参数的字段和标头的字段。 其中一个标头是“Ocp-Apim-Subscription-Key”，用于提供和此 API 关联的产品订阅密钥。 如果创建了 API 管理实例，那么你已是管理员，因此会自动填充该密钥。

4. 按“发送”。 

    后端以“200 正常”和某些数据做出响应  。

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>每个逻辑应用程序都有一个 **manual-invoke** 操作。 若要构造包含多个逻辑应用的 API，为避免冲突，需要将函数重命名。

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
>
> [转换和保护已发布的 API](transform-api.md)

