---
title: Microsoft 身分識別平台的憑證認證
titleSuffix: Microsoft identity platform
description: 本文討論如何註冊及使用憑證認證來進行應用程式驗證。
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: hirsin
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 47a35f70251622674205a28af9b7cc64132d0530
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690274"
---
# <a name="microsoft-identity-platform-application-authentication-certificate-credentials"></a>Microsoft 身分識別平台的應用程式驗證憑證認證

Microsoft 身分識別平台可讓應用程式使用自己的認證進行驗證；例如，在 [OAuth 2.0 用戶端認證授與流程 v2.0](v2-oauth2-client-creds-grant-flow.md) 和[代理者流程](v2-oauth2-on-behalf-of-flow.md)中。

應用程式可用於驗證的認證形式之一，是以應用程式擁有的憑證簽署的 JSON Web 權杖 (JWT) 判斷提示。

## <a name="assertion-format"></a>判斷提示格式
Microsoft 身分識別平台：若要計算判斷提示，您可使用所選語言中多個 [JSON Web 權杖](https://jwt.ms/)程式庫的其中一個。 權杖所承載的資訊如下︰

### <a name="header"></a>頁首

| 參數 |  備註 |
| --- | --- |
| `alg` | 應該是 **RS256** |
| `typ` | 應該是 **JWT** |
| `x5t` | 應該是 X.509 憑證 SHA-1 憑證指紋 |

### <a name="claims-payload"></a>宣告 (承載)

| 參數 |  備註 |
| --- | --- |
| `aud` | Audience：應該是 **https://login.microsoftonline.com/*tenant_Id*/oauth2/token** |
| `exp` | 到期日：權杖到期的日期。 時間會表示為從 1970 年 1 月 1 日 (1970-01-01T0:0:0Z) UTC 到權杖有效時間到期的秒數。|
| `iss` | 簽發者︰應該是 client_id (用戶端服務的應用程式識別碼) |
| `jti` | GUID：JWT ID |
| `nbf` | 生效時間：無法在此日期之前使用權杖。 時間會表示為從 1970 年 1 月 1 日 (1970-01-01T0:0:0Z) UTC 到權杖發出時間的秒數。 |
| `sub` | 主旨：至於 `iss`，應該是 client_id (用戶端服務的應用程式識別碼) |

### <a name="signature"></a>簽章

簽章是使用 [JSON Web 權杖 RFC7519 規格](https://tools.ietf.org/html/rfc7519) 中所述的憑證計算的

## <a name="example-of-a-decoded-jwt-assertion"></a>已解碼的 JWT 判斷提示範例

```JSON
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="example-of-an-encoded-jwt-assertion"></a>已編碼的 JWT 判斷提示範例

下列字串是已編碼判斷提示的範例。 如果您仔細看，會發現三個以點 (.) 分隔的區段：
* 第一個區段編碼標頭
* 第二個區段編碼承載
* 最後一個區段則是使用前兩個區段的內容中包含的憑證計算出來的簽章

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-microsoft-identity-platform"></a>向 Microsoft 身分識別平台註冊憑證

您可使用以下任一種方法，透過 Azure 入口網站在 Microsoft 身分識別平台中建立憑證認證與用戶端應用程式之間的關聯：

### <a name="uploading-the-certificate-file"></a>上傳憑證檔案

在用戶端應用程式的 Azure 應用程式註冊中：
1. 選取 [憑證和秘密]。
2. 按一下 [上傳憑證]，然後選取要上傳的憑證檔案。
3. 按一下 [新增] 。
  上傳憑證之後，即會顯示指紋、開始日期和到期值。

### <a name="updating-the-application-manifest"></a>更新應用程式資訊清單

擁有憑證之後，您必須計算︰

- `$base64Thumbprint`，這是憑證雜湊的 base64 編碼
- `$base64Value`，它是憑證未經處理資料的 base64 編碼

您也必須提供 GUID 來識別應用程式資訊清單中的金鑰 (`$keyId`)。

在用戶端應用程式的 Azure 應用程式註冊中：
1. 選取 [資訊清單]，以開啟應用程式資訊清單。
2. 使用下列結構描述，將 *keyCredentials* 屬性取代為您新的憑證資訊。

   ```JSON
   "keyCredentials": [
       {
           "customKeyIdentifier": "$base64Thumbprint",
           "keyId": "$keyid",
           "type": "AsymmetricX509Cert",
           "usage": "Verify",
           "value":  "$base64Value"
       }
   ]
   ```
3. 儲存對應用程式資訊清單所做的編輯，然後將資訊清單上傳到 Microsoft 身分識別平台。

   `keyCredentials` 屬性是多重值，因此您可以上傳多個憑證以進行更豐富的金鑰管理。

## <a name="code-sample"></a>程式碼範例

> [!NOTE]
> 您必須使用憑證的雜湊，將 X5T 標頭轉換成 Base 64 字串，才能計算該標頭。 在 C# 中執行此作業的程式碼為 `System.Convert.ToBase64String(cert.GetCertHash());`。

[使用 Microsoft 身分識別平台的 .NET Core 精靈主控台應用程式](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)這個程式碼範例顯示應用程式如何使用自己的認證進行驗證。 此外也說明如何使用 `New-SelfSignedCertificate` PowerShell 命令[建立自我簽署憑證](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#optional-use-the-automation-script)。 您也可以利用[應用程式建立指令碼](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/AppCreationScripts-withCert/AppCreationScripts.md)來建立憑證、計算指紋，和執行其他作業。
