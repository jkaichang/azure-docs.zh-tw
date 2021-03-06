---
title: Azure 進階儲存體-針對高效能進行設計
description: 使用 Azure 進階 SSD 受控磁碟設計高效能應用程式。 「進階儲存體」可針對在「Azure 虛擬機器」上執行且需要大量 I/O 的工作負載，提供高效能、低延遲的磁碟支援。
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 46c917a2d27f5efaa1e902db0d5da5375578b8b8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526025"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure 進階儲存體：專為高效能而設計
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> 有時候，看似磁碟效能問題的情況，其實是網路瓶頸。 在這些情況下，您應該將您的[網路效能](../../virtual-network/virtual-network-optimize-network-bandwidth.md)最佳化。
>
> 如果要對磁碟進行基準測試，請參閱[對磁碟進行效能評定](disks-benchmarks.md)的文章。
>
> 如果您的 VM 支援加速網路，您應確實加以啟用。 如果未啟用，您可以對 [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) 和 [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) 上已部署的 VM 加以啟用。

開始之前，如果您不熟悉進階儲存體，請先閱讀[為 IaaS VM 選取 Azure 磁碟類型](disks-types.md) \(英文\) 和[進階儲存體帳戶的延展性目標](../../storage/blobs/scalability-targets-premium-page-blobs.md)。

[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

如果要對磁碟進行基準測試，請參閱[對磁碟進行效能評定](disks-benchmarks.md)的文章。

深入了解可用的磁碟類型：[選取磁碟類型](disks-types.md)  

若為 SQL Server 使用者，請參閱「SQL Server 的效能最佳作法」文章：

* [Azure 虛擬機器中的 SQL Server 效能最佳作法](../../azure-sql/virtual-machines/windows/performance-guidelines-best-practices.md)
* [Azure 進階儲存體為 Azure VM 中的 SQL Server 提供最高效能](https://cloudblogs.microsoft.com/sqlserver/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm/)
