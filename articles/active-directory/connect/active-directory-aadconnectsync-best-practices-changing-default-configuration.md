---
title: Azure AD Connect 同步：更改默认配置 | Microsoft 文档
description: 提供有关更改 Azure AD Connect 同步默认配置的最佳实践。
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2c2fc3bcba4b685fba36683f89c0b6ad877dbb1d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595132"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect 同步：有关更改默认配置的最佳实践
本主题旨在说明支持和不支持的 Azure AD Connect 同步更改。

通过 Azure AD Connect 创建的配置无需更改即可适用于同步本地 Active Directory 与 Azure AD 的大多数环境。 但是，在某些情况下，有必要将某些更改应用于配置，以满足特殊需求或要求。

## <a name="changes-to-the-service-account"></a>服务帐户的更改
Azure AD Connect 同步在安装向导创建的服务帐户下运行。 此服务帐户保存了同步使用的数据库加密密钥。它是使用 127 个字符长的密码创建的，密码设置为永不过期。

* **不支持**更改或重置服务帐户的密码。 这样做会破坏加密密钥，服务将无法访问数据库且无法启动。

## <a name="changes-to-the-scheduler"></a>计划程序的更改
从内部版本 1.1（2016 年 2 月）开始，可以将[计划程序](active-directory-aadconnectsync-feature-scheduler.md)配置为使用非默认的同步周期（默认周期为 30 分钟）。

## <a name="changes-to-synchronization-rules"></a>同步规则的更改
安装向导提供的配置应该适用于最常见的方案。 如果需要对配置进行更改，必须遵循这些规则，以便仍可保留支持的配置。

* 如果默认的直接属性流不适用于组织，可以[更改属性流](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes)。
* 如果希望[属性不流动](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute)并要删除 Azure AD 中的任何现有属性值，需要为此方案创建规则。
* [禁用不需要的同步规则](#disable-an-unwanted-sync-rule)而不是删除它。 升级期间将重新创建已删除的规则。
* 若要[更改现成的规则](#change-an-out-of-box-rule)，应复制原始规则并禁用现成的规则。 同步规则编辑器会显示提示并提供帮助。
* 使用同步规则编辑器导出自定义同步规则。 编辑器提供一个 PowerShell 脚本，可以在灾难恢复方案中使用它轻松重新创建同步规则。

> [!WARNING]
> 现成的同步规则具有指纹。 如果更改这些规则，指纹不再匹配。 今后尝试应用 Azure AD Connect 的新版本时可能会遇到问题。 只能根据本文所述的方式进行更改。

### <a name="disable-an-unwanted-sync-rule"></a>禁用不需要的同步规则
不要删除现成的同步规则。 下一次升级期间会重新创建该规则。

在某些情况下，安装向导生成的配置不适用于拓扑。 例如，如果使用帐户资源林拓扑，但已在具有 Exchange 架构的帐户林中扩展该架构，则系统将针对帐户林和资源林创建适用于 Exchange 的规则。 在此情况下，需要禁用适用于 Exchange 的同步规则。

![已禁用同步规则](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

在上图中，安装向导已在帐户林中找到旧的 Exchange 2003 架构。 此架构扩展是在 Fabrikam 环境中引入资源林之前添加的。 若要确保不同步任何来自旧 Exchange 实现的属性，应该按所述方式禁用同步规则。

### <a name="change-an-out-of-box-rule"></a>更改现成的规则
仅当需要更改联接规则时，才需更改现成规则。 如果需要更改属性流，则应创建具有比现成规则优先级更高的同步规则。 实际上，只有 **In from AD - User Join** 这一条规则需要克隆。 可使用优先级更高的规则替代所有其他规则。

如果需要对现成的规则进行更改，应该复制该现成的规则，并禁用原始规则。 然后对克隆的规则进行更改。 同步规则编辑器会帮助完成这些步骤。 打开现成的规则时，会显示此对话框：  
![对现成规则的警告](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

选择“是”创建规则的副本。 随后会打开克隆的规则。  
![克隆的规则](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

在这个克隆的规则中，对范围、联接和转换进行任何必要的更改。

## <a name="next-steps"></a>后续步骤
**概述主题**

* [Azure AD Connect 同步：理解和自定义同步](active-directory-aadconnectsync-whatis.md)
* [将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)
