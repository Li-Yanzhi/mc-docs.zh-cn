---
title: Windows 启动管理器错误 - 0xC0000428 状态无效的映像哈希
titlesuffix: Azure Virtual Machines
description: 本文提供了用于解决以下问题的步骤：使用预览版映像且试用期已过期，这会阻止启动 Azure 虚拟机 (VM)。
services: virtual-machines-windows, azure-resource-manager
manager: dcscontentpm
tags: azure-resource-manager
ms.assetid: 26c411f9-6c22-4f45-a445-c5781e547828
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
origin.date: 09/03/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: no
ms.testdate: 10/19/2020
ms.author: v-yeche
ms.openlocfilehash: 1a297e301d4a2cb319f8f28254b8324780f42a11
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93106322"
---
<!--Verfied Successfully-->
# <a name="windows-boot-manager-error---0xc0000428-status-invalid-image-hash"></a>Windows 启动管理器错误 - 0xC0000428 状态无效的映像哈希

本文提供了用于解决以下问题的步骤：使用预览版映像且试用期已过期，这会阻止启动 Azure 虚拟机 (VM)。

## <a name="symptom"></a>症状

使用[启动诊断](./boot-diagnostics.md)查看 VM 的屏幕截图时，你将看到屏幕截图显示 Windows 引导管理器，并显示以下消息：

  `File: \windows\system32\boot\winload.exe`

  `Status: 0xc0000428`

  `Info: Windows cannot verify the digital signature for this file.`

  ![图 1 显示状态为“Ox0000428”的 Windows 引导管理器，以及信息“Windows 无法验证此文件的数字签名”。](./media/windows-boot-error-invalid-image-hash/1-cannot-verify-signature.png)

或消息：

  `File: \Windows\system32\winload.exe`

  `Status: 0xc0000428`

  `Info: The digital signature for this file couldn't be verified.`

  ![图 2 显示状态为“Ox0000428”的 Windows 引导管理器，以及信息“无法验证此文件的数字签名”。](./media/windows-boot-error-invalid-image-hash/2-digital-signature-not-verified.png)

## <a name="cause"></a>原因

用于生成 VM 的映像是具有到期日期的预览版映像，而不是 RTM（发布到制造）映像。 

预览版映像具有指定的生命周期，并且在超过其到期日期（即此映像的试用期已结束）时，就会显示你看到的屏幕截图。

### <a name="example-of-preview-images"></a>预览版映像示例

`a699494373c04fc0bc8f2bb1389d6106__Windows-Server-Technical-Preview-201505.01-en.us-127GB.vhd`

无法延长预览映像的到期日期。 预览到期后，VM 无法再启动。

- 试用期因产品而异。 例如，Windows 预览版映像的试用期为 180 天。

- 在 Azure 中，所有 Windows 预览版映像都将在其说明中包含注释，该注释表示这些映像不用于生产，只能在指定的试用期内使用或作为“预览版”使用。

## <a name="solution"></a>解决方案

如果你的映像是预览版映像，则无法延长所使用的映像的有效期，你需要使用非预览版映像[部署新的 VM](../windows/quick-create-portal.md)。 以下步骤将帮助你确定是否使用了预览版映像，并提供资源来帮助你将数据从相应 VM 传输到新的 VM。 如果确实已将映像标识为预览版映像，则该映像将不可恢复，因为它现在已过期。

根据你的喜好，你可以使用 Azure PowerShell 或 Azure CLI 查询映像，以确定它是否为预览版映像。 可以使用这些命令来确认映像为预览版映像。

### <a name="query-using-azure-powershell"></a>使用 Azure PowerShell 进行查询

1. 打开 Windows PowerShell 应用程序。
1. 运行以下命令：

    ```powershell
    $locName = "<LOCATION>" 
    $pubName = "<PUBLISHER NAME>" 
    $offerName = "<OFFER NAME>" 
    $skuName= "<YEAR WHEN THIS IMAGE WAS RELEASED>"
    Get-AzVMImagePublisher -Location $locName | Select $pubName
    Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
    Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
    Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
    ```

- 在前面的命令中，将 `<LOCATION>`、`<PUBLISHER NAME>`、`<OFFER NAME>`和 `<YEAR WHEN THIS IMAGE WAS RELEASED>` 替换为所述的信息。 同时删除“<”和“>”符号。

    请参阅以下示例：

    ```powershell
    $locName = "China North" 
    $pubName = "MicrosoftWindowsServer" 
    $offerName = "WindowsServer" 
    $skuName= "2016-Datacenter"
    Get-AzVMImagePublisher -Location $locName | Select $pubName
    Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
    Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
    Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
    ```

### <a name="query-using-the-azure-cli"></a>使用 Azure CLI 进行查询

1. 如果尚未安装，则需要[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli)。
1. 下载后，使用命令提示符或 PowerShell 输入 `az login` 命令，然后使用你的帐户凭据登录。
1. 登录后，请输入以下命令：

    ```powershell
    az vm image list-publishers --location "<LOCATION>"
    az vm image list-offers --location "China North" --publisher "<PUBLISHER NAME>"
    az vm image list-skus --location "China North" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>"
    az vm image list  --location "China North" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>" --sku "<YEAR WHEN THIS IMAGE WAS RELEASED>"
    ```

- 在前面的命令中，将 `<LOCATION>`、`<PUBLISHER NAME>`、`<OFFER NAME>`和 `<YEAR WHEN THIS IMAGE WAS RELEASED>` 替换为所述的信息。 同时删除“<”和“>”符号。

    请参阅以下示例：

    ```powershell
    az vm image list-publishers --location "China North"
    az vm image list-offers --location "China North" --publisher "MicrosoftWindowsServer"
    az vm image list-skus --location "China North" --publisher "MicrosoftWindowsServer" --offer "WindowsServer"
    az vm image list  --location "China North" --publisher "MicrosoftWindowsServer" --offer "WindowsServer" --sku "2016-Datacenter"
    ```

<!-- Update_Description: update meta properties, wording update, update link -->