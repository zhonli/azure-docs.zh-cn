---
title: 教程：Azure Active Directory 与 BlueJeans 的集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 BlueJeans 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: d485c16719c07062249e8d40f0feca9685851834
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043459"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>教程：Azure Active Directory 与 BlueJeans 集成

在本教程中，了解如何将 BlueJeans 与 Azure Active Directory (Azure AD) 进行集成。

将 BlueJeans 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权限访问 BlueJeans
- 可以让用户使用其 Azure AD 帐户自动登录到 BlueJeans（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 BlueJeans 的集成，需要以下项：

- Azure AD 订阅
- 启用了 BlueJeans 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 BlueJeans
2. 配置和测试 Azure AD 单一登录

## <a name="adding-bluejeans-from-the-gallery"></a>从库中添加 BlueJeans
若要配置 BlueJeans 与 Azure AD 的集成，需要从库中将 BlueJeans 添加到托管 SaaS 应用列表。

若要从库中添加 BlueJeans，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入 BlueJeans。

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. 在“结果”窗格中，选择“BlueJeans”，再单击“添加”按钮，添加该应用程序。

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，根据名为“Britta Simon”的测试用户的指示配置和测试 BlueJeans 的 Azure AD 单一登录。

若要使用单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 BlueJeans 用户。 换句话说，需要建立 Azure AD 用户与 BlueJeans 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 BlueJeans 中“用户名”的值来建立此链接关系。

若要配置并测试 BlueJeans 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 BlueJeans 测试用户](#creating-a-bluejeans-test-user)** - 在 BlueJeans 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 BlueJeans 应用程序中配置单一登录。

若要配置 BlueJeans 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的“BlueJeans”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. 在“BlueJeans 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<companyname>.BlueJeans.com`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > 这些不是实际值。 请使用实际登录 URL 和标识符更新这些值。 若要获取这些值，请与 [BlueJeans 客户端支持团队](https://support.bluejeans.com/contact)联系。

4. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. 单击“保存”按钮。

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_general_400.png)

6. 在“BlueJeans 配置”部分，单击“配置 BlueJeans”，以打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL、更改密码 URL 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. 在其他 Web 浏览器窗口中，以管理员身份登录 BlueJeans 公司站点。

8. 依次转到“管理员”\>“组设置”\>“安全”。

   ![管理员](./media/bluejeans-tutorial/IC785868.png "管理员")

9. 在“安全”部分中，执行以下步骤：

   ![SAML 单一登录](./media/bluejeans-tutorial/IC785869.png "SAML 单一登录")

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 选择“SAML 单一登录”。

   b. 选择“启用自动预配”。

10. 使用以下步骤继续：

    ![证书路径](./media/bluejeans-tutorial/IC785870.png "证书路径")

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 单击“选择文件”，并上载所下载的证书。

    b. 将“SAML 单一登录服务 URL”粘贴到“登录 URL”文本框。

    c. 将“更改密码 URL”粘贴到“密码更改 URL”文本框。

    d. 将“注销 URL”粘贴到“注销 URL”文本框。

11. 使用以下步骤继续：

    ![保存更改](./media/bluejeans-tutorial/IC785874.png "保存更改")

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“用户 ID”文本框中，键入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    b. 在“电子邮件”文本框中，键入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    c. 单击“保存更改”。

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/create_aaduser_01.png)

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/create_aaduser_02.png)

3. 若要打开“用户”对话框，请在对话框顶部单击“添加”。

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/create_aaduser_03.png)

4. 在“用户”对话框页上，执行以下步骤：

    ![创建 Azure AD 测试用户](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。

### <a name="creating-a-bluejeans-test-user"></a>创建 BlueJeans 测试用户

本部分的目的是在 BlueJeans 中创建名为“Britta Simon”的用户。 BlueJeans 支持在默认情况下启用的自动用户预配。 有关如何配置自动用户预配的更多详细信息，请参见[此处](bluejeans-provisioning-tutorial.md)。

如果需要手动创建用户，请执行以下步骤：

1. 以管理员身份登录到 **BlueJeans** 公司站点。

2. 依次转到“管理员”\>“管理用户”\>“添加用户”。

   ![管理员](./media/bluejeans-tutorial/IC785877.png "管理员")

   >[!IMPORTANT]
   >“添加用户”选项卡仅在“安全”选项卡中的的“启用自动预配”处于未选中状态时可用。 

3. 在“添加用户”部分中，执行以下步骤：

    ![添加用户](./media/bluejeans-tutorial/IC785886.png "添加用户")

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在相关文本框中键入 **BlueJeans 用户名**、**电子邮件地址**、**BlueJeans 会议 ID**、**审查方密码**、**全名**、要配置的有效 AAD 帐户的**公司**。

    b. 单击“添加用户”。

>[!NOTE]
>可使用 BlueJeans 提供的任何其他 BlueJeans 用户帐户创建工具或 API 预配 AAD 用户帐户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 BlueJeans 的权限，以支持其使用 Azure 单一登录。

![分配用户][200]

若要将 Britta Simon 分配到 BlueJeans，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201]

2. 在应用程序列表中选择“BlueJeans”。

    ![配置单一登录](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。

### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 BlueJeans 磁贴时，应显示 BlueJeans 应用程序的登录页。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)
* [配置用户预配](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png