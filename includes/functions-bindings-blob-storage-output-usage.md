---
title: include 文件
description: include 文件
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 02/17/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 0745dbdc50822dc9a2336132039bd373a8bc0f2b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77494591"
---
可绑定到以下类型以编写 blob：

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> function.json 中需有 "in" 绑定 `direction` 或 C# 类库中需有 `FileAccess.Read`。 但是，可以使用运行时提供的容器对象来执行写入操作，例如将 Blob 上传到容器。

<sup>2</sup> function.json 中需有 "inout" 绑定 `direction` 或 C# 类库中需有 `FileAccess.ReadWrite`。

如果尝试绑定到某个存储 SDK 类型并收到错误消息，请确保已引用[正确的存储 SDK 版本](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)。

在异步函数中，请使用返回值或 `IAsyncCollector` 而非 `out` 参数。

由于整个 Blob 内容都会加载到内存中，因此，只有当 Blob 较小时才建议绑定到 `string` 或 `Byte[]`。 平时，最好使用 `Stream` 或 `CloudBlockBlob` 类型。 有关详细信息，请参阅本文前文中的[并发和内存使用情况](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage)。

