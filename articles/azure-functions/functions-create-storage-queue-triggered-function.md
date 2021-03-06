---
title: 在 Azure 中创建由队列消息触发的函数
description: 使用 Azure Functions 创建由提交到 Azure 队列的消息调用的无服务器函数。
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 09/28/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 2c2bf297a5fe85403158a2af3ee8b6d855869553
ms.sourcegitcommit: 63b9abc3d062616b35af24ddf79679381043eec1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2020
ms.locfileid: "91937249"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>创建由 Azure 队列存储触发的函数

了解如何创建在将消息提交到 Azure 存储队列时触发的函数。

## <a name="prerequisites"></a>先决条件

- Azure 订阅。 如果没有订阅，请在开始之前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-an-azure-function-app"></a>创建 Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-create-success.png" alt-text="已成功创建函数应用。" border="true":::

接下来，在新的 Function App 中创建一个函数。

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>创建队列触发的函数

1. 选择“函数”，然后选择“+ 添加”以添加新函数。

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-app-quickstart-choose-template.png" alt-text="已成功创建函数应用。" border="true":::

1. 选择“Azure 队列存储触发器”模板。

1. 使用图像下的表中指定的设置。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png" alt-text="已成功创建函数应用。" border="true":::


    | 设置 | 建议的值 | 说明 |
    |---|---|---|
    | **名称** | 在 Function App 中唯一 | 此队列触发函数的名称。 |
    | **队列名称**   | myqueue-items    | 要连接到存储帐户中的队列的名称。 |
    | **存储帐户连接** | AzureWebJobsStorage | 可以使用 Function App 已在使用的存储帐户连接，也可以创建一个新的存储帐户连接。  |    

1. 选择“创建函数”以创建函数。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-3.png" alt-text="已成功创建函数应用。" border="true":::

接下来，连接到 Azure 存储帐户并创建 myqueue-items 存储队列。

## <a name="create-the-queue"></a>创建队列

1. 在函数的“概览”页上，选择资源组。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-resource-group.png" alt-text="已成功创建函数应用。" border="true":::

1. 查找并选择资源组的存储帐户。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-account-access.png" alt-text="已成功创建函数应用。" border="true":::

1. 选择“队列”，然后选择“+ 队列”。 

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-add-queue.png" alt-text="已成功创建函数应用。" border="true":::

1. 在“名称”字段中键入 `myqueue-items`，然后选择“创建”。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-name-queue.png" alt-text="已成功创建函数应用。" border="true":::

现在已有存储队列，可以通过将消息添加到队列来测试函数。

## <a name="test-the-function"></a>测试函数

1. 返回到 Azure 门户中，浏览到函数，展开页面底部的“日志”并确保日志流式处理未暂停。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-queue-storage-log-expander.png" alt-text="已成功创建函数应用。" border="true":::

1. 在单独的浏览器窗口中，转到 Azure 门户中的资源组，然后选择存储帐户。

1. 选择“队列”，然后选择“myqueue-items”容器。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue.png" alt-text="已成功创建函数应用。" border="true":::

1. 选择“添加消息”，然后在“消息文本”中 键入“Hello World!”。 选择“确定”。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue-test.png" alt-text="已成功创建函数应用。" border="true":::

1. 等待几秒钟，返回到函数日志并验证是否已从队列中读取新消息。

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png" alt-text="已成功创建函数应用。" border="true":::

1. 返回到存储队列中，选择“刷新”，并验证该消息是否已处理且不再在队列中。

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>后续步骤

已创建将消息添加到存储队列时运行的函数。 有关队列存储触发器的详细信息，请参阅 [Azure Functions 存储队列绑定](functions-bindings-storage-queue.md)。

创建第一个函数以后，即可向这个可以将消息写回另一队列的函数添加输出绑定。

> [!div class="nextstepaction"]
> [使用 Functions 将消息添加到 Azure 存储队列](functions-integrate-storage-queue-output-binding.md)

