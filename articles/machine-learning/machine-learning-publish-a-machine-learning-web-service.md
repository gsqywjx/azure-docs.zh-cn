---
title: "部署机器学习 Web 服务 | Microsoft Docs"
description: "如何将训练实验转换为预测实验、准备进行部署、并将其部署为 Azure 机器学习 Web 服务。"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 44afeaa05ff2a8ab5a685d9f82880b0fa50f490b


---
# <a name="deploy-an-azure-machine-learning-web-service"></a>部署 Azure 机器学习 Web 服务
Azure 机器学习让用户能够生成、测试和部署预测分析解决方案。

从高级视角来看，这通过三个步骤来完成：

* **[创建训练实验]** - Azure 机器学习工作室是一个协作的可视化开发环境，可用于使用你所提供的训练数据来训练和测试预测分析模型。
* **[将其转换为预测实验]** - 利用现有数据训练模型后，你就可以使用它来对新数据进行评分，为预测准备并简化你的实验。
* **将其部署为 Web 服务** - 可将预测实验部署为[全新]或[经典] Azure Web 服务。 用户可以将数据发送到你的模型并接收该模型的预测。

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>创建训练实验
为了训练预测分析模型，使用 Azure 机器学习工作室创建一个训练实验，其中包括加载训练数据、准备必要数据、应用机器学习算法和评估结果的各种模块。 你可以迭代实验，尝试不同的机器学习算法来比较和评估结果。

在别处对创建和管理训练实验的过程进行了更详细地介绍。 有关详细信息，请参阅以下文章：

* [在Azure 机器学习工作室中创建一个简单的实验](machine-learning-create-experiment.md)
* [使用 Azure 机器学习开发预测解决方案](machine-learning-walkthrough-develop-predictive-solution.md)
* [将训练数据导入 Azure 机器学习工作室](machine-learning-data-science-import-data.md)
* [在 Azure机器学习工作室中管理实验迭代](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>将训练实验转换为预测实验
模型训练完成后，你就可以将训练实验转化为预测实验来对新数据进行评分。

通过转换为预测实验，可将训练的模型部署为评分 Web 服务。 Web 服务的用户会将输入的数据发送到模型，模型返回预测结果。 转换为预测实验后，需记住自己期望别人如何使用模型。

若要将训练实验转换为预测实验，请单击实验画布底部的“运行”，然后单击“设置 Web 服务”，选择“预测 Web 服务”。

![转换为评分实验](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

有关如何执行此转换的详细信息，请参阅[将机器学习训练实验转换为预测实验](machine-learning-convert-training-experiment-to-scoring-experiment.md)。

以下步骤描述如何将预测实验部署为新的 Web 服务。 还可将实验部署为经典 Web 服务。

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>将预测实验部署为新 Web 服务
现在，预测实验已做好准备，可将其部署为 Azure Web 服务。 使用 web 服务，用户可以将数据发送到模型，该模型将返回其预测。

若要部署预测实验，请单击实验画布底部的“运行”。 实验运行完毕后，单击“部署 Web 服务”或“部署 Web 服务[新]”。  将打开机器学习 Web 服务门户的部署页。

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>机器学习 Web 服务门户部署实验页
在部署实验页上，输入 Web 服务的名称。
选择定价计划。 如果你有现有的定价计划，可以选择它，否则你必须为该服务创建新的定价计划。

1. 在“定价计划”下拉菜单中选择一个现有计划，或选择“选择新计划”选项。
2. 在“计划名称”中，键入用于标识帐单上的计划的名称。
3. 从“每月计划层”中选择一个计划层。 计划层默认为默认区域的计划，并且 Web 服务将部署到该区域。

单击“部署”和“快速入门”页面，打开 Web 服务。

Web 服务快速入门页面提供了有关在创建 Web 服务后将执行的最常见任务的访问和指导。 从此处，你可以轻松访问测试页和使用页。

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>测试 Web 服务
若要测试新 Web 服务，请在常见任务下单击“测试 Web 服务”。 在测试页上，可将 Web 服务作为请求 - 响应服务 (RRS) 或 Batch 执行服务 (BES) 进行测试。

RRS 测试页显示你为实验定义的输入、输出和任何全局参数。 若要测试 Web 服务，可手动输入适当的输入值，或提供包含测试值的逗号分隔值 (CSV) 格式的文件。

若要使用 RRS 进行测试，请从列表视图模式中为输入键入适当的值，然后单击“测试请求 - 响应”。 预测结果显示在左侧的输出列中。

![部署 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

若要测试 BES，请单击“Batch”。 在 Batch 测试页上，单击输入下的“浏览”，然后选择包含相应示例值的 CSV 文件。 如果你没有 CSV 文件，并且已使用机器学习工作室创建了预测实验，则可以下载预测实验的数据集并使用它。

若要下载数据集，请打开机器学习工作室。 打开预测实验，右键单击实验的输入。 从上下文菜单中，选择“数据集”，然后选择“下载”。

![部署 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

单击“测试”。 Batch 执行作业的状态显示在“测试 Batch 作业”的下方。

![部署 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

在“配置”页上，可更改描述、标题，更新存储帐户密钥，以及启用 Web 服务的示例数据。

![配置 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

部署 Web 服务后，可以：

* 通过 Web 服务 API 进行**访问**。
* 通过 Azure 机器学习 Web 服务门户或 Azure 经典门户进行**管理**。
* 如果模型发生更改则对其进行**更新**。

### <a name="access-the-web-service"></a>访问 Web 服务
从机器学习工作室部署 Web 服务后，可向服务发送数据并以编程方式接收响应。

**使用**页提供访问 Web 服务所需的所有信息。 例如，提供 API 密钥以允许对服务的授权访问。

有关访问机器学习 Web 服务的详细信息，请参阅[如何使用已部署的 Azure 机器学习 Web 服务](machine-learning-consume-web-services.md)。

### <a name="manage-your-new-web-service"></a>管理新 Web 服务
可管理经典 Web 服务机器学习 Web 服务门户。 在[主门户页](https://services.azureml-test.net/)中，单击“Web 服务”。 在 Web 服务页中，可删除或复制服务。 若要监视特定服务，请单击该服务，然后单击“仪表板”。 若要监视与 Web 服务相关联的 Batch 作业，请单击“Batch 请求日志”。

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>将预测实验部署为经典 Web 服务
现在，预测试验已充分做好准备，可将其部署为 Azure web 服务。 使用 web 服务，用户可以将数据发送到模型，该模型将返回其预测。

若要部署预测实验，请单击实验画布底部的“运行”，然后单击“部署 Web 服务”。 已设置 Web 服务，你现在正位于 Web 服务仪表板中。

![部署 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

可在机器学习 Web 服务门户或机器学习工作室中测试 Web 服务。

若要测试请求响应 Web 服务，请单击 Web 服务仪表板中的“测试”按钮。 将弹出一个对话框，要求你输入服务的输入数据。 下面是评分实验预期的列。 输入一组数据，然后单击“确定”。 Web 服务生成的结果显示在仪表板的底部。

可单击“测试”预览链接，在 Azure 机器学习 Web 服务门户中测试服务，如新 Web 服务部分中所示。

若要测试 Batch 执行服务，请单击“测试”预览链接。 在 Batch 测试页上，单击输入下的“浏览”，然后选择包含相应示例值的 CSV 文件。 如果你没有 CSV 文件，并且已使用机器学习工作室创建了预测实验，则可以下载预测实验的数据集并使用它。

![测试 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

在“配置”页上，你可以更改服务的显示名称并提供说明。 名称和说明会显示在 [Azure 经典门户](http://manage.windowsazure.com/)中，可在其中管理 Web 服务。

可通过在**输入架构**、**输出架构**和 **Web 服务参数**下为每列输入字符串来为输入数据、输出数据和 Web 服务参数提供描述。 这些描述用于为 Web 服务提供的示例代码文档中。

可启用日志记录来诊断在访问 Web 服务时遇到的任何故障。 有关详细信息，请参阅[为机器学习 Web 服务启用日志记录](machine-learning-web-services-logging.md)。

![配置 Web 服务](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

还可在 Azure 机器学习 Web 服务门户中配置 Web 服务的终结点，类似于之前在“新建 Web 服务”部分中显示的过程。 选项有所不同，你可以添加或更改服务描述、启用日志记录，并启用样本数据进行测试。

### <a name="access-the-web-service"></a>访问 Web 服务
从机器学习工作室部署 Web 服务后，可向服务发送数据并以编程方式接收响应。

仪表板提供访问 Web 服务所需的所有信息。 例如，提供 API 密钥以允许对服务的授权访问，并提供 API 帮助页以帮助你开始编写代码。

有关访问机器学习 Web 服务的详细信息，请参阅[如何使用已部署的 Azure 机器学习 Web 服务](machine-learning-consume-web-services.md)。

### <a name="manage-the-web-service"></a>管理 Web 服务
可通过执行各种操作来监视 Web 服务。 可以将其更新，也可以将其删除。 除了部署经典 Web 服务时创建的默认终结点外，还可向其添加其他终结点。

有关详细信息，请参阅[管理 Azure 机器学习工作区](machine-learning-manage-workspace.md)和[使用 Azure 机器学习 Web 服务门户管理 Web 服务](machine-learning-manage-new-webservice.md)。

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>更新 Web 服务
可更改 Web 服务，例如使用其他训练数据更新模型、重新部署、覆盖原始 Web 服务等。

若要更新 Web 服务，请打开用于部署 Web 服务的原始预测实验，然后单击“另存为”以创建可编辑的副本。 进行更改，然后单击“部署 Web 服务”。

由于之前已部署此实验，系统会询问是要覆盖（经典 Web 服务）还是更新（新 Web 服务）现有服务。 单击“是”或“更新”将停止现有 Web 服务并部署新预测实验。

> [!NOTE]
> 如果在原始 Web 服务中进行了配置更改，例如输入新的显示名称或描述，则需要重新输入这些值。
> 
> 

更新 Web 服务的一种方法是以编程方式重新训练模型。 有关详细信息，请参阅[以编程方式重新训练机器学习模型](machine-learning-retrain-models-programmatically.md)。

<!-- internal links -->
[创建训练实验]: #create-a-training-experiment
[将其转换为预测实验]: #convert-the-training-experiment-to-a-predictive-experiment
[全新]: #deploy-the-predictive-experiment-as-a-new-Web-service
[经典]: #deploy-the-predictive-experiment-as-a-new-Web-service
[访问]: #access-the-Web-service
[管理]: #manage-the-Web-service-in-the-azure-management-portal
[更新]: #update-the-Web-service



<!--HONumber=Nov16_HO3-->

