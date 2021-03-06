---
title: Azure 数据工厂连接器概述
description: 了解数据工厂中支持的连接器。
services: data-factory
author: WenJason
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 08/31/2020
ms.date: 09/21/2020
ms.author: v-jay
ms.reviewer: craigg
ms.openlocfilehash: ff7d21eb5fc8ff3da9c50e79c5a9248c5fa761ca
ms.sourcegitcommit: f5d53d42d58c76bb41da4ea1ff71e204e92ab1a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90523795"
---
# <a name="azure-data-factory-connector-overview"></a>Azure 数据工厂连接器概述

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure 数据工厂通过复制、查找、获取元数据和删除活动支持以下数据存储和格式。 单击每个数据存储以了解详细信息中支持的功能和相应的配置。

## <a name="supported-data-stores"></a>支持的数据存储

[!INCLUDE [Connector overview](../../includes/data-factory-v2-connector-overview.md)]

## <a name="integrate-with-more-data-stores"></a>与更多数据存储集成

Azure 数据工厂可以访问比上述列表更广泛的数据存储集。 如果需要将数据移入/移出 Azure 数据工厂内置连接器列表中未包含的数据存储，以下是一些可扩展选项：
- 对于数据库和数据仓库，通常可以找到相应的 ODBC 驱动程序，借助该驱动程序，你可使用[通用 ODBC 连接器](connector-odbc.md)。
- 对于 SaaS 应用程序：
    - 如果它提供 RESTful API，你可使用[通用 REST 连接器](connector-rest.md)。
    - 如果它具有 OData 源，你可使用[通用 OData 连接器](connector-odata.md)。
    - 如果它提供 SOAP API，你可使用[通用 HTTP 连接器](connector-http.md)。
    - 如果它具有 ODBC 驱动程序，你可使用[通用 ODBC 连接器](connector-odbc.md)。
- 对于其他情况，请检查能否将数据加载或公开到 ADF 支持的任何数据存储（例如 Azure Blob/文件/FTP/SFTP 等），然后让 ADF 从中选取数据。 可以通过 [Azure 函数](control-flow-azure-function-activity.md)、[自定义活动](transform-data-using-dotnet-custom-activity.md)、[HDInsight](transform-data-using-hadoop-hive.md)、[Web 活动](control-flow-web-activity.md)等来调用自定义数据加载机制。

## <a name="supported-file-formats"></a>支持的文件格式

Azure 数据工厂支持以下文件格式。 请参阅每一篇介绍基于格式的设置的文章。

- [Avro 格式](format-avro.md)
- [二进制格式](format-binary.md)
- [带分隔符的文本格式](format-delimited-text.md)
- [Excel 格式](format-excel.md)
- [JSON 格式](format-json.md)
- [ORC 格式](format-orc.md)
- [Parquet 格式](format-parquet.md)
- [XML 格式](format-xml.md)

## <a name="next-steps"></a>后续步骤

- [Copy 活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [Get Metadata 活动](control-flow-get-metadata-activity.md)
- [删除活动](delete-activity.md)
