---
title: 使用模板在 Azure 虚拟机规模集上配置托管服务标识
description: 分步说明如何使用 Azure 资源管理器模板在 Azure VMSS 上配置托管服务标识。
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 68304b3e5eea50aba28f46344abcbd7ad060c5c8
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "42144174"
---
# <a name="configure-managed-service-identity-on-virtual-machine-scale-using-a-template"></a>使用模板在虚拟机规模集上配置托管服务标识

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

托管服务标识为 Azure 服务提供了 Azure Active Directory 中的自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

在本文中，你将了解如何使用 Azure 资源管理器部署模板在 Azure 虚拟机规模集上执行以下托管服务标识操作：
- 在 Azure 虚拟机规模集上启用和禁用系统分配的标识
- 在 Azure 虚拟机规模集上添加和删除用户分配的标识

## <a name="prerequisites"></a>先决条件

- 如果不熟悉托管服务标识，请查阅[概述部分](overview.md)。 请务必了解[系统分配标识与用户分配标识之间的差异](overview.md#how-does-it-work)。
- 如果没有 Azure 帐户，请在继续前[注册免费帐户](https://azure.microsoft.com/free/)。
- 若要执行本文中的管理操作，帐户需要分配以下角色：
    - [虚拟机参与者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)，可创建虚拟机规模集，并从虚拟机规模集启用和删除系统和/或用户分配的托管标识。
    - [托管标识参与者](/azure/role-based-access-control/built-in-roles#managed-identity-contributor)角色，可创建用户分配标识。
    - [托管标识操作员](/azure/role-based-access-control/built-in-roles#managed-identity-operator)角色，可在虚拟机规模集中分配和删除用户分配标识。

## <a name="azure-resource-manager-templates"></a>Azure 资源管理器模板

与 Azure 门户和脚本一样，[Azure 资源管理器](../../azure-resource-manager/resource-group-overview.md)模板支持部署由 Azure 资源组定义的新资源或修改后的资源。 有多种可用于执行模板编辑和部署的方法（包括本地方法和基于门户的方法），包括：

   - 使用 [Azure Marketplace 中的自定义模板](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template)，这样可以从头开始创建模板，也可以在现有常见模板或[快速入门模板](https://azure.microsoft.com/documentation/templates/)的基础之上操作。
   - 派生自现有资源组，具体方法是从[原始部署](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history)或[当前部署](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group)导出模板。
   - 使用本地 [JSON 编辑器（例如 VS Code）](../../azure-resource-manager/resource-manager-create-first-template.md)，然后使用 PowerShell 或 CLI 进行上传和部署。
   - 使用 Visual Studio [Azure 资源组项目](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)同时创建和部署模板。  

无论选择哪种方法，在初始部署和重新部署期间，模板语法都是相同的。 在新 VM 或现有 VM 上启用托管服务标识的方式相同。 此外，默认情况下，Azure 资源管理器还会对部署执行[增量更新](../../azure-resource-manager/deployment-modes.md)。

## <a name="system-assigned-identity"></a>系统分配标识

在此部分中，将使用 Azure 资源管理器模板启用和禁用系统分配标识。

### <a name="enable-system-assigned-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-a-existing-virtual-machine-scale-set"></a>在创建虚拟机规模集期间或在现有的虚拟机规模集上启用系统分配的标识

1. 无论是在本地登录到 Azure 还是通过 Azure 门户登录，请使用与包含虚拟机规模集的 Azure 订阅关联的帐户。
   
2. 若要启用系统分配的标识，请将模板加载到编辑器中，在 resources 节中找到所关注的 `Microsoft.Compute/virtualMachinesScaleSets` 资源，并在与 `identity` 属性相同的级别添加 `"type": "Microsoft.Compute/virtualMachines"` 属性。 使用以下语法：

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

3. （可选）添加虚拟机规模集托管服务标识扩展作为 `extensionsProfile` 元素。 此步骤是可选的，因为你也可以使用 Azure 实例元数据服务 (IMDS) 标识来检索令牌。  使用以下语法：

   >[!NOTE] 
   > 下面的示例假定正在部署的是 Windows 虚拟机规模集扩展 (`ManagedIdentityExtensionForWindows`)。 对于 Linux，还可以改用 `ManagedIdentityExtensionForLinux` 来配置 `"name"` 和 `"type"` 元素。
   >

   ```json
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
   ```

4. 完成后，以下各节应当会添加到模板的 resource 节并应当呈现如下：

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ``` 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>从 Azure 虚拟机规模集中禁用系统分配标识

如果虚拟机规模集不再需要托管服务标识，请执行以下操作：

1. 无论是在本地登录到 Azure 还是通过 Azure 门户登录，请使用与包含虚拟机规模集的 Azure 订阅关联的帐户。

2. 将模板加载到[编辑器](#azure-resource-manager-templates)，并在 `resources` 部分找到相关的 `Microsoft.Compute/virtualMachineScaleSets` 资源。 如果 VM 只有系统分配标识，则可以将标识类型更改为 `None` 来禁用它。

   **Microsoft.Compute/virtualMachineScaleSets API 版本 2018-06-01**

   如果 apiVersion 为 `2018-06-01` 并且 VM 同时具有系统和用户分配的标识，请从标识类型中删除 `SystemAssigned` 并保留 `UserAssigned` 以及 userAssignedIdentities 字典值。

   **Microsoft.Compute/virtualMachineScaleSets API 版本 2018-06-01 及早期版本**

   如果 apiVersion 为 `2017-12-01` 并且虚拟机规模集同时具有系统分配标识和用户分配标识，请从标识类型中删除 `SystemAssigned`，并保留 `UserAssigned` 以及用户分配标识的 `identityIds` 数组。 
   
    

   以下示例演示如何从没有用户分配标识的虚拟机规模集中删除系统分配标识：
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-identity"></a>用户分配标识

在本部分中，将使用 Azure 资源管理器模板向虚拟机规模集分配用户分配的标识。

> [!Note]
> 若要使用 Azure 资源管理器模板创建用户分配标识，请参阅[创建用户分配标识](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity)。

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>向 Azure VMSS 分配用户分配标识

1. 在 `resources` 元素下添加以下条目，以向虚拟机规模集分配用户分配的标识。  请务必将 `<USERASSIGNEDIDENTITY>` 替换为你创建的用户分配标识的名称。
   
   **Microsoft.Compute/virtualMachineScaleSets API 版本 2018-06-01**

   如果 apiVersion 为 `2018-06-01`，则用户分配的标识以 `userAssignedIdentities` 字典格式存储，并且 `<USERASSIGNEDIDENTITYNAME>` 值必须存储在模板的 `variables` 节中定义的某个变量中。

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }
    
   }
   ```   

   **Microsoft.Compute/virtualMachineScaleSets API 版本 2017-12-01**
    
   如果 `apiVersion` 为 `2017-12-01` 或早期版本，则用户分配的标识存储在 `identityIds` 数组中，并且 `<USERASSIGNEDIDENTITYNAME>` 值必须存储在模板的 variables 节中定义的某个变量中。

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ``` 

2. （可选）在 `extensionProfile` 元素下添加以下条目，以向 VMSS 分配托管标识扩展。 此步骤是可选的，因为也可以使用 Azure 实例元数据服务 (IMDS) 标识终结点来检索令牌。 使用以下语法：
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "MSIWindowsExtension",
                    "properties": {
                        "publisher": "Microsoft.ManagedIdentity",
                        "type": "ManagedIdentityExtensionForWindows",
                        "typeHandlerVersion": "1.0",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "port": 50342
                        },
                        "protectedSettings": {}
                    }
                }
    ```

3. 完成后，模板应当类似于以下示例：
   
   **Microsoft.Compute/virtualMachineScaleSets API 版本 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ```

   **Microsoft.Compute/virtualMachines API 版本 2017-12-01 和早期版本**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            } 
                        ]
                    }
                }
            }
        }
    ]
   ```
### <a name="remove-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>从 Azure 虚拟机规模集中删除用户分配标识

如果虚拟机规模集不再需要托管服务标识，请执行以下操作：

1. 无论是在本地登录到 Azure 还是通过 Azure 门户登录，请使用与包含虚拟机规模集的 Azure 订阅关联的帐户。

2. 将模板加载到[编辑器](#azure-resource-manager-templates)，并在 `resources` 部分找到相关的 `Microsoft.Compute/virtualMachineScaleSets` 资源。 如果虚拟机规模集只有用户分配标识，则可以将标识类型更改为 `None` 来禁用它。

   以下示例演示如何从没有系统分配标识的 VM 中删除所有用户分配标识：

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachineScaleSets API 版本 2018-06-01**
    
   若要从虚拟机规模集中删除单个用户分配的标识，请将其从 `userAssignedIdentities` 字典中删除。

   如果你具有系统分配的标识，请将其保持在 `identity` 值下的 `type` 值中。

   **Microsoft.Compute/virtualMachineScaleSets API 版本 2017-12-01**

   若要从虚拟机规模集中删除单个用户分配的标识，请将其从 `identityIds` 数组中删除。

   如果你具有系统分配的标识，请将其保持在 `identity` 值下的 `type` 值中。
   
## <a name="next-steps"></a>后续步骤

- 若要更广泛地了解托管服务标识，请阅读[托管服务标识概述](overview.md)。

