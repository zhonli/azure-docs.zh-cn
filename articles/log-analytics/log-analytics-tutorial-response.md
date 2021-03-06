---
title: 使用 Azure Log Analytics 警报对事件做出响应 | Microsoft Docs
description: 本教程介绍如何通过 Log Analytics 中的警报来确定工作区中的重要信息，并主动将存在的问题通知给你，或者通过调用相关操作来尝试纠正问题。
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: c6c7b3f897e38fbd67098c9f881380bc073f13da
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432644"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>借助 Azure Monitor 警报对事件做出响应
日志搜索规则由 Azure 警报创建，用于定期自动运行指定的日志查询。  如果日志查询的结果符合特定条件，则会创建警报记录。 然后，该规则可以使用[操作组](../monitoring-and-diagnostics/monitoring-action-groups.md)自动运行一个或多个操作。  本教程是[创建和共享 Log Analytics 数据的仪表板](log-analytics-tutorial-dashboards.md)教程的延续。   

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建警报规则
> * 配置用于发送电子邮件通知的操作组

要完成本教程中的示例，必须将现有虚拟机[连接到 Log Analytics 工作区](log-analytics-quick-collect-azurevm.md)。

## <a name="sign-in-to-azure-portal"></a>登录到 Azure 门户
通过 [https://portal.azure.com](https://portal.azure.com) 登录到 Azure 门户。

## <a name="create-alerts"></a>创建警报
警报通过警报规则在 Azure Monitor 中创建，可以按固定的时间间隔自动运行保存的查询或自定义日志搜索。  可按以下条件创建警报：基于特定性能指标、在创建某些事件时、缺少某事件时，或者在特定时间范围内创建大量事件时。  例如，警报可用于在以下情况下通知你：平均 CPU 使用率超过特定阈值、检测到缺少更新，或者在检测到未运行特定 Windows 服务或 Linux 后台程序时生成了某个事件。  如果日志搜索的结果符合特定条件，则会创建警报。 然后，该规则可自动运行一个或多个操作，例如通知你存在警报或调用另一个进程。

在以下示例中，请根据保存在[可视化数据教程](log-analytics-tutorial-dashboards.md)中的“Azure VM - 处理器利用率”查询创建一条指标度量警报规则。 将会为每个超出阈值 (90%) 的虚拟机创建一条警报。

1. 在 Azure 门户中，单击“所有服务”。 在资源列表中，键入“监视器”。 开始键入时，会根据输入筛选该列表。 选择“监视器”。
1. 在左窗格中选择“警报”，然后单击页面顶部的“新建警报规则”，以便创建新的警报。

    ![创建新的警报规则](./media/log-analytics-tutorial-response/alert-rule-02.png)

1. 第一步是在“创建警报”部分选择充当资源的 Log Analytics 工作区，因为这是基于日志的警报信号。  对结果进行筛选，方法是：从下拉列表中选择特定的**订阅**（如果有多个），其中包含此前创建的 VM 和 Log Analytics 工作区。  从下拉列表中选择“Log Analytics”，对“资源类型”进行筛选。  最后，选择**资源** **DefaultLAWorkspace**，然后单击“完成”。

    ![创建警报步骤 1 任务](./media/log-analytics-tutorial-response/alert-rule-03.png)

1. 在“警报条件”部分下，单击“添加条件”来定义查询，然后指定警报规则遵循的逻辑。 从“配置信号逻辑”窗格中，选择“自定义日志搜索”作为信号名称，在“搜索查询”中输入你的查询。

    例如：
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    此窗格会进行更新，以呈现警报的配置设置。  在顶部，它会显示所选信号在过去 30 分钟的结果。

1. 使用以下信息配置警报：  
   a. 从“基于”下拉列表中选择“指标度量”*。  指标度量将为查询中其值超出指定阈值的每个对象创建一个警报。  
   b. 选择“大于”作为“条件”，然后输入 **90** 作为“阈值”。  
   c. 在“触发警报的条件”部分选择“连续违规”，然后从下拉列表中选择“大于”并输入 3 作为值。  
   d. 在“评估条件”部分，接受默认值。 此规则将每五分钟运行一次，返回在当前时间的这个范围内创建的记录。  
1. 单击“完成”，完成警报规则。

    ![配置警报信号](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

1. 现在转到第二步，在“警报规则名称”字段中提供警报的名称，例如“CPU 百分比大于 90%”。  指定“说明”，详细描述该警报的具体信息，并从提供的选项中选择“关键(严重性 0)”作为“严重性”值。

    ![配置警报详细信息](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

1. 若要在创建后立即激活警报规则，请接受“创建后启用规则”选项的默认值。  
1. 第三步也是最后一步，指定“操作组”，确保每次触发警报时都执行相同的操作，而且这些操作可以用于定义的每项规则。  使用以下信息配置新操作组：  
   a. 选择“新建操作组”，此时会显示“添加操作组”窗格。  
   b. 对于“操作组名称”，请指定一个长名称，例如“IT 操作 - 通知”，以及一个“短名称”，例如“itops-n”。  
   c. 验证“订阅”和“资源组”的默认值是否正确。 如果否，请从下拉列表中选择正确的值。  
   d. 在“操作”部分指定操作的名称，例如“发送电子邮件”，然后在“操作类型”下的下拉列表中选择“电子邮件/短信/推送/语音”。 “电子邮件/短信/推送/语音”属性窗格会在右侧打开，其中包含更多的信息。  
   e. 在“电子邮件/短信/推送/语音”窗格中启用“电子邮件”，并提供有效的可以接收邮件的电子邮件 SMTP 地址。  
   f. 单击“确定”  保存更改。  
       ![创建新的操作组](./media/log-analytics-tutorial-response/action-group-properties-01.png)

1. 单击“确定”，完成操作组。
1. 单击“创建警报规则”，完成警报规则。 该警报会立即开始运行。

    ![完成新警报规则的创建](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>在 Azure 门户中查看警报
创建警报后，即可在单个窗格中查看 Azure 警报，并可跨 Azure 订阅管理所有警报规则。 此页面列出所有警报规则（已启用的或已禁用的），这些规则可以根据目标资源、资源组、规则名称或状态排序。 包含所有已触发警报和所有已配置/已启用警报规则的聚合摘要。

![Azure 警报状态页](./media/log-analytics-tutorial-response/azure-alerts-02.png)

警报触发时，此表会反映相关条件以及警报在所选时间范围（默认为过去六小时）内发生的次数。  收件箱中会有一封相应的电子邮件，该邮件类似于以下示例，说明了有问题的虚拟机以及在这种情况下与搜索查询最匹配的结果。

![警报电子邮件操作示例](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>后续步骤
在本教程中，介绍了警报规则按照计划的时间间隔运行且匹配特定条件时如何主动识别问题并做出响应。

请访问以下链接，查看预生成的 Log Analytics 脚本示例。

> [!div class="nextstepaction"]
> [Log Analytics 脚本示例](powershell-samples.md)
