---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 MongoDB Cloud 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 MongoDB Cloud 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ef392044-235b-4d80-8a33-eeba9b142849
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a502a8273aef3df3488764ea7b01c21c512ba43
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85800203"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mongodb-cloud"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 MongoDB Cloud 整合

在本教學課程中，您會了解如何整合 MongoDB Cloud 與 Azure Active Directory (Azure AD)。 在整合 MongoDB Cloud 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 MongoDB Cloud、MongoDB Atlas、MongoDB 社群、MongoDB University 和 MongoDB 支援的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 MongoDB Cloud。
* 在 Azure 入口網站中集中管理您的帳戶。

若要深入了解軟體即服務 (SaaS) 應用程式與 Azure AD 的整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用單一登入 (SSO) 的 MongoDB Cloud 組織，您可以註冊[免費叢集](https://www.mongodb.com/cloud)

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* MongoDB Cloud 支援由 **SP** 和 **IDP** 起始的 SSO。
* MongoDB Cloud 支援 **Just In Time** 使用者佈建。
* 設定 MongoDB Cloud 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 如需詳細資訊，請參閱[了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="add-mongodb-cloud-from-the-gallery"></a>從資源庫新增 MongoDB Cloud

若要設定將 MongoDB Cloud 整合到 Azure AD 中，您需要從資源庫將 MongoDB Cloud 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左窗格上，選取 [Azure Active Directory]  。
1. 移至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增新的應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **MongoDB Cloud**。
1. 從結果中選取 [MongoDB Cloud]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-single-sign-on-for-mongodb-cloud"></a>設定及測試 MongoDB Cloud 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 MongoDB Cloud 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 MongoDB Cloud 中相關使用者之間的連結關聯性。

若要設定及測試與 MongoDB Cloud 搭配運作的 Azure AD SSO，請完成下列建構元素：

1. [設定 Azure AD SSO](#configure-azure-ad-sso)，讓您的使用者能夠使用此功能。
    1. [建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)，以使用 B.Simon 測試 Azure AD 單一登入。
    1. [指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)，讓 B.Simon 能夠使用 Azure AD 單一登入。
1. [設定 MongoDB Cloud SSO](#configure-mongodb-cloud-sso) 以在應用程式端設定單一登入設定。
    1. [建立 MongoDB Cloud 測試使用者](#create-a-mongodb-cloud-test-user)，以使 MongoDB Cloud 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
1. [測試 SSO](#test-sso)，以驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [MongoDB Cloud]  應用程式整合頁面上，尋找 [管理]  區段。 選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，選取 [基本 SAML 設定]  的鉛筆圖示，以編輯設定。

   ![以 SAML 設定單一登入頁面的螢幕擷取畫面，其中的鉛筆圖示已醒目提示](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段中，如果您想要以 **IDP** 起始模式設定應用程式，請輸入下列欄位的值：

    a. 在 [識別碼]  文字方塊中，以下列模式輸入 URL：`https://www.okta.com/saml2/service-provider/<Customer_Unique>`

    b. 在 [回覆 URL]  文字方塊中，以下列模式輸入 URL︰`https://auth.mongodb.com/sso/saml2/<Customer_Unique>`

1. 如果您想要以 **SP** 起始的模式設定應用程式，請選取 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://cloud.mongodb.com/sso/<Customer_Unique>`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的「識別碼」、「回覆 URL」及「登入 URL」來更新這些值。 若要取得這些值，請連絡 [MongoDB Cloud 用戶端支援小組](https://support.mongodb.com/)。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

1. MongoDB Cloud 應用程式需要特定格式的 SAML 判斷提示，因此您必須將自訂屬性對應新增至 SAML 權杖屬性設定。 以下螢幕擷取畫面顯示預設屬性清單。

    ![預設屬性的螢幕擷取畫面](common/default-attributes.png)

1. 除了以上屬性外，MongoDB Cloud 應用程式還需要在 SAML 回應中傳回更多屬性。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。
    
    | 名稱 | 來源屬性|
    | ---------------| --------- |
    | 電子郵件 | user.userprincipalname |
    | firstName | user.givenname |
    | lastName | user.surname |

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，尋找**同盟中繼資料 XML**。 選取 [下載]  以下載憑證，並將其儲存在您的電腦上。

    ![已醒目提示 [下載] 連結的 [SAML 簽署憑證] 區段螢幕擷取畫面](common/metadataxml.png)

1. 在 [設定 MongoDB Cloud]  區段上，依據您的需求複製適當的 URL。

    ![[設定 MongoDB Cloud] 區段的螢幕擷取畫面，其中的 URL 已醒目提示](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，選取 [Azure Active Directory]   > [使用者]   > [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下密碼。
   1. 選取 [建立]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 MongoDB Cloud 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]   > [所有應用程式]  。
1. 在應用程式清單中，選取 [MongoDB Cloud]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![管理區段的螢幕擷取畫面，醒目提示使用者和群組](common/users-groups-blade.png)

1. 選取 [新增使用者]  。 然後，在 [新增指派]  對話方塊中，選取 [使用者和群組]  。

    ![使用者和群組使用者及群組頁面的螢幕擷取畫面，其中已醒目提示 [新增使用者]](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的使用者清單中，選取 [B.Simon]  。 然後，選擇畫面底部的 [選取]  按鈕。
1. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色]  對話方塊的清單中選取適當的使用者角色。 然後，選擇畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，選取 [指派]  。

## <a name="configure-mongodb-cloud-sso"></a>設定 MongoDB Cloud SSO

若要在 MongoDB Cloud 端設定單一登入，您需要從 Azure 入口網站複製的適當 URL。 您也需要為 MongoDB Cloud 組織設定同盟應用程式。 依照 [MongoDB Cloud 文件](https://docs.atlas.mongodb.com/security/federated-auth-azure-ad/)中的指示操作。 如有任何問題，請連絡 [MongoDB Cloud 支援小組](https://support.mongodb.com/)。

### <a name="create-a-mongodb-cloud-test-user"></a>建立 MongoDB Cloud 測試使用者

MongoDB Cloud 支援預設啟用的 Just-In-Time 使用者佈建。 沒有您需要進行的額外動作。 如果 MongoDB Cloud 中還沒有使用者存在，在驗證之後就會建立新的使用者。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入組態。

當您在存取面板中選取 MongoDB Cloud 圖格時，您會自動登入您已設定 SSO 的 MongoDB Cloud。 如需詳細資訊，請參閱[登入我的應用程式入口網站並啟動應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [整合 SaaS 應用程式與 Azure Active Directory 的教學課程](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [在 Azure 上註冊 MongoDB Atlas](https://azuremarketplace.microsoft.com/marketplace/apps/mongodb.mongodb_atlas_may_2020?tab=Overview)

- [嘗試搭配 Azure AD 使用 MongoDB Cloud](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [使用進階可見性和控制項保護 MongoDB Cloud](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

