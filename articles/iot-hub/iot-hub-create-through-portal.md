---
title: 使用 Azure 门户创建 IoT 中心 | Microsoft Docs
description: 如何通过 Azure 门户创建、管理和删除 Azure IoT 中心。 包括有关定价层、缩放、安全性和消息传递配置的信息。
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 0b03ae434e93dbab45235fe67c499497e1257064
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2018
ms.locfileid: "42140432"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>使用 Azure 门户创建 IoT 中心

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

本文介绍：

* 如何在 Azure 门户中查找 IoT 中心服务。
* 如何创建和管理 IoT 中心。

## <a name="where-to-find-the-iot-hub-service"></a>在何处可以找到 IoT 中心服务

可以在门户中的以下位置找到 IoT 中心服务：

* 选择“+ 新建”，然后选择“物联网”。
* 在市场中选择“物联网”。

## <a name="create-an-iot-hub"></a>创建 IoT 中心

可以使用以下方法创建 IoT 中心：

* “+ 新建”选项可以打开下方屏幕截图中显示的边栏选项卡。 通过此方法和通过市场创建 IoT 中心的步骤完全相同。

* 在市场中选择“创建”可打开下方屏幕截图中显示的边栏选项卡。

以下各部分介绍了创建 IoT 中心所需的几个步骤。

### <a name="choose-the-name-of-the-iot-hub"></a>选择 IoT 中心的名称

若要创建 IoT 中心，必须为 IoT 中心命名。 此名称在所有 IoT 中心中必须唯一。

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>选择定价层

可以根据你需要的功能数以及每天通过解决方案发送的消息数从多个层中进行选择。 免费层适用于测试和评估。 它允许 500 台设备连接到 IoT 中心，并且每天最多传输 8,000 条信息。 每个 Azure 订阅可以在免费层中创建一个 IoT 中心。 

有关其他层选项的详细信息，请参阅[选择合适的 IoT 中心层](iot-hub-scaling.md)。

### <a name="iot-hub-units"></a>IoT 中心单位

每日每单位允许的消息数取决于中心的定价层。 例如，如果希望 IoT 中心支持 700,000 条消息输入，则选择两个 S1 层单位。

### <a name="device-to-cloud-partitions-and-resource-group"></a>设备到云分区和资源组

可以更改 IoT 中心的分区数目。 默认分区数目为 4，但可以从下拉列表中选择一个不同的数目。

不需要显式创建空资源组。 在创建资源时，可以选择创建新的资源组，也可以使用现有的资源组。

![显示了在 Azure 门户中创建中心的屏幕截图](./media/iot-hub-create-through-portal/location1.png)

### <a name="choose-subscription"></a>选择订阅

Azure IoT 中心自动列出用户帐户所链接的 Azure 订阅。 可以选择与 IoT 中心关联的 Azure 订阅。

### <a name="choose-the-location"></a>选择位置

位置选项提供可在其中使用 IoT 中心的区域列表。

### <a name="create-the-iot-hub"></a>创建 IoT 中心

完成上述所有步骤后，便可以创建 IoT 中心。 单击“创建”启动后端进程，使用所选择的选项创建和部署 IoT 中心。

创建 IoT 中心可能需要几分钟时间，因为在适当的位置服务器中运行后端部署需要花费时间。

## <a name="change-the-settings-of-the-iot-hub"></a>更改 IoT 中心的设置
<!--robinsh these screenshots are out of date -->

可以在通过“IoT 中心”边栏选项卡创建 IoT 中心后更改现有 IoT 中心的设置。

![显示了 IoT 中心设置的屏幕截图](./media/iot-hub-create-through-portal/portal-settings.png)

**共享访问策略**：这些策略定义设备与服务连接到 IoT 中心所需的权限。 可以单击“常规”下面的“共享访问策略”来访问这些策略。 在此边栏选项卡中，可以修改现有的策略或添加新策略。

### <a name="create-a-policy"></a>创建策略

* 单击“**添加**”打开边栏选项卡。 可在此处输入新的策略名称以及想要与此策略关联的权限，如下图所示：

    有许多权限可与这些共享策略相关联。 “注册表读取”和“注册表写入”策略用于向标识注册表授予读取和写入访问权限。 选择写入选项会自动选择读取选项。

    “服务连接”策略授予访问服务终结点的权限，例如“接收设备到云”。 “设备连接”策略授予使用 IoT 中心的设备端终结点发送和接收消息的权限。

* 单击“**创建**”将此新建策略添加到现有列表。

   ![显示了添加共享访问策略的屏幕截图](./media/iot-hub-create-through-portal/shared-access-policies.png)

## <a name="endpoints"></a>终结点

单击“终结点”可显示要修改的 IoT 中心的终结点列表。 有两种类型的终结点：内置到 IoT 中心的终结点，以及创建后添加到 IoT 中心的终结点。

![显示了添加终结点的屏幕截图](./media/iot-hub-create-through-portal/messaging-settings.png)

### <a name="built-in-endpoints"></a>内置终结点

有两个内置终结点：**云到设备反馈**和**事件**。

* **云到设备反馈**设置：此设置有两项子设置：消息的**云到设备 TTL**（生存时间）和**保留时间**（以小时为单位）。 首次创建 IoT 中心时，这两个设置的默认值均为一小时。 若要调整这些设置，请使用滑块或键入值。

* **事件**设置：此设置有多个子设置，其中一些为只读。 以下列表对这些设置进行了说明：

  * **分区**：创建 IoT 中心时，会设置一个默认值。 可以通过此设置更改分区数目。

  * **与事件中心兼容的名称和终结点**：创建 IoT 中心时，会在内部创建事件中心，供用户在特定情况下根据需要访问。 不能自定义与事件中心兼容的名称和终结点值，但可以单击“复制”来复制它们。

  * **保留时间**：默认设置为一天，但可以使用下拉列表对其进行更改。 对于设备到云的设置，此值以天为单位。

  * 使用者组：通过使用者组，多个读者可以从 IoT 中心独立读取消息。 创建的每个 IoT 中心都包含一个默认使用者组。 但是，可以使用此设置在 IoT 中心添加或删除使用者组。

  > [!NOTE]
  > 无法编辑或删除默认使用者组。

### <a name="custom-endpoints"></a>自定义终结点

可通过门户将自定义终结点添加到 IoT 中心。 在“终结点”边栏选项卡中，单击顶部的“添加”，打开“添加终结点”边栏选项卡。 输入所需的信息，并单击“确定”。 自定义终结点现在会在主“终结点”边栏选项卡中列出。

![显示了创建自定义终结点的屏幕截图](./media/iot-hub-create-through-portal/endpoint-creation.png)

有关自定义终结点的详细信息，请阅读[参考 - IoT 中心终结点]( iot-hub-devguide-endpoints.md)。

## <a name="routes"></a>路由

单击“路由”，管理 IoT 中心发送设备到云消息的方式。

![显示了添加新路由的屏幕截图](./media/iot-hub-create-through-portal/routes-list.png)

单击“路由”边栏选项卡顶部的“添加”，输入所需信息，然后单击“确定”，即可将路由添加到 IoT 中心。 然后路由就会在主“路由”边栏选项卡中列出。 在路由列表中单击路由即可对其进行编辑。 要启用路由，请在路由列表中单击它，然后将“启用”切换按钮设置为“关”。 若要保存更改，单击边栏选项卡底部的“确定”。

![显示了编辑新路由规则的屏幕截图](./media/iot-hub-create-through-portal/route-edit.png)

## <a name="delete-the-iot-hub"></a>删除 IoT 中心

可以浏览到想要删除的 IoT 中心，方法是单击“**浏览**”，然后选择要删除的相应中心。 若要删除 IoT 中心，单击 IoT 中心名称下面的“删除”按钮。

## <a name="next-steps"></a>后续步骤

若要了解有关如何管理 Azure IoT 中心的详细信息，请参阅以下链接：

* [批量管理 IoT 设备](iot-hub-bulk-identity-mgmt.md)
* [IoT 中心指标](iot-hub-metrics.md)
* [操作监视](iot-hub-operations-monitoring.md)

若要进一步探索 IoT 中心的功能，请参阅：

* [IoT 中心开发人员指南](iot-hub-devguide.md)
* [使用 Azure IoT Edge 将 AI 部署到边缘设备](../iot-edge/tutorial-simulate-device-linux.md)
* [从根本上保护 IoT 解决方案](../iot-fundamentals/iot-security-ground-up.md)
