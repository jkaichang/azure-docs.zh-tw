---
title: 在自訂原則中定義電話要素技術設定檔
titleSuffix: Azure AD B2C
description: 在 Azure Active Directory B2C 的自訂原則中定義電話要素技術設定檔。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/31/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10d5fda526c41704381bb544bdfd0589063c1d15
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85203855"
---
# <a name="define-a-phone-factor-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>在 Azure Active Directory B2C 自訂原則中定義電話要素技術設定檔

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C （Azure AD B2C）提供註冊和驗證電話號碼的支援。 此技術設定檔：

- 提供使用者介面來與使用者互動，以驗證或註冊電話號碼。
- 支援撥打電話和文字訊息來驗證電話號碼。
- 支援多個電話號碼。 使用者可以選取其中一個電話號碼進行驗證。  
- 傳回宣告，指出使用者是否提供新的電話號碼。 您可以使用此宣告來決定是否要將電話號碼保存到 Azure AD B2C 的使用者設定檔。  
- 會使用[內容定義](contentdefinitions.md)來控制外觀與風格。

## <a name="protocol"></a>通訊協定

**Protocol** 元素的 **Name** 屬性必須設定為 `Proprietary`。 **Handler**屬性必須包含通訊協定處理常式元件的完整名稱，而 Azure AD B2C 用於電話因素：`Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

下列範例顯示註冊和驗證的電話要素技術設定檔：

```xml
<TechnicalProfile Id="PhoneFactor-InputOrVerify">
  <DisplayName>PhoneFactor</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims-transformations"></a>輸入宣告轉換

InputClaimsTransformations 元素可能包含輸入宣告轉換的集合，可用來修改輸入宣告或產生新的宣告。 下列輸入宣告轉換 `UserId` 會產生稍後在輸入宣告集合中使用的宣告。

```xml
<InputClaimsTransformations>
  <InputClaimsTransformation ReferenceId="CreateUserIdForMFA" />
</InputClaimsTransformations>
```

## <a name="input-claims"></a>輸入宣告

InputClaims 元素必須包含下列宣告。 您也可以將宣告的名稱對應至電話要素技術設定檔中定義的名稱。 

|  資料類型| 必要 | Description |
| --------- | -------- | ----------- | 
| 字串| Yes | 使用者的唯一識別碼。 宣告名稱或 PartnerClaimType 必須設定為 `UserId` 。 此宣告不應包含個人識別資訊。|
| 字串| Yes | 宣告類型清單。 每個宣告都包含一個電話號碼。 如果任何輸入宣告未包含電話號碼，系統會要求使用者註冊並驗證新的電話號碼。 已驗證的電話號碼會以輸出宣告的形式傳回。 如果其中一個輸入宣告包含電話號碼，系統會要求使用者進行驗證。 如果有多個輸入宣告包含電話號碼，系統會要求使用者選擇並確認其中一個電話號碼。 |

下列範例示範如何使用多個電話號碼。 如需詳細資訊，請參閱[範例原則](https://github.com/azure-ad-b2c/samples/tree/master/policies/mfa-add-secondarymfa)。

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
  <InputClaim ClaimTypeReferenceId="secondaryStrongAuthenticationPhoneNumber" />
</InputClaims>
```

## <a name="output-claims"></a>輸出宣告

OutputClaims 元素包含電話要素技術設定檔傳回的宣告清單。

|  資料類型| 必要 | 描述 |
|  -------- | ----------- |----------- |
| boolean | Yes | 指出使用者是否已輸入新的電話號碼。 宣告名稱或 PartnerClaimType 必須設定為`newPhoneNumberEntered`|
| 字串| Yes | 已驗證的電話號碼。 宣告名稱或 PartnerClaimType 必須設定為 `Verified.OfficePhone` 。|

OutputClaimsTransformations 元素可能包含 OutputClaimsTransformation 元素的集合，這些專案是用來修改輸出宣告，或產生新的宣告。

## <a name="cryptographic-keys"></a>密碼編譯金鑰

不使用 **CryptographicKeys** 元素。


## <a name="metadata"></a>中繼資料

| 屬性 | 必要 | 說明 |
| --------- | -------- | ----------- |
| ContentDefinitionReferenceId | Yes | 與此技術設定檔相關聯的[內容定義](contentdefinitions.md)識別碼。 |
| ManualPhoneNumberEntryAllowed| No | 指定是否允許使用者手動輸入電話號碼。 可能的值為：`true` 或 `false` (預設)。|
| 設定. authenticationMode | No | 驗證電話號碼的方法。 可能的值： `sms` 、 `phone` 或 `mixed` （預設）。|
| 設定。自動撥號| No| 指定技術設定檔是否應該自動撥號或自動傳送 SMS。 可能的值為：`true` 或 `false` (預設)。 自動撥號需要將 `setting.authenticationMode` 中繼資料設定為 `sms` 、或 `phone` 。 輸入宣告集合必須有單一電話號碼。 |

### <a name="ui-elements"></a>UI 元素

可以[當地語系化](localization-string-ids.md#azure-mfa-error-messages)[電話要素驗證] 頁面使用者介面元素。

## <a name="next-steps"></a>後續步驟

- 使用 MFA 入門套件來檢查[社交和本機帳戶](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccountsWithMfa)。
