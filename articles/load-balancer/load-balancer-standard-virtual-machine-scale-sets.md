---
title: Azure Standard Load Balancer 和虛擬機器擴展集
titleSuffix: Azure Standard Load Balancer and Virtual Machine Scale Sets
description: 透過此學習路徑，開始使用 Azure Standard Load Balancer 和虛擬機器擴展集。
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: irenehua
ms.openlocfilehash: f5a8453f84854108facb4da4616a7d362e0fb38c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531305"
---
# <a name="azure-load-balancer-with-azure-virtual-machine-scale-sets"></a>使用 Azure 虛擬機器擴展集 Azure Load Balancer

使用虛擬機器擴展集和負載平衡器時，應該考慮下列指導方針：

## <a name="multiple-virtual-machine-scale-sets-cant-use-the-same-load-balancer"></a>多個虛擬機器擴展集不能使用相同的負載平衡器
## <a name="port-forwarding-and-inbound-nat-rules"></a>埠轉送和輸入 NAT 規則：
  * 建立擴展集之後，無法修改負載平衡器健全狀況探查所使用的負載平衡規則的後端埠。 若要變更埠，您可以藉由更新 Azure 虛擬機器擴展集來移除健康情況探查、更新埠，然後再次設定健康情況探查。
  * 在負載平衡器的後端集區中使用虛擬機器擴展集時，會自動建立預設的輸入 NAT 規則。
## <a name="inbound-nat-pool"></a>輸入 NAT 集區：
  * 每個虛擬機器擴展集必須至少有一個輸入 NAT 集區。 
  * 輸入 NAT 集區是輸入 NAT 規則的集合。 一個輸入 NAT 集區無法支援多個虛擬機器擴展集。
  
## <a name="load-balancing-rules"></a>負載平衡規則：
  * 在負載平衡器的後端集區中使用虛擬機器擴展集時，會自動建立預設的負載平衡規則。
## <a name="outbound-rules"></a>輸出規則：
  *  若要建立負載平衡規則已參考之後端集區的輸出規則，您必須先在建立輸入負載平衡規則時，將入口網站中的 **[建立隱含輸出規則]** 標示為 [**否**]。

  :::image type="content" source="./media/vm-scale-sets/load-balancer-and-vm-scale-sets.png" alt-text="建立負載平衡規則" border="true":::

下列方法可用於部署具有現有 Azure 負載平衡器的虛擬機器擴展集。

* [使用 Azure 入口網站，設定具有現有 Azure Load Balancer 的虛擬機器擴展集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-portal)。
* [使用 Azure PowerShell，設定具有現有 Azure Load Balancer 的虛擬機器擴展集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-powershell)。
* [使用 Azure CLI，設定具有現有 Azure Load Balancer 的虛擬機器擴展集](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-cli)。
