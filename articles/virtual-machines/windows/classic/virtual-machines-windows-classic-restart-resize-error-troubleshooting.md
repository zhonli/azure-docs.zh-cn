---
title: VM 重新启动或大小调整问题 | Microsoft Docs
description: 排查在 Azure 中重新启动或调整现有 Windows 虚拟机时遇到的经典部署问题
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 11/03/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: bed5da25042d29983bad9a80cd44bdd7df261c2e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2017
ms.locfileid: "23989288"
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>排查在 Azure 中重新启动或调整现有 Windows 虚拟机时遇到的经典部署问题
> [!div class="op_single_selector"]
> * [经典](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

尝试启动已停止的 Azure 虚拟机 (VM)，或调整现有 Azure VM 的大小时，经常遇到的错误是分配失败。 当群集或区域没有可用的资源或无法支持所请求的 VM 大小时，将发生此错误。

> [!IMPORTANT]
> Azure 具有用于创建和处理资源的两个不同的部署模型：[Resource Manager 和经典](../../../azure-resource-manager/resource-manager-deployment-model.md)。  本文介绍使用经典部署模型。 Microsoft 建议大多数新部署使用 Resource Manager 模型。
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>收集审核日志
若要开始故障排除，请收集审核日志，以识别与问题相关的错误。

在 Azure 门户中，单击“浏览” > “虚拟机” >  *Windows 虚拟机*  > “设置” > “审核日志”。

## <a name="issue-error-when-starting-a-stopped-vm"></a>问题：启动已停止的 VM 时发生错误
尝试启动已停止的 VM，但出现分配失败。

### <a name="cause"></a>原因
必须在托管云服务的原始群集上尝试发出启动已停止 VM 的请求。 但是，群集没有足够的空间可完成该请求。

### <a name="resolution"></a>解决方法
* 创建新的云服务，并将它与区域或基于区域的虚拟网络（而不是地缘组）关联。
* 删除已停止的 VM。
* 使用磁盘在新的云服务中重新创建 VM。
* 启动重新创建的 VM。

如果在尝试创建新的云服务时收到错误，请稍后再试一次，或更改云服务的区域。

> [!IMPORTANT]
> 新的云服务将有新的名称和 VIP，因此需要为所有将信息用于现有云服务的依赖性更改该信息。
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>问题：调整现有 VM 的大小时发生错误
尝试调整现有 VM 的大小，但出现分配失败。

### <a name="cause"></a>原因
必须在托管云服务的原始群集上尝试发出调整 VM 大小的请求。 但是，群集不支持请求的 VM 大小。

### <a name="resolution"></a>解决方法
减少请求的 VM 大小，并重试调整大小请求。

* 单击“浏览全部” > “虚拟机(经典)” > *虚拟机* > “设置” > “大小”。 有关详细步骤，请参阅[调整虚拟机的大小](https://msdn.microsoft.com/library/dn168976.aspx)。

如果无法减少 VM 大小，请遵循以下步骤：

* 创建新的云服务，确保它不链接到地缘组，并且未与链接到地缘组的虚拟网络相关联。
* 在其中创建更大的新 VM。

可以在同一个云服务中合并所有 VM。 如果现有的云服务和基于区域的虚拟网络相关联，可以将新云服务连接到现有虚拟网络。

如果现有的云服务未与基于区域的虚拟网络相关联，则必须删除现有云服务中的 VM，并在新云服务中从其磁盘重新创建 VM。 然而，请务必记得新的云服务将有新的名称和 VIP，因此需要为所有目前将此信息用于现有云服务的依赖性更新该信息。

## <a name="next-steps"></a>后续步骤
如果在 Azure 中创建 Windows VM 时遇到问题，请参阅[排查在 Azure 中创建 Windows 虚拟机时遇到的部署问题](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

