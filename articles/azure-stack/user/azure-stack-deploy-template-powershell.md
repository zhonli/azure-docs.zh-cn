---
title: 在 Azure Stack 中使用 PowerShell 部署模板 | Microsoft Docs
description: 使用 PowerShell 将模板部署到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 445628679a09a1884f63cdce446adec476af39af
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2018
ms.locfileid: "42139332"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>使用 PowerShell 将模板部署到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以使用 PowerShell 将 Azure 资源管理器模板部署到 Azure Stack。 本文介绍如何使用 PowerShell 部署模板。

## <a name="run-azurerm-powershell-cmdlets"></a>运行 AzureRM PowerShell cmdlet

此示例使用 AzureRM PowerShell cmdlet 和存储在 GitHub 上的模板。 此模板创建 Windows Server 2012 R2 Datacenter 虚拟机。

>[!NOTE]
>在尝试此示例之前，请确保已为 Azure Stack 用户[配置了 PowerShell](azure-stack-powershell-configure-user.md)。

1. 转到 <http://aka.ms/AzureStackGitHub> 并找到 **101-simple-windows-vm** 模板。 将该模板保存到此位置：C:\\templates\\azuredeploy-101-simple-windows-vm.json。
2. 打开权限提升的 PowerShell 命令提示符。
3. 将以下脚本中的*用户名*和*密码*替换为你的用户名和密码，然后运行脚本。

   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```

   >[!IMPORTANT]
   >每次运行此脚本时，都应递增“$myNum”参数的值，以避免覆盖你的部署。

4. 打开 Azure Stack 门户，选择“浏览”，然后选择“虚拟机”以查找新虚拟机 (*myDeployment001*)。

## <a name="next-steps"></a>后续步骤

[通过 Visual Studio 部署模板](azure-stack-deploy-template-visual-studio.md)
