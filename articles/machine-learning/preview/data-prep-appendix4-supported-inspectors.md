---
title: "适用于 Azure 机器学习数据准备的受支持检查器 | Microsoft Docs"
description: "本文档提供适用于 Azure 机器学习数据准备的检查器的完整列表"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 35d7c04f245e93d8cc795dca7c01c2bab5a14eb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>适用于 Azure 机器学习数据准备预览版的受支持检查器
本文档概述了可用于此预览版的一组检查器。

## <a name="the-halo-effect"></a>光圈效果 
某些检查器支持 halo 效果。 该效果使用两种不同的颜色立即直观地显示转换所带来的更改。 灰色表示最新转换之前的值，蓝色表示当前值。 可以在选项中启用和禁用此效果。

## <a name="graphical-filtering"></a>图形筛选 
某些检查器支持通过将其用作编辑器来进行数据筛选。 将检查器用作编辑器涉及选择图形元素，然后使用检查器窗口右上方的工具栏来筛选所选值。 

## <a name="column-statistics"></a>列统计信息
对于数值列，该检查器提供有关列的各种不同统计信息。 统计信息包括以下度量值： 
- 最小值
- 下四分位数
- 中值
- 上四分位数
- 最大值
- 平均值
- 标准偏差


### <a name="options"></a>选项 
- 无

## <a name="histogram"></a>直方图 
计算和显示单个数值列的直方图。 使用 Scott 规则计算默认存储桶数。 但是，可通过选项重写规则。

此检查器支持 halo 效果。


### <a name="options"></a>选项
- 最小存储桶数（即使在选中默认存储桶时也适用）
- 默认存储桶数（Scott 规则） 
- 显示 Halo
- 内核密度绘图覆盖（高斯内核） 


### <a name="actions"></a>操作
此检查器支持通过存储桶进行筛选，这可能包括单选或多选存储桶。 按前面所述应用筛选器。

## <a name="value-counts"></a>值计数
此检查器显示当前选择列的值的频率表。 默认显示前六个值的数据。 但是，可将此限制更改为任意数字。 还可将显示内容设置为从底部而非顶部计数。 此检查器支持 halo 效果。

### <a name="options"></a>选项 
- 上限值​​数
- 降序
- 包含 NULL/错误值
- 显示 Halo


### <a name="actions"></a>操作 
此检查器支持通过条形进行筛选，这可能包括单选或多选条形。 按前面所述应用筛选器。

## <a name="box-plot"></a>盒须图 
数值列的盒须图。

### <a name="options"></a>选项 
- 按列分组

## <a name="scatter-plot"></a>散点图
两个数值列的散点图。 出于性能原因降低了数据采样率。 可以在选项中重写样本大小。

### <a name="options"></a>选项  
- X 轴列
- Y 轴列
- 样本大小
- 按列分组


## <a name="time-series"></a>时序
X 轴上具有时间感知的折线图。

### <a name="options"></a>选项
- 日期列
- 数值列
- 样本大小


### <a name="actions"></a>操作
此检查器支持通过单击和拖动选择方法进行筛选，以在图形上选择一个范围。 完成选择后，按前面所述应用筛选器。


## <a name="map"></a>映射 
绘制了点的地图假定已指定纬度和经度。 必须先选择纬度。

### <a name="options"></a>选项
- 纬度列
- 经度列
- 启用群集
- 按列分组


### <a name="actions"></a>操作
此检查器支持通过在图上选择点来进行筛选。 按 **Ctrl** 键，然后单击并使用鼠标拖动以在点周围形成矩形。 然后按前面所述应用筛选器。

通过按地图左侧的 **E**，可以快速调整地图的大小以便仅显示可能的点。
