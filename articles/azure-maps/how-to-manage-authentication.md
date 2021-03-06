---
title: 管理驗證
titleSuffix: Azure Maps
description: 使用 Azure 入口網站來管理 Microsoft Azure 對應中的驗證。
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 870ecb8bda9f07c9270724002d381a4f58bc4d13
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87531722"
---
# <a name="manage-authentication-in-azure-maps"></a>管理 Azure 地圖服務中的驗證

建立 Azure 地圖服務帳戶之後，會建立用戶端識別碼和金鑰，以支援 Azure Active Directory （Azure AD）驗證和共用金鑰驗證。

## <a name="view-authentication-details"></a>檢視驗證詳細資料

建立 Azure 地圖服務帳戶之後，就會產生主要和次要金鑰。 當您[使用共用金鑰驗證來呼叫 Azure 地圖服務](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication)時，建議您使用主要金鑰做為訂用帳戶金鑰。 您可以在像是輪流金鑰變更的情況下使用次要金鑰。 如需詳細資訊，請參閱[Azure 地圖服務中的驗證](https://aka.ms/amauth)。

您可以在 Azure 入口網站中查看驗證詳細資料。 在您的帳戶中，選取 [**設定**] 功能表上的 [**驗證**]。

> [!div class="mx-imgBorder"]
> ![驗證詳細資料](./media/how-to-manage-authentication/how-to-view-auth.png)

## <a name="discover-category-and-scenario"></a>探索類別和案例

視應用程式的需求而定，有特定的路徑來保護應用程式。 Azure AD 會定義類別，以支援各種不同的驗證流程。 請參閱[應用程式類別](https://docs.microsoft.com/azure/active-directory/develop/authentication-flows-app-scenarios#application-categories)，以瞭解應用程式適合的類別。

> [!NOTE]
> 即使您使用共用金鑰驗證，瞭解類別和案例也可協助您保護應用程式的安全。

## <a name="determine-authentication-and-authorization"></a>判斷驗證和授權

下表概述 Azure 地圖服務中的常見驗證和授權案例。 下表提供每個案例所提供的保護類型比較。

> [!IMPORTANT]
> Microsoft 建議使用生產應用程式的角色型存取控制（RBAC），來執行 Azure Active Directory （Azure AD）。

| 狀況                                                                                    | 驗證 | 授權 | 開發成果 | 營運工作 |
| ------------------------------------------------------------------------------------------- | -------------- | ------------- | ------------------ | ------------------ |
| [受信任的 daemon/非互動式用戶端應用程式](./how-to-secure-daemon-app.md)        | 共用金鑰     | N/A           | 中             | 高               |
| [受信任的 daemon/非互動式用戶端應用程式](./how-to-secure-daemon-app.md)        | Azure AD       | 高          | 低度                | 中             |
| [具有互動式單一登入的 Web 單一頁面應用程式](./how-to-secure-spa-users.md) | Azure AD       | 高          | 中             | 適中             |
| [具有非互動式登入的 Web 單一頁面應用程式](./how-to-secure-spa-app.md)      | Azure AD       | 高          | 中             | 適中             |
| [具有互動式單一登入的 Web 應用程式](./how-to-secure-webapp-users.md)          | Azure AD       | 高          | 高               | 中             |
| [IoT 裝置/輸入限制裝置](./how-to-secure-device-code.md)                     | Azure AD       | 高          | 中             | 適中             |

資料表中的連結會帶您前往每個案例的詳細設定資訊。

## <a name="view-role-definitions"></a>查看角色定義

若要查看可用於 Azure 地圖服務的 Azure 角色，請移至 **[存取控制（IAM）**]。 選取 [**角色**]，然後搜尋以*Azure 地圖服務*開頭的角色。 這些 Azure 地圖服務角色是您可以授與存取權的角色。

> [!div class="mx-imgBorder"]
> ![檢視可用的角色](./media/how-to-manage-authentication/how-to-view-avail-roles.png)

## <a name="view-role-assignments"></a>檢視角色指派

若要查看已授與 Azure 地圖服務之 RBAC 的使用者和應用程式，請移至**存取控制（IAM）**。 在該處選取 [**角色指派**]，然後依**Azure 地圖服務**篩選。

> [!div class="mx-imgBorder"]
> ![查看已授與 RBAC 的使用者和應用程式](./media/how-to-manage-authentication/how-to-view-amrbac.png)

## <a name="request-tokens-for-azure-maps"></a>要求 Azure 地圖服務的權杖

從 Azure AD token 端點要求權杖。 在您的 Azure AD 要求中，請使用下列詳細資料：

| Azure 環境      | Azure AD token 端點             | Azure 資源識別碼              |
| ---------------------- | ----------------------------------- | ------------------------------ |
| Azure 公用雲端     | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Azure Government 雲端 | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` |

如需從 Azure AD 針對使用者和服務主體要求存取權杖的詳細資訊，請參閱[Azure AD 的驗證案例](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)和在[案例](./how-to-manage-authentication.md#determine-authentication-and-authorization)的表格中查看特定案例。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱[Azure AD 和 Azure 地圖服務 WEB SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control)。

尋找 Azure 地圖服務帳戶的 API 使用計量：
> [!div class="nextstepaction"]
> [檢視使用計量](how-to-view-api-usage.md)

探索示範如何將 Azure AD 與 Azure 地圖服務整合的範例：

> [!div class="nextstepaction"]
> [Azure AD 驗證範例](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
