---
ms.assetid: ''
title: Azure Key Vault 安全体系 | Microsoft Docs
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 7fd7357a317d5059d6169de2c1536568a254e016
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2018
ms.locfileid: "42145684"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault 安全体系和地理边界

Azure Key Vault 是一项多租户服务，在每个 Azure 位置使用一个硬件安全模块 (Hsm) 池。 

同一地理区域中 Azure 位置的所有 HSM 都共享相同的加密边界（Thales 安全体系）。 例如，美国东部和美国西部共享相同的安全体系，因为它们都属于地理位置美国。 同样，位于日本的所有 Azure 位置共享相同的安全体系，位于澳大利亚、印度等地理位置的所有 Azure 位置也是如此。 

## <a name="backup-and-restore-behavior"></a>备份和还原行为

只要满足以下两个条件，从某一 Azure 位置中的 Key Vault 对密钥所执行的备份可以还原到另一 Azure 位置中的 Key Vault：

- 两个 Azure 位置属于相同的地理位置
- 两个 Key Vault 属于同一个 Azure 订阅

例如，在印度西部的 Key Vault 中由给定订阅对密钥执行的备份，只能还原到相同订阅和地理位置（印度西部、印度中部或印度南部）中的另一个 Key Vault。

## <a name="regions-and-products"></a>区域和产品

- [Azure 区域](https://azure.microsoft.com/regions/)
- [Microsoft 产品（按区域）](https://azure.microsoft.com/regions/services/)

区域映射到安全体系，如表中的主要标题所示：

例如，在产品（按区域）文章中，“美国”标签包含美国东部、美国中部、美国西部，这些都会映射到美国区域。 

>[!NOTE]
>例外情况是美国 DOD 东部和美国 DOD 中部具有自己的安全体系。 

同样，在“欧洲”标签上，欧洲北部和欧洲西部都映射到欧洲区域。 “亚太区”标签也是如此。



