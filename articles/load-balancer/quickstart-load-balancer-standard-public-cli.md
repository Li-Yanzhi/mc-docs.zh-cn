---
title: 快速入门：创建公共负载均衡器 - Azure CLI
titleSuffix: Azure Load Balancer
description: 本快速入门介绍如何使用 Azure CLI 创建公共负载均衡器
services: load-balancer
documentationcenter: na
author: WenJason
manager: digimobile
tags: azure-resource-manager
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/23/2020
ms.date: 11/16/2020
ms.author: v-jay
ms.custom: mvc, devx-track-js, devx-track-azurecli
ms.openlocfilehash: d522e7bb6277001504c96bdca1e5e5d38ceebbf7
ms.sourcegitcommit: 39288459139a40195d1b4161dfb0bb96f5b71e8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590498"
---
# <a name="quickstart-create-a-public-load-balancer-to-load-balance-vms-using-azure-cli"></a>快速入门：使用 Azure CLI 创建公共负载均衡器以对 VM 进行负载均衡

使用 Azure CLI 创建公共负载均衡器和三个虚拟机，通过这种方式开始使用 Azure 负载均衡器。

## <a name="prerequisites"></a>先决条件

- 具有活动订阅的 Azure 帐户。 [创建试用帐户](https://wd.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

本快速入门需要 Azure CLI 2.0.28 或更高版本。 若要查找版本，请运行 `az --version`。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="create-a-resource-group"></a>创建资源组

Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。

使用 [az group create](/cli/group?view=azure-cli-latest#az-group-create) 创建资源组：

* 命名为“myResourceGroupLB”。 
* 在“chinaeast2”位置。

```azurecli
  az group create \
    --name myResourceGroupLB \
    --location chinaeast2
```
---

# <a name="standard-sku"></a>[**标准 SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>对于生产型工作负载，建议使用标准 SKU 负载均衡器。 有关 sku 的详细信息，请参阅 [Azure 负载均衡器 SKU](skus.md)。

## <a name="configure-virtual-network"></a>配置虚拟网络

需要先创建支持的虚拟网络资源，然后才能部署 VM 和测试负载均衡器。

### <a name="create-a-virtual-network"></a>创建虚拟网络

使用 [az network vnet create](/cli/network/vnet?view=azure-cli-latest#az-network-vnet-createt) 创建虚拟网络：

* 命名为“myVNet”。
* 地址前缀为 10.1.0.0/16。
* 子网命名为“myBackendSubnet”。
* 子网前缀为 10.1.0.0/24。
* 在 myResourceGroupLB 资源组中。
* “chinaeast2”的位置。

```azurecli
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location chinaeast2 \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-network-security-group"></a>创建网络安全组

对于标准负载均衡器，后端地址中的 VM 需要具有属于网络安全组的网络接口。 

使用 [az network nsg create](/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) 创建网络安全组：

* 命名为“myNSG”。
* 在资源组“myResourceGroupLB”中。

```azurecli
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>创建网络安全组规则

使用 [az network nsg rule create](/cli/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) 创建网络安全组规则：

* 命名为“myNSGRuleHTTP”。
* 在上一步创建的网络安全组“myNSG”中。
* 在资源组“myResourceGroupLB”中。
* 协议为“(*)”。
* 方向为“入站”。
* 源为“(*)”。
* 目标为“(*)”。
* 目标端口为“端口 80”。
* 访问为“允许”。
* 优先级为“200”。

```azurecli
  az network nsg rule create \
    --resource-group myResourceGroupLB \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>为虚拟机创建网络接口

使用 [az network nic create](/cli/network/nic?view=azure-cli-latest#az-network-nic-create) 创建三个网络接口：

#### <a name="vm1"></a>VM1

* 命名为“myNicVM1”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。
* 在网络安全组“myNSG”中。

```azurecli

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* 命名为“myNicVM2”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。

```azurecli
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm3"></a>VM3

* 命名为“myNicVM3”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。
* 在网络安全组“myNSG”中。

```azurecli
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>创建后端服务器

在本节中，创建以下项：

* 用于服务器配置的名为 cloud-init.txt 的云配置文件。
* 三个要用作负载均衡器后端服务器的虚拟机。

### <a name="create-cloud-init-configuration-file"></a>创建 cloud-init 配置文件

使用 cloud-init 配置文件在 Linux 虚拟机上安装 NGINX 并运行“Hello World”Node.js 应用。 

在当前 shell 中，创建一个名为 cloud-init.txt 的文件。 复制以下配置并将其粘贴到 shell 中。 请确保正确复制整个 cloud-init 文件，尤其是第一行：

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### <a name="create-virtual-machines"></a>创建虚拟机

使用 [az vm create](/cli/vm?view=azure-cli-latest#az-vm-create) 创建虚拟机：

#### <a name="vm1"></a>VM1
* 命名为“myVM1”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM1”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。

```azurecli
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait
    
```
#### <a name="vm2"></a>VM2
* 命名为“myVM2”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM2”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。

```azurecli
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait
```

#### <a name="vm3"></a>VM3
* 命名为“myVM3”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM3”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。

```azurecli
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait
```
可能需要花费几分钟时间才能部署 VM。

## <a name="create-a-public-ip-address"></a>创建公共 IP 地址

若要通过 Internet 访问 Web 应用，需要负载均衡器有一个公共 IP 地址。 

使用 [az network public-ip create](/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) 执行以下操作：

* 创建名为“myPublicIP”的标准公共 IP 地址。
* 在“myResourceGroupLB”中。

```azurecli
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Standard
```


## <a name="create-standard-load-balancer"></a>创建标准负载均衡器

本部分详细介绍如何创建和配置负载均衡器的以下组件：

  * 前端 IP 池，用于在负载均衡器上接收传入网络流量。
  * 后端 IP 池，前端池将负载均衡的网络流量发送到此处。
  * 运行状况探测，用于确定后端 VM 实例的运行状况。
  * 负载均衡器规则，用于定义如何将流量分配给 VM。

### <a name="create-the-load-balancer-resource"></a>创建负载均衡器资源

使用 [az network lb create](/cli/network/lb?view=azure-cli-latest#az-network-lb-create) 创建公共负载均衡器：

* 命名为 myLoadBalancer。
* 前端池命名为 myFrontEnd。
* 后端池命名为 myBackEndPool。
* 与你在上一步中创建的公共 IP 地址 myPublicIP 关联。 

```azurecli
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Standard \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>创建运行状况探测

运行状况探测会检查所有虚拟机实例，以确保它们可以发送网络流量。 

从负载均衡器中删除未通过探测检查的虚拟机。 解决故障后，虚拟机将重新添加到负载均衡器中。

使用 [az network lb probe create](/cli/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create) 创建运行状况探测：

* 监视虚拟机的运行状况。
* 命名为“myHealthProbe”。
* 协议为“TCP”。
* 监视“端口 80”。

```azurecli
  az network lb probe create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>创建负载均衡器规则

负载均衡器规则定义：

* 针对传入流量的前端 IP 配置。
* 用于接收流量的后端 IP 池。
* 所需的源和目标端口。 

使用 [az network lb rule create](/cli/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) 创建负载均衡器规则：

* 命名为“myHTTPRule”
* 对前端池“myFrontEnd”中的“端口 80”进行侦听 。
* 使用“端口 80”将负载均衡的网络流量发送到后端地址池“myBackEndPool” 。 
* 使用运行状况探测“myHealthProbe”。
* 协议为“TCP”。
* 空闲超时 15 分钟。
* 启用 TCP 重置。


```azurecli
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true \
    --idle-timeout 15 \
    --enable-tcp-reset true

```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>将虚拟机添加到负载均衡器后端池

使用 [az network nic ip-config address-pool add](/cli/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) 将虚拟机添加到后端池：

#### <a name="vm1"></a>VM1
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM1 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM2 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM3 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

## <a name="create-outbound-rule-configuration"></a>创建出站规则配置
负载均衡器出站规则为后端池中的 VM 配置出站 SNAT。 

有关出站连接的详细信息，请参阅 [Azure 中的出站连接](load-balancer-outbound-connections.md)。

### <a name="create-outbound-public-ip-address-or-public-ip-prefix"></a>创建出站公共 IP 地址或公共 IP 前缀。

使用 [az network public-ip create](/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) 为出站连接创建单个 IP。  

使用 [az network public-ip create](/cli/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-create) 为出站连接创建公共 IP 前缀。

有关缩放出站 NAT 和出站连接的详细信息，请参阅[使用多个 IP 地址缩放出站 NAT](/load-balancer/load-balancer-outbound-connections#scale)。

#### <a name="public-ip"></a>公共 IP

* 命名为 myPublicIPOutbound。
* 在“myResourceGroupLB”中。

```azurecli
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIPOutbound \
    --sku Standard
```

#### <a name="public-ip-prefix"></a>公共 IP 前缀

* 命名为 myPublicIPPrefixOutbound。
* 在“myResourceGroupLB”中。
* 前缀长度为 28。

```azurecli
  az network public-ip prefix create \
    --resource-group myResourceGroupLB \
    --name myPublicIPPrefixOutbound \
    --length 28
```

### <a name="create-outbound-frontend-ip-configuration"></a>创建出站前端 IP 配置

使用 [az network lb frontend-ip create](/cli/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create) 创建新的前端 IP 配置：

根据上一步中的决定选择公共 IP 或公共 IP 前缀命令。

#### <a name="public-ip"></a>公共 IP

* 命名为“myFrontEndOutbound”。
* 在资源组“myResourceGroupLB”中。
* 与公共 IP 地址 myPublicIPOutbound 关联。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-address myPublicIPOutbound 
```

#### <a name="public-ip-prefix"></a>公共 IP 前缀

* 命名为“myFrontEndOutbound”。
* 在资源组“myResourceGroupLB”中。
* 与公共 IP 前缀 myPublicIPPrefixOutbound 关联。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-prefix myPublicIPPrefixOutbound 
```

### <a name="create-outbound-pool"></a>创建出站池

使用 [az network lb address create](/cli/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) 创建新的出站池：

* 命名为“myBackEndPoolOutbound”。
* 在资源组“myResourceGroupLB”中。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network lb address-pool create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myBackendPoolOutbound
```
### <a name="create-outbound-rule"></a>创建出站规则

使用 [az network lb outbound-rule create](/cli/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-create) 为出站后端池创建新的出站规则：

* 命名为“myOutboundRule”。
* 在资源组“myResourceGroupLB”中。
* 与负载均衡器 myLoadBalancer 关联
* 与前端 myFrontEndOutbound 关联。
* 协议为“所有”。
* 空闲超时时间为 15。
* 出站端口为 10000 个。
* 与后端池 myBackEndPoolOutbound 关联。

```azurecli
  az network lb outbound-rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myOutboundRule \
    --frontend-ip-configs myFrontEndOutbound \
    --protocol All \
    --idle-timeout 15 \
    --outbound-ports 10000 \
    --address-pool myBackEndPoolOutbound
```
### <a name="add-virtual-machines-to-outbound-pool"></a>向出站池添加虚拟机

使用 [az network nic ip-config address-pool add](/cli/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) 将虚拟机添加到出站池：


#### <a name="vm1"></a>VM1
* 在后端地址池“myBackEndPoolOutbound”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM1 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* 在后端地址池“myBackEndPoolOutbound”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM2 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* 在后端地址池“myBackEndPoolOutbound”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM3 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

# <a name="basic-sku"></a>[**基本 SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>对于生产型工作负载，建议使用标准 SKU 负载均衡器。 有关 sku 的详细信息，请参阅 [Azure 负载均衡器 SKU](skus.md)。

## <a name="configure-virtual-network"></a>配置虚拟网络

需要先创建支持的虚拟网络资源，然后才能部署 VM 和测试负载均衡器。

### <a name="create-a-virtual-network"></a>创建虚拟网络

使用 [az network vnet create](/cli/network/vnet?view=azure-cli-latest#az-network-vnet-createt) 创建虚拟网络：

* 命名为“myVNet”。
* 地址前缀为 10.1.0.0/16。
* 子网命名为“myBackendSubnet”。
* 子网前缀为 10.1.0.0/24。
* 在 myResourceGroupLB 资源组中。
* “chinaeast2”的位置。

```azurecli
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location chinaeast2 \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-network-security-group"></a>创建网络安全组

对于标准负载均衡器，后端地址中的 VM 需要具有属于网络安全组的网络接口。 

使用 [az network nsg create](/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) 创建网络安全组：

* 命名为“myNSG”。
* 在资源组“myResourceGroupLB”中。

```azurecli
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>创建网络安全组规则

使用 [az network nsg rule create](/cli/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) 创建网络安全组规则：

* 命名为“myNSGRuleHTTP”。
* 在上一步创建的网络安全组“myNSG”中。
* 在资源组“myResourceGroupLB”中。
* 协议为“(*)”。
* 方向为“入站”。
* 源为“(*)”。
* 目标为“(*)”。
* 目标端口为“端口 80”。
* 访问为“允许”。
* 优先级为“200”。

```azurecli
  az network nsg rule create \
    --resource-group myResourceGroupLB \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>为虚拟机创建网络接口

使用 [az network nic create](/cli/network/nic?view=azure-cli-latest#az-network-nic-create) 创建三个网络接口：

#### <a name="vm1"></a>VM1

* 命名为“myNicVM1”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。
* 在网络安全组“myNSG”中。

```azurecli

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* 命名为“myNicVM2”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。
* 在网络安全组“myNSG”中。

```azurecli
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm3"></a>VM3

* 命名为“myNicVM3”。
* 在资源组“myResourceGroupLB”中。
* 在虚拟网络“myVNet”中。
* 在子网“myBackendSubnet”中。
* 在网络安全组“myNSG”中。

```azurecli
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>创建后端服务器

在本节中，创建以下项：

* 用于服务器配置的名为 cloud-init.txt 的云配置文件。 
* 虚拟机的可用性集
* 三个要用作负载均衡器后端服务器的虚拟机。


若要验证负载均衡器是否已成功创建，请在虚拟机上安装 NGINX。

### <a name="create-cloud-init-configuration-file"></a>创建 cloud-init 配置文件

使用 cloud-init 配置文件在 Linux 虚拟机上安装 NGINX 并运行“Hello World”Node.js 应用。 

在当前 shell 中，创建一个名为 cloud-init.txt 的文件。 复制以下配置并将其粘贴到 shell 中。 请确保正确复制整个 cloud-init 文件，尤其是第一行：

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### <a name="create-availability-set-for-virtual-machines"></a>创建虚拟机的可用性集

使用 [az vm availability-set create](/cli/vm/availability-set?view=azure-cli-latest#az-vm-availability-set-create) 创建可用性集：

* 命名为“myAvSet”。
* 在资源组“myResourceGroupLB”中。
* 位置“chinaeast2”。

```azurecli
  az vm availability-set create \
    --name myAvSet \
    --resource-group myResourceGroupLB \
    --location chinaeast2 
    
```

### <a name="create-virtual-machines"></a>创建虚拟机

使用 [az vm create](/cli/vm?view=azure-cli-latest#az-vm-create) 创建虚拟机：

#### <a name="vm1"></a>VM1
* 命名为“myVM1”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM1”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。
* 在“myAvSet”可用性集中。

```azurecli
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet \
    --no-wait 
```
#### <a name="vm2"></a>VM2
* 命名为“myVM2”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM2”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。

```azurecli
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```

#### <a name="vm3"></a>VM3
* 命名为“myVM3”。
* 在资源组“myResourceGroupLB”中。
* 附加到网络接口“myNicVM3”。
* 虚拟机映像 UbuntuLTS。
* 你在上述步骤中创建的配置文件 cloud-init.txt。

```azurecli
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```
可能需要花费几分钟时间才能部署 VM。


## <a name="create-a-public-ip-address"></a>创建公共 IP 地址

若要通过 Internet 访问 Web 应用，需要负载均衡器有一个公共 IP 地址。 

使用 [az network public-ip create](/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) 执行以下操作：

* 创建名为“myPublicIP”的标准公共 IP 地址。
* 在“myResourceGroupLB”中。

```azurecli
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Basic
```

## <a name="create-basic-load-balancer"></a>创建基本负载均衡器

本部分详细介绍如何创建和配置负载均衡器的以下组件：

  * 前端 IP 池，用于在负载均衡器上接收传入网络流量。
  * 后端 IP 池，前端池将负载均衡的网络流量发送到此处。
  * 运行状况探测，用于确定后端 VM 实例的运行状况。
  * 负载均衡器规则，用于定义如何将流量分配给 VM。

### <a name="create-the-load-balancer-resource"></a>创建负载均衡器资源

使用 [az network lb create](/cli/network/lb?view=azure-cli-latest#az-network-lb-create) 创建公共负载均衡器：

* 命名为 myLoadBalancer。
* 前端池命名为 myFrontEnd。
* 后端池命名为 myBackEndPool。
* 与你在上一步中创建的公共 IP 地址 myPublicIP 关联。 

```azurecli
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Basic \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>创建运行状况探测

运行状况探测会检查所有虚拟机实例，以确保它们可以发送网络流量。 

从负载均衡器中删除未通过探测检查的虚拟机。 解决故障后，虚拟机将重新添加到负载均衡器中。

使用 [az network lb probe create](/cli/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create) 创建运行状况探测：

* 监视虚拟机的运行状况。
* 命名为“myHealthProbe”。
* 协议为“TCP”。
* 监视“端口 80”。

```azurecli
  az network lb probe create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>创建负载均衡器规则

负载均衡器规则定义：

* 针对传入流量的前端 IP 配置。
* 用于接收流量的后端 IP 池。
* 所需的源和目标端口。 

使用 [az network lb rule create](/cli/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) 创建负载均衡器规则：

* 命名为“myHTTPRule”
* 对前端池“myFrontEnd”中的“端口 80”进行侦听 。
* 使用“端口 80”将负载均衡的网络流量发送到后端地址池“myBackEndPool” 。 
* 使用运行状况探测“myHealthProbe”。
* 协议为“TCP”。
* 空闲超时 15 分钟。

```azurecli
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15
```

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>将虚拟机添加到负载均衡器后端池

使用 [az network nic ip-config address-pool add](/cli/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) 将虚拟机添加到后端池：


#### <a name="vm1"></a>VM1
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM1 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM2 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* 在后端地址池“myBackEndPool”中。
* 在资源组“myResourceGroupLB”中。
* 与网络接口 myNicVM3 和 ipconfig1 关联 。
* 与负载均衡器 myLoadBalancer 关联。

```azurecli
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```
---

## <a name="test-the-load-balancer"></a>测试负载均衡器

若要获取负载均衡器的公共 IP 地址，请使用 [az network public-ip show](/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-show)。 

复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。

```azurecli
  az network public-ip show \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```
:::image type="content" source="./media/load-balancer-standard-public-cli/running-nodejs-app.png" alt-text="测试负载均衡器" border="true":::

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组、负载均衡器和所有相关的资源，使用 [az group delete](/cli/group?view=azure-cli-latest#az-group-delete) 命令将它们删除。

```azurecli
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>后续步骤
在本快速入门中

* 你创建了一个标准或公共负载均衡器
* 附加了虚拟机。 
* 配置了负载均衡器流量规则和运行状况探测。
* 测试了负载均衡器。

若要详细了解 Azure 负载均衡器，请继续学习
> [!div class="nextstepaction"]
> [什么是 Azure 负载均衡器？](load-balancer-overview.md)
