---
title: 透過 Azure Site Recovery 遷移內部部署機器
description: 本文說明如何使用 Azure Site Recovery 將內部部署機器移轉至 Azure。
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2020
ms.author: raynew
ms.openlocfilehash: 3c421845d4e15ef13ce98d0de111270159f564fe
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285351"
---
# <a name="migrate-on-premises-machines-to-azure"></a>將內部部署機器移轉至 Azure

本文說明將內部部署機器遷移至 Azure 的選項。 

## <a name="migrate-with-azure-migrate"></a>使用 Azure Migrate 遷移

建議您使用 [Azure Migrate](../migrate/migrate-services-overview.md) 服務，將機器移轉到 Azure。 Azure Migrate 提供使用 Azure Migrate、其他 Azure 服務及協力廠商工具，來評估內部部署機器並將其移轉到 Azure 的集中式中樞。

請遵循下列連結，以 Azure Migrate 進行遷移：

- [深入了解](../migrate/server-migrate-overview.md) VMware VM 的移轉選項，然後使用[無代理程式](../migrate/tutorial-migrate-vmware.md)或[代理程式型](../migrate/tutorial-migrate-vmware-agent.md) 移轉，將 VM 遷移至 Azure。
- [將 Hyper-V VM 遷移至 Azure](../migrate/tutorial-migrate-hyper-v.md)。
- 將[實體伺服器或其他 VM](../migrate/tutorial-migrate-physical-virtual-machines.md) (包括 [AWS執行個體](../migrate/tutorial-migrate-aws-virtual-machines.md)) 遷移到 Azure。

## <a name="migrate-with-site-recovery"></a>使用 Site Recovery 遷移
Site Recovery 應該僅用於災害復原，而不應用於移轉。

如果您已經在使用 Azure Site Recovery，而想繼續用其進行移轉，請遵循您用來進行災害復原的相同步驟。

- VMware VM：[準備 Azure](tutorial-prepare-azure.md)和 [VMware](vmware-azure-tutorial-prepare-on-premises.md)，開始[複寫機器](vmware-azure-tutorial.md)，[檢查](tutorial-dr-drill-azure.md)一切是否如常運作，並[執行容錯移轉](vmware-azure-tutorial-failover-failback.md)。
- Hyper-V VM：[準備 Azure](tutorial-prepare-azure-for-hyperv.md)和 [Hyper-V](hyper-v-prepare-on-premises-tutorial.md)，開始[複寫機器](hyper-v-azure-tutorial.md)，[檢查](tutorial-dr-drill-azure.md)一切是否如常運作，並[執行容錯移轉](hyper-v-azure-failover-failback-tutorial.md)。
- 實體伺服器：[遵循逐步解說](physical-azure-disaster-recovery.md)來準備 Azure、準備進行災害復原的機器，以及設定複寫。

> [!NOTE]
> 執行容錯移轉以進行災害復原時，您會在最後一個步驟中認可容錯移轉。 遷移內部部署機器時，[認可] 選項並不相關。 相反地，您可以選取 [完成移轉] 選項。 

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [檢閱關於 Azure Migrate 的常見問題](../migrate/resources-faq.md)。

  
