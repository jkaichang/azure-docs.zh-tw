---
title: 概觀
description: 了解 Azure App Service 如何協助您開發和裝載 Web 應用程式
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.topic: overview
ms.date: 04/30/2020
ms.custom: mvc, seodec18
ms.openlocfilehash: f4b42f2ed43da68daabb3bd334a362071202be42
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073727"
---
# <a name="app-service-overview"></a>App Service 概觀

*Azure App Service* 是 HTTP 型服務，用來裝載 Web 應用程式、REST API 和行動後端。 您可以使用您慣用的語言進行開發，不管是 .NET、.NET Core、Java、Ruby、Node.js、PHP 還是 Python 都可以。 應用程式在 Windows 和 Linux 環境中都可輕易執行及調整。 若是 Linux 環境，請參閱 [Linux 上的 App Service](containers/app-service-linux-intro.md)。 

App Service 不只能將 Microsoft Azure 的功能新增到您的應用程式，例如安全性、負載平衡、自動調整和自動化管理。 您也可以利用其 DevOps 功能，例如從 Azure DevOps、GitHub、Docker Hub 和其他來源進行持續部署、套件管理、預備環境、自訂網域和 TLS/SSL 憑證。 

在使用 App Service 時，您只需就您所使用的 Azure 計算資源支付費用。 您所使用的計算資源取決於您用來執行應用程式的 _App Service 方案_。 如需詳細資訊，請參閱 [Azure App Service 方案概觀](overview-hosting-plans.md)。

## <a name="why-use-app-service"></a>為何要使用 App Service？

以下是 App Service 的一些主要功能︰

* **多種語言和架構** - App Service 具有 ASP.NET、ASP.NET Core、Java、Ruby、Node.js、PHP 或 Python 的第一級支援。 您也可以將 [PowerShell 和其他指令碼或可執行檔](webjobs-create.md) 做為背景服務來執行。
* **受控的實際執行環境** - App Service 會自動為您[修補及維護 OS 和語言架構](overview-patch-os-runtime.md)。 您可以將時間用在撰寫絕佳的應用程式上，而讓 Azure 來擔心平台問題。
* **DevOps 最佳化** - 使用 Azure DevOps、GitHub、BitBucket、Docker Hub 或 Azure Container Registry 設定[持續整合和部署](deploy-continuous-deployment.md)。 透過 [測試和預備環境](deploy-staging-slots.md)升級更新。 使用 [Azure PowerShell](/powershell/azure/) 或[跨平台命令列介面 (CLI)](/cli/azure/install-azure-cli)，在 App Service 中管理您的應用程式。
* **具高可用性的全域調整** - 以手動或自動方式相應[增加](manage-scale-up.md)或[放大](../monitoring-and-diagnostics/insights-how-to-scale.md)。 在 Microsoft 的通用資料中心基礎結構中隨處裝載您的應用程式，而 App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) 會承諾高可用性。
* **SaaS 平台和內部部署資料的連線** - 有超過 50 種適用於企業系統 (例如 SAP)、SaaS 服務 (例如 Salesforce) 和網際網路服務 (例如 Facebook) 的[連接器](../connectors/apis-list.md)可供選擇。 使用[混合式連線](app-service-hybrid-connections.md)和 [Azure 虛擬網路](web-sites-integrate-with-vnet.md)存取內部部署資料。
* **安全性和法規遵循** - App Service 為 [ISO、SOC 和 PCI 相容](https://www.microsoft.com/en-us/trustcenter)。 可使用 [Azure Active Directory](configure-authentication-provider-aad.md) 或社交登入 ([Google](configure-authentication-provider-google.md)、[Facebook](configure-authentication-provider-facebook.md)、[Twitter](configure-authentication-provider-twitter.md) 和 [Microsoft](configure-authentication-provider-microsoft.md)) 驗證使用者。 建立 [IP 位址限制](app-service-ip-restrictions.md)和[管理服務身分識別](overview-managed-identity.md)。
* **應用程式範本** - 從 [Azure Marketplace](https://azure.microsoft.com/marketplace/) 中的廣泛應用程式範本清單中進行選擇，例如 WordPress、Joomla 和 Drupal。
* **Visual Studio 整合** - Visual Studio 中的專用工具可簡化建立、部署和偵錯的工作。
* **API 和行動功能** - App Service 可提供適用於 RESTful API 案例的現成 CORS 支援，並可藉由啟用驗證、離線資料同步和推播通知等功能，簡化行動應用程式案例。
* **無伺服器程式碼** - 可隨需執行程式碼片段或指令碼，而不必明確佈建或管理基礎結構，而且只須就程式碼實際使用的計算時間支付費用 (請參閱 [Azure Functions](/azure/azure-functions/))。

除了 App Service，Azure 還提供可用來裝載網站和 Web 應用程式的其他服務。 在大部分的情況下，App Service 是最佳選擇。  若是微服務架構，請考慮使用 [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)。 如果您需要能更加充分地掌控程式碼執行所在的 VM，則請考慮使用 [Azure 虛擬機器](https://azure.microsoft.com/documentation/services/virtual-machines/)。 如需如何在這些 Azure 服務之間做選擇的詳細資訊，請參閱 [Azure App Service、虛擬機器、Service Fabric 及雲端服務的比較](overview-compare.md)。

## <a name="next-steps"></a>後續步驟

建立您的第一個 Web 應用程式。

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Ruby (在 Linux 上)](containers/quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python (位於 Linux 上)](containers/quickstart-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)
