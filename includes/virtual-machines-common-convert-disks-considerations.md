---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 091e4dc0759ce1b227349d5d1b6b08b9eda66ee0
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2020
ms.locfileid: "84237696"
---
* 轉換需要重新啟動 VM，因此請在預先存在的維護期間排定 VM 移轉。 

* 轉換是無法復原的。 

* 請注意任何具有[虛擬機器參與者](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor)角色的使用者將無法變更 VM 大小 (因為它們可能為轉換前)。 這是因為具有受控磁碟的 VM 需要使用者具有 OS 磁碟上的 Microsoft.Compute/disks/write 權限。

* 請務必先進行轉換測試。 在生產環境中執行移轉之前，請先移轉測試虛擬機器。

* 在轉換期間，您會解除配置 VM。 在轉換後啟動 VM 時，VM 會接收新的 IP 位址。 如有必要，您可以對 VM [指派靜態 IP 位址](../articles/virtual-network/public-ip-addresses.md)。

* 檢閱支援轉換程序所需的 Azure VM 代理程式最小版本。 如需如何檢查和更新代理程式版本的詳細資訊，請參閱[在 Azure 中 VM 代理程式的最小版本支援](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)