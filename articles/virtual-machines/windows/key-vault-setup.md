---
title: 使用 PowerShell 设置 Key Vault
description: 如何使用 PowerShell 设置用于虚拟机的 Key Vault。
manager: vashan
ms.service: virtual-machines
ms.subservice: security
ms.workload: infrastructure-services
ms.topic: how-to
origin.date: 01/24/2017
author: rockboyfor
ms.date: 11/16/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 0c7ae1ae8bafae1d619b1685380e555f86b06b34
ms.sourcegitcommit: 39288459139a40195d1b4161dfb0bb96f5b71e8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590638"
---
# <a name="set-up-key-vault-for-virtual-machines-using-azure-powershell"></a>使用 Azure PowerShell 为虚拟机设置 Key Vault

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

在 Azure Resource Manager 堆栈中，密码/证书被建模为密钥保管库资源提供程序所提供的资源。 若要了解有关 Key Vault 的详细信息，请参阅[什么是 Azure Key Vault？](../../key-vault/general/overview.md)

> [!NOTE]
> 1. 为了让密钥保管库能与 Azure Resource Manager 虚拟机搭配使用，必须将密钥保管库上的 **EnabledForDeployment** 属性设置为 true。 可以在各种客户端中执行此操作。
> 2. 需要在与虚拟机相同的订阅和位置中创建 Key Vault。
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>使用 PowerShell 设置密钥保管库
若要使用 PowerShell 创建密钥保管库，请参阅[使用 PowerShell 在 Azure 密钥保管库中设置和检索机密](../../key-vault/secrets/quick-create-powershell.md)。

对于新的密钥保管库，可以使用此 PowerShell cmdlet：

```azurepowershell
New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'China East' -EnabledForDeployment
```

对于现有的密钥保管库，可以使用此 PowerShell cmdlet：

```azurepowershell
Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment
```

## <a name="use-cli-to-set-up-key-vault"></a>使用 CLI 设置密钥保管库
若要使用命令行接口 (CLI) 创建密钥保管库，请参阅[使用 CLI 管理密钥保管库](../../key-vault/general/manage-with-cli2.md#create-a-key-vault)。

使用 CLI 时，必须先创建密钥保管库，然后分配部署策略。 可以使用以下命令来执行此操作：

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "ChinaEast"
```

然后，要启用密钥保管库以用于模板部署，请运行以下命令：

```azurecli
az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"
```

## <a name="use-templates-to-set-up-key-vault"></a>使用模板设置密钥保管库
使用模板时，必须将 Key Vault 资源的 `enabledForDeployment` 属性设置为 `true`。

```config
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
        "enabledForDeployment": "true",
        ....
        ....
    }
}
```

有关使用模板创建密钥保管库时可以配置的其他选项，请参阅 [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create/)（创建密钥保管库）。

<!--MOONCAKE CUSTOMIZATION ON KEEPING THE NOTES-->

>[!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的位置、VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。

<!--MOONCAKE CUSTOMIZATION ON KEEPING THE NOTES-->
<!-- Update_Description: update meta properties, wording update, update link -->