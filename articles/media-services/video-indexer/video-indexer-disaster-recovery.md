---
title: 影片索引器容錯移轉和災害復原
titleSuffix: Azure Media Services
description: 瞭解發生區域資料中心失敗或嚴重損壞時，如何容錯移轉至次要影片索引子帳戶。
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: eab376c44065979de86e5c70b796be952fccffaa
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87065400"
---
# <a name="video-indexer-failover-and-disaster-recovery"></a>影片索引器容錯移轉和災害復原

如果發生區域資料中心中斷或失敗，Azure 媒體服務影片索引器不會立即將服務容錯移轉。 本文說明如何設定您的環境以進行容錯移轉，以確保應用程式的最佳可用性，並在發生嚴重損壞時減少復原時間。

我們建議您設定跨區域配對的商務持續性災害復原 (BCDR)，以善用 Azure 的隔離與可用性原則。 如需詳細資訊，請參閱 [Azure 配對區域](../../best-practices-availability-paired-regions.md)。

## <a name="prerequisites"></a>先決條件

Azure 訂用帳戶。 如果您還沒有 Azure 訂用帳戶，請註冊[azure 免費試用](https://azure.microsoft.com/free/)。

## <a name="failover-to-a-secondary-account"></a>容錯移轉至次要帳戶

若要執行 BCDR，您需要有兩個影片索引子帳戶來處理冗余。

1. 建立兩個連線到 Azure 的影片索引子帳戶（請參閱[建立影片索引子帳戶](connect-to-azure.md)）。 為主要區域和配對的 azure 區域建立一個帳戶。
1. 如果您的主要區域發生失敗，請使用次要帳戶切換到索引編制。

> [!TIP]
> 您可以根據針對[服務通知建立活動記錄警示](../../service-health/alerts-activity-log-service-notifications-portal.md)的方式，設定服務健康狀態通知的活動記錄警示，以自動化 BCDR。

如需使用多個租使用者的相關資訊，請參閱[管理多個](manage-multiple-tenants.md)租使用者。 若要執行 BCDR，請選擇下列兩個選項之一：每個租使用者的[影片索引子帳戶](manage-multiple-tenants.md#video-indexer-account-per-tenant)或[每個租使用者的 Azure 訂](manage-multiple-tenants.md#azure-subscription-per-tenant)用帳戶

## <a name="next-steps"></a>接下來的步驟

[管理連線到 Azure 的影片索引子帳戶](manage-account-connected-to-azure.md)。
