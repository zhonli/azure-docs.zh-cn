---
title: 教程：创建返回关键短语的 LUIS 应用 - Azure | Microsoft Docs
description: 本教程介绍如何在 LUIS 应用中添加和返回 keyPhrase 实体，以分析有关关键主题的陈述。
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: d5b92681b9677db085b3c271c37490154d781d84
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494304"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>教程：8. 添加 keyPhrase 实体 
在本教程中，我们将使用一个应用，它演示了如何从陈述中提取关键主题。

<!-- green checkmark -->
> [!div class="checklist"]
> * 了解 keyPhrase 实体 
> * 在人力资源 (HR) 域中使用 LUIS 应用 
> * 添加用于从陈述中提取内容的 keyPhrase 实体
> * 训练并发布应用
> * 查询应用的终结点以查看包含关键段落的 LUIS JSON 响应

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>开始之前
如果尚未获得[简单实体](luis-quickstart-primary-and-secondary-data.md)教程中所述的人力资源应用，请将 JSON [导入](luis-how-to-start-new-app.md#import-new-app)到 [LUIS](luis-reference-regions.md#luis-website) 网站上的一个新应用中。 要导入的应用位于 [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json) Github 存储库中。

若要保留原始人力资源应用，请在[设置](luis-how-to-manage-versions.md#clone-a-version)页上克隆版本，并将其命名为 `keyphrase`。 克隆非常适合用于演练各种 LUIS 功能，且不会影响原始版本。 

## <a name="keyphrase-entity-extraction"></a>keyPhrase 实体提取
关键主题由预生成的实体 **keyPhrase** 提供。 此实体返回陈述中的关键主题。

以下陈述显示了关键短语的示例：

|陈述|keyPhrase 实体值|
|--|--|
|Is there a new medical plan with a lower deductible offered next year?|"lower deductible"<br>"new medical plan"<br>"year"|
|Is vision therapy covered in the high deductible medical plan?|"high deductible medical plan"<br>"vision therapy"|

客户端应用程序可以使用这些值以及其他已提取的实体来决定对话中的下一步骤。

## <a name="add-keyphrase-entity"></a>添加 keyPhrase 实体 
添加 keyPhrase 预生成实体，以从陈述中提取主题。

1. LUIS 的“生成”部分包含你的人力资源应用。 在右上方的菜单栏中选择“生成”可切换到此部分。 

2. 在左侧菜单中选择“实体”。

    [![“生成”部分左侧导航栏中突出显示的“实体”的屏幕截图](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. 选择“管理预生成实体”。

    [![“实体列表”弹出对话框的屏幕截图](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. 在弹出对话框中选择“keyPhrase”，然后选择“完成”。 

    [![“实体列表”弹出对话框的屏幕截图](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. 从左侧菜单中选择“意向”，然后选择 **Utilities.Confirm** 意向。 keyPhrase 实体在多个陈述中进行了标记。 

    [ ![Utilities.Confirm 意向的屏幕截图，其中在陈述中标记出了 keyPhrase](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>训练 LUIS 应用

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>将应用发布到终结点

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]


## <a name="query-the-endpoint-with-an-utterance"></a>使用陈述查询终结点

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. 将光标定位到地址中 URL 的末尾，并输入 `does form hrf-123456 cover the new dental benefits and medical plan`。 最后一个查询字符串参数为 `q`，表示陈述**查询**。 

```
{
  "query": "does form hrf-123456 cover the new dental benefits and medical plan",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9300641
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.9300641
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0359598845
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0141798034
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0112197418
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00507669244
    },
    {
      "intent": "None",
      "score": 0.00238501839
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00202810857
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00102957746
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0008688423
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.557994E-05
    }
  ],
  "entities": [
    {
      "entity": "hrf-123456",
      "type": "HRF-number",git 
      "startIndex": 10,
      "endIndex": 19
    },
    {
      "entity": "new dental benefits",
      "type": "builtin.keyPhrase",
      "startIndex": 31,
      "endIndex": 49
    },
    {
      "entity": "medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 55,
      "endIndex": 66
    },
    {
      "entity": "hrf",
      "type": "builtin.keyPhrase",
      "startIndex": 10,
      "endIndex": 12
    },
    {
      "entity": "-123456",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 19,
      "resolution": {
        "value": "-123456"
      }
    }
  ]
}
```

搜索窗体时，用户提供的信息比找到窗体所需的信息要多。 额外的信息将作为 **builtin.keyPhrase** 返回。 客户端应用程序可以将该额外的信息用于后续问题，例如“是否要跟人力资源代表谈谈新的牙科福利”，或者提供具有更多选项的菜单，这些选项包括“有关新的牙科福利或医疗计划的详细信息”。

## <a name="what-has-this-luis-app-accomplished"></a>此 LUIS 应用实现了哪些目的？
此应用具有 keyPhrase 实体检测功能，识别了自然语言查询意向，并返回了包含主要主题的提取数据。 

现在，聊天机器人已获得足够的信息，可以确定对话中的下一步。 

## <a name="where-is-this-luis-data-used"></a>在何处使用此 LUIS 数据？ 
LUIS 已完成此请求。 调用方应用程序（例如聊天机器人）可以提取 topScoringIntent 结果和陈述中的 keyPhrase 数据，以执行下一步骤。 LUIS 不会针对机器人或调用方应用程序执行编程工作。 LUIS 只确定用户的意向是什么。 

## <a name="clean-up-resources"></a>清理资源

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将情绪分析添加到应用](luis-quickstart-intent-and-sentiment-analysis.md)