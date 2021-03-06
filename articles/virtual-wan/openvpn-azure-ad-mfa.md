---
title: 使用 Azure AD 身份验证为 VPN 用户启用 MFA
description: 了解如何使用 Azure AD 身份验证为 VPN 用户启用 Azure 多重身份验证 (MFA)。
services: virtual-wan
ms.service: virtual-wan
ms.topic: how-to
origin.date: 09/22/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: yes
ms.testdate: 09/28/2020
ms.author: v-yeche
ms.openlocfilehash: 6e37b20d1be71e560ad9c26dc86aead2459db1b9
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93105212"
---
<!--Verified successfully-->
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users-by-using-azure-ad-authentication"></a>使用 Azure AD 身份验证为 VPN 用户启用 Azure 多重身份验证 (MFA)

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

<a name="enableauth"></a>
## <a name="enable-authentication"></a>启用身份验证

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

<a name="enablesign"></a>
## <a name="configure-sign-in-settings"></a>配置登录设置

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

<a name="peruser"></a>
## <a name="option-1---per-user-access"></a>选项 1 -“按用户”访问

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

<!--Not Available on ## Option 2 - Conditional Access-->

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件。 请参阅[为与 Azure 的点到站点连接配置 Azure AD 身份验证](virtual-wan-point-to-site-azure-ad.md)。

<!-- Update_Description: update meta properties, wording update, update link -->