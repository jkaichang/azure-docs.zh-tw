---
title: 使用 Azure CDN 自訂網域透過 HTTPS 存取儲存體 Blob
description: 了解如何為自訂 Blob 儲存體端點新增 Azure CDN 自訂網域，並在該網域上啟用 HTTPS。
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 5b6fe2b2704f101a7775b7eb700375105b0a9eca
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81259879"
---
# <a name="tutorial-access-storage-blobs-using-an-azure-cdn-custom-domain-over-https"></a>教學課程：使用 Azure CDN 自訂網域透過 HTTPS 存取儲存體 Blob

在您整合 Azure 儲存體帳戶與 Azure 內容傳遞網路 (CDN) 之後，即可新增自訂網域，並在該網域上啟用自訂 Blob 儲存體端點的 HTTPS。 

## <a name="prerequisites"></a>Prerequisites

您必須先整合 Azure 儲存體帳戶與 Azure CDN，才能完成本教學課程中的步驟。 如需詳細資訊，請參閱[快速入門：整合 Azure 儲存體帳戶與 Azure CDN](cdn-create-a-storage-account-with-cdn.md)。

## <a name="add-a-custom-domain"></a>新增自訂網域
當您在設定檔中建立 CDN 端點時，根據預設，端點名稱 (即 azureedge.net 的子網域) 會包含在傳遞 CDN 內容的 URL 中。 您也可以選擇建立自訂網域與 CDN 端點的關聯。 使用此選項時，您會在 URL 中使用自訂網域來傳遞內容，而不是使用端點名稱。 若要將自訂網域新增至您的端點，請遵循本教學課程中的指示︰[將自訂網域新增至 Azure CDN 端點](cdn-map-content-to-custom-domain.md)。

## <a name="configure-https"></a>設定 HTTPS
在自訂網域上使用 HTTPS 通訊協定，確保在網際網路上透過 TLS/SSL 加密安全地傳遞資料。 當網頁瀏覽器透過 HTTPS 連線到網站時，它會驗證網站的安全性憑證，並確認憑證是由合法的憑證授權單位所核發的。 若要在自訂網域上設定 HTTPS，請遵循本教學課程中的指示︰[在 Azure CDN 自訂網域上設定 HTTPS](cdn-custom-ssl.md)。

## <a name="shared-access-signatures"></a>共用存取簽章
如果您的 Blob 儲存體端點設定為不允許匿名讀取權限，則應該在您對您的自訂網域提出的每個要求中提供[共用存取簽章 (SAS)](cdn-sas-storage-support.md) 權杖。 根據預設，Blob 儲存體端點不允許匿名讀取權限。 如需 SAS 的詳細資訊，請參閱[管理對容器與 Blob 的匿名讀取權限](../storage/blobs/storage-manage-access-to-resources.md)。

Azure CDN 會忽略任何新增至 SAS 權杖的限制。 例如，所有 SAS 權杖都有到期時間，表示除非從 CDN 存在點 (POP) 伺服器清除該內容，否則仍然可以存取具有過期 SAS 的內容。 您可以設定快取回應標頭，以控制可在 Azure CDN 上快取資料多久時間。 如需詳細資訊，請參閱[在 Azure CDN 中管理 Azure 儲存體 Blob 的到期](cdn-manage-expiration-of-blob-content.md)。

如果您為相同的 Blob 端點建立多個 SAS URL，則請考慮使用啟用查詢字串快取。 這麼做確保將每個 URL 都視為唯一實體。 如需詳細資訊，請參閱[使用查詢字串控制 Azure CDN 快取行為](cdn-query-string.md)。

## <a name="http-to-https-redirection"></a>HTTP 到 HTTPS 重新導向
您可以選擇使用[標準規則引擎](cdn-standard-rules-engine.md)或 [Verizon 進階規則引擎](cdn-verizon-premium-rules-engine.md)建立 URL 重新導向規則，以將 HTTP 流量重新導向至 HTTPS。 標準規則引擎僅適用於來自 Microsoft 的 Azure CDN 設定檔，而 Verizon 進階規則引擎僅適用於來自 Verizon 的 Azure CDN 進階設定檔。

![Microsoft 重新導向規則](./media/cdn-storage-custom-domain-https/cdn-standard-redirect-rule.png)

在上述規則中，保留 [主機名稱]、[路徑]、[查詢字串] 和 [片段]，會致使在重新導向時使用傳入的值。 

![Verizon 重新導向規則](./media/cdn-storage-custom-domain-https/cdn-url-redirect-rule.png)

在上述規則中，*Cdn-endpoint-name* 指的是針對您可從下拉式清單中選取為 CDN 端點設定的名稱。 *origin-path* 值指的是您靜態內容所在原始儲存體帳戶的路徑。 如果您將所有靜態內容裝載在單一容器中，則請將 origin-path  取代為該容器的名稱。

## <a name="pricing-and-billing"></a>價格和計費
當您透過 Azure CDN 存取 Blob 時，需支付 POP 伺服器與原點 (Blob 儲存體) 之間流量的 [Blob 儲存體價格](https://azure.microsoft.com/pricing/details/storage/blobs/)，以及從 POP 伺服器存取資料的 [Azure CDN 定價](https://azure.microsoft.com/pricing/details/cdn/)。

例如，如果您在北美洲中具有將使用 Azure CDN 存取的儲存體帳戶，而且歐洲中有人透過 Azure CDN 嘗試存取該儲存體帳戶中的其中一個 Blob，則 Azure CDN 會先檢查該 Blob 最接近歐洲的 POP。 如果找到，則 Azure CDN 會存取該 Blob 複本而且會使用 CDN 定價，因為將在 Azure CDN 上存取。 如果找不到，Azure CDN 會將 Blob 複製至 POP 伺服器，這會產生輸出和交易費用 (如 Blob 儲存體定價中所指定)，然後存取 POP 伺服器上的檔案，而這會產生 Azure CDN 費用。

## <a name="next-steps"></a>後續步驟
[教學課程：設定 Azure CDN 快取規則](cdn-caching-rules-tutorial.md)




