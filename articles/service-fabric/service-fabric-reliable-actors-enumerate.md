---
title: 枚举 Azure Service Fabric 中的执行组件
description: 了解如何按照示例在 Azure Service Fabric 应用程序中枚举 Reliable Actors 及其元数据。
ms.topic: conceptual
origin.date: 03/19/2018
author: rockboyfor
ms.date: 09/14/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: devx-track-csharp
ms.openlocfilehash: 813dae0a397fb51249aafd81ce34164d4d5286e0
ms.sourcegitcommit: e1cd3a0b88d3ad962891cf90bac47fee04d5baf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89655679"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>枚举 Service Fabric Reliable Actors
Reliable Actors 允许客户端枚举有关该服务托管的执行组件的元数据。 由于执行组件服务是已分区的有状态服务，因此将按分区执行枚举。 因为每个分区可能包含许多执行组件，所以枚举以一组分页结果的形式返回。 将循环读取这些页面，直到读取所有页面。 以下示例演示了如何创建执行组件服务的一个分区中所有活动执行组件的列表：

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

## <a name="next-steps"></a>后续步骤
* [执行组件状态管理](service-fabric-reliable-actors-state-management.md)
* [执行组件生命周期和垃圾回收](service-fabric-reliable-actors-lifecycle.md)
* [执行组件 API 参考文档](https://docs.microsoft.com/previous-versions/azure/dn971626(v=azure.100))
* [.NET 代码示例](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java 代码示例](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->

[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png

<!-- Update_Description: update meta properties, wording update, update link -->