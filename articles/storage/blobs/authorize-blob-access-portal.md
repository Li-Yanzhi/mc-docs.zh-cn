---
title: 选择如何在 Azure 门户中授予对 blob 数据的访问权限
titleSuffix: Azure Storage
description: 使用 Azure 门户访问 Blob 数据时，门户会在后台对 Azure 存储发出请求。 可以使用 Azure AD 帐户或存储帐户访问密钥对这些 Azure 存储请求进行身份验证和授权。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 09/08/2020
ms.date: 11/16/2020
ms.author: v-jay
ms.reviewer: ozgun
ms.subservice: blobs
ms.custom: contperfq1
ms.openlocfilehash: ce923b681241c81435692d2a9459e6fa2ec7982b
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552991"
---
# <a name="choose-how-to-authorize-access-to-blob-data-in-the-azure-portal"></a>选择如何在 Azure 门户中授予对 blob 数据的访问权限

使用 [Azure 门户](https://portal.azure.cn)访问 Blob 数据时，门户会在后台对 Azure 存储发出请求。 可以使用 Azure AD 帐户或存储帐户访问密钥对 Azure 存储请求进行授权。 门户会指示使用的是哪种方法，如果你有相应的权限，则门户还允许在这两种方法之间切换。  

还可以指定如何在 Azure 门户中授权单个 blob 上传操作。 门户默认使用已用于授权 blob 上传操作的任何方法，但你可以选择在上传 blob 时更改此设置。

## <a name="permissions-needed-to-access-blob-data"></a>访问 Blob 数据所需的权限

视你要如何在 Azure 门户中授权访问 Blob 数据而定，你将需要特定权限。 在大多数情况下，这些权限是通过 Azure 基于角色的访问控制 (Azure RBAC) 提供的。 有关 Azure RBAC 的详细信息，请参阅[什么是 Azure 基于角色的访问控制 (Azure RBAC)？](../../role-based-access-control/overview.md)。

### <a name="use-the-account-access-key"></a>使用帐户访问密钥

若要使用帐户访问密钥访问 blob 数据，你必须已分配到一个 Azure 角色，此角色包含 Azure RBAC 操作 **Microsoft.Storage/storageAccounts/listkeys/action**。 此 Azure 角色可以是内置角色，也可以是自定义角色。 支持 **Microsoft.Storage/storageAccounts/listkeys/action** 的内置角色包括：

- Azure 资源管理器[所有者](../../role-based-access-control/built-in-roles.md#owner)角色
- Azure 资源管理器[参与者](../../role-based-access-control/built-in-roles.md#contributor)角色
- [存储帐户参与者](../../role-based-access-control/built-in-roles.md#storage-account-contributor)角色

尝试在 Azure 门户中访问 Blob 数据时，门户首先会检查你是否拥有一个包含 **Microsoft.Storage/storageAccounts/listkeys/action** 的角色。 如果你被分配了包含此操作的角色，则门户将使用帐户密钥来访问 blob 数据。 如果你不拥有包含此操作的角色，则门户会尝试使用你的 Azure AD 帐户访问数据。

> [!NOTE]
> 经典订阅管理员角色“服务管理员”和“共同管理员”具有 Azure 资源管理器[所有者](../../role-based-access-control/built-in-roles.md#owner)角色的等效权限。 “所有者”角色包含所有操作，其中包括 **Microsoft.Storage/storageAccounts/listkeys/action**，因此，拥有其中一种管理角色的用户也可以使用帐户密钥访问 Blob 数据。 有关详细信息，请参阅[经典订阅管理员角色、Azure 角色和 Azure AD 管理员角色](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles)。

### <a name="use-your-azure-ad-account"></a>使用 Azure AD 帐户

若要使用 Azure AD 帐户从 Azure 门户访问 Blob 数据，必须符合以下条件：

- 至少拥有 Azure 资源管理器[读取者](../../role-based-access-control/built-in-roles.md#reader)角色，该角色的权限范围为存储帐户或更高级别。 “读取者”角色授予限制性最高的权限，但也接受可授予存储帐户管理资源访问权限的其他 Azure 资源管理器角色。
- 拥有一个可提供 blob 数据访问权限的内置角色或自定义角色。

必须提供“读取者”角色分配或其他 Azure 资源管理器角色分配，使用户能够在 Azure 门户中查看和导航存储帐户管理资源。 授予 blob 数据访问权限的 Azure 角色不会授予存储帐户管理资源访问权限。 若要在门户中访问 Blob 数据，用户需要拥有在存储帐户资源中导航的权限。 有关此要求的详细信息，请参阅[分配“读取者”角色以访问门户](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access)。

支持访问 Blob 数据的内置角色包括：

- [存储 Blob 数据所有者](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)：用于对 Azure Data Lake Storage Gen2 进行 POSIX 访问控制。
- [存储 Blob 数据参与者](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)：对 Blob 的读取/写入/删除权限。
- [存储 Blob 数据读取者](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader)：对 Blob 的只读权限。

自定义角色能够支持内置角色所提供的相同权限的不同组合。 若要详细了解如何创建 Azure 自定义角色，请参阅 [Azure 自定义角色](../../role-based-access-control/custom-roles.md)和[了解 Azure 资源的角色定义](../../role-based-access-control/role-definitions.md)。

## <a name="navigate-to-blobs-in-the-azure-portal"></a>在 Azure 门户中导航到 Blob

若要在门户中查看 Blob 数据，请导航到存储帐户的“概述”，然后单击“Blob”对应的链接。  或者，可以在菜单中导航到“Blob 服务”部分。

:::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="显示如何在 Azure 门户中导航到 Blob 数据的屏幕截图":::

## <a name="determine-the-current-authentication-method"></a>确定当前的身份验证方法

导航到容器时，Azure 门户会指示当前是使用帐户访问密钥还是 Azure AD 帐户进行身份验证。

### <a name="authenticate-with-the-account-access-key"></a>使用帐户访问密钥进行身份验证

如果使用帐户访问密钥进行身份验证，则会在门户中看到“访问密钥”已指定为身份验证方法：

:::image type="content" source="media/authorize-blob-access-portal/auth-method-access-key.png" alt-text="显示用户当前正在使用帐户密钥访问容器的屏幕截图":::

若要改用 Azure AD 帐户，请单击图中突出显示的链接。 如果你通过分配给你的 Azure 角色获得了相应的权限，则可以继续访问。 但是，如果你缺少相应的权限，则会看到如下所示的错误消息：

:::image type="content" source="media/authorize-blob-access-portal/auth-error-azure-ad.png" alt-text="Azure AD 帐户不支持访问时显示的错误":::

请注意，如果你的 Azure AD 帐户缺少 Blob 查看权限，则列表中不会显示任何 Blob。 单击“切换为访问密钥”链接，以再次使用访问密钥进行身份验证。

### <a name="authenticate-with-your-azure-ad-account"></a>使用 Azure AD 帐户进行身份验证

如果使用 Azure AD 帐户进行身份验证，则会在门户中看到“Azure AD 用户帐户”已指定为身份验证方法：

:::image type="content" source="media/authorize-blob-access-portal/auth-method-azure-ad.png" alt-text="显示用户当前正在使用 Azure AD 帐户访问容器的屏幕截图":::

若要改用帐户访问密钥，请单击图中突出显示的链接。 如果你有权访问帐户密钥，则可以继续访问。 但是，如果你缺少帐户密钥的访问权限，则会看到如下所示的错误消息：

:::image type="content" source="media/authorize-blob-access-portal/auth-error-access-key.png" alt-text="无权访问帐户密钥时显示的错误":::

请注意，如果你无权访问帐户密钥，则列表中不会显示任何 Blob。 单击“切换为 Azure AD 用户帐户”链接，以再次使用 Azure AD 帐户进行身份验证。

## <a name="specify-how-to-authorize-a-blob-upload-operation"></a>指定如何授权 blob 上传操作

从 Azure 门户上传 blob 时，可以指定是使用帐户访问密钥还是使用 Azure AD 凭据对该操作进行身份验证和授权。 门户默认使用当前身份验证方法，如[确定当前身份验证方法](#determine-the-current-authentication-method)中所示。

要指定如何授权 blob 上传操作，请按照以下步骤操作：

1. 在 Azure 门户中，导航到要在其中上传 blob 的容器。
1. 选择“上传”按钮。
1. 展开“高级”部分，显示 blob 的高级属性。
1. 在“身份验证类型”字段中，指示是使用 Azure AD 帐户还是帐户访问密钥授权上传操作，如下图所示：

    :::image type="content" source="media/authorize-blob-access-portal/auth-blob-upload.png" alt-text="显示如何在上传 blob 时更改授权方法的屏幕截图":::

## <a name="next-steps"></a>后续步骤

- [使用 Azure Active Directory 验证对 Azure Blob 和队列的访问权限](../common/storage-auth-aad.md)
- [使用 Azure 门户为 blob 和队列数据分配 Azure 角色](../common/storage-auth-aad-rbac-portal.md)
- [使用 Azure CLI 分配用于访问 blob 和队列数据的 Azure 角色](../common/storage-auth-aad-rbac-cli.md)
- [使用 Azure PowerShell 模块分配用于访问 blob 和队列数据的 Azure 角色](../common/storage-auth-aad-rbac-powershell.md)
