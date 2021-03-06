---
title: 使用组进行访问管理的后续步骤 - Azure AD | Microsoft Docs
description: 有关如何管理安全组，以及如何使用这些组来管理对资源的访问的高级操作指南。
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/12/2017
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: bdc8754253ce2567d957b4d6240fe52242aea2ea
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448752"
---
# <a name="managing-owners-for-a-group"></a>管理组的所有者
当资源所有者将资源访问权限分配给一个 Azure AD 组，该组所有者即可管理组的成员身份。 实际上，资源所有者是将其资源的用户访问权限委派给了组的所有者。

## <a name="add-an-owner-to-a-group"></a>向组添加所有者

1. 在 [Azure AD 管理中心](https://aad.portal.azure.com)内，选择“用户和组”。
2. 选择“所有组”，再打开要向其中添加所有者的组。
3. 选择“添加所有者”。
4. 在“添加所有者”页上，选择要添加为该组所有者的用户，并确保该用户名已添加到“选定”窗格中。

## <a name="remove-an-owner-from-a-group"></a>删除组所有者

1. 在 [Azure AD 管理中心](https://aad.portal.azure.com)内，选择“用户和组”。
2. 选择“所有组”，再打开要从中删除所有者的组。
3. 选择“所有者”选项卡。
4. 选择要从该组中删除的所有者，并选择“删除”。

## <a name="additional-information"></a>其他信息
这些文章提供了有关 Azure Active Directory 的更多信息。

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)
* [用于配置组设置的 Azure Active Directory cmdlet](../users-groups-roles/groups-settings-cmdlets.md)
* [有关 Azure Active Directory 中应用程序管理的文章索引](../active-directory-apps-index.md)
* [什么是 Azure Active Directory？](active-directory-whatis.md)
* [将本地标识与 Azure Active Directory 集成](../connect/active-directory-aadconnect.md)
