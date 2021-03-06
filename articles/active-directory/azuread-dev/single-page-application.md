---
title: Azure Active Directory 中的單頁應用程式
description: 說明何謂單頁應用程式 (SPA)，以及此應用程式類型的通訊協定流程、註冊與權杖到期等基本概念。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: adf3c5b5cd40a9ea3f07ba9c92cfc4544ca60f1e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "80154741"
---
# <a name="single-page-applications"></a>單頁應用程式

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

單頁應用程式（Spa）通常是以 JavaScript 展示層（前端）來結構化，它會在瀏覽器中執行，並在伺服器上執行的 Web API 後端，並實行應用程式的商務邏輯。若要深入瞭解隱含授權授與，並協助您決定其是否適合您的應用程式案例，請參閱[瞭解 Azure Active Directory 中的隱含授與流程 OAuth2](v1-oauth2-implicit-grant-flow.md)。

在此案例中，當使用者登入時，JavaScript 前端會使用 [Active Directory Authentication Library for JavaScript (ADAL.JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) 和隱含授權授與，從 Azure AD 取得的識別碼權杖 (id_token)。 權杖會留在快取中，而用戶端呼叫 Web API 後端時會將權杖附加至要求做為持有人權杖，並使用 OWIN 中介軟體來保護。

## <a name="diagram"></a>圖表

![單頁應用程式圖表](./media/authentication-scenarios/single-page-app.png)

## <a name="protocol-flow"></a>通訊協定流程

1. 使用者導覽至 Web 應用程式。
1. 應用程式將 JavaScript 前端 (展示層) 傳回至瀏覽器。
1. 使用者起始登入，例如按一下登入連結。 瀏覽器傳送 GET 給 Azure AD 授權端點來要求識別碼權杖。 此要求在查詢參數中包含應用程式識別碼和回覆 URL。
1. Azure AD 會以 Azure 入口網站中所設的註冊回覆 URL 為依據，驗證回覆 URL。
1. 使用者在登入頁面上登入。
1. 如果驗證成功，Azure AD 會建立一個識別碼權杖，並將它當做 URL 片段（#）傳回給應用程式的回復 URL。 對於實際執行應用程式，此回覆 URL 應該為 HTTPS。 傳回的權杖包含應用程式驗證權杖所需的使用者與 Azure AD 宣告。
1. 在瀏覽器中執行的 JavaScript 用戶端程式代碼會從回應中解壓縮權杖，以用來保護對應用程式的 Web API 後端的呼叫。
1. 瀏覽器會使用 authorization 標頭中的識別碼權杖來呼叫應用程式的 Web API 後端。 Azure AD 驗證服務會發出識別碼權杖，如果資源與用戶端識別碼相同，即可用來作為持有人權杖 (在此情況下，這是因為 Web API 是應用程式本身的後端)。

## <a name="code-samples"></a>程式碼範例

請參閱[單頁應用程式案例的程式碼範例](sample-v1-code.md#single-page-applications)。 請務必經常回來看看，因為我們會隨時新增新的範例。

## <a name="app-registration"></a>應用程式註冊

* 單一租使用者-如果您要為組織建立僅限應用程式，則必須使用 Azure 入口網站在公司的目錄中註冊。
* 多租使用者-如果您所建立的應用程式可供組織外的使用者使用，則必須在您公司的目錄中註冊，但也必須在將使用應用程式的每個組織目錄中註冊。 若要讓您的應用程式出現在他們的目錄中，您可以為客戶加上註冊程序，讓他們同意應用程式。 當他們註冊您的應用程式時，他們會看到對話方塊顯示應用程式所需的權限，以及是否同意的選項。 根據所需的權限，其他組織的系統管理員可能必須同意。 當使用者或系統管理員同意時，應用程式就會註冊在他們的目錄中。

註冊應用程式之後，它必須設定為使用 OAuth 2.0 隱含授權通訊協定。 根據預設，應用程式都停用此通訊協定。 若要為您的應用程式啟用 OAuth2 隱含授與通訊協定，請從 Azure 入口網站編輯其應用程式資訊清單，並將 "oauth2AllowImplicitFlow" 值設為 true。 如需詳細資訊，請參閱[應用程式資訊清單](../develop/reference-app-manifest.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)。

## <a name="token-expiration"></a>權杖到期

使用 ADAL.js 有助於：

* 重新整理過期的權杖
* 要求存取權杖以呼叫 Web API 資源

成功驗證之後，Azure AD 會將 Cookie 寫入使用者的瀏覽器以建立工作階段。 請注意，工作階段存在於使用者與 Azure AD 之間，而不是在使用者與 Web 應用程式之間。 當權杖過期時，ADAL.js 會使用此工作階段來自動取得另一個權杖。 ADAL.js 會使用隱藏的 iFrame 並透過 OAuth 隱含授權通訊協定，來傳送和接收要求。 ADAL.js 也可以使用這個相同的機制，以無訊息方式取得應用程式所呼叫之其他 Web API 資源的存取權杖，只要這些資源支援跨原始來源資源分享（CORS）、已在使用者的目錄中註冊，而且使用者在登入期間提供任何必要的同意。

## <a name="next-steps"></a>後續步驟

* 深入了解其他[應用程式類型和案例](app-types.md)
* 瞭解 Azure AD[驗證基本概念](v1-authentication-scenarios.md)
