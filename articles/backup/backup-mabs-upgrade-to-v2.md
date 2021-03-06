---
title: 安装 Azure 备份服务器 v2
description: Azure 备份服务器 v2 可提供用于保护 VM、文件和文件夹、工作负载等的增强备份功能。 了解如何安装或升级到 Azure 备份服务器 v2。
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: a458a46f3775a593f369d5acb967fc90d61efde8
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628335"
---
# <a name="install-azure-backup-server-v2"></a>安装 Azure 备份服务器 v2

Azure 备份服务器可帮助保护虚拟机 (VM)、工作负载、文件和文件夹等。 Azure 备份服务器 v2 以 Azure 备份服务器 v1 为基础进行构建，提供了在 v1 中不可用的新功能。 有关 v1 与 v2 功能的比较，请参阅 [Azure 备份服务器保护矩阵](backup-mabs-protection-matrix.md)。 

备份服务器 v2 中的其他功能是针对备份服务器 v1 的升级。 但是，备份服务器 v1 不是安装备份服务器 v2 的先决条件。 如果要从备份服务器 v1 升级到备份服务器 v2，请在备份服务器保护服务器上安装备份服务器 v2。 现有备份服务器设置保持不变。

可在 Windows Server 2016 或 Windows Server 2012 R2 上安装备份服务器 v2。 若要利用新功能（如 System Center 2016 Data Protection Manager 新式备份存储），必须在 Windows Server 2016 上安装备份服务器 v2。 升级到或安装备份服务器 v2 之前，请阅读[安装先决条件](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)。

> [!NOTE]
> Azure 备份服务器的基本代码与 System Center Data Protection Manager 相同。 备份服务器 v1 相当于 Data Protection Manager 2012 R2。 备份服务器 v2 相当于 Data Protection Manager 2016。 本文有时会引用 Data Protection Manager 文档。
>
>

## <a name="upgrade-backup-server-to-v2"></a>将备份服务器升级到 v2
若要从备份服务器 v1 升级到备份服务器 v2，请确保安装具有所需更新：

- 在受保护的服务器上[更新保护代理](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent)。
- 将 Windows Server 2012 R2 升级到 Windows Server 2016。
- 在所有生产服务器上升级 Azure 备份服务器远程管理员。
- 确保备份设置为继续进行而不重新启动生产服务器。


### <a name="upgrade-steps-for-backup-server-v2"></a>备份服务器 v2 的升级步骤

1. 在下载中心内，[下载升级安装程序](https://go.microsoft.com/fwlink/?LinkId=626082)。

2. 提取安装向导之后，确保选择“执行 setup.exe”，然后选择“完成”。

  ![安装程序 - 运行安装程序](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. 在 Microsoft Azure 备份服务器向导中的“安装”下，选择“Microsoft Azure 备份服务器”。

  ![安装程序 - 选择“安装”](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. 在“欢迎”页上，检查警告，然后选择“下一步”。

  ![安装程序 -“欢迎”页](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. 安装向导会执行先决条件检查，以确保环境可以进行升级。 在“先决条件检查”页上，选择“检查”。

  ![安装程序 -“先决条件检查”页](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. 环境必须通过先决条件检查。 如果环境未通过检查，请记下问题并修复它们。 然后选择“再次检查”。 通过先决条件检查之后，选择“下一步”。

  ![安装程序 -“再次检查”按钮](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. 在“SQL 设置”页上，选择 SQL 安装的相关选项，然后选择“检查并安装”。

  ![安装程序 -“SQL 设置”页](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  检查可能需要几分钟。 检查完成之后，选择“下一步”。

  ![安装程序 - SQL 设置“检查并安装”按钮](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. 在“安装设置”页上，对安装备份服务器的位置或“暂存位置”进行任何更改。 选择“**下一步**”。

  ![安装程序 -“安装设置”页](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. 若要完成安装向导，请选择“完成”。

  ![安装程序 - 完成](./media/backup-mabs-upgrade-to-v2/run-setup.png)

## <a name="add-storage-for-modern-backup-storage"></a>为 Modern Backup Storage 添加存储

为了提高备份存储效率，备份服务器 v2 添加了卷支持。 备份服务器 v1 和备份服务器 v2 都支持磁盘。

### <a name="add-volumes-and-disks"></a>添加卷和磁盘

如果在 Windows Server 2016 上运行备份服务器 v2，则可以使用卷存储备份数据。 卷可节省存储并加快备份。 因为卷是备份服务器的新增功能，所以必须添加它们。 

将卷添加到备份服务器时，可以为卷提供友好名称。 选择要命名的卷的“友好名称”列。 可以在以后需要时更改名称。 还可以使用 PowerShell 为卷添加或更改友好名称。

在管理员控制台中添加卷：

1. 在 Azure 备份服务器管理员控制台中，选择“管理” > “磁盘存储” > “添加”。

  ![打开“添加磁盘存储”向导](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

  此时会打开“添加磁盘存储”向导。

2. 在“添加磁盘存储”页上的“可用卷”框中，选择卷，然后选择“添加”。

3. 在“所选卷”框中，输入卷的友好名称，然后选择“确定”。

  ![“添加磁盘存储”向导 - 添加卷](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  如果要添加磁盘，则磁盘必须属于具有旧存储的保护组。 这些磁盘只能用于这些保护组。 如果备份服务器没有具有旧保护的源，则不会列出磁盘。

  有关添加磁盘的详细信息，请参阅[添加磁盘以增大旧存储](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage)。 无法为磁盘提供友好名称。


### <a name="assign-workloads-to-volumes"></a>将工作负载分配给卷

在备份服务器中，可指定将哪些工作负载分配给哪些卷。 例如，可以将支持较高每秒输入/输出操作次数 (IOPS) 的昂贵卷设置为仅存储需要频繁的高容量备份的工作负载。 一个示例是具有事务日志的 SQL Server。

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

若要更新备份服务器上存储池中的卷的属性，请使用 PowerShell cmdlet **Update-DPMDiskStorage**。

语法：

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [<CommonParameters>]
```

使用 PowerShell 进行的所有更改都会在 UI 中得到反映。


## <a name="protect-data-sources"></a>保护数据源
若要开始保护数据源，请创建保护组。 以下步骤重点说明对新建保护组向导进行的更改或添加。

创建保护组：

1. 在备份服务器管理员控制台中，选择“保护”。

2. 在工具功能区中，选择“新建”。

  此时会打开“创建新保护组”向导。

  ![“创建新保护组”向导](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. 在“欢迎”页上，选择“下一步”。

4. 在“选择保护组类型”页上，选择要创建的保护组的类型，然后选择“下一步”。

  ![“选择保护组类型”页](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. “选择组成员”页上的“可用成员”框中列出了具有保护代理的成员。 对于此示例，请选择卷 D:\ 和 E:\,，然后将其添加到“所选成员”中。 选择“**下一步**”。

  ![“选择组成员”页](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. 在“选择数据保护方法”页上，输入“保护组名称”，选择保护方法，然后选择“下一步”。 若要进行短期保护，请选择“磁盘”备份方法。

  ![“选择数据保护方法”页](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. 在“指定短期目标”页上，选择“保持期”和“同步频率”的详细信息。 然后，选择“下一步”。 （可选）若要针对恢复点执行时间更改计划，请选择“修改”。

  ![“指定短期目标”页](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. 在“检查磁盘存储分配”页上，检查有关所选数据源的详细信息，包括数据源的大小、要预配的空间值和目标存储卷。

  ![“检查磁盘存储分配”页](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  存储卷基于工作负载卷分配（使用 PowerShell 设置）和可用存储。 可以通过在下拉菜单中选择其他卷来更改存储卷。 如果你更改“目标存储”的值，则“可用磁盘存储”的值会动态更改以在“可用空间”和“设置不足的空间”下反映值。

  如果数据源按计划增长，则“可用磁盘存储”中的“设置不足的空间”列的值反映所需额外存储量。 使用此值可帮助规划存储需求以实现平稳备份。 如果该值为零，则在可预见的将来内不存在潜在存储问题。 如果该值为非零数字，则未分配足够存储（基于保护策略和受保护成员的数据大小）。

  ![分配不足的磁盘存储](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

  若要完成保护组创建，请完成向导。

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>将旧存储迁移到 Modern Backup Storage
升级到或安装备份服务器 v2 并将操作系统升级到 Windows Server 2016 之后，可更新保护组以使用新式备份存储。 默认情况下，保护组不会进行更改。 它们会继续按照初始设置运行。 

可以选择更新保护组以使用新式备份存储。 若要更新保护组，请使用保留数据选项停止所有数据源的保护。 然后，将数据源添加到新保护组。

1. 在 System Center 2016 DPM 管理员控制台中，选择“保护”功能。 在“保护组成员”列表中，右键单击成员，然后选择“停止保护成员”。

  ![停止保护成员](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. 在“从组中删除”对话框中，检查存储池的已用磁盘空间和可用空闲空间。 默认设置是在磁盘上保留恢复点，并让它们可以按照关联保留策略过期。 选择“确定”。

  如果要立即将已用磁盘空间返回到可用存储池，则选中“删除磁盘上的副本”复选框以删除与成员关联的备份数据（和恢复点）。

  ![“从组中删除”对话框](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. 创建一个使用新式备份存储的保护组。 包括未受保护的数据源。


## <a name="add-disks-to-increase-legacy-storage"></a>添加磁盘以增大旧存储

如果要将旧存储与备份服务器一起使用，则可能需要添加磁盘以增大旧存储。 

添加磁盘存储：

1. 在 System Center 2016 DPM 管理员控制台中，选择“管理” > “磁盘存储” > “添加”。

  ![“添加磁盘存储”对话框](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. 在“添加磁盘存储”对话框中，选择“添加磁盘”。

3. 在可用磁盘列表中，选择要添加的磁盘。 依次选择“添加”、“确定”。

## <a name="update-the-data-protection-manager-protection-agent"></a>更新 Data Protection Manager 保护代理

备份服务器将 System Center Data Protection Manager 保护代理用于更新。 若要升级未连接到网络的保护代理，则无法使用 Data Protection Manager 管理员控制台完成已连接代理升级。 必须在非活动域环境中升级保护代理。 在客户端计算机连接到网络之前，Data Protection Manager 管理员控制台会显示保护代理更新处于挂起状态。

以下部分介绍如何为连接的客户端计算机和未连接的客户端计算机更新保护代理。

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>为连接的客户端计算机更新保护代理

1. 在备份服务器管理员控制台中，选择“管理” > “代理”。

2. 在显示窗格中，选择要为其更新保护代理的客户端计算机。

  > [!NOTE]
  > “代理更新”列指示每个受保护计算机何时有保护代理更新可用。 在“操作”窗格中，仅当选择了受保护计算机并且有可用更新时，“更新”操作才可用。

3. 要在所选计算机上安装更新的保护代理，请在“操作”窗格中，选择“更新”。

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>在未连接的客户端计算机上更新保护代理

1. 在备份服务器管理员控制台中，选择“管理” > “代理”。

2. 在显示窗格中，选择要为其更新保护代理的客户端计算机。

  > [!NOTE]
  > “代理更新”列指示每个受保护计算机何时有保护代理更新可用。 在“操作”窗格中，“更新”操作在选择了受保护计算机时不可用，除非有更新可用。

3. 要在所选计算机上安装更新的保护代理，请选择“更新”。

4. 对于未连接到网络的客户端计算机，在计算机连接到网络之前，“代理状态”列会显示“挂起更新”状态。

  在客户端计算机连接到网络之后，客户端计算机的“代理更新”列会显示“正在更新”状态。
  
### <a name="move-legacy-protection-groups-from-the-old-version-and-sync-the-new-version-with-azure"></a>通过 Azure 将旧保护组从旧版本中移出并同步新版本

Azure 备份服务器和 OS 均更新后，便可以使用新式备份存储保护新的数据源。 已受保护的数据源继续以之前存储在 Azure 备份服务器时的保护方式（旧式）接受保护。 所有新保护组使用新式备份存储。

将数据源从旧式保护模式迁移到新式备份存储：

1.  将新卷添加到 Data Protection Manager 存储池。 还可以分配友好名称并选择数据源标记。

2. 对于处于旧模式的每个数据源，请停止保护数据源。 然后选择“保留受保护的数据”。  这样，便可以在迁移后恢复旧恢复点。

3. 创建新保护组。 然后，选择要使用新格式存储的数据源。

  Data Protection Manager 将旧式备份存储中的副本存储在新式备份存储卷的本地。
    > [!NOTE] 
    > 创建副本的操作如同恢复后的操作作业。

  然后，所有新同步点和恢复点将存储在新式备份存储中。

  旧恢复点在过期后将被删除。 删除旧恢复点后，将释放磁盘空间。

  删除旧存储中的所有旧式卷后，可从 Azure 备份中删除磁盘。 然后可从系统中删除磁盘。

4. 创建 Data Protection Manager 数据库的备份。

  > [!IMPORTANT]
  > - 新服务器的名称必须与原始 Azure 备份服务器实例的名称相同。 若要使用以前的存储池和 Data Protection Manager 数据库来保留恢复点，则不能更改新 Azure 备份服务器实例的名称。
  > - 必须创建 Data Protection Manager 数据库的备份。 稍后需要还原数据库。

5. 关闭原始 Azure 备份服务器实例，或使服务器脱机。

6. 重置 Active Directory 中的计算机帐户。

7. 在新计算机上安装 Windows Server 2016。 对于服务器名称，请使用与原始 Azure 备份服务器实例相同的计算机名称。

8. 加入域。

9. 安装 Azure 备份服务器 v2。 （从旧服务器中删除 Data Protection Manager 存储池磁盘，并将其导入新服务器。）

10. 还原在步骤 4 中创建的 Data Protection Manager 数据库。

11. 将存储从原始备份服务器连接到新服务器。

12. 在 SQL Server 中，还原 Data Protection Manager 数据库。

13. 在新服务器上的管理员命令行中，使用 `cd` 转到 Microsoft Azure 备份安装位置和 bin 文件夹。  

  示例：  
  C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\ to Azure Backup

14. 运行 `DPMSYNC -SYNC`。
  
  > [!NOTE]
  > 如果已将新磁盘添加到 Data Protection Manager 存储池（而不是移动旧磁盘），请运行 `DPMSYNC -Reallocatereplica`。

## <a name="new-powershell-cmdlets-in-backup-server-v2"></a>备份服务器 v2 中的新 PowerShell cmdlet

安装 Azure 备份服务器 v2 时，有两个新 cmdlet 可用： 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>后续步骤

了解如何准备服务器或开始保护工作负载：
- [准备备份服务器工作负载](backup-azure-microsoft-azure-backup.md)
- [使用备份服务器备份 VMware 服务器](backup-azure-backup-server-vmware.md)
- [使用备份服务器备份 SQL Server](backup-azure-sql-mabs.md)
- [将新式备份存储与备份服务器一起使用](backup-mabs-add-storage.md)

