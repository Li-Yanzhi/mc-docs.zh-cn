---
title: 使用 Node 将文件从设备上传到 Azure IoT 中心 | Microsoft Docs
description: 如何使用用于 Node.js 的 Azure IoT 设备 SDK 从设备将文件上传到云中。 上传的文件存储在 Azure 存储 Blob 容器中。
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
origin.date: 06/28/2017
ms.date: 03/09/2020
ms.author: v-yiso
ms.openlocfilehash: 8def0d34143e4b2caee8f1b75e5d76f3439c40e9
ms.sourcegitcommit: 1118dd532a865ae25a63cf3e7e2eec2d7bf18acc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2020
ms.locfileid: "91394758"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-nodejs"></a>通过 IoT 中心将设备中的文件上传到云 (Node.js)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

本教程的内容基于[使用 IoT 中心发送云到设备的消息](iot-hub-node-node-c2d.md)教程中所述的代码，介绍如何使用 [IoT 中心的文件上传功能](iot-hub-devguide-file-upload.md)将文件上传到 [Azure Blob 存储](../storage/index.yml)。 本教程介绍如何：

* 安全提供具有 Azure blob URI 的设备，用于上传文件。

* 使用 IoT 中心文件上传通知触发处理应用后端中的文件。

[将遥测数据从设备发送到 IoT 中心](quickstart-send-telemetry-node.md)快速入门展示了 IoT 中心基本的设备到云消息传送功能。 但是，在某些情况下，无法轻松地将设备发送的数据映射为 IoT 中心接受的相对较小的设备到云消息。 例如：

* 包含图像的大型文件
* 视频
* 以高频率采样的振动数据
* 某种形式的预处理数据。

通常使用 [Azure 数据工厂](../data-factory/introduction.md)或 [Hadoop](../hdinsight/index.yml) 堆栈等工具在云中批处理这些文件。 需要从设备上传文件时，仍可以使用 IoT 中心的安全性和可靠性。

在本教程结束时，会运行两个 Node.js 控制台应用：

* **SimulatedDevice.js**，它使用 IoT 中心提供的 SAS URI 将文件上传到存储。
* **ReadFileUploadNotification.js**，它可以接收来自 IoT 中心的文件上传通知。

> [!NOTE]
> IoT 中心通过 Azure IoT 设备 SDK 来支持许多设备平台和语言（包括 C、.NET、Javascript、Python 和 Java）。 有关如何将设备连接到 Azure IoT 中心的分步说明，请参阅 [Azure IoT 开发人员中心]。

[!INCLUDE [iot-hub-include-x509-ca-signed-file-upload-support-note](../../includes/iot-hub-include-x509-ca-signed-file-upload-support-note.md)]

## <a name="prerequisites"></a>先决条件

* Node.js 版本 10.0.x 或更高版本。 [准备开发环境](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md)介绍了如何在 Windows 或 Linux 上安装本教程所用的 Node.js。

* 有效的 Azure 帐户。 （如果没有帐户，只需几分钟即可创建一个[试用帐户](http://www.azure.cn/pricing/1rmb-trial/)。）

* 确保已在防火墙中打开端口 8883。 本文中的设备示例使用 MQTT 协议，该协议通过端口 8883 进行通信。 在某些公司和教育网络环境中，此端口可能被阻止。 有关解决此问题的更多信息和方法，请参阅[连接到 IoT 中心(MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)。

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>从设备应用上传文件

本部分中操作将会创建可将文件上传到 IoT 中心的设备应用。

1. 创建名为 ```simulateddevice``` 的空文件夹。  在 ```simulateddevice``` 文件夹的命令提示符处，使用以下命令创建 package.json 文件。  接受所有默认值：

    ```cmd/sh
    npm init
    ```

2. 在 ```simulateddevice``` 文件夹的命令提示符处，运行下述命令以安装 azure-iot-device 设备 SDK 包和 azure-iot-device-mqtt 包：

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. 在 ```simulateddevice``` 文件夹中，利用文本编辑器创建 SimulatedDevice.js 文件。

4. 在 **SimulatedDevice.js** 文件的开头添加以下 ```require``` 语句：

    ```javascript
    'use strict';
    
    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

5. 添加 `deviceconnectionstring` 变量，并使用它创建一个客户端实例。  将 `{deviceconnectionstring}` 替换为在“创建 IoT 中心”部分中创建的设备的名称。

    ```javascript
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > 为方便起见，代码中包含了连接字符串：这不是建议的做法，根据用例和体系结构，请考虑以更安全的方式存储此机密。

1. 添加以下代码用于连接客户端：

    ```javascript
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

1. 创建一个回调，并使用 **uploadToBlob** 函数上传文件。

    ```javascript
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);
    
        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

1. 保存并关闭 **SimulatedDevice.js** 文件。

1. 将一个图像文件复制到 `simulateddevice` 文件夹并将其重命名为 `myimage.png`。

## <a name="get-the-iot-hub-connection-string"></a>获取 IoT 中心连接字符串

在本文中，你将创建一项后端服务，用于从你在[将遥测数据从设备发送到 IoT 中心](quickstart-send-telemetry-node.md)中创建的 IoT 中心接收文件上传通知消息。 若要接收文件上传通知消息，服务需要“服务连接”权限。 默认情况下，每个 IoT 中心都使用名为“服务”的共享访问策略创建，该策略会授予此权限。

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>接收文件上传通知

本部分中的操作将会创建一个 Node.js 控制台应用，用于接收来自 IoT 中心的文件上传通知消息。

可以使用 IoT 中心的 **iothubowner** 的连接字符串完成本部分的操作。 可以在 [Azure 门户](https://portal.azure.cn/)上的“共享访问策略”边栏选项卡中找到该连接字符串。

1. 创建名为 ```fileuploadnotification``` 的空文件夹。  在 ```fileuploadnotification``` 文件夹的命令提示符处，使用以下命令创建 package.json 文件。  接受所有默认值：

    ```cmd/sh
    npm init
    ```

2. 在 ```fileuploadnotification``` 文件夹中的命令提示符下，运行以下命令安装 **azure-iothub** SDK 包：

    ```cmd/sh
    npm install azure-iothub --save
    ```

1. 使用文本编辑器在 `fileuploadnotification` 文件夹中创建 **FileUploadNotification.js** 文件。

1. 在 **FileUploadNotification.js** 文件的开头添加以下 `require` 语句：

    ```javascript
    'use strict';
    
    var Client = require('azure-iothub').Client;
    ```

5. 添加 `iothubconnectionstring` 变量，并使用它创建一个客户端实例。  将 `{iothubconnectionstring}` 占位符值替换为先前在[获取 IoT 中心连接字符串](#get-the-iot-hub-connection-string)中复制的 IoT 中心连接字符串：

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > 为方便起见，代码中包含了连接字符串：这不是建议的做法，根据用例和体系结构，请考虑以更安全的方式存储此机密。

1. 添加以下代码用于连接客户端：

    ```javascript
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

1. 打开客户端，并使用 **getFileNotificationReceiver** 函数接收状态更新。

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

1. 保存并关闭 **FileUploadNotification.js** 文件。

## <a name="run-the-applications"></a>运行应用程序

现在，已准备就绪，可以运行应用程序了。

在 `fileuploadnotification` 文件夹中的命令提示符下运行以下命令：

```cmd/sh
node FileUploadNotification.js
```

在 `simulateddevice` 文件夹中的命令提示符下运行以下命令：

```cmd/sh
node SimulatedDevice.js
```

以下屏幕截图显示 **SimulatedDevice** 应用的输出：

![simulated-device 应用的输出](./media/iot-hub-node-node-file-upload/simulated-device.png)

以下屏幕截图显示 **FileUploadNotification** 应用的输出：

![read-file-upload-notification 应用的输出](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

可以使用门户查看所配置的存储容器中上传的文件：

![上传的文件](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何使用 IoT 中心的文件上传功能来简化从设备进行的文件上传。 可以使用以下文章继续探索 IoT 中心功能和方案：

* [以编程方式创建 IoT 中心](iot-hub-rm-template-powershell.md)

* [C SDK 简介](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK](iot-hub-devguide-sdks.md)
