---
title: Azure 事件网格传送和重试
description: 介绍 Azure 事件网格如何传送事件以及如何处理未送达的消息。
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: tomfitz
ms.openlocfilehash: b34386a7b416d6f7d8b008a9cb5ef142948a370f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005389"
---
# <a name="event-grid-message-delivery-and-retry"></a>事件网格消息传送和重试 

本文介绍了未确认送达时 Azure 事件网格如何处理事件。

事件网格提供持久传送。 它会将每个订阅的每条消息至少发送一次。 事件会立即发送到每个订阅的已注册 webhook。 如果 webhook 在首次发送后 60 秒内未确认已接收事件，则事件网格会重试事件传送。 

目前，事件网格单独将每个事件发送到订阅者。 订阅者接收包含单个事件的数组。

## <a name="message-delivery-status"></a>消息传送状态

事件网格使用 HTTP 响应代码确认已接收事件。 

### <a name="success-codes"></a>成功代码

以下 HTTP 响应代码表示事件已成功发送至 webhook。 事件网格会视为传送已完成。

- 200 正常
- 202 已接受

### <a name="failure-codes"></a>失败代码

以下 HTTP 响应代码表示事件传送尝试失败。 

- 400 错误请求
- 401 未授权
- 404 未找到
- 408 请求超时
- 414 URI 太长
- 429 请求过多
- 500 内部服务器错误
- 503 服务不可用
- 504 网关超时

如果事件网格收到的错误指出终结点暂时不可用或将来的请求可能会成功，则它将尝试重新发送事件。 如果事件网格收到一个指示传递永远不会成功且[已配置死信终结点](manage-event-delivery.md)的错误，则会将事件发送到死信终结点。 

## <a name="retry-intervals-and-duration"></a>重试间隔和持续时间

对于事件传送，事件网格使用指数性的回退重试策略。 如果 webhook 未响应或返回失败代码，事件网格会按照以下计划重新尝试传送：

1. 10 秒
2. 30 秒
3. 1 分钟
4. 5 分钟
5. 10 分钟
6. 30 分钟
7. 1 小时	

事件网格允许所有重试间隔可以略微随机。 一个小时后，事件传送每小时重试一次。

默认情况下，事件网格会使所有在 24 小时内未送达的事件过期。 创建事件订阅时，可[自定义重试策略](manage-event-delivery.md)。 提供最大传递尝试次数（默认值为 30）和事件生存时间（默认为 1440 分钟）。

## <a name="dead-letter-events"></a>死信事件

当事件网格无法传递事件时，它会将未送达的事件发送到某个存储帐户。 此过程称为死信处理。 要查看未传送的事件，可从死信位置拉取它们。 有关详细信息，请参阅[死信与重试策略](manage-event-delivery.md)。

## <a name="next-steps"></a>后续步骤

* 若要查看事件传送的状态，请参阅[监视事件网格消息传送](monitor-event-delivery.md)。
* 要自定义事件传递选项，请参阅[管理事件网格传递设置](manage-event-delivery.md)。
* 有关事件网格的介绍，请参阅[关于事件网格](overview.md)。
* 若要快速开始使用事件网格，请参阅[使用 Azure 事件网格创建和路由自定义事件](custom-event-quickstart.md)。