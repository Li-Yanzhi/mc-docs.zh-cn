---
title: 使用 Azure Powershell 创建和加密 Linux VM
description: 本快速入门介绍如何使用 Azure Powershell 创建和加密 Linux 虚拟机
author: Johnnytechn
ms.author: v-johya
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: quickstart
origin.date: 05/17/2019
ms.date: 09/03/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 83465b1509054e15bd369fab49779fc0a6d52015
ms.sourcegitcommit: f45809a2120ac7a77abe501221944c4482673287
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2020
ms.locfileid: "90057529"
---
<!--Verified successfully-->
# <a name="quickstart-create-and-encrypt-a-linux-vm-in-azure-with-azure-powershell"></a>快速入门：在 Azure 中使用 Azure PowerShell 创建和加密 Linux VM

Azure PowerShell 模块用于从 PowerShell 命令行或脚本创建和管理 Azure 资源。 本快速入门介绍如何使用 Azure PowerShell 模块创建 Linux 虚拟机 (VM)、创建用于存储加密密钥的密钥保管库以及加密 VM。 本快速入门使用 Canonical 提供的 Ubuntu 16.04 LTS 市场映像和 VM Standard_D2S_V3 大小。 

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器：

```azurepowershell
New-AzResourceGroup -Name "myResourceGroup" -Location "ChinaEast2"
```

## <a name="create-a-virtual-machine"></a>创建虚拟机

使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) 创建 Azure 虚拟机，并将前面创建的 VM 配置对象传递给它。

```powershell
$cred = Get-Credential

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:18.04-LTS:latest -Size Standard_D2S_V3
```

部署 VM 需要数分钟。 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>创建为加密密钥配置的密钥保管库

Azure 磁盘加密将其加密密钥存储在 Azure 密钥保管库中。 使用 [New-AzKeyvault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) 创建一个密钥保管库。 要使密钥保管库能够存储加密密钥，请使用 -EnabledForDiskEncryption 参数。

> [!Important]
> 每个密钥保管库必须有一个在 Azure 中唯一的名称。 在下面的示例中，将 <your-unique-keyvault-name> 替换为你选择的名称。

```azurepowershell
New-AzKeyvault -name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location ChinaEast2 -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>加密虚拟机

使用 [Set-AzVmDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) 加密 VM。 

Set-AzVmDiskEncryptionExtension 需要密钥保管库对象中的一些值。 可以通过将密钥保管库的唯一名称传递给 [Get-AzKeyvault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault) 来获取这些值。

```azurepowershell
$KeyVault = Get-AzKeyVault -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup"

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName "MyVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

几分钟后，进程将返回以下内容：

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

可以通过运行 [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus) 来验证加密过程。

```azurepowershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

启用加密后，你将在返回的输出中看到以下内容：

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>清理资源

不再需要时，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) cmdlet 删除资源组、VM 和所有相关资源：

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了一个虚拟机，创建了一个启用加密密钥的密钥保管库，并对 VM 进行了加密。  请继续学习下一篇文章，详细了解 Linux VM 的 Azure 磁盘加密。

> [!div class="nextstepaction"]
> [Azure 磁盘加密概述](disk-encryption-overview.md)

<!-- Update_Description: new article about disk encryption powershell quickstart -->
<!--NEW.date: 11/11/2019-->