---
title: 为 Service Fabric 客户端身份验证设置 Azure Active Directory | Microsoft Docs
description: 了解如何设置 Azure Active Directory (Azure AD) 来对 Service Fabric 群集的客户端进行身份验证。
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: 9b00f6e5c4dbd2fe2c6aab4c62cecc2f2e2640f0
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2018
ms.locfileid: "42140438"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>为客户端身份验证设置 Azure Active Directory

对在 Azure 上运行的群集，建议使用 Azure Active Directory (Azure AD) 来保护对管理终结点的访问。  本文介绍了如何设置 Azure AD 来对 Service Fabric 群集的客户端进行身份验证，这必须在[创建群集](service-fabric-cluster-creation-via-arm.md)之前完成。  通过 Azure AD，组织（称为租户）可管理用户对应用程序的访问。 应用程序分为采用基于 Web 的登录 UI 的应用程序和采用本地客户端体验的应用程序。 本文假设已创建了一个租户。 如果未创建，请先阅读[如何获取 Azure Active Directory 租户][active-directory-howto-tenant]。

## <a name="create-azure-ad-applications"></a>创建 Azure AD 应用程序
Service Fabric 群集提供其管理功能的各种入口点，包括基于 Web 的 [Service Fabric Explorer][service-fabric-visualizing-your-cluster] 和 [Visual Studio][service-fabric-manage-application-in-visual-studio]。 因此，需要创建两个 Azure AD 应用程序来控制对群集的访问：一个 Web 应用程序和一个本机应用程序。  创建应用程序后，将用户分配到只读和管理员角色。

为了简化涉及到配置 Azure AD 与 Service Fabric 群集的一些步骤，我们创建了一组 Windows PowerShell 脚本。

> [!NOTE]
> 在创建群集之前，请完成以下步骤。 因为脚本需要群集名称和终结点，这些值应是规划的值，而不是已创建的值。

1. [将脚本下载][sf-aad-ps-script-download]到计算机。
2. 右键单击 zip 文件，选择“属性”，“解除阻止”复选框，并单击“应用”。
3. 解压缩 zip 文件。
4. 运行 `SetupApplications.ps1` 并提供 TenantId、ClusterName 和 WebApplicationReplyUrl 作为参数。 例如：

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
```

> [!NOTE]
> 对于国家/地区云（Azure 政府、Azure 中国、Azure 德国），还应指定 `-Location` 参数。

执行 PowerShell 命令 `Get-AzureSubscription`，可找到租户 ID。 执行此命令，为每个订阅显示 TenantId。

将 ClusterName 用作脚本创建的 Azure AD 应用程序的前缀。 它不需要完全匹配实际的群集名称。 旨在更加轻松地将 Azure AD 项目映射到其配合使用的 Service Fabric 群集。

WebApplicationReplyUrl 是 Azure AD 在完成登录过程之后返回给用户的默认终结点。 将此终结点设置为群集的 Service Fabric Explorer 的终结点，默认值为：

https://&lt;cluster_domain&gt;:19080/Explorer

系统会提示登录到具有 Azure AD 租户管理权限的帐户。 完成此操作后，脚本会创建 Web 和本机应用程序来代表 Service Fabric 群集。 在 [Azure 门户][azure-portal]中查看租户的应用程序时，应会看到两个新条目：

   * *ClusterName*\_Cluster
   * *ClusterName*\_Client

在下一部分创建群集时该脚本显示 Azure 资源管理器模板所需的 JSON，因此最好不要关闭 PowerShell 窗口。

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>将用户分配到角色
创建用于表示群集的应用程序后，请将用户分配到 Service Fabric 支持的角色：只读和管理员。可使用 [Azure 门户][azure-portal]来分配这些角色。

1. 在 Azure 门户中，选择右上角的租户。

    ![“选择租户”按钮][select-tenant-button]
2. 在左侧选项卡中选择“Azure Active Directory”，然后选择“企业应用程序”。
3. 选择“所有应用程序”，然后找到并选择名称为 `myTestCluster_Cluster` 的 Web 应用程序。
4. 单击“用户和组”选项卡。

    ![“用户和组”选项卡][users-and-groups-tab]
5. 单击新页面上的“添加用户”，选择要分配的用户和角色，然后单击页面底部的“选择”按钮。

    ![“将用户分配到角色”页面][assign-users-to-roles-page]
6. 单击页面底部的“分配”按钮。

    ![添加分配确认][assign-users-to-roles-confirm]

> [!NOTE]
> 有关 Service Fabric 中角色的详细信息，请参阅[适用于 Service Fabric 客户端的基于角色的访问控制](service-fabric-cluster-security-roles.md)。
>
>

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>有关排查 Azure Active Directory 设置问题的帮助
Azure AD 的设置和使用可能有一定难度，可以参考下面的一些指导来调试问题。

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer 提示选择证书
#### <a name="problem"></a>问题
成功登录到 Service Fabric Explorer 中的 Azure AD 后，浏览器返回到主页，但会出现提示用户选择证书的消息。

![SFX 证书对话框][sfx-select-certificate-dialog]

#### <a name="reason"></a>原因
未在 Azure AD 群集应用程序中为用户分配角色。 因此，Service Fabric 群集的 Azure AD 身份验证失败。 Service Fabric Explorer 将故障回复到证书身份验证。

#### <a name="solution"></a>解决方案
遵循有关设置 Azure AD 的说明操作，并为用户分配角色。 此外，我们建议打开“访问应用需要的用户分配”，如 `SetupApplications.ps1` 所示。

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>使用 PowerShell 连接失败并出现错误：“指定的凭据无效”
#### <a name="problem"></a>问题
使用 PowerShell 以“AzureActiveDirectory”安全模式连接到群集时，成功登录到 Azure AD 后，连接失败并显示错误：“指定的凭据无效”。

#### <a name="solution"></a>解决方案
解决方案同上。

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>登录时，Service Fabric Explorer 返回失败信息：“AADSTS50011”
#### <a name="problem"></a>问题
用户尝试登录到 Service Fabric Explorer 中的 Azure AD 时，页面返回故障：“AADSTS50011：回复地址 &lt;url&gt; 与针对应用程序 &lt;guid&gt; 配置的回复地址不匹配”。

![SFX 回复地址不匹配][sfx-reply-address-not-match]

#### <a name="reason"></a>原因
代表 Service Fabric Explorer 的群集 (web) 应用程序尝试针对 Azure AD 进行身份验证，在执行请求的过程中提供了重定向返回 URL。 但是，该 URL 并未列在 Azure AD 应用程序的“回复 URL”列表中。

#### <a name="solution"></a>解决方案
在 AAD 页面中选择“应用注册”，选择你的群集应用程序，然后选择“回复 URL”按钮。 在“回复 URL”页面上，将 Service Fabric Explorer 的 URL 添加到列表中或替换列表中的某一项。 完成后，保存所做的更改。

![Web 应用程序回复 URL][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>使用 Azure AD 身份验证通过 PowerShell 连接群集
若要连接 Service Fabric 群集，请使用以下 PowerShell 命令示例：

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

若要了解详细信息，请参阅 [Connect-ServiceFabricCluster cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster)。

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>是否可将同一个 Azure AD 租户用于多个群集？
是的。 请记得将 Service Fabric Explorer 的 URL 添加到群集 (Web) 应用程序。 否则 Service Fabric Explorer 无法正常工作。

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>为何启用 Azure AD 时仍然需要服务器证书？
FabricClient 和 FabricGateway 执行相互身份验证。 使用 Azure AD 身份验证时，Azure AD 集成可将客户端标识提供给服务器，服务器证书用于验证服务器标识。 有关 Service Fabric 证书的详细信息，请参阅 [X.509 证书和 Service Fabric][x509-certificates-and-service-fabric]。

## <a name="next-steps"></a>后续步骤
在设置 Azure Active Directory 应用程序并为用户设置角色后，[配置并部署群集](service-fabric-cluster-creation-via-arm.md)。


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[select-tenant-button]: ./media/service-fabric-cluster-creation-setup-aad/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-setup-aad/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
