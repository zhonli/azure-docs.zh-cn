---
title: Azure 文件同步代理发行说明 | Microsoft Docs
description: Azure 文件同步代理发行说明。
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 08/21/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 3cd178333ee0d8d92db08fb08cbd02b05112f58b
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "42445016"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure 文件同步代理发行说明
借助 Azure 文件同步，既可将组织的文件共享集中在 Azure 文件中，又不失本地文件服务器的灵活性、性能和兼容性。 Windows Server 安装可转换为 Azure 文件共享的快速缓存。 可以使用 Windows Server 上提供的任意协议（包括 SMB、NFS 和 FTPS）以本地方式访问数据， 并且可以根据需要在世界各地设置多个缓存。

本文提供受支持的 Azure 文件同步代理版本的发行说明。

## <a name="supported-versions"></a>支持的版本
以下版本是 Azure 文件同步代理支持的：

| 里程碑 | 代理版本号 | 发行日期 | 状态 |
|----|----------------------|--------------|------------------|
| 8 月更新汇总 | 3.2.0.0 | 2018 年 8 月 15 日 | 支持（建议的版本） |
| 正式版 | 3.1.0.0 | 2018 年 7 月 19日 | 支持 |
| 6 月更新汇总 | 3.0.13.0 | 2018 年 6 月 29日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 刷新 2 | 3.0.12.0 | 2018 年 5 月 22 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 4 月更新汇总 | 2.3.0.0 | 2018 年 5 月 8 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 3 月更新汇总 | 2.2.0.0 | 2018 年 3 月 12 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 2 月更新汇总 | 2.1.0.0 | 2018 年 2 月 28 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 刷新 1 | 2.0.11.0 | 2018 年 2 月 8 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 1 月更新汇总 | 1.4.0.0 | 2018 年 1 月 8 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 11 月更新汇总 | 1.3.0.0 | 2017 年 11 月 30 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 10 月更新汇总 | 1.2.0.0 | 2017 年 10 月 31 日 | 代理版本将于 2018 年 9 月 4 日到期 |
| 初始预览版 | 1.1.0.0 | 2017 年 9 月 26 日 | 代理版本将于 2018 年 9 月 4 日到期 |

### <a name="azure-file-sync-agent-update-policy"></a>Azure 文件同步代理更新策略
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-3200"></a>代理版本 3.2.0.0
以下发行说明适用于 Azure 文件同步代理版本 3.2.0.0（2018 年 8 月 15 日发布）。 这些说明是对针对版本 3.1.0.0 列出的发行说明的补充。

此版本包括以下修复：
- 由于内存泄漏，同步失败并出现内存不足错误 (0x8007000e)

## <a name="agent-version-3100"></a>代理版本 3.1.0.0
以下发行说明适用于 Azure 文件同步代理版本 3.1.0.0（2018 年 7 月 19 日发布）。

### <a name="agent-installation-and-server-configuration"></a>代理安装和服务器配置
若要详细了解如何使用 Windows Server 安装和配置 Azure 文件同步代理，请参阅[规划 Azure 文件同步部署](storage-sync-files-planning.md)和[如何部署 Azure 文件同步](storage-sync-files-deployment-guide.md)。

- 代理安装包必须使用提升的（管理员）权限进行安装。
- 此代理在 Windows Server Core 或 Nano Server 部署选项上不受支持。
- 只有 Windows Server 2016 和 Windows Server 2012 R2 上支持此代理。
- 代理需要至少 2 GB 的物理内存。
- 存储同步代理 (FileSyncSvc) 服务不支持进行了系统卷信息 (SVI) 目录压缩的卷上的服务器终结点。 此配置会导致意外结果。

### <a name="interoperability"></a>互操作性
- 防病毒应用程序、备份应用程序和其他应用程序在访问分层文件时可能会导致不需要的撤回，除非这些应用程序遵从脱机属性，在读取这些文件的内容时跳过。 有关详细信息，请参阅[对 Azure 文件同步进行故障排除](storage-sync-files-troubleshoot.md)。
- 请勿使用文件服务器资源管理器 (FSRM) 或其他文件屏蔽。 当文件因文件屏蔽而被阻止时，文件屏蔽可能导致无休止的同步故障。
- 不支持在安装了 Azure 文件同步代理的服务器上运行 sysprep，那样做会导致意外结果。 应该在部署服务器映像并完成 sysprep 迷你安装后再安装代理和注册服务器。
- 不支持在同一卷上进行重复数据删除和云分层操作。

### <a name="sync-limitations"></a>同步限制
以下各项不同步，但系统其余部分仍会继续正常运行：
- 长度超过 2,048 个字符的路径。
- 安全描述符的自定义访问控制列表 (DACL) 部分（如果其大小大于 2 KB）。 （只有在单个项上的访问控制条目 (ACE) 大于某个数（大约为 40）的情况下，这才是一个问题。）
- 安全描述符的系统访问控制列表 (SACL) 部分，用于审核。
- 扩展的属性。
- 备用数据流。
- 重分析点。
- 硬链接。
- 将更改从其他终结点同步到服务器文件时，不会保留压缩（如果在该文件上设置了压缩）。
- 任何使用 EFS（或其他用户模式加密方式）加密的文件，此类加密会阻止服务读取数据。

    > [!Note]  
    > Azure 文件同步始终加密传输中的数据， 而在 Azure 中，数据始终进行静态加密。
 
### <a name="server-endpoint"></a>服务器终结点
- 服务器终结点只能在 NTFS 卷上创建。 Azure 文件同步目前不支持 ReFS、FAT、FAT32 等文件系统。
- 如果没有在删除服务器终结点之前撤回分层的文件，则这些文件会变得不可用。
- 系统卷上不支持云分层。 要在系统卷上创建服务器终结点，请在创建服务器终结点时禁用云分层。
- 故障转移群集仅适用于群集磁盘，而不适用于群集共享卷 (CSV)。
- 服务器终结点不能嵌套， 但可以与另一终结点并行共存于同一卷上。
- 请勿在服务器终结点中存储 OS 或应用程序分页文件。
- 如果重命名服务器，则不会更新门户中的服务器名称。 若要更新门户中的服务器名称，请注销并重新注册服务器。

### <a name="cloud-endpoint"></a>云终结点
- Azure 文件同步支持直接对 Azure 文件共享进行更改。 但是，首先需要通过 Azure 文件同步更改检测作业来发现对 Azure 文件共享进行的更改。 每 24 小时针对云终结点启动一次更改检测作业。 此外，通过 REST 协议对 Azure 文件共享所做的更改将不会更新 SMB 上次修改时间，亦不会被视为同步更改。
- 存储同步服务和/或存储帐户可以移动到不同的资源组或订阅。 如果移动了存储帐户，则需要向混合文件同步服务授予对存储帐户的访问权限（请参阅[确保 Azure 文件同步可以访问存储帐户](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)）。

### <a name="cloud-tiering"></a>云分层
- 如果使用 Robocopy 将分层的文件复制到另一位置，生成的文件不会分层。 可能会对脱机属性进行设置，因为 Robocopy 会在复制操作中错误地包括该属性。
- 从 SMB 客户端查看文件属性时，脱机属性可能会显示为设置不正确，因为对文件元数据进行了 SMB 缓存。

## <a name="agent-version-30130"></a>代理版本 3.0.13.0
以下发行说明适用于 Azure 文件同步代理版本 3.0.13.0（2018 年 6 月 29 日发布）。 这些说明附加到针对版本 3.0.12.0 列出的发行说明。

此版本包括以下修复：
- 如果服务器上的服务器终结点位置中存在重新分析点，则将服务器添加到现有同步组时，同步将失败。

## <a name="agent-version-30120"></a>代理版本 3.0.12.0
以下发行说明适用于 Azure 文件同步代理版本 3.0.12.0（2018 年 5 月 22 日发布）。

### <a name="agent-installation-and-server-configuration"></a>代理安装和服务器配置
若要详细了解如何使用 Windows Server 安装和配置 Azure 文件同步代理，请参阅[规划 Azure 文件同步部署](storage-sync-files-planning.md)和[如何部署 Azure 文件同步](storage-sync-files-deployment-guide.md)。

- 代理安装包必须使用提升的（管理员）权限进行安装。
- 此代理在 Windows Server Core 或 Nano Server 部署选项上不受支持。
- 只有 Windows Server 2016 和 Windows Server 2012 R2 上支持此代理。
- 代理需要至少 2 GB 的物理内存。
- 存储同步代理 (FileSyncSvc) 服务不支持进行了系统卷信息 (SVI) 目录压缩的卷上的服务器终结点。 此配置会导致意外结果。

### <a name="interoperability"></a>互操作性
- 防病毒应用程序、备份应用程序和其他应用程序在访问分层文件时可能会导致不需要的撤回，除非这些应用程序遵从脱机属性，在读取这些文件的内容时跳过。 有关详细信息，请参阅[对 Azure 文件同步进行故障排除](storage-sync-files-troubleshoot.md)。
- 请勿使用文件服务器资源管理器 (FSRM) 或其他文件屏蔽。 当文件因文件屏蔽而被阻止时，文件屏蔽可能导致无休止的同步故障。
- 不支持在安装了 Azure 文件同步代理的服务器上运行 sysprep，那样做会导致意外结果。 应该在部署服务器映像并完成 sysprep 迷你安装后再安装代理和注册服务器。
- 不支持在同一卷上进行重复数据删除和云分层操作。

### <a name="sync-limitations"></a>同步限制
以下各项不同步，但系统其余部分仍会继续正常运行：
- 长度超过 2,048 个字符的路径。
- 安全描述符的自定义访问控制列表 (DACL) 部分（如果其大小大于 2 KB）。 （只有在单个项上的访问控制条目 (ACE) 大于某个数（大约为 40）的情况下，这才是一个问题。）
- 安全描述符的系统访问控制列表 (SACL) 部分，用于审核。
- 扩展的属性。
- 备用数据流。
- 重分析点。
- 硬链接。
- 将更改从其他终结点同步到服务器文件时，不会保留压缩（如果在该文件上设置了压缩）。
- 任何使用 EFS（或其他用户模式加密方式）加密的文件，此类加密会阻止服务读取数据。 
    
    > [!Note]  
    > Azure 文件同步始终加密传输中的数据， 而在 Azure 中，数据始终进行静态加密。
 
### <a name="server-endpoints"></a>服务器终结点
- 服务器终结点只能在 NTFS 卷上创建。 Azure 文件同步目前不支持 ReFS、FAT、FAT32 等文件系统。
- 系统卷上不支持云分层。 要在系统卷上创建服务器终结点，请在创建服务器终结点时禁用云分层。
- 故障转移群集仅适用于群集磁盘，而不适用于群集共享卷 (CSV)。
- 服务器终结点不能嵌套， 但可以与另一终结点并行共存于同一卷上。
- 请勿在服务器终结点中存储 OS 或应用程序分页文件。
- 如果没有在删除服务器终结点之前撤回分层的文件，则这些文件会变得不可用。
 
### <a name="cloud-tiering"></a>云分层
- 如果使用 Robocopy 将分层的文件复制到另一位置，生成的文件不会分层。 可能会对脱机属性进行设置，因为 Robocopy 会在复制操作中错误地包括该属性。
- 从 SMB 客户端查看文件属性时，脱机属性可能会显示为设置不正确，因为对文件元数据进行了 SMB 缓存。

## <a name="agent-version-2300"></a>代理版本 2.3.0.0
以下发行说明适用于 Azure 文件同步代理版本 2.3.0.0（2018 年 5 月 8 日发布）。 这些说明附加到针对版本 2.0.11.0 列出的发行说明。

此版本包括以下修复：
- 在云分层筛选器驱动程序没有卸载的情况下，代理更新可能会挂起。
- 在同步许多文件时，同步性能可能下降。

## <a name="agent-version-2200"></a>代理版本 2.2.0.0
以下发行说明适用于 Azure 文件同步代理版本 2.2.0.0（2018 年 3 月 12 日发布）。  这些说明是对针对版本 2.1.0.0 和 2.0.11.0 列出的发行说明的补充

由于 FileSyncSvc 未停止，某些客户的 v2.1.0.0 安装会失败。 此更新可修复该问题。

## <a name="agent-version-2100"></a>代理版本 2.1.0.0
以下发行说明适用于 2018 年 2 月 28 日发布的 Azure 文件同步代理版本 2.1.0.0。 这些说明附加到针对版本 2.0.11.0 列出的发行说明。

此版本包括以下更改：
- 改进了群集故障转移处理。
- 改进了对分层文件的可靠处理。
- 支持在添加到 Windows Server 2008 R2 域环境的域控制器计算机上安装代理。
- 此版本修复了：在包含许多文件的服务器上生成诊断过多的问题。
- 改进了会话故障的错误处理。
- 改进了文件传输问题的错误处理。
- 此版本中的更改：运行云分层（如果已在服务器终结点上启用）的默认时间间隔现在为 1 小时。 
- 临时阻止问题：将 Azure 文件同步（存储同步服务）资源移到新的 Azure 订阅。

## <a name="agent-version-20110"></a>代理版本 2.0.11.0
以下发行说明适用于 Azure 文件同步代理版本 2.0.11.0（2018 年 2 月 9 日发布）。 

### <a name="agent-installation-and-server-configuration"></a>代理安装和服务器配置
若要详细了解如何使用 Windows Server 安装和配置 Azure 文件同步代理，请参阅[规划 Azure 文件同步部署](storage-sync-files-planning.md)和[如何部署 Azure 文件同步](storage-sync-files-deployment-guide.md)。

- 代理安装包 (MSI) 必须使用提升的（管理员）权限进行安装。
- 此代理在 Windows Server Core 或 Nano Server 部署选项上不受支持。
- 只有 Windows Server 2016 和 Windows Server 2012 R2 上支持此代理。
- 代理需要至少 2 GB 的物理内存。

### <a name="interoperability"></a>互操作性
- 防病毒应用程序、备份应用程序和其他应用程序在访问分层文件时可能会导致不需要的撤回，除非这些应用程序遵从脱机属性，在读取这些文件的内容时跳过。 有关详细信息，请参阅[对 Azure 文件同步进行故障排除](storage-sync-files-troubleshoot.md)。
- 此版本添加了对 DFS-R 的支持。 有关详细信息，请参阅[规划指南](storage-sync-files-planning.md#distributed-file-system-dfs)。
- 请勿使用文件服务器资源管理器 (FSRM) 或其他文件屏蔽。 当文件因文件屏蔽而被阻止时，文件屏蔽可能导致无休止的同步故障。
- 复制已注册服务器（包括 VM 克隆）可能导致意外结果。 具体而言，同步可能永远不会聚合。
- 不支持在同一卷上进行重复数据删除和云分层操作。
 
### <a name="sync-limitations"></a>同步限制
以下各项不同步，但系统其余部分仍会继续正常运行：
- 长度超过 2,048 个字符的路径。
- 安全描述符的自定义访问控制列表 (DACL) 部分（如果其大小大于 2 KB）。 （只有在单个项上的访问控制条目 (ACE) 大于某个数（大约为 40）的情况下，这才是一个问题。）
- 安全描述符的系统访问控制列表 (SACL) 部分，用于审核。
- 扩展的属性。
- 备用数据流。
- 重分析点。
- 硬链接。
- 将更改从其他终结点同步到服务器文件时，不会保留压缩（如果在该文件上设置了压缩）。
- 任何使用 EFS（或其他用户模式加密方式）加密的文件，此类加密会阻止服务读取数据。 
    
    > [!Note]  
    > Azure 文件同步始终加密传输中的数据， 而在 Azure 中，数据始终进行静态加密。
 
### <a name="server-endpoints"></a>服务器终结点
- 服务器终结点只能在 NTFS 卷上创建。 Azure 文件同步目前不支持 ReFS、FAT、FAT32 等文件系统。
- 服务器终结点不能位于系统卷上。 例如，不能接受 C:\MyFolder 作为路径，除非 C:\MyFolder 为装入点。
- 故障转移群集仅适用于群集磁盘，而不适用于群集共享卷 (CSV)。
- 服务器终结点不能嵌套， 但可以与另一终结点并行共存于同一卷上。
- 此版本增加了对同步根目录的支持，允许将其置于卷的根目录处。
- 请勿在服务器终结点中存储 OS 或应用程序分页文件。
- 此版本中的更改：添加了新的事件，用于跟踪云分层 (EventID 9016)、同步上传进度 (EventID 9302) 以及未同步文件 (EventID 9900) 的总运行时。
- 此版本中的功能改进： 
- 快速 DR 命名空间同步性能大幅提高。
- 删除大量目录（超过 10,000 个）不需要使用 v2* 批量完成。
 
### <a name="cloud-tiering"></a>云分层
- 对以前版本的更改：新文件在 1 小时（以前为 32 小时）内分层，具体取决于分层策略设置。 我们提供 PowerShell cmdlet 进行按需分层。 可以使用此 cmdlet 更有效地评估分层，不需等待后台进程。
- 如果使用 Robocopy 将分层的文件复制到另一位置，生成的文件不会分层。 可能会对脱机属性进行设置，因为 Robocopy 会在复制操作中错误地包括该属性。
- 从 SMB 客户端查看文件属性时，脱机属性可能会显示为设置不正确，因为对文件元数据进行了 SMB 缓存。
- 对以前版本的更改：文件现在是作为分层文件下载到其他服务器上的，但前提是文件是新的，或者已经是分层的文件。

## <a name="agent-version-1100"></a>代理版本 1.1.0.0
以下发行说明适用于 Azure 文件同步代理版本 1.1.0.0（2017 年 9 月 9 日发布，初始预览版）。 

### <a name="agent-installation-and-server-configuration"></a>代理安装和服务器配置
若要详细了解如何使用 Windows Server 安装和配置 Azure 文件同步代理，请参阅[规划 Azure 文件同步部署](storage-sync-files-planning.md)和[如何部署 Azure 文件同步](storage-sync-files-deployment-guide.md)。

- 代理安装包 (MSI) 必须使用提升的（管理员）权限进行安装。
- 此代理在 Windows Server Core 或 Nano Server 部署选项上不受支持。
- 此代理仅在 Windows Server 2016 和 2012 R2 上受支持。
- 代理需要至少 2 GB 的物理内存。

### <a name="interoperability"></a>互操作性
- 防病毒应用程序、备份应用程序和其他应用程序在访问分层文件时可能会导致不需要的撤回，除非这些应用程序遵从脱机属性，在读取这些文件的内容时跳过。 有关详细信息，请参阅[对 Azure 文件同步进行故障排除](storage-sync-files-troubleshoot.md)。
- 请勿使用 FSRM 或其他文件屏蔽。 当文件因文件屏蔽而被阻止时，文件屏蔽可能导致无休止的同步故障。
- 复制已注册服务器（包括 VM 克隆）可能导致意外结果。 具体而言，同步可能永远不会聚合。
- 不支持在同一卷上进行重复数据删除和云分层操作。
 
### <a name="sync-limitations"></a>同步限制
以下各项不同步，但系统其余部分仍会继续正常运行：
- 长度超过 2,048 个字符的路径。
- 安全描述符的 DACL 部分（如果其大小大于 2 KB）。 （只有在单个项上的 ACE 大于某个数（大约为 40）的情况下，这才是一个问题。）
- 安全描述符的 SACL 部分，用于审核。
- 扩展的属性。
- 备用数据流。
- 重分析点。
- 硬链接。
- 将更改从其他终结点同步到服务器文件时，不会保留压缩（如果在该文件上设置了压缩）。
- 任何使用 EFS（或其他用户模式加密方式）加密的文件，此类加密会阻止服务读取数据。 
    
    > [!Note]  
    > Azure 文件同步始终加密传输中的数据， 而在 Azure 中，数据始终进行静态加密。
 
### <a name="server-endpoints"></a>服务器终结点
- 服务器终结点只能在 NTFS 卷上创建。 Azure 文件同步目前不支持 ReFS、FAT、FAT32 等文件系统。
- 服务器终结点不能位于系统卷上。 例如，不能接受 C:\MyFolder 作为路径，除非 C:\MyFolder 为装入点。
- 故障转移群集仅适用于群集磁盘，而不适用于 CSV。
- 服务器终结点不能嵌套， 但可以与另一终结点并行共存于同一卷上。
- 一次从服务器中删除大量（超过 10,000）目录可能会导致同步失败。 请按批删除目录，每批少于 10,000 个。 请确保删除操作同步成功，然后再删除下一批。
- 目前不支持卷的根目录下的服务器终结点。
- 请勿在服务器终结点中存储 OS 或应用程序分页文件。
 
### <a name="cloud-tiering"></a>云分层
- 为了确保可以正确地回调文件，系统可能会在长达 32 小时的时间内不自动对新文件或更改的文件分层。 此过程包括在新服务器终结点进行配置之后的首次分层。 我们提供 PowerShell cmdlet 进行按需分层。 可以使用此 cmdlet 更有效地评估分层，不需等待后台进程。
- 如果使用 Robocopy 将分层的文件复制到另一位置，生成的文件不会分层。 可能会对脱机属性进行设置，因为 Robocopy 会在复制操作中错误地包括该属性。
- 从 SMB 客户端查看文件属性时，脱机属性可能会显示为设置不正确，因为对文件元数据进行了 SMB 缓存。
