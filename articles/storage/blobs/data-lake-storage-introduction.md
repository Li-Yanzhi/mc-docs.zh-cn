---
title: Azure Data Lake Storage Gen2 简介
description: 查看 Azure Data Lake Storage Gen2 简介。 了解关键功能。 查看支持的 Blob 存储功能、Azure 服务集成和平台。
author: WenJason
ms.service: storage
ms.topic: overview
origin.date: 02/25/2020
ms.date: 11/16/2020
ms.author: v-jay
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 822b64fd9ae58405922dfe59cd842e5cb87b030c
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552118"
---
# <a name="introduction-to-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 简介

‎Azure Data Lake Storage Gen2 是一组专用于大数据分析的功能，以 [Azure Blob 存储](storage-blobs-introduction.md)为基础而构建。

## <a name="designed-for-enterprise-big-data-analytics"></a>专为企业大数据分析而设计

Data Lake Storage Gen2 使 Azure 存储成为在 Azure 上构建企业 Data Lake 的基础。 Data Lake Storage Gen2 从一开始就设计为存储数千万亿字节的信息，同时保持数百千兆位的吞吐量，允许你轻松管理大量数据。

Data Lake Storage Gen2 的一个基本部分是向 Blob 存储添加[分层命名空间](data-lake-storage-namespace.md)。 分层命名空间将对象/文件组织到目录层次结构中，以便进行有效的数据访问。 常见的对象存储命名约定在名称中使用斜杠来模拟分层目录结构。 这种结构在 Data Lake Storage Gen2 中得以真正实现。 诸如重命名或删除目录之类的操作在目录上成为单个原子元数据操作，而不是枚举或处理共享目录名称前缀的所有对象。

Data Lake Storage Gen2 在 Blob 存储的基础上构建，并通过以下方式增强了性能、管理和安全性：

-   优化了性能，因为你不需要将复制或转换数据作为分析的先决条件。 与 Blob 存储上的平面命名空间相比，分层命名空间极大地提高了目录管理操作的性能，从而提高了整体作业性能。

-   管理更为容易，因为你可以通过目录和子目录来组织和操作文件。

-   安全性是可以强制实施的，因为可以在目录或单个文件上定义 POSIX 权限。

另外，Data Lake Storage Gen2 非常经济高效，因为它构建在低成本的 [Azure Blob 存储](storage-blobs-introduction.md)之上。 这些新增功能进一步降低了在 Azure 上运行大数据分析的总拥有成本。

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake Storage Gen2 的主要功能

-   Hadoop 兼容访问：使用 Data Lake Storage Gen2，可以像使用 [Hadoop 分布式文件系统 (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) 一样管理和访问数据。 可在 [Azure HDInsight](/hdinsight/index)、Azure Databricks 和 [Azure Synapse Analytics](/synapse-analytics) 等所有 Apache Hadoop 环境中使用新的 [ABFS 驱动程序](data-lake-storage-abfs-driver.md)来访问 Data Lake Storage Gen2 中存储的数据。

-   POSIX 权限的超集：Data Lake Gen2 的安全模型支持 ACL 和 POSIX 权限，以及特定于 Data Lake Storage Gen2 的一些额外粒度。 可以通过存储资源管理器或 Hive 和 Spark 等框架来配置设置。

-   **经济高效**：Data Lake Storage Gen2 提供了低成本的存储容量和事务。 随着数据在其整个生命周期中的转换，记帐费率变化通过诸如 [Azure Blob 存储生命周期](storage-lifecycle-management-concepts.md)的内置功能使成本保持在最低水平。

-   **优化的驱动程序**：ABFS 驱动程序已针对大数据分析进行 [专门优化](data-lake-storage-abfs-driver.md)。 相应的 REST API 通过终结点 `dfs.core.chinacloudapi.cn` 进行显示。

### <a name="scalability"></a>可伸缩性

按照设计，无论是通过 Data Lake Storage Gen2 还是 Blob 存储接口进行访问，Azure 存储都可自如缩放。 它可以存储和处理许多百亿亿字节的数据。 这种存储量可用于在每秒高级别的输入/输出操作 (IOPS) 下以每秒千兆位 (Gbps) 的速度测量的吞吐量。 除持久性之外，还会根据在服务、帐户和文件级别上测量的持续请求延迟来执行处理。

### <a name="cost-effectiveness"></a>成本效益

基于 Azure Blob 存储生成 Data Lake Storage Gen2 的多个好处之一是存储容量和事务的低成本。 与其他云存储服务不同，在执行分析之前不需要移动或转换存储在 Data Lake Storage Gen2 中的数据。 有关定价的详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage)。

此外，[分层命名空间](data-lake-storage-namespace.md)等功能可显著提高许多分析作业的整体性能。 这一性能方面的提升意味着你需要较少的计算能力来处理相同数量的数据，从而降低端到端分析作业的总拥有成本 (TCO)。

### <a name="one-service-multiple-concepts"></a>一个服务，多个概念

Data Lake Storage Gen2 是用于大数据分析的附加功能，基于 Azure Blob 存储而构建。 虽然利用 Blob 的现有平台组件来创建和操作数据库进行分析有很多好处，但它确实导致了描述相同共享内容的多个概念。

以下是不同概念所描述的等效实体。 除非另有说明，否则这些实体是直接同义的：

| 概念                                | 顶级组织 | 较低级别的组织                                            | 数据容器 |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| Blob - 常规用途对象存储 | 容器              | 虚拟目录（仅限 SDK - 不提供原子操作） | Blob           |
| Azure Data Lake Storage Gen2 - 分析存储          | 容器            | Directory                                                           | 文件           |

## <a name="supported-blob-storage-features"></a>支持的 Blob 存储功能

Blob 存储功能（例如[诊断日志记录](../common/storage-analytics-logging.md)、[访问层](storage-blob-storage-tiers.md)、[Blob 存储生命周期管理策略](storage-lifecycle-management-concepts.md)）现在适用于具有分层命名空间的帐户。 因此，你可以在 Blob 存储帐户上启用分层命名空间，而不会失去对这些功能的访问权限。 

有关受支持的 Blob 存储功能的列表，请参阅 [Azure Data Lake storage Gen2 中提供的 Blob 存储功能](data-lake-storage-supported-blob-storage-features.md)。

## <a name="supported-azure-service-integrations"></a>支持的 Azure 服务集成

Data Lake Storage gen2 支持多个可用于引入数据、执行分析和创建可视化表示形式的 Azure 服务。 有关受支持的 Azure 服务的列表，请参阅[支持 Azure Data Lake Storage Gen2 的 Azure 服务](data-lake-storage-supported-azure-services.md)。

## <a name="supported-open-source-platforms"></a>支持的开源平台

多个开源平台支持 Data Lake Storage Gen2。 有关完整列表，请参阅[支持 Azure Data Lake Storage Gen2 的开源平台](data-lake-storage-supported-open-source-platforms.md)。

## <a name="see-also"></a>另请参阅

- [Azure Data Lake Storage Gen2 的已知问题](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 的多协议访问](data-lake-storage-multi-protocol-access.md)


