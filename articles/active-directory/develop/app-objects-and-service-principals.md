---
title: Azure Active Directory 应用程序和服务主体对象
description: 介绍 Azure Active Directory 中应用程序对象与服务主体对象之间的关系
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
services: active-directory
editor: ''
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/19/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 057465567217cff080b189bcdabee3042f41468d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595858"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD) 中的应用程序对象和服务主体对象
有时在 Azure AD 的上下文中使用时，术语“应用程序”的含义可能会被误解。 本文旨在阐述 Azure AD 应用程序集成的概念和具体层面，并演示如何注册和同意[多租户应用程序](developer-glossary.md#multi-tenant-application)。

## <a name="overview"></a>概述
已与 Azure AD 集成的应用程序具有超出软件方面的含意。 “应用程序”常作为一个概念性术语，不仅指应用程序软件，而且还指其 Azure AD 注册和运行时在身份验证/授权“对话”中的角色。 根据定义，应用程序能够以[客户端](developer-glossary.md#client-application)角色（使用资源）和/或[资源服务器](developer-glossary.md#resource-server)角色（向客户端公开 API）运行。 对话协议由 [OAuth 2.0 授权流](developer-glossary.md#authorization-grant)定义，目标是要让客户端/资源能够各自访问/保护资源的数据。 现在让我们再深入一点，看看 Azure AD 应用程序模型在设计时和运行时如何代表应用程序。 

## <a name="application-registration"></a>应用程序注册
在 [Azure 门户][AZURE-Portal]中注册 Azure AD 应用程序时，会在 Azure AD 租户中创建两个对象：应用程序对象和服务主体对象。

#### <a name="application-object"></a>应用程序对象
Azure AD 应用程序由其唯一一个应用程序对象来定义，该对象位于应用程序注册到的 Azure AD 租户（称为应用程序的“宿主”租户）中。 Azure AD Graph [Application 实体][AAD-Graph-App-Entity]定义应用程序对象属性的架构。 

#### <a name="service-principal-object"></a>服务主体对象
若要访问受 Azure AD 租户保护的资源，需要访问的实体必须由安全主体来表示。 这同时适用于用户（用户主体）和应用程序（服务主体）。 安全主体定义该租户中用户/应用程序的访问策略和权限。 这样便可实现核心功能，如在登录时对用户/应用程序进行身份验证，在访问资源时进行授权。

当应用程序被授予了对租户中资源的访问权限时（根据注册或[许可](developer-glossary.md#consent)），将创建一个服务主体对象。 Azure AD Graph [ServicePrincipal 实体][AAD-Graph-Sp-Entity]定义服务主体对象属性的架构。 

#### <a name="application-and-service-principal-relationship"></a>应用程序和服务主体的关系
可以将应用程序对象视为应用程序的*全局*表示形式（供所有租户使用），将服务主体视为*本地*表示形式（在特定租户中使用）。 应用程序对象用作模板，常见属性和默认属性从其中*派生*，以便在创建相应服务主体对象时使用。 因此，应用程序对象与软件应用程序存在 1 对 1 关系，而与其对应的服务主体对象存在 1 对多关系。

必须在将使用应用程序的每个租户中创建服务主体，让它能够建立用于登录和/或访问受租户保护的资源的标识。 单租户应用程序只有一个服务主体（在其宿主租户中），在应用程序注册期间创建并被允许使用。 多租户 Web 应用程序/API 还会在租户中的某个用户已同意使用它的每个租户中创建服务主体。 

> [!NOTE]
> 对应用程序对象所做的任何更改也只反映在该对象在应用程序宿主租户（其注册所在的租户）的服务主体对象中。 对于多租户应用程序，在通过[应用程序访问面板](https://myapps.microsoft.com)删除该访问权限并重新授予访问权限之前，对应用程序对象所做的更改不会反映在任何使用者租户的服务主体对象中。
><br>  
> 另请注意，默认情况下本机应用程序注册为多租户应用程序。
> 
> 

## <a name="example"></a>示例
下图演示了应用程序的应用程序对象和对应的服务主体对象之间的关系，其上下文是在名为 **HR 应用**的示例多租户应用程序中。 此方案中有三个 Azure AD 租户： 

* **Adatum** - 开发 **HR 应用**的公司使用的租户
* **Contoso** - Contoso 组织使用的租户，即 **HR 应用**的使用者
* **Fabrikam** - Fabrikam 组织使用的租户，它也使用 **HR 应用**

![应用程序对象与服务主体对象之间的关系](./media/app-objects-and-service-principals/application-objects-relationship.png)

在上图中，步骤 1 是在应用程序的宿主租户中创建应用程序对象和服务主体对象的过程。

在步骤 2 中，当 Contoso 和 Fabrikam 的管理员完成同意并向应用程序授予访问权限时，会在其公司的 Azure AD 租户中创建服务主体对象，并向其分配管理员所授予的权限。 另请注意，HR 应用可能配置/设计为允许由用户同意以供个人使用。

在步骤 3 中，HR 应用程序的使用者租户（例如 Contoso 和 Fabrikam）各有自己的服务主体对象。 每个对象代表其在运行时使用的应用程序实例，该实例受相关管理员同意的权限控制。

## <a name="next-steps"></a>后续步骤
可以通过 Azure AD Graph API、[Azure 门户的][AZURE-Portal]应用程序清单编辑器或 [Azure AD PowerShell cmdlet](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0) 访问应用程序的应用程序对象（由其 OData [Application 实体][AAD-Graph-App-Entity]表示）。

可以通过 Azure AD Graph API 或 [Azure AD PowerShell cmdlet](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0) 访问应用程序的服务主体对象（由其 OData [ServicePrincipal 实体][AAD-Graph-Sp-Entity]表示）。

[Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) 可用于查询应用程序和服务主体对象。

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
