---
title: 在 Azure 虛擬機器擴展集中管理容錯網域
description: 了解如何在建立虛擬機器擴展集時選擇正確的 FD 數目。
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 12/18/2018
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: bf55c1f7de751f03fb804eb263cf0810a48378e1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494882"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>為虛擬機器擴展集選擇正確的容錯網域數目
在沒有任何區域 (zone) 的 Azure 區域 (region) 中，預設會建立具有五個容錯網域的虛擬機器擴展集。 針對支援區域部署虛擬機器擴展集的地區，如果選取此選項，則每個區域的容錯網域計數預設值為1。 在此情況下，FD = 1 表示屬於擴展集的 VM 執行個體會儘可能分散於許多機架。

您也可以考慮讓擴展集容錯網域數目與受控磁碟容錯網域數目保持一致。 如果整個受控磁碟容錯網域停止運作，此一致狀態有助於防止仲裁遺失。 您可將 FD 計數設定為小於或等於每個區域中可用的受控磁碟容錯網域數目。 請參考這份[文件](../virtual-machines/windows/manage-availability.md)，了解各區域的受控磁碟容錯網域數目。

## <a name="rest-api"></a>REST API
您可以將屬性設定 `properties.platformFaultDomainCount` 為1、2或3（如果未指定，則預設值為3）。 請參考[這裡](/rest/api/compute/virtualmachinescalesets/createorupdate)的 REST API 文件。

## <a name="azure-cli"></a>Azure CLI
您可以將參數設定 `--platform-fault-domain-count` 為1、2或3（如果未指定，則預設值為3）。 請參考[這裡](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)的 Azure CLI 文件。

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

建立及設定所有擴展集資源和 VM 需要幾分鐘的時間。

## <a name="next-steps"></a>後續步驟
- 深入了解 Azure 環境的[可用性和備援功能](../virtual-machines/windows/availability.md)。
