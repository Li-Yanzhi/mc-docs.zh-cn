---
title: Web 服务参数 - Azure 机器学习工作室（经典）| Microsoft Docs
description: 在 Web 服务受访问时，如何使用 Azure 机器学习 Web 服务参数修改模型行为。
services: machine-learning
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/12/2017
ms.openlocfilehash: c9f725d77535cd5254c93b4ece2819e5d084ef0d
ms.sourcegitcommit: c2c9dc65b886542d220ae17afcb1d1ab0a941932
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94978208"
---
# <a name="use-azure-machine-learning-studio-classic-web-service-parameters"></a>使用 Azure 机器学习工作室（经典）Web 服务参数

**适用对象：** ![适用于.](../../../includes/media/aml-applies-to-skus/yes.png)机器学习工作室（经典）   ![不适用于.](../../../includes/media/aml-applies-to-skus/no.png)[Azure 机器学习](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)


发布包含可配置参数的模块的实验，创建 Azure 机器学习 Web 服务。 在某些情况下，当 Web 服务在运行时，可能会要更改模块行为。 *Web 服务参数* 允许执行此任务。 

常见示例为设置[导入数据][reader]模块，以便 Web 服务受访问时，已发布的 Web 服务的用户能够指定不同的数据源。 或配置[导出数据][writer]模块以指定其他目标。 其他一些示例包括为[特征哈希][feature-hashing]模块更改位数或为[基于筛选器的功能选择][filter-based-feature-selection]模块更改所需功能数。 

可设置 Web 服务参数并在实验中将它们与一个或多个模块参数关联，并且可以指定它们是必需项还是可选项。 然后在调用 Web 服务时，Web 服务的用户可提供这些参数的值。 



## <a name="how-to-set-and-use-web-service-parameters"></a>如何设置和使用 Web 服务参数
单击模块参数旁边的图标，并选择“设为 Web 服务参数”，定义 Web 服务参数。 这创建新的 Web 服务参数，并将其连接到该模块参数。 然后在访问 Web 服务时，用户可指定 Web 服务参数的值，并将它应用到模块参数。

定义 Web 服务参数后，它可用于实验中其他任何模块参数。 如果定义关联一个模块参数的 Web 服务参数，可将同一 Web 服务参数用于其他任何模块，前提是参数使用相同类型的值。 例如，如果 Web 服务参数是数字值，则它仅可用于使用数字值的模块参数。 当用户设置 Web 服务参数的值时，该参数将应用于所有关联的模块参数。

可以决定是否提供 Web 服务参数的默认值。 如果提供，则 Web 服务的用户可选择是否使用该参数。 如果不提供默认值，则在 Web 服务受访问时，用户要输入值。

Web 服务的 API 文档包括向 Web 服务用户提供的有关访问 Web 服务时如何以编程方式指定 Web 服务参数的信息。

> [!NOTE]
> 经典 Web 服务的 API 文档通过机器学习工作室（经典）的 Web 服务“仪表板”中的“API 帮助页”链接提供。 新 Web 服务的 API 文档通过 Web 服务“使用”和“Swagger API”页上的 [Azure 机器学习 Web 服务](https://services.azureml.net/Quickstart)门户提供。
> 
> 

## <a name="example"></a>示例
例如，假设我们在做将信息发送到 Azure Blob 存储的[导出数据][writer]模块的实验。 我们将定义名为“Blob 路径”的 Web 服务参数，该参数允许 Web 服务用户在服务受访问时更改指向 Blob 存储的路径。

1. 在机器学习工作室（经典）中，单击[导出数据][writer]模块，将其选中。 属性显示在实验画布右侧的“属性”窗格中。
2. 指定存储类型：
   
   * 在“请指定数据目标”下，选择“Azure Blob 存储”。
   * 在“请指定身份验证类型”下，选择“帐户”。
   * 输入 Azure Blob 存储的帐户信息。 

3.  单击 **指向以容器参数开头的 blob 的路径 右侧的图标**。 如下所示：
   
   ![Web 服务参数图标](./media/web-service-parameters/icon.png)
   
   选择“设为 Web 服务参数”。
   
   “属性”窗格底部的“Web 服务参数”下添加了名为“指向以容器参数开头的 blob 的路径”的条目。 这即是 Web 服务参数，此时已与此[导出数据][writer]模块参数关联。
4. 要重命名 Web 服务参数，请单击名称、输入“Blob 路径”，并按“Enter”键。 
5. 要提供 Web 服务参数的默认值，请单击名称右侧的图标、选择“提供默认值”、输入值（例如“container1/output1.csv”），并按“Enter”键。
   
   ![Web 服务参数](./media/web-service-parameters/parameter.png)
6. 单击 **“运行”** 。 
7. 单击“部署 Web 服务”时，并选择“部署 Web 服务[经典]”或“部署 Web 服务[新]”，部署 Web 服务。

> [!NOTE] 
> 若要部署新的 Web 服务，必须对要部署 Web 服务的订阅拥有充分的权限。 有关详细信息，请参阅[使用 Azure 机器学习 Web 服务门户管理 Web 服务](manage-new-webservice.md)。 

访问 Web 服务时，Web 服务的用户现在可为[导出数据][writer]模块指定新目标。

## <a name="more-information"></a>详细信息
有关更详细的示例，请参阅 [机器学习博客](/archive/blogs/machinelearning/azureml-web-service-parameters) 中的 [Web 服务参数](https://docs.microsoft.com/archive/blogs/machinelearning/azureml-web-service-parameters) 条目。

有关访问机器学习 Web 服务的详细信息，请参阅[如何使用 Azure 机器学习 Web 服务](consume-web-services.md)。

<!-- Module References -->
[feature-hashing]: /machine-learning/studio-module-reference/feature-hashing
[filter-based-feature-selection]: /previous-versions/azure/dn905854(v=azure.100)
[reader]: /machine-learning/studio-module-reference/import-data
[writer]: /machine-learning/studio-module-reference/export-data