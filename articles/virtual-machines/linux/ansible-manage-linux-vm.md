---
title: 使用 Ansible 在 Azure 中管理 Linux 虚拟机
description: 了解如何使用 Ansible 在 Azure 中管理 Linux 虚拟机
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 66106346b298fae22cce47081916a6c8eec8fd40
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250635"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>使用 Ansible 在 Azure 中管理 Linux 虚拟机
使用 Ansible 可以在环境中自动部署和配置资源。 可以使用 Ansible 管理 Azure 虚拟机，就像管理任何其他资源一样。 本文介绍如何使用 Ansible playbook 启动和停止 Linux 虚拟机。 

## <a name="prerequisites"></a>先决条件

- **Azure 订阅** - 如果没有 Azure 订阅，请创建一个[免费帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。

- **配置 Azure Cloud Shell** 或**在 Linux 虚拟机上安装和配置 Ansible**

  **配置 Azure Cloud Shell**

  1. **配置 Azure Cloud Shell** - 如果不熟悉 Azure Cloud Shell，请参阅 [Bash in Azure Cloud Shell 快速入门](/azure/cloud-shell/quickstart)一文，了解如何启动和配置 Cloud Shell。 

  1. **Linux 虚拟机** - 如果无法访问 Linux 虚拟机，可以[使用 Ansible 创建虚拟机](ansible-create-vm.md)。

  **--或者--**

  **在 Linux 虚拟机上安装和配置 Ansible**

  1. **安装 Ansible** - 在[支持的 Linux 平台](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine)上安装 Ansible。

  1. **配置 Ansible** - [创建 Azure 凭据并配置 Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>使用 Ansible 解除分配（停止）Azure 虚拟机
本部分演示如何使用 Ansible 解除分配（停止）Azure 虚拟机

1. 登录到 [Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)。

1. 打开 [Cloud Shell](/azure/cloud-shell/overview)。

1. 创建名为 `azure_vm_stop.yml` 的文件（用于包含 playbook）并在 VI 编辑器中将其打开，如下所示：

  ```azurecli-interactive
  vi azure_vm_stop.yml
  ```

1. 按 **I** 键进入插入模式。

1. 将以下示例代码粘贴到编辑器中：

    ```yaml
    - name: Stop Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
            allocated: no 
    ```

1. 按 **Esc** 键退出插入模式。

1. 保存文件，然后输入以下命令退出 vi 编辑器：

    ```bash
    :wq
    ```

1. 运行示例 Ansible playbook。

  ```bash
  ansible-playbook azure_vm_stop.yml
  ```

1. 输出如以下示例所示，其中显示虚拟机已成功地解除分配（停止）：

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>使用 Ansible 启动已解除分配（停止）的 Azure 虚拟机
本部分演示如何使用 Ansible 启动已解除分配（停止）的 Azure 虚拟机

1. 登录到 [Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)。

1. 打开 [Cloud Shell](/azure/cloud-shell/overview)。

1. 创建名为 `azure_vm_start.yml` 的文件（用于包含 playbook）并在 VI 编辑器中将其打开，如下所示：

  ```azurecli-interactive
  vi azure_vm_start.yml
  ```

1. 按 **I** 键进入插入模式。

1. 将以下示例代码粘贴到编辑器中：

    ```yaml
    - name: Start Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Start the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
    ```

1. 按 **Esc** 键退出插入模式。

1. 保存文件，然后输入以下命令退出 vi 编辑器：

    ```bash
    :wq
    ```

1. 运行示例 Ansible playbook。

  ```bash
  ansible-playbook azure_vm_start.yml
  ```

1. 输出如以下示例所示，其中显示虚拟机已成功启动：

    输出如以下示例所示，其中显示虚拟机已成功启动：

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [使用 Ansible 管理 Azure 动态库存](../../ansible/ansible-manage-azure-dynamic-inventories.md)