---
title: "如何通过 Ruby 使用 Azure 表存储 | Microsoft Docs"
description: "使用 Azure 表存储（一种 NoSQL 数据存储）将结构化数据存储在云中。"
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 372bc89f75ad4730f0defbf9d6f9f041ae5ce1bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-table-storage-from-ruby"></a>如何通过 Ruby 使用 Azure 表存储
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>概述
本指南演示如何使用 Azure 表服务执行常见任务。 相关示例是使用 Ruby API 编写的。 涉及的情景包括**创建和删除表、在表中插入和查询条目**。

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>创建 Ruby 应用程序
有关如何创建 Ruby 应用程序的说明，请参阅 [Azure VM 上的 Ruby on Rails Web 应用程序](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)。

## <a name="configure-your-application-to-access-storage"></a>配置应用程序以访问存储
要使用 Azure 存储，需要下载和使用 Ruby azure 包，其中包括一组便于与存储 REST 服务进行通信的库。

### <a name="use-rubygems-to-obtain-the-package"></a>使用 RubyGems 获取该程序包
1. 使用命令行接口，例如 **PowerShell** (Windows)、**Terminal** (Mac) 或 **Bash** (Unix)。
2. 在命令窗口中键入 **gem install azure** 以安装 gem 和依赖项。

### <a name="import-the-package"></a>导入包
使用常用的文本编辑器将以下内容添加到要在其中使用存储的 Ruby 文件的顶部：

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>设置 Azure 存储连接
Azure 模块会读取环境变量 **AZURE\_STORAGE\_ACCOUNT** 和 **AZURE\_STORAGE\_ACCESS\_KEY** 以获取连接到 Azure 存储帐户所需的信息。 如果未设置这些环境变量，则在使用 **Azure::TableService** 之前必须通过以下代码指定帐户信息：

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

从 Azure 门户中的经典或 Resource Manager 存储帐户中获取这些值：

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 导航到要使用的存储帐户。
3. 在右侧的“设置”边栏选项卡中，单击“访问密钥”。
4. 在出现的“访问密钥”边栏选项卡中，将看到访问密钥 1 和访问密钥 2。 可以使用其中任意一个。
5. 单击复制图标以将键复制到剪贴板。

## <a name="create-a-table"></a>创建表
使用 **Azure::TableService** 对象可以对表和实体进行操作。 若要创建表，请使用 **create\_table()** 方法。 以下示例将创建表或输出存在的错误。

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>向表中添加条目
要添加实体，应首先创建一个定义了实体属性的哈希对象。 请注意，必须为每个实体指定 **PartitionKey** 和 **RowKey**。 这些值是实体的唯一标识符，并且查询它们比查询其他属性快很多。 Azure 存储使用 **PartitionKey** 将表的实体自动分发到多个存储节点。 具有相同 **PartitionKey** 的实体存储在同一个节点上。 **RowKey** 是实体在其所属分区内的唯一 ID。

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>更新实体
可使用多种方法来更新现有实体：

* **update\_entity()：**通过替换更新现有实体。
* **merge\_entity()：**通过将新属性值合并到现有实体来更新现有实体。
* **insert\_or\_merge\_entity()：**通过替换更新现有实体。 如果不存在实体，将插入一个新实体。
* **insert\_or\_replace\_entity()：**通过将新属性值合并到现有实体来更新现有实体。 如果不存在实体，将插入一个新实体。

以下示例演示使用 **update\_entity()** 更新实体：

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

对于 **update\_entity()** 和 **merge\_entity()**，如果要更新的实体不存在，更新操作会失败。 因此，如果希望存储某个实体而不用考虑它是否已存在，则应改用 **insert\_or\_replace\_entity()** 或 **insert\_or\_merge\_entity()**。

## <a name="work-with-groups-of-entities"></a>使用实体组
有时，有必要批量同时提交多项操作以确保通过服务器进行原子处理。 要完成此操作，首先要创建一个 **Batch** 对象，然后对 **TableService** 使用 **execute\_batch()** 方法。 下面的示例演示在一个批次中提交 RowKey 为 2 和 3 的两个实体。 请注意，这仅适用于具有相同 PartitionKey 的实体。

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>查询实体
若要查询表中的实体，请使用 **get\_entity()** 方法并传递表名称、**PartitionKey** 和 **RowKey**。

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>查询实体集
若要查询表中的一组实体，请创建查询哈希对象并使用 **query\_entities()** 方法。 以下示例演示如何获取具有相同 **PartitionKey** 的所有实体：

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> 如果结果集太大，一个查询无法全部返回，会返回一个继续标记，可以使用该标记检索后续页面。
>
>

## <a name="query-a-subset-of-entity-properties"></a>查询一部分实体属性
对表的查询可以只检索实体中的少数几个属性。 此方法称为“投影”，可减少带宽并提高查询性能，尤其适用于大型实体。 请使用 select 子句并传递希望显示给客户端的属性的名称。

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>删除实体
若要删除实体，请使用 **delete\_entity()** 方法。 需要传入包含该实体的表的名称、实体的 PartitionKey 和 RowKey。

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>删除表
若要删除表，请使用 **delete\_table()** 方法并传入要删除的表的名称。

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>后续步骤

* [Microsoft Azure 存储资源管理器](../vs-azure-tools-storage-manage-with-storage-explorer.md)是 Microsoft 免费提供的独立应用，适用于在 Windows、macOS 和 Linux 上以可视方式处理 Azure 存储数据。
* GitHub 上的 [Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) 存储库

