---
title: Azure Active Directory 報告 API 的必要條件 |Microsoft Docs
description: 了解存取 Azure AD 報告 API 的必要條件
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: c4702b12bea2e078af4d10fe9d709d16c361b3f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608257"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>存取 Azure Active Directory 報告 API 的必要條件

[Azure Active Directory (Azure AD) 報告 API](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-reporting-api) 透過一組以 REST 為基礎的 API 為您提供資料的程式設計方式存取。 您可以從程式設計語言和工具呼叫這些 Api。

報告 API 會使用 [OAuth](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) 授權存取 Web API。

若要準備存取報告 API，您必須︰

1. [指派角色](#assign-roles)
2. [授權需求](#license-requirements)
3. [註冊應用程式](#register-an-application)
4. [授與權限](#grant-permissions)
5. [收集組態設定](#gather-configuration-settings)

## <a name="assign-roles"></a>指派角色

若要透過 API 來存取報告資料，您必須已具備下列其中一個指派的角色︰

- 安全性讀取者

- 安全性系統管理員

- 全域管理員

## <a name="license-requirements"></a>授權需求

若要存取租使用者的登入報告，Azure AD 租使用者必須有相關聯的 Azure AD Premium 授權。 需要 Azure AD Premium P1 （或更新版本）授權，才能存取任何 Azure AD 租使用者的登入報告。 或者，如果目錄類型是 Azure AD B2C，則可以透過 API 存取登入報告，而不需要任何額外的授權需求。 


## <a name="register-an-application"></a>註冊應用程式

即使您是使用腳本來存取報告 API，也需要註冊。 註冊會提供**應用程式識別碼**，此為授權呼叫所需，並可讓您的程式碼接收權杖。

若要設定您的目錄以存取 Azure AD 報告 API，您必須以 Azure 管理員帳戶登入 [Azure 入口網站](https://portal.azure.com)，而且該帳戶同時也是 Azure AD 租用戶**全域管理員**目錄角色的成員。

> [!IMPORTANT]
> 在具有管理員權限的認證之下執行的應用程式會非常強大，請務必將應用程式的識別碼和祕密認證保存在安全的位置。
> 

**若要註冊 Azure AD 應用程式：**

1. 在 [Azure 入口網站](https://portal.azure.com) 的左導覽窗格中，選取 [Azure Active Directory]****。
   
    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. 在 [Azure Active Directory]**** 頁面中，選取 [應用程式註冊]****。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. 從 [**應用程式註冊**] 頁面上，選取 [**新增註冊**]。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. [**註冊應用程式**] 頁面：

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. 在 [名稱]**** 文字方塊中，輸入 `Reporting API application`。

    b. 針對 [**支援的帳戶類型**]，選取 [**僅限此組織中的帳戶**]。

    c. 在 [重新**導向 URL** ] [選取**Web** ] 文字方塊中，輸入 `https://localhost` 。

    d. 選取 [註冊]。 


## <a name="grant-permissions"></a>授與權限 

根據您想要存取的 API，您需要授與應用程式下列權限：  

| API | 權限 |
| --- | --- |
| Windows Azure Active Directory | 讀取目錄資料 |
| Microsoft Graph | 讀取所有稽核記錄資料 |


![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/36.png)

下節列出適用於這兩個 API 的步驟。 如果您不想存取其中一個 API，可以跳過相關步驟。

**若要對應用程式授與 API 使用權限：**


1. 選取 [ **API 許可權**]，然後**新增許可權**。 

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. 在 [**要求 API 許可權] 頁面**上，找出 [**支援舊版 API** **Azure Active Directory 圖形]**。 

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. 在 [**必要許可權**] 頁面上，依序選取 [**應用程式許可權**] 和 [**目錄**] 核取方塊**ReadAll**。  選取 [新增權限]。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. 在 [**報告 API 應用程式-Api 許可權**] 頁面上，選取 **[授與系統管理員同意**]。 

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. 注意：在 API 註冊期間，預設會新增**Microsoft Graph** 。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/15.png)

## <a name="gather-configuration-settings"></a>收集組態設定 

本節說明如何從您的目錄取得下列設定︰

- 網域名稱
- 用戶端識別碼
- 用戶端密碼

您在設定報告 API 的呼叫時需要這些值。 

### <a name="get-your-domain-name"></a>取得您的網域名稱

**若要取得網域名稱：**

1. 在 [Azure 入口網站](https://portal.azure.com) 的左導覽窗格上，選取 [Azure Active Directory]****。
   
    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. 在 [Azure Active Directory]**** 頁面中，選取 [自訂網域名稱]****。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. 從網域清單中複製網域名稱。


### <a name="get-your-applications-client-id"></a>取得應用程式的用戶端識別碼

**若要取得應用程式的用戶端識別碼：**

1. 在 [Azure 入口網站](https://portal.azure.com)的左方瀏覽窗格中，按一下 [Azure Active Directory]****。
   
    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. 從 [應用程式註冊]**** 頁面中選取您的應用程式。

3. 從應用程式頁面，瀏覽至 [應用程式識別碼]**** 並選取 [按一下以複製]****。

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>取得應用程式的用戶端祕密
 避免嘗試使用 API 來存取 audit 記錄或登入時發生錯誤。

**若要取得應用程式的用戶端祕密：**

1. 在 [Azure 入口網站](https://portal.azure.com)的左方瀏覽窗格中，按一下 [Azure Active Directory]****。
   
    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  從 [應用程式註冊]**** 頁面中選取您的應用程式。

3.  在 [ **API 應用程式**] 頁面上選取 [**憑證和秘密**]，在 [**用戶端密碼**] 區段中按一下 [ **+ 新增用戶端密碼**]。 

    ![註冊應用程式](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. 在 [**新增用戶端密碼**] 頁面上，新增：

    a. 在 [描述]**** 文字方塊中，輸入 `Reporting API`。

    b. 選取 [2 年後]**** 來作為 [到期]****。

    c. 按一下 [檔案] 。

    d. 複製金鑰值。

## <a name="troubleshoot-errors-in-the-reporting-api"></a>針對報告 API 中的錯誤進行疑難排解

本節列出當您使用 Microsoft Graph API 存取活動報告時，可能會遇到的常見錯誤訊息，以及其解決方式的步驟。

### <a name="error-failed-to-get-user-roles-from-microsoft-graph"></a>錯誤：無法從 Microsoft Graph 取得使用者角色

 使用 Graph Explorer UI 中的兩個登入按鈕來登入您的帳戶，以避免在嘗試使用 Graph Explorer 登入時收到錯誤。 

![Graph 總管](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-microsoft-graph"></a>錯誤：無法從 Microsoft Graph 進行 premium 授權檢查 

如果您在嘗試使用 Graph 總管存取登入時，遇到這個錯誤訊息，請在您帳戶左側導覽列底下選擇 [修改權限]****，然後選取 [Tasks.ReadWrite]**** 和 [Directory.Read.All]****。 

![修改權限 UI](./media/troubleshoot-graph-api/modify-permissions.png)

### <a name="error-tenant-is-not-b2c-or-tenant-doesnt-have-premium-license"></a>錯誤：租使用者不是 B2C，或租使用者沒有 premium 授權

存取登入報表需要 Azure Active Directory 進階 1 (P1) 授權。 如果您在存取登入時看到這個錯誤訊息，請確定您的租用戶已獲得 Azure AD P1 授權。

### <a name="error-the-allowed-roles-does-not-include-user"></a>錯誤：允許的角色不包含使用者。 

 避免嘗試使用 API 來存取 audit 記錄或登入時發生錯誤。 請確定您的帳戶屬於 Azure Active Directory 租使用者中的**安全性讀取**者或**報告讀取**者角色。

### <a name="error-application-missing-aad-read-directory-data-permission"></a>錯誤：應用程式遺漏 AAD「讀取目錄資料」權限 

### <a name="error-application-missing-microsoft-graph-api-read-all-audit-log-data-permission"></a>錯誤：應用程式遺失 Microsoft Graph API 「讀取所有的 audit 記錄資料」許可權

依照必要條件中的步驟[存取 Azure Active Directory 報告 API](howto-configure-prerequisites-for-reporting-api.md) ，以確保您的應用程式是以正確的許可權集執行。 

## <a name="next-steps"></a>後續步驟

* [使用 Azure Active Directory 報告 API 搭配憑證來取得資料](tutorial-access-api-with-certificates.md)
* [稽核 API 參考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [登入活動報告 API 參考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
