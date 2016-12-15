---
title: "监视和管理 Azure 数据工厂管道"
description: "了解如何使用监视和管理应用来监视和管理 Azure 数据工厂和管道。"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 0bb9269d20c157221faa5604e68acc3410a5247b
ms.openlocfilehash: d8e5e8e99d40af959f8fa1dd4bf7b636a9f2343b


---
# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>使用新的监视和管理应用来监视和管理 Azure 数据工厂管道
> [!div class="op_single_selector"]
> * [使用 Azure 门户/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [使用监视和管理应用](data-factory-monitor-manage-app.md)
> 
> 

本文介绍如何使用**监视和管理应用**监视、管理和调试管道，并创建警报以接收故障通知。 还可观看以下视频了解如何使用监视和管理应用。

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
> 
> 

## <a name="launching-the-monitoring-and-management-app-a"></a>启动监视和管理应用
若要启动监视和管理应用，请针对数据工厂单击“数据工厂”边栏选项卡上的“监视和管理”磁贴。

![在数据工厂主页上监视磁贴](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

可以看到管理和监视应用在单独选项卡/窗口中启动。  

![监视和管理应用](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> 如果 Web 浏览器卡在“正在授权...”处，请禁用或取消选中“阻止第三方 Cookie 和站点数据”设置，或在保持启用的状态下为 **login.microsoftonline.com** 创建一个例外，然后尝试再次启动该应用。
> 
> 

如果未在底部的列表中看到活动窗口，请单击工具栏上的“刷新”按钮以刷新列表。 此外，为“开始时间”和“结束时间”筛选器设置正确的值。  

## <a name="understanding-the-monitoring-and-management-app"></a>了解监视和管理应用
左侧有三个选项卡（“资源浏览器”、“监视视图”和“警报”），默认选中第一个选项卡（资源浏览器）。 

### <a name="resource-explorer"></a>资源浏览器
您会看到如下内容： 

* 左侧窗格中的资源浏览器**树视图**。
* 顶部的**图示视图**。
* 中间窗格底部的“活动窗口”列表。
* 右侧窗格中的“属性”/“活动窗口资源管理器”选项卡。 

在资源浏览器中，可在树视图的数据工厂中查看所有资源（管道、数据集、链接服务）。 在资源浏览器中选择对象时，请注意以下内容： 

* 在图示视图中突出显示相关的数据工厂实体。
* 在底部“活动窗口”列表中突出显示相关活动窗口（若要了解活动窗口，请单击[此处](data-factory-scheduling-and-execution.md)）。  
* 右侧窗格“属性”窗口中所选对象的属性。 
* 所选对象的 JSON 定义（如果适用）。 例如：链接服务、数据集或管道。 

![资源浏览器](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

有关活动窗口的详细概念性信息，请参阅[计划和执行](data-factory-scheduling-and-execution.md)一文。 

### <a name="diagram-view"></a>图示视图
数据工厂的图示视图提供单个窗格来监视和管理数据工厂及其资产。 在图示视图中选择数据工厂实体（数据集/管道）时，请注意以下内容：

* 在树视图中选择数据工厂实体
* 在“活动窗口”列表中突出显示相关的活动窗口。
* “属性”窗口中所选对象的属性

管道已启用（未处于暂停状态）时，将显示绿线。 

![正在运行的管道](./media/data-factory-monitor-manage-app/PipelineRunning.png)

可注意到图示视图中有三个管道命令按钮。 可以使用第二个按钮暂停管道。 暂停不会终止当前正在运行的活动，使其能够继续完成。 第三个按钮暂停管道并终止其现有的执行活动。 第一个按钮恢复管道。 暂停管道后，可以注意到管道磁贴的颜色发生更改，如下所示。

![在磁贴上暂停/恢复](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

每次可（使用 Ctrl）选择两个或多个管道，并使用命令栏按钮来暂停/恢复多个管道。

![在命令栏上暂停/恢复](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

通过右键单击管道磁贴，然后单击“打开管道”可看到管道中的所有活动。

![“打开管道”菜单](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

在打开的管道视图中，可以看到管道中的所有活动。 在此示例中，只有一个活动：复制活动。 若要返回到上一视图，请单击顶部痕迹菜单中的数据工厂名称。

![打开的管道](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

在管道视图中，单击输出数据集或将鼠标移动到输出数据集时，可以看到该数据集的活动窗口弹出消息。

![活动窗口弹出消息](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

可以在右侧窗格的“属性”窗口中单击活动窗口以查看其详细信息。 

![活动窗口属性](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

在右侧窗格中，切换到“活动窗口资源管理器”选项卡以查看更多详细信息。

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

还可在“尝试”部分看到每个活动运行尝试的**已解析变量**。 

![已解析变量](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

切换到“脚本”选项卡以查看所选对象的 JSON 脚本定义。   

![脚本选项卡](./media/data-factory-monitor-manage-app/ScriptTab.png)

可在三处看到活动窗口：

* 图示视图（中间窗格）中的活动窗口弹出消息。
* 右侧窗格中的活动窗口资源管理器。
* 底部窗格中的活动窗口列表。

在活动窗口弹出消息和活动窗口资源管理器中，可使用向左、向右键滚动到上一周和下一周。

![活动窗口资源管理器向左/向右键](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

在图示视图底部，可看到用于放大、缩小、缩放到合适大小、比例 100%、锁定布局的按钮。 锁定布局按钮可阻止意外移动图示视图中的表和管道，其默认为“打开”。 可将其关闭，还可在图示中移动实体。 关闭后，可使用最后一个按钮来自动放置表和管道。 还可以使用鼠标滚轮进行放大/缩小。

![图示视图缩放命令](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>活动窗口列表
中间窗格底部的活动窗口列表可显示在资源浏览器或图示视图中所选数据集的所有活动窗口。 默认情况下，列表按降序排序，这意味着可在顶部看到最新的活动窗口。 

![活动窗口列表](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

此列表不会自动刷新，因此请使用工具栏上的刷新按钮对其进行手动刷新。  

活动窗口可处于以下任一状态：

<table>
<tr>
    <th align="left">状态</th><th align="left">子状态</th><th align="left">说明</th>
</tr>
<tr>
    <td rowspan="8">等待</td><td>ScheduleTime</td><td>未到运行活动窗口的时间。</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>上游依赖关系未准备就绪。</td>
</tr>
<tr>
<td>ComputeResources</td><td>计算资源不可用。</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>所有活动实例都忙于运行其他活动窗口。</td>
</tr>
<tr>
<td>ActivityResume</td><td>活动暂停，且在恢复活动之前无法运行活动窗口。</td>
</tr>
<tr>
<td>重试</td><td>已重试活动执行。</td>
</tr>
<tr>
<td>验证</td><td>验证尚未开始。</td>
</tr>
<tr>
<td>ValidationRetry</td><td>正在等待重试验证。</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>正在验证</td><td>正在进行验证。</td>
</tr>
<td>-</td>
<td>正在处理活动窗口。</td>
</tr>
<tr>
<td rowspan="4">已失败</td><td>已超时</td><td>执行时间超过活动允许时间。</td>
</tr>
<tr>
<td>已取消</td><td>已由用户操作取消。</td>
</tr>
<tr>
<td>验证</td><td>验证失败。</td>
</tr>
<tr>
<td>-</td><td>未能生成和/或验证活动窗口。</td>
</tr>
<td>就绪</td><td>-</td><td>活动窗口已就绪，可供使用。</td>
</tr>
<tr>
<td>已跳过</td><td>-</td><td>未处理活动窗口。</td>
</tr>
<tr>
<td>无</td><td>-</td><td>用于以不同状态存在但已被重置的活动窗口。</td>
</tr>
</table>


单击列表中的活动窗口时，可在右侧“活动窗口资源管理器”或“属性”窗口中看到其详细信息。

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>刷新活动窗口
详细信息不会自动刷新，因此需使用命令栏上的“刷新”按钮（第二个按钮）手动刷新活动窗口列表。  

### <a name="properties-window"></a>属性窗口
属性窗口位于监视和管理应用的最右侧窗格中。 

![属性窗口](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

它将显示在资源浏览器（树视图）（或）图示视图（或）活动窗口列表中所选项的属性。 

### <a name="activity-window-explorer"></a>活动窗口资源管理器
**活动窗口资源管理器**位于监视和管理应用的最右侧窗格中。 它将显示在活动窗口弹出消息或活动窗口列表中所选活动窗口的相关详细信息。 

![活动窗口资源管理器](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

通过在顶部日历视图中单击活动窗口资源管理器，可切换到另一活动窗口。 还可使用顶部的“向左键”/“向右键”按钮查看上/下周的活动窗口。

在底部窗格中，可使用工具栏按钮来**重新运行**活动窗口或**刷新**窗格中的详细信息。 

### <a name="script"></a>脚本
可以使用“脚本”选项卡来查看所选数据工厂实体（链接服务、数据集和管道）的 JSON 定义。 

![脚本选项卡](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>使用系统视图
使用监视和管理应用附带的预建系统视图（“最近活动窗口”、“失败活动窗口”、“进行中活动窗口”），可以查看数据工厂的最近/失败/进行中活动窗口。 

通过单击左侧“监视视图”选项卡切换到此视图。 

![监视视图选项卡](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

目前支持三种系统视图。 选择一个选项以查看活动窗口列表（位于中间窗格底部）中的最近活动窗口（或）失败活动窗口（或）进行中活动窗口。 

选择“最近活动窗口”选项时，可看到按**上次尝试时间**降序排序的所有最近活动窗口。 

可以使用“失败活动窗口”视图来查看列表中所有失败的活动窗口。 选择列表中失败的活动窗口，可在“属性”窗口（或）“活动窗口资源管理器”中查看其详细信息。 还可以下载失败活动窗口的任何日志。 

## <a name="sorting-and-filtering-activity-windows"></a>排序和筛选活动窗口
更改命令栏中的“开始时间”和“结束时间”设置，以筛选活动窗口。 更改“开始时间”和“结束时间”后，单击结束时间旁边的按钮以刷新活动窗口列表。

![开始和结束时间](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> 目前，监视和管理应用中的所有时间均为 UTC 格式。 
> 
> 

在“活动窗口列表”中，单击某一列的名称（例如：状态）。 

![活动窗口列表列菜单](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

你可以执行以下操作：

* 按升序排序。
* 按降序排序。
* 按一个或多个值（就绪、等待等）筛选

指定列上的筛选器时，将看到为该列启用的筛选器按钮，以指示列中的值为已筛选值。 

![在活动窗口列表的列中进行筛选](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

可以使用相同的弹出窗口来清除筛选器。 若要清除活动窗口列表的所有筛选器，请单击命令栏上的清除筛选器按钮。 

![清除活动窗口列表中的所有筛选器](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="performing-batch-actions"></a>执行批处理操作
### <a name="rerun-selected-activity-windows"></a>重新运行选定的活动窗口
选择活动窗口，单击第一个命令栏按钮的向下箭，然后选择“重新运行” / “重新运行管道上游”。 选择“重新运行管道上游”选项时，将重新运行所有上游活动窗口。 
    ![重新运行活动窗口](./media/data-factory-monitor-manage-app/ReRunSlice.png)

还可在列表中选择多个活动窗口，并同时重新运行它们。 可能希望基于状态筛选活动窗口（例如：**失败**），然后在解决导致活动窗口失败的问题后重新运行失败的活动窗口。 有关在列表中筛选活动窗口的详细信息，请参阅以下部分。  

### <a name="pauseresume-multiple-pipelines"></a>暂停/恢复多个管道
每次可（使用 Ctrl）选择两个或多个管道，然后使用命令栏按钮（在下图中以红色矩形突出显示）来暂停/恢复它们。

![在命令栏上挂起/恢复](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>创建警报
使用“警报”页，可创建警报、查看/编辑/删除现有警报。 还可以禁用/启用警报。 若要查看“警报”页，请单击“警报”选项卡。

![警报选项卡](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>创建警报的方式
1. 单击“添加警报”以添加警报。 随即显示“详细信息”页。 
   
    ![创建警报 -“详细信息”页](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. 指定警报的“名称”和“说明”，然后单击“下一步”。 随即显示“筛选器”页。
   
    ![创建警报 -“筛选器”页](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. 选择希望数据工厂服务对其发出通知的“事件”、“状态”和“子状态”（可选），然后单击“下一步”。 随即显示“收件人”页。
   
    ![创建警报 -“收件人”页](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
4. 选择“电子邮件订阅管理员”选项和/或输入“其他管理员电子邮件”，然后单击“完成”。 随即可在列表中看到警报。 
   
    ![警报列表](./media/data-factory-monitor-manage-app/AlertsList.png)

在“警报”列表中，使用与警报关联的按钮编辑/删除/禁用/启用警报。 

### <a name="eventstatussubstatus"></a>事件/状态/子状态
下表提供了可用事件和状态（和子状态）的列表。

| 事件名称 | 状态 | 子状态 |
| --- | --- | --- |
| 活动运行已启动 |已启动 |正在启动 |
| 活动运行已完成 |已成功 |已成功 |
| 活动运行已完成 |已失败 |资源分配失败<br/><br/>执行失败<br/><br/>已超时<br/><br/>验证失败<br/><br/>已放弃 |
| 按需 HDI 群集创建已开始 |已启动 |-|
| 已成功创建按需 HDI 群集 |已成功 |-|
| 按需 HDI 群集已删除 |已成功 |-|

### <a name="to-editdeletedisable-an-alert"></a>编辑/删除/禁用警报
![警报按钮](./media/data-factory-monitor-manage-app/AlertButtons.png)




<!--HONumber=Nov16_HO3-->

