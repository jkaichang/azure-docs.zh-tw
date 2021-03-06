---
title: Microsoft Azure 資料箱自我管理寄送 | 資料中的 Microsoft Docs
description: 說明 Azure 資料箱裝置的自我管理寄送工作流程
services: databox
author: priestlg
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 05/20/2020
ms.author: v-grpr
ms.openlocfilehash: 99c7021744467484656c52cf72378c6f553c3913
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84608582"
---
# <a name="use-self-managed-shipping-for-azure-data-box-in-the-azure-portal"></a>在 Azure 入口網站中使用 Azure 資料箱的自我管理寄送

本文說明 Azure 資料箱裝置訂購、取貨和送貨的自我管理寄送工作。 您可以使用 Azure 入口網站管理資料箱裝置。

## <a name="prerequisites"></a>Prerequisites

當您[訂購 Azure 資料箱](data-box-deploy-ordered.md)時，系統會提供自我管理寄送選項。 自我管理寄送僅適用於以下地區：

* 美國政府
* 西歐
* 日本
* 新加坡
* 南韓

## <a name="use-self-managed-shipping"></a>使用自我管理寄送

當您下單訂購資料箱時，您可以選擇自我管理寄送選項。

1. 在您的 Azure 資料箱訂單中，於 [連絡人詳細資料] 下，選取 [+ 新增交貨地址]。
   ![自我管理寄送](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. 選擇出貨類型時，選取 [自我管理寄送] 選項。 只有您的所在區域支援時 (如必要條件中所述)，此選項才可供使用。

3. 在您提供交貨地址之後，您必須驗證該地址並完成您的訂單。
   ![自我管理寄送](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. 備妥裝置並收到電子郵件通知之後，您就可以排程取貨。

   在您的 Azure 資料箱訂單中，移至 [概觀]，然後選取 [排程取貨]。

   ![訂購要領取的資料箱裝置](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-01.png)

5. 依照 [排程到 Azure 取貨] 中的指示進行。

   您必須先傳送電子郵件至 [adbops@microsoft.com](mailto:adbops@microsoft.com) 以安排到您所在的區域資料中心取貨的時間，才能得到授權碼。

   ![排程取貨](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-email-01.png)

6. 排程裝置取貨時間後，您便能夠在 [排程到 Azure 取貨] 窗格中檢視您的裝置授權碼。

   ![檢視您的授權碼](media\data-box-portal-customer-managed-shipping\data-box-portal-auth-01b.png)

   請記下此**授權碼**。 根據安全性需求，在排程挑選時，必須為要取得挑選的人員，出示名稱，才會出現。

   您也必須提供將前往資料中心取貨人員的詳細資料。 您或連絡人必須攜帶政府核准使用附相片的身分證，以供資料中心驗證。

   此外，取貨人員也必須有**授權碼**。 在資料中心取貨時，會驗證授權碼。

7. 從資料中心領取裝置後，您的訂單就會自動變成  [已取貨] 狀態。

    ![已取貨](media\data-box-portal-customer-managed-shipping\data-box-portal-picked-up-boxed-01.png)

8. 領取裝置後，請將資料複製至您所在地點的資料箱。 資料複製完成之後，您就可以準備寄送資料箱。 如需詳細資訊，請移至[準備寄送](data-box-deploy-picked-up.md#prepare-to-ship)。

   **寄送準備**步驟必須完成且沒有發生任何嚴重的錯誤，否則您必須進行必要的修正，然後再次執行此步驟。 成功完成準備寄送的作業後，您可以在裝置的本機使用者介面上檢視送貨用的授權碼。

   > [!NOTE]
   > 請勿透過電子郵件分享授權碼。 在送貨期間，此授權碼只會在資料中心進行驗證。

9. 如果您已收到送貨約會，則訂單在 Azure 入口網站中應該是 [Azure 資料中心準備好收貨] 狀態。 依照 [排程送貨] 下的指示交回裝置。

   ![檢視您的授權碼](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-02b.png)

10. 在驗證您的識別碼和授權碼，並在資料中心卸下裝置後，訂單狀態應為 [已收到]。

    ![完成接收](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-01.png)

11. 在收到裝置後，資料複製將會繼續進行。 當複製完成時，訂單便完成。

## <a name="next-steps"></a>後續步驟

* [開始使用 Azure 資料箱](data-box-quickstart-portal.md)
