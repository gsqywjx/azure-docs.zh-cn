---
title: "使用 Azure CLI 1.0 在 Linux VM 上安装 MongoDB | Microsoft Docs"
description: "了解如何使用 Resource Manager 部署模型在 Azure 中的 Linux 虚拟机上安装和配置 MongoDB。"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>如何使用 Azure CLI 1.0 在 Linux VM 上安装和配置 MongoDB
[MongoDB](http://www.mongodb.org) 是一个流行的开源、高性能 NoSQL 数据库。 本文说明如何使用 Resource Manager 部署模型在 Azure 中的 Linux VM 上安装和配置 MongoDB。 文中提供了一些示例，详细说明如何执行以下操作：

* [手动安装和配置基本 MongoDB 实例](#manually-install-and-configure-mongodb-on-a-vm)
* [使用 Resource Manager 模板创建基本 MongoDB 实例](#create-basic-mongodb-instance-on-centos-using-a-template)
* [使用 Resource Manager 模板创建包含副本集的复杂 MongoDB 分片群集](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>用于完成任务的 CLI 版本
可以使用以下 CLI 版本之一完成任务：

- Azure CLI 1.0 – 用于经典部署模型和资源管理部署模型（本文）的 CLI
- [Azure CLI 2.0](create-cli-complete-nodejs.md) - 适用于资源管理部署模型的下一代 CLI


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>在 VM 上手动安装和配置 MongoDB
MongoDB 为 Red Hat/CentOS、SUSE、Ubuntu 和 Debian 等 Linux 分发版[提供了安装说明](https://docs.mongodb.com/manual/administration/install-on-linux/)。 以下示例使用 ~/.ssh/id_rsa.pub 中存储的 SSH 密钥创建 CentOS VM。 出现提供存储帐户名称、DNS 名称和管理员凭据的提示时，请输入所需的信息：

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

使用上述 VM 创建步骤结束时显示的公共 IP 地址登录到 VM：

```bash
ssh azureuser@40.78.23.145
```

若要为 MongoDB 添加安装源，请按如下所示创建一个 yum 存储库文件：

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

打开该 MongoDB 存储库文件进行编辑。 添加以下行：

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

按如下所示使用 yum 安装 MongoDB：

```bash
sudo yum install -y mongodb-org
```

默认情况下，CentOS 映像中强制实施 SELinux，阻止访问 MongoDB。 安装策略管理工具并配置 SELinux 可让 MongoDB 在其默认 TCP 端口 27017 上运行，如下所示。 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

按如下所示启动 MongoDB 服务：

```bash
sudo service mongod start
```

通过使用本地 `mongo` 客户端进行连接来验证 MongoDB 安装：

```bash
mongo
```

现在，通过添加一些数据并执行搜索来测试 MongoDB 实例：

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

如果需要，可将 MongoDB 配置为在系统重新引导期间自动启动：

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>使用模板在 CentOS 上创建基本 MongoDB 实例
可以使用 GitHub 中的以下 Azure 快速入门模板，在单个 CentOS VM 上创建基本的 MongoDB 实例。 此模板使用适用于 Linux 的自定义脚本扩展将 `yum` 存储库添加到新建的 CentOS VM，然后安装 MongoDB。

* [CentOS 上的基本 MongoDB 实例](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

以下示例在 `eastus` 区域中创建名为 `myResourceGroup` 的资源组。 按如下所示输入自己的值：

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> 创建部署后的几秒内，Azure CLI 会返回到提示符，但安装和配置需要几分钟才能完成。 使用 `azure group deployment show myResourceGroup` 检查部署状态，并相应地输入资源组的名称。 等到 ProvisioningState 显示“已成功”后，尝试通过 SSH 连接到 VM。

完成部署后，通过 SSH 连接到 VM。 按以下示例中所示，使用 `azure vm show` 命令获取 VM 的 IP 地址：

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

在靠近输出末尾处显示了公共 IP 地址。 使用 VM 的 IP 地址通过 SSH 连接到 VM：

```bash
ssh azureuser@138.91.149.74
```

如下所示，通过使用本地 `mongo` 客户端进行连接来验证 MongoDB 安装：

```bash
mongo
```

现在，按如下所示，通过添加一些数据并执行搜索来测试实例：

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>使用模板在 CentOS 上创建复杂的 MongoDB 分片群集
可以使用 Github 中的以下 Azure 快速入门模板创建复杂的 MongoDB 分片群集。 此模板遵循 [MongoDB 分片群集最佳实践](https://docs.mongodb.com/manual/core/sharded-cluster-components/)来提供冗余和高可用性。 该模板将创建两个分片，其中每个副本集包含三个节点。 另外，它还会创建一个配置服务器副本集和两个 mongos 路由器服务器，前者包含 3 个节点的，后者用于确保各分片中的应用程序保持一致。

* [CentOS 上的 MongoDB 分片群集](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> 部署这种复杂 MongoDB 分片群集需要 20 个以上的核心（每个区域中一个订阅的默认核心计数通常为 20 个）。 请提出 Azure 支持请求，以增加核心计数。

以下示例在 eastus 区域中创建名为 myResourceGroup 的资源组。 按如下所示输入自己的值：

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> 创建部署后的几秒内，Azure CLI 会返回到提示符，但安装和配置可能需要一小时以上才能完成。 使用 `azure group deployment show myResourceGroup` 检查部署状态，并相应地调整资源组的名称。 等到 ProvisioningState 显示“已成功”后，可连接到 VM。


## <a name="next-steps"></a>后续步骤
在上述示例中，已在本地从 VM 连接到 MongoDB 实例。 如果想要从另一个 VM 或网络连接到 MongoDB 实例，请确保[创建相应的网络安全组规则](nsg-quickstart.md)。

有关使用模板创建这些规则的详细信息，请参阅 [Azure Resource Manager 概述](../../azure-resource-manager/resource-group-overview.md)。

Azure Resource Manager 模板使用自定义脚本扩展在 VM 上下载并执行脚本。 有关详细信息，请参阅[在 Linux 虚拟机上使用 Azure 自定义脚本扩展](extensions-customscript.md)。

