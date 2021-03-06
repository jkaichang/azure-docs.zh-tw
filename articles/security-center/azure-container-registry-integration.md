---
title: Azure 資訊安全中心和 Azure Container Registry
description: 瞭解如何使用 Azure 資訊安全中心掃描您的容器登錄
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2020
ms.author: memildin
ms.openlocfilehash: b66969b26a801e6bd9aacf999c1c1ef9179ef1bd
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87534663"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>資訊安全中心 Azure Container Registry 影像掃描

Azure Container Registry （ACR）是受控的私用 Docker Registry 服務，可在中央登錄中儲存及管理 Azure 部署的容器映射。 它是以開放原始碼的 Docker Registry 2.0 為基礎。

如果您是在 Azure 資訊安全中心的標準層，您可以新增容器登錄套件組合。 這項選擇性功能可讓您更深入瞭解以 Azure Resource Manager 為基礎的登錄中的映射弱點。 啟用或停用訂用帳戶層級的配套，以涵蓋訂用帳戶中的所有登錄。 這項功能是按映射收費，如[定價頁面](security-center-pricing.md)所示。 啟用容器登錄庫組合，可確保資訊安全中心已準備好掃描已推送至登錄的映射。 

## <a name="availability"></a>可用性

- 發行狀態：**公開上市**
- 必要角色：**安全性讀取者**和[Azure Container Registry 讀者角色](https://docs.microsoft.com/azure/container-registry/container-registry-roles)
- 支援的登錄和映射：
    - ✔可從公用網際網路存取並提供 shell 存取的 Linux 託管 ACR 登錄。
    - ✘ Windows 託管的 ACR 登錄。
    - ✘「私用」登錄-資訊安全中心需要可從公用網際網路存取您的登錄。 資訊安全中心目前無法連線到或掃描具有受防火牆限制、服務端點或私人端點（例如 Azure 私人連結）之存取權的登錄。
    - ✘超級極簡映射（例如[Docker 待用](https://hub.docker.com/_/scratch/)映射），或只包含應用程式及其執行時間相依性（不含套件管理員、SHELL 或 OS）的 "Distroless" 映射。
- 雲端： 
    - ✔ 商用雲端
    - ✘美國政府雲端
    - ✘中國政府雲端，其他政府雲端


## <a name="when-are-images-scanned"></a>影像何時會掃描？

當映射推送至您的登錄時，資訊安全中心會自動掃描該映射。 若要觸發映射掃描，請將其推送至您的存放庫。

當掃描完成時（通常是大約2分鐘，但最長可達15分鐘），結果會以資訊安全中心建議的形式提供，如下所示：

[![Azure Container Registry （ACR）主控映射中探索到之弱點的範例 Azure 資訊安全中心建議](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>整合的優點

資訊安全中心可識別您訂用帳戶中以 Azure Resource Manager 為基礎的 ACR 登錄，並順暢地提供：

* **Azure-原生弱點掃描**所有已推送的 Linux 映射。 資訊安全中心使用領先業界的弱點掃描廠商 Qualys 掃描影像。 這個原生解決方案預設會緊密整合。

* 具有已知弱點之 Linux 映射的**安全性建議**。 資訊安全中心提供每個回報的弱點和嚴重性分類的詳細資料。 此外，它也提供指引，說明如何補救每個推送至登錄的映射上找到的特定弱點。

![Azure 資訊安全中心和 Azure Container Registry （ACR）高階總覽](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Azure Container Registry 影像掃描的常見問題

### <a name="how-does-security-center-scan-an-image"></a>資訊安全中心掃描影像的方式為何？
映射會從登錄提取。 然後，它會在隔離的沙箱中執行，並在其中解壓縮已知弱點清單的 Qualys 掃描器。

資訊安全中心篩選並分類掃描器的發現結果。 當影像狀況良好時，資訊安全中心將其標示為。 資訊安全中心只會針對具有要解決之問題的映射產生安全性建議。 藉由只在發生問題時發出通知，資訊安全中心降低不必要資訊警示的可能性。

### <a name="how-often-does-security-center-scan-my-images"></a>資訊安全中心掃描影像的頻率為何？
系統會在每次推送時觸發影像掃描。

### <a name="can-i-get-the-scan-results-via-rest-api"></a>我可以透過 REST API 取得掃描結果嗎？
是。 結果會在[子評量 REST API](/rest/api/securitycenter/subassessments/list/)之下。 此外，您可以使用 Azure Resource Graph （ARG），這是適用于所有資源的類似 Kusto API：查詢可以提取特定的掃描。
 
### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>掃描的登錄類型為何？ 哪些類型會計費？
[[可用性] 區段](#availability)會列出 container registry 配套所支援的容器登錄類型。 

如果不支援的登錄已連線到您的 Azure 訂用帳戶，則不會進行掃描，而且不會向您收取費用。


## <a name="next-steps"></a>後續步驟

若要深入瞭解資訊安全中心的容器安全性功能，請參閱：

* [Azure 資訊安全中心和容器安全性](container-security.md)

* [與 Azure Kubernetes Service 整合](azure-kubernetes-service-integration.md)

* [虛擬機器保護](security-center-virtual-machine-protection.md)-描述資訊安全中心的建議
