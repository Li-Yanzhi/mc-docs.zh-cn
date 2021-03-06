---
title: 客户从购买阶段到 Azure Stack Hub 部署后阶段的过程 | Microsoft Docs
description: 了解在现场成功部署 Modular Data Center (MDC) 后预期产生的效果（从规划到部署后）。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/27/2020
ms.date: 11/09/2020
ms.author: v-jay
ms.reviewer: asganesh
ms.lastreviewed: 10/27/2020
ms.openlocfilehash: 1e6f3d01ef44ef04f2639961cd5a9f2c12401d98
ms.sourcegitcommit: f187b1a355e2efafea30bca70afce49a2460d0c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93330754"
---
# <a name="mdc-integration-overview"></a>MDC 集成概述

本文介绍 MDC 集成的端到端过程，涉及从购买到部署后的整个过程。 集成是客户和 Azure 之间的协作项目。 以下各部分介绍项目时间线上的不同阶段和项目成员要完成的具体步骤。

## <a name="introduction"></a>介绍

下表描述了各部署阶段的预期步骤。

|   |订单处理  |部署前 |集成、验证、传输 |现场部署  |部署之后 |
|---|---------------|---------------|-----------------------------------|--------------------|----------------|
|Azure  |- 中国位置交付信号 |提供收集数据中心要求所需的工具和文档  |- 验证配置项目并查看验证结果<br>- 确保交付硬件  |- 机架和堆栈<br>- 网络集成<br>- Azure Stack Hub 部署<br>- 移交给客户    |注册和市场联合|
|客户   |信号购买   |- 在部署工作表中填写网络详细信息<br>- 收集证书<br>- 获取 Azure AD 帐户<br>- 运行提供的任何验证工具    |确保站点已满足网络、电源、冷却先决条件    |- 已准备好部署配置项目<br>- 客户的网络工程师已准备好提供服务   |     |


## <a name="order-process"></a>订单处理

你的组织将使用 Azure 根据分配的系统数量下订单。 你下订单后，Azure 会将 MDC 交付到你的美国位置。 Azure 将确保满足所有安全供应链要求。 


## <a name="pre-deployment"></a>部署前

决定如何将 Azure Stack Hub 集成到数据中心。 Microsoft 发布了[部署工作表](../operator/azure-stack-deployment-worksheet.md)，可指导你收集成功集成到数据中心所需的所有必要信息。 此外，在部署时需要一组特定的证书。 为了帮助你获得这些证书，Azure 提供一个名为 [Azure Stack Hub 就绪性检查器](../operator/azure-stack-validation-report.md)的工具。 你可以借助此工具创建要向内部 CA 提供的证书签名请求 (CSR)。 

>[!Important]
>验证所有先决条件，以帮助防止部署延迟。 验证先决条件可能需要一些时间，并且需要组织中的不同部门进行协调和数据收集。

需选择以下项：

- Azure Stack Hub 连接模型和标识提供者。 可以选择在[已连接到 Internet（和 Azure）](../operator/azure-stack-connected-deployment.md)时或[断开连接](../operator/azure-stack-disconnected-deployment.md)时部署 Azure Stack Hub。 若要从 Azure Stack Hub（包括混合方案）获得最大效益，建议在连接到 Azure 时进行部署。 选择 Active Directory 联合身份验证服务 (AD FS) 还是 Azure Active Directory (Azure AD) 是在部署时必须进行的一次性决策。 以后，除非重新部署整个系统，否则将无法更改标识提供者。
- 网络集成。 [网络集成](../operator/azure-stack-network.md)对于部署、操作和管理 Azure Stack Hub 系统至关重要。 需要考虑到多种因素才能确保 Azure Stack Hub 解决方案具有复原能力，并提供一个高可用性物理基础设施来支持其操作。
- 防火墙集成。 建议[使用防火墙](../operator/azure-stack-firewall.md)来帮助保护 Azure Stack Hub。 防火墙有助于防止 DDOS 攻击，以及执行入侵检测和内容检查。 但应注意，它可能成为 Azure 存储服务的吞吐量瓶颈。
- 证书要求。 必须在现场工程师抵达数据中心进行部署之前准备好全部[所需的证书](../operator/azure-stack-pki-certs.md)。

通过部署工作表收集所有先决条件信息后，Azure 将确保我们验证所有验证工具是否已运行，并协助解决可能遇到的任何其他问题。 

## <a name="site-preparation"></a>安装位置准备

有关安装位置准备要求的详细信息，请参阅快速入门指南。

## <a name="hardware-delivery"></a>硬件交付

Azure 将与你合作，确保所有必需的硬件在分配的时间内到达中国位置。  

在现场 Azure 部署工程师抵达开始部署解决方案之前，将所有必要数据锁住并准备好，这一点至关重要。

- 部署工作表上已填写所有数据。 
- 必须验证并准备好所有证书。
- 必须确定区域名。
- 已确定和完成所有网络集成参数。

>[!Tip]
>如果上述任何信息发生了更改，请务必与内部组织合作，确保在现场部署工程师到达之前更新信息。 这将防止部署过程发生延迟。

## <a name="onsite-deployment"></a>现场部署

若要部署 Azure Stack Hub，会有 Azure 部署工程师来启动部署。 我们要求在现场部署期间，你组织有随时待命的网络工程师。

以下检查是部署体验期间现场工程师应执行的操作：

- 硬件的取消装箱和盘存
- 连接电源并为解决方案接通电源
- 验证物理硬件运行状况
- 检查所有布线和边界连接，确保解决方案正确定位在一起，且符合要求
- 配置解决方案的 HLH（硬件生命周期主机）
- 数据中心网络集成
- 检查并确保所有物理硬件设置正确
- 确保所有组件的固件使用解决方案批准的最新版本
- 开始部署

## <a name="post-deployment"></a>后期部署

Azure 部署工程师必须先执行几个步骤，然后才能将解决方案移交给客户。 在此阶段，验证非常重要，它可以确保系统部署正确且运行正常。

Azure 部署工程师应执行的操作：

- 启用增值资源提供程序 (RP)
- 运行 [test-azurestack](../operator/azure-stack-diagnostic-test.md)
- [注册](../operator/azure-stack-registration-role.md)到 Azure
- [市场联合](../operator/azure-stack-marketplace.md)
- 备份交换机配置文件和 HLH 配置文件
- 为客户准备部署摘要
- [检查更新以确保解决方案软件已更新到最新版本](../operator/azure-stack-updates.md)

## <a name="next-steps"></a>后续步骤

详细了解[安装和配置 Modular Data Center 的步骤](deployment-overview.md)。

