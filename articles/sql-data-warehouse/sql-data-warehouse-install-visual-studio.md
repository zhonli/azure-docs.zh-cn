---
title: 安装适用于 SQL 数据仓库的 Visual Studio 和 SSDT | Microsoft Docs
description: 安装适用于 Azure SQL 数据仓库的 Visual Studio 和 SQL Server 开发工具 (SSDT)
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 11e4e6e887f7c182c1c321d8837e11f07afc2f6b
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444719"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>安装适用于 SQL 数据仓库的 Visual Studio 和 SSDT
若要开发 SQL 数据仓库的应用程序，建议使用最新版本的 Visual Studio，并结合最新版本的 SQL Server Data Tools (SSDT)。  向后兼容也支持装有 SSDT 的 Visual Studio 2013 Update 5。  

通过使用带有 SSDT 的 Visual Studio，可以使用 SQL Server 对象资源管理器以可视化方式浏览 SQL 数据仓库中的表格、视图、存储过程和其他更多对象，并运行查询。

> [!NOTE]
> SQL 数据仓库尚不支持 Visual Studio 数据库项目。 若要接收有关此功能的定期更新，请在 [UserVoice] 上投票。
> 
> 

## <a name="step-1-install-visual-studio"></a>步骤 1：安装 Visual Studio
遵循以下链接，下载并安装 Visual Studio。 如果已安装 Visual Studio 2013 或更高版本，请跳到步骤 2 以安装 SSDT。

1. [下载 Visual Studio][]。
2. 按照 MSDN 上的 [安装 Visual Studio][Installing Visual Studio] 指南安装，并选择默认配置。

## <a name="step-2-install-ssdt"></a>步骤 2：安装 SSDT
若要安装适用于 Visual Studio 的 SSDT，请先遵循以下步骤从 Visual Studio 内查找 SSDT 更新。

1. 在 Visual Studio 中，单击“工具” / “扩展和更新...” / “更新”
2. 选择“**产品更新**”，并查找“**数据库工具的 Microsoft SQL Server 更新**”

如果找不到更新，则表示已安装最新版本。  要确认是否已安装 SSDT，请单击“**帮助**” / “**关于 Microsoft Visual Studio**”，并在列表中查找 SQL Server Data Tools。 SSDT 的最新版本是 14.0.60525.0。 如果 Visual Studio 中的安装选项不可用，也可以访问 [SSDT 下载][SSDT Download]页面来手动下载并安装 SSDT。

## <a name="next-steps"></a>后续步骤
拥有最新版本的 SSDT 后，便可以[连接][connect]到 SQL 数据仓库。

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[下载 Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
