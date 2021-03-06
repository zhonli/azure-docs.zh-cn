---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e49cfe786272b34675ca377808e2961e61a757e4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197578"
---
可以连接到已部署到 VNet 的 VM，方法是创建到 VM 的远程桌面连接。 若要通过初始验证来确认能否连接到 VM，最好的方式是使用其专用 IP 地址而不是计算机名称进行连接。 这种方式是测试能否进行连接，而不是测试名称解析是否已正确配置。 

1. 定位 VM 的专用 IP 地址。 查找 VM 的专用 IP 地址时，可以通过 Azure 门户或 PowerShell 查看 VM 的属性。
2. 验证你是否已使用点到站点 VPN 连接连接到 VNet。 
3. 打开远程桌面连接，方法是：在任务栏的搜索框中键入“RDP”或“远程桌面连接”，并选择“远程桌面连接”。 也可在 PowerShell 中使用“mstsc”命令打开远程桌面连接。 
3. 在远程桌面连接中，输入 VM 的专用 IP 地址。 可以通过单击“显示选项”来调整其他设置，并进行连接。

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>排查到 VM 的 RDP 连接的问题

如果无法通过 VPN 连接连接到虚拟机，可以查看一些项目。 如需更多故障排除信息，请参阅[排查到 VM 的远程桌面连接问题](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)。

- 验证 VPN 连接是否成功。
- 验证是否已连接到 VM 的专用 IP 地址。
- 使用“ipconfig”检查分配给以太网适配器的 IPv4 地址，该适配器所在的计算机正是你要从其进行连接的计算机。 如果该 IP 地址位于要连接到的 VNet 的地址范围内，或者位于 VPNClientAddressPool 的地址范围内，则称为地址空间重叠。 当地址空间以这种方式重叠时，网络流量不会抵达 Azure，而是呆在本地网络中。
- 如果可以使用专用 IP 地址连接到 VM，但不能使用计算机名称进行连接，则请验证是否已正确配置 DNS。 若要详细了解如何对 VM 进行名称解析，请参阅[针对 VM 的名称解析](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。
- 验证是否在为 VNet 指定 DNS 服务器 IP 地址之后，才生成 VPN 客户端配置包。 如果更新了 DNS 服务器 IP 地址，请生成并安装新的 VPN 客户端配置包。