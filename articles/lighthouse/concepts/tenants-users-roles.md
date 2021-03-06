---
title: Azure Lighthouse 案例中的租用戶、角色和使用者
description: 了解 Azure Active Directory 租用戶、使用者和角色的概念，以及如何在 Azure Lighthouse 案例中使用它們。
ms.date: 07/03/2020
ms.topic: conceptual
ms.openlocfilehash: 6bcfd1603469ba27971fffa8e7c46f0f696bb6a2
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105382"
---
# <a name="tenants-roles-and-users-in-azure-lighthouse-scenarios"></a>Azure Lighthouse 案例中的租用戶、角色和使用者

將[Azure 燈塔](../overview.md)的客戶上線之前，請務必瞭解 Azure Active Directory （Azure AD）租使用者、使用者和角色如何運作，以及如何在 azure 燈塔案例中使用它們。

*租用戶*是專用且受信任的 Azure AD 執行個體。 每個租用戶通常都代表一個組織。 [Azure 委派的資源管理](azure-delegated-resource-management.md)可讓您將資源從一個租使用者邏輯投影至另一個租使用者。 如此可讓管理租用戶中的使用者 (例如，屬於服務提供者的使用者) 存取客戶租用戶中的委派資源，或讓[具有多個租用戶的企業集中管理營運](enterprise.md)。

為完成這個邏輯投影，客戶租用戶中的訂用帳戶 (或訂用帳戶內的一或多個資源群組) 必須是*上線*，才能進行 Azure 委派的資源管理。 此上線程序可以[透過 Azure Resource Manager 範本](../how-to/onboard-customer.md)，或藉由[將公用或私用供應項目發佈至 Azure Marketplace](../how-to/publish-managed-services-offers.md) 來完成。

無論您選擇哪一種上線方法，都必須定義*授權*。 每個授權都會在管理租用戶中指定一個可存取委派資源的使用者帳戶，以及一個可設定每個使用者對這些資源所擁有之權限的內建角色。

## <a name="role-support-for-azure-lighthouse"></a>Azure 燈塔的角色支援

定義授權時，每個使用者帳戶都必須獲指派其中一個[角色型存取控制 (RBAC) 內建角色](../../role-based-access-control/built-in-roles.md)。 此外，不支援自訂角色與[傳統訂用帳戶管理員角色](../../role-based-access-control/classic-administrators.md) \(部分機器翻譯\)。

Azure 燈塔目前支援所有[內建角色](../../role-based-access-control/built-in-roles.md)，但有下列例外狀況：

- 不支援[擁有者](../../role-based-access-control/built-in-roles.md#owner)角色。
- 不支援具有 [DataActions](../../role-based-access-control/role-definitions.md#dataactions) 權限的任何內建角色。
- 支援[使用者存取系統管理員](../../role-based-access-control/built-in-roles.md#user-access-administrator)內建角色，但僅限於[將角色指派給客戶租用戶中的受控識別](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)。 將不適用此角色通常授與的其他任何權限。 如果您定義具有此角色的使用者，您也必須指定此使用者可以指派給受控識別的內建角色。

> [!NOTE]
> 將新的適用內建角色新增至 Azure 之後，您就可以在[使用 Azure Resource Manager 範本將客戶上架](../how-to/onboard-customer.md)時指派。 在[發佈受管理的服務供應](../how-to/publish-managed-services-offers.md)專案的 Cloud Partner 入口網站中，可能會有一段延遲時間，新加入的角色才會變成可用。

## <a name="best-practices-for-defining-users-and-roles"></a>定義使用者和角色的最佳做法

建立授權時，建議您遵循下列最佳做法：

- 在多數情況下，建議您指派權限給 Azure AD 使用者群組或服務主體，而不是指派給一系列個別使用者帳戶。 如此一來，當您的存取需求變更時，就可以新增或移除個別使用者的存取權，而不需要更新並重新發佈方案。
- 請務必遵循最少特殊權限準則，讓使用者只具備完成其工作所需的權限，以協助減少發生意外錯誤的機會。 如需詳細資訊，請參閱[建議的安全性作法](../concepts/recommended-security-practices.md)。
- 包含具有[受控服務註冊指派刪除角色](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)的使用者，讓您可以在稍後，視需要[移除對委派的存取權](../how-to/remove-delegation.md)。 如果未指派此角色，則只有客戶租用戶中的使用者可以移除委派的資源。
- 請確定需要[在 Azure 入口網站檢視 [我的客戶] 頁面](../how-to/view-manage-customers.md)的任何使用者都具有[讀取者](../../role-based-access-control/built-in-roles.md#reader)角色 (或包含讀取者存取權的其他內建角色)。

> [!IMPORTANT]
> 若要新增 Azure AD 群組的許可權，[群組類型] 必須是 [安全性]，而不是 [Office 365]。 建立群組時，會選取此選項。 如需詳細資訊，請參閱[使用 Azure Active Directory 建立基本群組並新增成員](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)。

## <a name="next-steps"></a>後續步驟

- 瞭解[Azure 燈塔的建議安全性做法](recommended-security-practices.md)。
- 藉由[使用 Azure Resource Manager 範本](../how-to/onboard-customer.md)或將[私人或公用受控服務供應專案發佈至 Azure Marketplace](../how-to/publish-managed-services-offers.md)，將您的客戶上線至 Azure 燈塔。
