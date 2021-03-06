---
author: rockboyfor
ms.service: service-bus-messaging
ms.topic: include
origin.date: 11/25/2018
ms.date: 07/27/2020
ms.testscope: yes|no
ms.testdate: 07/27/2020Null
ms.author: v-yeche
ms.openlocfilehash: 119e980750a440daf99b2a667516bcc4eb3dea7f
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "88946868"
---
## <a name="what-are-service-bus-queues"></a>什么是 Service Bus 队列？

服务总线队列支持中转消息传送  通信模型。 在使用队列时，分布式应用程序的组件不会直接相互通信，而是通过充当中介（代理）的队列交换消息。 消息创建方（发送方）将消息传送到队列，然后继续对其进行处理。 消息使用方（接收方）以异步方式从队列中提取消息并处理它。 创建方不必等待使用方的答复即可继续处理并发送更多消息。 队列为一个或多个竞争使用方提供**先入先出 (FIFO)** 消息传递方式。 也就是说，接收方通常会按照消息添加到队列中的顺序来接收并处理消息，并且每条消息仅由一个消息使用方接收并处理。

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Service Bus 队列是一种可用于各种应用场景的通用技术：

* 多层 Azure 应用程序中 Web 角色和辅助角色之间的通信。
* 混合解决方案中本地应用程序和 Azure 托管应用程序之间的通信。
* 在不同组织或组织的各部门中本地运行的分布式应用程序组件之间的通信。

利用队列，可以更轻松地缩放应用程序，并增强体系结构的弹性。

<!-- Update_Description: update meta properties, wording update, update link -->