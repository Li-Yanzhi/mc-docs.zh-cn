---
title: 快速入门 - 将 Docker 容器部署到容器实例 - 门户
description: 本快速入门将使用 Azure 门户快速部署在隔离的 Azure 容器实例中运行的容器化 Web 应用
ms.topic: quickstart
origin.date: 08/24/2020
author: rockboyfor
ms.date: 10/05/2020
ms.author: v-yeche
ms.custom: seodec18, mvc, devx-track-js
ms.openlocfilehash: 94829d49e5321efe28c5460349291e3a2387e5e2
ms.sourcegitcommit: 29a49e95f72f97790431104e837b114912c318b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564298"
---
<!--Verified successfully-->
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>快速入门：使用 Azure 门户在 Azure 中部署容器实例

使用 Azure 容器实例在 Azure 中快速方便地运行无服务器 Docker 容器。 当你不需要像 AzureKubernetes 服务这样的完整容器业务流程平台时，可以按需将应用程序部署到容器实例。

本快速入门将使用 Azure 门户部署一个独立的 Docker 容器，并使其应用程序可通过完全限定的域名 (FQDN) 使用。 在配置一些设置并部署容器后，你可以浏览到正在运行的应用程序：

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-07.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.cn 登录到 Azure 门户。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户][azure-free-account]。

## <a name="create-a-container-instance"></a>创建容器实例

<!--MOONCAKE: CUSTOMIZED ON AZURE PORTAL-->

选择“+ 创建资源”****，在“新建”**** 页的搜索筛选器中键入“容器实例”****。

![开始在 Azure 门户中创建新的容器实例][aci-portal-01]

在市场结果中选择“容器实例”**** 项。

<!--MOONCAKE: CUSTOMIZED ON AZURE PORTAL-->

在“基本信息”页的“资源组”、“容器名称”和“容器映像”文本框中输入以下值     。 将其他值保留为默认值，然后选择“确定”  。

* 资源组：“新建” > `myresourcegroup`
* 容器名称：`mycontainer`
* 映像源：**快速启动映像**
* 容器映像：`mcr.microsoft.com/azuredocs/aci-helloworld` (Linux)

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-03.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

对于本快速入门，请使用默认设置来部署公共 Microsoft `aci-helloworld` 映像。 此示例 Linux 映像打包了一个用 Node.js 编写的小型 Web 应用，该应用提供静态 HTML 页面。 还可以将自己的容器映像存储在 Azure 容器注册表、Docker Hub 或其他注册表中。

<!--CORRECT ON public Microsoft `aci-helloworld`-->

在“网络”页，为容器指定一个“DNS 名称标签”   。 该名称在创建容器实例的 Azure 区域中必须是唯一的。 容器将可通过 `<dns-name-label>.<region>.azurecontainer.console.azure.cn` 公开访问。 如果收到“DNS 名称标签不可用”错误消息，请尝试使用一个不同的 DNS 名称标签。

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-04.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

将其他设置保留为默认设置，然后选择“查看 + 创建”  。

完成验证后，将显示容器的设置摘要。 选择“创建”  提交容器部署请求。

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-05.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

当部署启动时，会显示一个通知，指出正在进行部署。 部署了容器组后，会显示另一个通知。

通过导航到“资源组” > “myresourcegroup” > “mycontainer”打开容器组的概述    。 记下容器实例的 **FQDN**（完全限定的域名）及其**状态**。

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-06.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

在其**状态**为“正在运行”  后，在浏览器中导航到容器的 FQDN。

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-07.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

祝贺你！ 仅仅配置了几个设置，你便已在 Azure 容器实例中部署了一个可供公开访问的应用程序。

## <a name="view-container-logs"></a>查看容器日志

当排查容器或其运行的应用程序的问题时，查看容器实例的日志非常有用。

若要查看容器的日志，请在“设置”  下选择“容器”  ，然后选择“日志”  。 应当会看到在浏览器中查看应用程序时生成的 HTTP GET 请求。

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-11.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

## <a name="clean-up-resources"></a>清理资源

完成容器后，选择 *mycontainer* 容器实例的“概述”，然后选择“删除”。  

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-09.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

在出现确认对话框时选择“是”。 

:::image type="content" source="media/container-instances-quickstart-portal/qs-portal-10.png" alt-text="在浏览器中显示的使用 Azure 容器实例部署的应用":::

## <a name="next-steps"></a>后续步骤

在本快速入门中，你已基于公共 Microsoft 映像创建了一个 Azure 容器实例。 若要基于专用 Azure 容器注册表生成容器映像并部署它，请继续学习 Azure 容器实例教程。

<!--CORRECT ON public Microsoft image-->

> [!div class="nextstepaction"]
> [Azure 容器实例教程](./container-instances-tutorial-prepare-app.md)

<!-- LINKS - External -->

[azure-free-account]: https://www.azure.cn/pricing/1rmb-trial/

<!-- Update_Description: update meta properties, wording update, update link -->