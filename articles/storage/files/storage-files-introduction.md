---
title: Azure 文件简介 | Microsoft Docs
description: 本文概述 Azure 文件服务，使用该服务可以通过行业标准 SMB 协议在云中创建和使用网络文件共享。
author: WenJason
ms.service: storage
ms.topic: overview
origin.date: 09/15/2020
ms.date: 11/16/2020
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: aaca621c7276d883fecb41420d10fb7a626012e4
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552545"
---
# <a name="what-is-azure-files"></a>什么是 Azure 文件？
Azure 文件在云端提供完全托管的文件共享，这些共享项可通过行业标准的[服务器消息块 (SMB) 协议](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)进行访问。 Azure 文件共享可通过云部署或者本地部署并行装载。 可从 Windows、Linux 和 macOS 客户端访问Azure 文件存储 SMB 文件共享。

## <a name="why-azure-files-is-useful"></a>为何 Azure 文件很有用
Azure 文件共享可用于：

* **取代或补充本地文件服务器**：  
    可以使用 Azure 文件来完全取代或补充传统的本地文件服务器或 NAS 设备。 流行的操作系统（例如 Windows、macOS 和 Linux）可在世界各地直接装载 Azure 文件共享。 此外，可以使用 Azure 文件同步将 Azure 文件共享复制到本地或云中的 Windows Server，以便在使用位置对数据进行高性能的分布式缓存。

* **“直接迁移”应用程序**：  
    借助 Azure 文件可以轻松地将预期使用文件共享存储文件应用程序或用户数据的应用程序“直接迁移”到云中。 Azure 文件既支持“经典”直接迁移方案（应用程序及其数据将移到 Azure 中），也支持“混合”直接迁移方案（应用程序数据将移到 Azure 文件中，应用程序继续在本地运行）。 

* **简化云开发**：  
    还可以通过众多方式使用 Azure 文件来简化新的云开发项目。 例如：
    * **共享应用程序设置**：  
        分布式应用程序的常见模式是将配置文件置于某个中心位置，然后可以从许多应用程序实例访问这些文件。 应用程序实例可以通过文件 REST API 加载其配置，人类可以根据需要通过本地装载 SMB 共享来访问这些配置。

    * **诊断共享**：  
        Azure 文件共享是云应用程序写入其日志、指标和故障转储的方便位置。 应用程序实例可以通过文件 REST API 写入日志，开发人员可以通过在其本地计算机上装载文件共享来访问这些日志。 这就带来了极大的灵活性，因为开发人员可以利用云开发，同时又不需要放弃他们所熟悉和喜爱的任何现有工具。

    * **开发/测试/调试**：  
        开发人员或管理员在云中的 VM 上工作时，通常需要一套工具或实用程序。 将此类实用程序和工具复制到每个 VM 可能非常耗时。 通过在 VM 上本地装载 Azure 文件共享，开发人员和管理员可以快速访问其工具和实用程序，而无需进行复制。
* **容器化**：  
    可以将 Azure 文件共享用作有状态容器的永久性卷。 容器提供了“一次构建，随处运行”功能，使开发人员能够加速创新。 对于在每次启动时都访问原始数据的容器，需要使用共享文件系统，以允许这些容器无论在哪个实例上运行都可以访问文件系统。

## <a name="key-benefits"></a>主要优点
* **共享访问**。 Azure 文件共享支持行业标准 SMB 协议，这意味着，你可以无缝地将本地文件共享替换为 Azure 文件共享，不需担心应用程序兼容性。 对于需要可共享性的应用程序来说，能够跨多个计算机、应用程序/实例共享文件系统是使用 Azure 文件的一项明显优势。 
* **完全托管**。 不需管理硬件或 OS 即可创建 Azure 文件共享。 这意味着，你不需使用关键的安全升级程序来修补服务器 OS，也不需更换故障硬盘。
* **脚本和工具**。 在管理 Azure 应用程序的过程中，可以使用 PowerShell cmdlet 和 Azure CLI 来创建、装载和管理 Azure 文件共享。可以使用 Azure 门户和 Azure 存储资源管理器来创建和管理 Azure 文件共享。 
* **复原能力**。 Azure 文件是从头开始构建的，我们的目的是确保其始终可用。 将本地文件共享取代为 Azure 文件之后，再也不需要半夜起来处理当地断电或网络问题。 
* **熟悉的可编程性**。 在 Azure 中运行的应用程序可以通过文件[系统 I/O API](https://msdn.microsoft.com/library/system.io.file.aspx) 访问共享中的数据。 因此，开发人员可以利用其现有代码和技术迁移现有应用程序。 除了系统 IO API，还可以使用 [Azure 存储客户端库](https://msdn.microsoft.com/library/azure/dn261237.aspx)或 [Azure 存储 REST API](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api)。

## <a name="next-steps"></a>后续步骤
* [创建 Azure 文件共享](storage-how-to-create-file-share.md)
* [在 Windows 上连接和装载 SMB 共享](storage-how-to-use-files-windows.md)
* [在 Linux 上连接和装载 SMB 共享](storage-how-to-use-files-linux.md)
* [在 macOS 上连接和装载 SMB 共享](storage-how-to-use-files-mac.md)
