---
title: Azure Cache for Redis 常見問題集
description: 了解 Azure Cache for Redis 常見問題、模式及最佳做法的解答
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 89a87e1658f413b0a8cd757525450de30277d943
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87086875"
---
# <a name="azure-cache-for-redis-faq"></a>Azure Cache for Redis 常見問題集
了解 Azure Redis 快取常見問題、模式及最佳做法的解答。

## <a name="what-if-my-question-isnt-answered-here"></a>如果這裡沒有解答我的問題該怎麼辦？
如果這裡未列出您的問題，請告訴我們，我們將協助您找到答案。

* 若要觸及更多讀者，您可以將問題張貼在 [Microsoft 問與答的 Azure 快取問題頁面](https://docs.microsoft.com/answers/topics/azure-cache-redis.html)，與 Azure 快取小組和社群的其他成員交流。
* 如果您想要提出功能要求，您可以向 [Azure Redis 快取 User Voice](https://feedback.azure.com/forums/169382-cache) 提交您的要求和想法。
* 您也可以傳送電子郵件到下列信箱： [Azure 快取外部意見反應](mailto:azurecache@microsoft.com)。

## <a name="azure-cache-for-redis-basics"></a>Azure Redis 快取基本知識
本節中的常見問題集涵蓋「Azure Redis 快取」的一些基本知識。

* [什麼是 Azure Redis 快取？](#what-is-azure-cache-for-redis)
* [如何開始使用 Azure Redis 快取？](#how-can-i-get-started-with-azure-cache-for-redis)

下列常見問題集涵蓋「Azure Redis 快取」的相關基本概念和問題，而解答則在其中一個其他常見問題集小節中。

* [應該使用哪個 Azure Redis 快取供應項目和大小？](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [我可以使用哪些 Azure Redis 快取用戶端？](#what-azure-cache-for-redis-clients-can-i-use)
* [Azure Redis 快取是否有本機模擬器？](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [如何監視快取的健全狀況和效能？](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>規劃常見問題集
* [應該使用哪個 Azure Redis 快取供應項目和大小？](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Azure Redis 快取效能](#azure-cache-for-redis-performance)
* [我應該在哪個區域找到快取？](#in-what-region-should-i-locate-my-cache)
* [我的快取資料位於何處？](#where-do-my-cached-data-reside)
* [Azure Redis 快取如何收費？](#how-am-i-billed-for-azure-cache-for-redis)
* [我可以將 Azure Cache for Redis 與 Azure Government 雲端、Azure 中國世紀雲端或 Microsoft Azure 德國搭配使用嗎？](#can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-21vianet-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>開發常見問題集
* [StackExchange.Redis 設定選項的作用為何？](#what-do-the-stackexchangeredis-configuration-options-do)
* [我可以使用哪些 Azure Redis 快取用戶端？](#what-azure-cache-for-redis-clients-can-i-use)
* [Azure Redis 快取是否有本機模擬器？](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [如何執行 Redis 命令？](#how-can-i-run-redis-commands)
* [Azure Redis 快取為什麼沒有像一些其他 Azure 服務有 MSDN 類別庫參考？](#why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [是否可以使用 Azure Redis 快取作為 PHP 工作階段快取？](#can-i-use-azure-cache-for-redis-as-a-php-session-cache)
* [什麼是 Redis 資料庫？](#what-are-redis-databases)

## <a name="security-faqs"></a>安全性常見問題集
* [何時應該啟用非 TLS/SSL 連接埠來連線至 Redis？](#when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>生產環境常見問題集
* [生產環境的最佳作法有哪些？](#what-are-some-production-best-practices)
* [使用常見 Redis 命令時的一些考量為何？](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [如何效能評定和測試我快取的效能？](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [執行緒集區成長的重要詳細資料](#important-details-about-threadpool-growth)
* [使用 StackExchange.Redis 時啟用伺服器 GC 在用戶端上取得更多輸送量](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [連線相關的效能考量](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>監視與疑難排解常見問題集
本節的常見問題集涵蓋常見的監視和疑難排解問題。 如需有關監視「Azure Cache for Redis」執行個體和進行疑難排解的詳細資訊，請參閱[如何監視 Azure Cache for Redis](cache-how-to-monitor.md) 和各種疑難排解指南。

* [如何監視快取的健全狀況和效能？](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [為什麼看到逾時？](#why-am-i-seeing-timeouts)
* [我的用戶端為什麼中斷與快取的連線？](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>先前的快取供應項目常見問題集
* [哪一個 Azure 快取供應專案適合我？](#which-azure-cache-offerings-is-right-for-me)

### <a name="what-is-azure-cache-for-redis"></a>什麼是 Azure Redis 快取？
[Azure Cache For Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-overview)是以熱門的開放原始碼軟體[Redis](https://redis.io/)為基礎。 它可讓您從 Azure 內的任何應用程式，存取由 Microsoft 管理的安全、專用「Azure Redis 快取」。 如需更詳細的總覽，請參閱[Azure Cache For Redis](https://azure.microsoft.com/services/cache/)產品頁面。

### <a name="how-can-i-get-started-with-azure-cache-for-redis"></a>如何開始使用 Azure Redis 快取？
有數種方式可讓您開始使用「Azure Redis 快取」。

* 您可以查看我們針對 [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)、[ASP.NET](cache-web-app-howto.md)、[Java](cache-java-get-started.md)、[Node.js](cache-nodejs-get-started.md) 和 [Python](cache-python-get-started.md) 提供的其中一套教學課程。
* 您可以觀看[如何使用 Microsoft Azure Redis 快取來建立高效能應用程式](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/) \(英文\)。
* 您可以取出符合您專案開發語言用戶端的用戶端文件，來查看如何使用 Redis。 有許多可與「Azure Redis 快取」搭配使用的 Redis 用戶端。 如需 Redis 用戶端清單，請參閱 [https://redis.io/clients](https://redis.io/clients)。

如果您還沒有 Azure 帳戶，您可以：

* [免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero)。 您將獲得能用來試用 Azure 付費服務的額度。 即使在額度用完後，您仍可保留帳戶並使用免費的 Azure 服務和功能。
* [啟用 Visual Studio 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero)。 您的 MSDN 訂用帳戶每月會提供您額度，您可以用在 Azure 付費服務。

<a name="cache-size"></a>

### <a name="what-azure-cache-for-redis-offering-and-size-should-i-use"></a>應該使用哪個 Azure Redis 快取供應項目和大小？
每個 Azure Redis 快取供應項目都提供不同層級的**大小**、**頻寬**、**高可用性**及 **SLA** 選項。

下列是選擇快取供應項目的考量。

* **記憶體**：「基本」層和「標準」層提供 250 MB – 53 GB。 進階層最多可提供 1.2 TB (作為叢集) 或 120 GB (非叢集)。 如需詳細資訊，請參閱 [Azure Redis 快取價格](https://azure.microsoft.com/pricing/details/cache/)。
* **網路效能**：如果您的工作負載需要高輸送量，與「標準」層或「基本」層相比，「進階」層可提供更大的頻寬。 此外，因為每一層內有裝載快取的基礎 VM，較大型快取還有更大頻寬。 如需詳細資訊，請參閱[下列表格](#cache-performance)。
* **輸送量**：「進階」層提供最大的可用輸送量。 如果快取伺服器或用戶端達到頻寬限制，您在用戶端可能會收到逾時。 如需詳細資訊，請參閱下列表格。
* **高可用性/SLA**：「Azure Redis 快取」保證「標準」/「進階」快取的可用性時間不低於 99.9%。 若要深入了解我們的 SLA，請參閱 [Azure Redis 快取價格](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)。 SLA 的範圍僅涵蓋與快取端點的連線。 SLA 未涵蓋資料遺失防護。 建議您使用高階層中的 Redis 資料永續性功能，以增加資料遺失時的復原能力。
* **Redis 資料永續性**：高階層可讓您將快取資料保存在 Azure 儲存體帳戶中。 在基本/標準快取中，所有資料都只儲存在記憶體中。 基礎結構發生問題，可能會導致資料遺失。 建議您使用高階層中的 Redis 資料永續性功能，以增加資料遺失時的復原能力。 Azure Cache for Redis 在 Redis 持續性中提供 RDB 和 AOF （預覽）選項。 如需詳細資訊，請參閱[如何設定進階 Azure Redis 快取的持續性](cache-how-to-premium-persistence.md)。
* **Redis 叢集**：若要建立大於 120 GB 的快取，或要跨多個 Redis 節點將資料分區，您可以使用「進階」層所提供的 Redis 叢集功能。 每個節點均包含一個主要/複本快取組以提供高可用性。 如需詳細資訊，請參閱 [如何設定進階 Azure Redis 快取的叢集功能](cache-how-to-premium-clustering.md)。
* **增強的安全性與網路隔離**：「Azure 虛擬網路」(VNET) 部署除了為「Azure Redis 快取」提供子網路、存取控制原則及其他可進一步限制存取的功能之外，也提供增強的安全性與隔離環境。 如需詳細資訊，請參閱[如何設定進階 Azure Cache for Redis 的虛擬網路支援](cache-how-to-premium-vnet.md)。
* **設定 Redis**：不論是在「標準」層還是「進階」層中，您都可以設定 Redis 以接收 Keyspace 通知。
* **用戶端連線數上限**：「進階」層提供可連線至 Redis 的最大用戶端數目，針對較大型的快取可提供較高的連線數。 叢集化不會增加叢集快取的可用連線數目。 如需詳細資訊，請參閱 [Azure Cache for Redis 價格](https://azure.microsoft.com/pricing/details/cache/)。
* **Redis 伺服器的專用核心**：在「進階」層中，所有快取大小都有 Redis 專用核心。 在基本/標準層中，C1 以上的大小有 Redis 伺服器專用核心。
* **Redis 為單一執行緒** ，因此兩個以上核心所提供的優點與只有兩個核心相同，但較大的 VM 大小通常會比較小的有更多頻寬。 如果快取伺服器或用戶端達到頻寬限制，您在用戶端會收到逾時。
* **效能改進**：「進階」層中的快取是部署在處理器較快的硬體上，因此效能優於「基本」層或「標準」層。 高階層快取的輸送量較高，延遲性較低。

<a name="cache-performance"></a>

### <a name="azure-cache-for-redis-performance"></a>Azure Redis 快取效能
下表顯示從 IaaS VM 使用 `redis-benchmark.exe` 對「Azure Redis 快取」端點測試各種大小的「標準」和「進階」快取時，觀察到的最大頻寬值。 針對 TLS 輸送量，Redis 基準可與 stunnel 搭配使用來連線到「 Azure Cache for Redis」端點。

>[!NOTE] 
>這些值並非保證值，也沒有關於這些數字的 SLA，只代表一般情況。 您應該載入測試自己的應用程式，以判斷應用程式的正確快取大小。
>隨著我們定期公佈較新的結果，這些數字可能會變更。
>

我們可以從這個表格中獲得下列結論：

* 進階層中相同大小快取的輸送量，比標準層的還高。 例如，在有 6 GB 快取的前提下，相較於 C3 的 100,000，P1 每秒是 180,000 個要求 (RPS)。
* 使用 Redis 叢集，當您增加叢集中的分區 (節點) 數目時，輸送量會呈線性增加。 例如，如果您建立具有 10 個分區的 P4 叢集，則可用的輸送量為 400,000 *10 = 400 萬個 RPS。
* 相較於標準層，進階層中的金鑰大小越大，輸送量就越高。

| 定價層 | 大小 | CPU 核心 | 可用的頻寬 | 1 KB 值大小 | 1 KB 值大小 |
| --- | --- | --- | --- | --- | --- |
| **標準快取大小** | | |**每秒 Mb (Mb/s) / 每秒 MB (MB/s)** |**每秒要求數目 (RPS) 非 SSL** |**每秒要求數目 (RPS) SSL** |
| C0 | 250 MB | 共用 | 100 / 12.5  |  15,000 |   7,500 |
| C1 |   1 GB | 1      | 500 / 62.5  |  38,000 |  20,720 |
| C2 | 2.5 GB | 2      | 500 / 62.5  |  41,000 |  37,000 |
| C3 |   6 GB | 4      | 1000 / 125  | 100,000 |  90,000 |
| C4 |  13 GB | 2      | 500 / 62.5  |  60,000 |  55,000 |
| C5 |  26 GB | 4      | 1,000 / 125 | 102,000 |  93,000 |
| C6 |  53 GB | 8      | 2,000 / 250 | 126,000 | 120,000 |
| **高階快取大小** | |**每一分區的 CPU 核心數目** | **每秒 Mb (Mb/s) / 每秒 MB (MB/s)** |**每秒要求數目 (RPS) 非 SSL，每個分區** |**每秒要求數目 (RPS) SSL，每個分區** |
| P1 |   6 GB |  2 | 1,500 / 187.5 | 180,000 | 172,000 |
| P2 |  13 GB |  4 | 3,000 / 375   | 350,000 | 341,000 |
| P3 |  26 GB |  4 | 3,000 / 375   | 350,000 | 341,000 |
| P4 |  53 GB |  8 | 6,000 / 750   | 400,000 | 373,000 |
| P5 | 120 GB | 20 | 6,000 / 750   | 400,000 | 373,000 |

如需設定 stunnel 或下載 Redis 工具 (例如，`redis-benchmark.exe`) 的指示，請參閱[如何執行 Redis 命令？](#cache-commands)小節。

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>我應該在哪個區域找到快取？
為獲得最佳效能和最低延遲，請將「Azure Redis 快取」放在與快取用戶端應用程式相同的區域中。

### <a name="where-do-my-cached-data-reside"></a>我的快取資料位於何處？
Azure Cache for Redis 會將您的應用程式資料儲存在 VM 或 Vm 的 RAM 中，視裝載您快取的層而定。 您的資料會嚴格地存放在您預設選取的 Azure 區域中。 在兩種情況下，您的資料可能會離開區域：
  1. 當您啟用快取的持續性時，Azure Cache for Redis 會將您的資料備份至您擁有的 Azure 儲存體帳戶。 如果您所提供的儲存體帳戶是在另一個區域中，則資料的複本將會在該處結束。
  1. 如果您設定了異地複寫，而次要快取位於不同的區域中（通常是如此），則您的資料將會複寫到該區域。

您必須明確地設定 Azure Cache for Redis，才能使用這些功能。 您也可以完全掌控儲存體帳戶或次要快取所在的區域。

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-cache-for-redis"></a>Azure Redis 快取如何收費？
如需了解「Azure Redis 快取」定價，請參閱[這裡](https://azure.microsoft.com/pricing/details/cache/)。 定價頁面會以每小時和每月費率列出定價。 快取是根據從建立快取到刪除快取的時間，以分鐘為單位來收費。 沒有用於停止或暫停快取收費的選項。

### <a name="can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-21vianet-cloud-or-microsoft-azure-germany"></a>我可以將 Azure Cache for Redis 與 Azure Government 雲端、Azure 中國世紀雲端或 Microsoft Azure 德國搭配使用嗎？
是，「Azure Government 雲端」、「Azure China 21Vianet 雲端」及「Microsoft Azure 德國」都有提供「Azure Cache for Redis」。 這些雲端中用來存取及管理「Azure Redis 快取」的 URL 與「Azure 公用雲端」不同。

| Cloud   | Redis 的 DNS 尾碼            |
|---------|---------------------------------|
| 公開  | *.redis.cache.windows.net       |
| US Gov  | *.redis.cache.usgovcloudapi.net |
| 德國 | *.redis.cache.cloudapi.de       |
| 中國   | *.redis.cache.chinacloudapi.cn  |

如需深入了解搭配其他雲端使用「Azure Redis 快取」的考量，請參閱下列連結。

- [Azure Government 資料庫 - Azure Redis 快取](../azure-government/documentation-government-services-database.md#azure-cache-for-redis)
- [Azure China 21Vianet 雲端 - Azure Cache for Redis](https://www.azure.cn/home/features/redis-cache/)
- [Microsoft Azure (德國)](https://azure.microsoft.com/overview/clouds/germany/)

如需有關在「Azure Government 雲端」、「Azure China 21Vianet 雲端」及「Microsoft Azure 德國」中搭配 PowerShell 使用「Azure Cache for Redis」的資訊，請參閱[如何連線到其他雲端 - Azure Cache for Redis PowerShell](cache-how-to-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds)。

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>StackExchange.Redis 設定選項的作用為何？
StackExchange.Redis 有許多選項。 本節談論一些常見設定。 如需 StackExchange.Redis 選項的詳細資訊，請參閱 [StackExchange.Redis 設定](https://stackexchange.github.io/StackExchange.Redis/Configuration)。

| ConfigurationOptions | 描述 | 建議 |
| --- | --- | --- |
| AbortOnConnectFail |設定為 true 時，在網路失敗之後不會重新連線。 |設定為 false，並讓 StackExchange.Redis 自動重新連線。 |
| ConnectRetry |初始連線期間的重複連線嘗試次數。 |如需指引，請參閱下列附註。 |
| ConnectTimeout |連線作業的逾時 (毫秒)。 |如需指引，請參閱下列附註。 |

用戶端的預設值通常就已足夠。 您可以根據工作負載來微調選項。

* **重試**
  * 對於 ConnectRetry 和 ConnectTimeout，一般指引是快速檢錯，然後再試一次。 此指引是根據您的工作負載，以及用戶端發出 Redis 命令到接收回應所需的平均時間。
  * 讓 StackExchange.Redis 自動重新連線，而不檢查連線狀態，並自行重新連線。 **避免使用 ConnectionMultiplexer.IsConnected 屬性**。
  * 滾雪球 - 有時，您可能會遇到問題，但越是重試，問題卻如雪球般越滾越大，不可能解決。 如果出現滾雪球的情況，您應該考慮使用指數輪詢重試演算法 (如 Microsoft Patterns & Practices 群組所發佈的[重試一般指引](../best-practices-retry-general.md)所述)。
  
* **逾時值**
  * 請考慮您的工作負載，並據此設定值。 如果您要儲存大的值，請將逾時設定為較高的值。
  * 將 `AbortOnConnectFail` 設為 false，並讓 StackExchange.Redis 為您重新連線。
  * 使用應用程式的單一 ConnectionMultiplexer 執行個體。 您可以使用 LazyConnection 建立 Connection 屬性所傳回的單一執行個體 (如 [使用 ConnectionMultiplexer 類別連線至快取](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)所示)。
  * 將 `ConnectionMultiplexer.ClientName` 屬性設定為應用程式執行個體唯一名稱，以進行診斷。
  * 針對自訂工作負載，使用多個 `ConnectionMultiplexer` 執行個體。
      * 如果您的應用程式中有不同的負載，則可以遵循此模型。 例如：
      * 您可以有一個多工器來處理大型索引鍵。
      * 您可以有一個多工器來處理小型索引鍵。
      * 您可以設定連線逾時的不同值，以及每個所使用 ConnectionMultiplexer 的重試邏輯。
      * 在每個多工器上設定 `ClientName` 屬性，以協助診斷。
      * 此指引可以疏解每個 `ConnectionMultiplexer` 的延遲。

### <a name="what-azure-cache-for-redis-clients-can-i-use"></a>我可以使用哪些 Azure Redis 快取用戶端？
Redis 最大的好處是，有許多用戶端支援許多不同的開發語言。 如需最新的用戶端清單，請參閱 [Redis 用戶端](https://redis.io/clients)。 如需涵蓋數個不同語言和用戶端的教學課程，請參閱[如何使用 Azure Cache for Redis](cache-dotnet-how-to-use-azure-redis-cache.md) 以及目錄中的同層級文章。

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-cache-for-redis"></a>Azure Redis 快取是否有本機模擬器？
「Azure Redis 快取」沒有本機模擬器，但您可以從本機電腦的 [Redis 命令列工具](https://github.com/MSOpenTech/redis/releases/)執行 MSOpenTech 版本的 redis-server.exe，然後與其連線以取得類似本機快取模擬器的體驗，如以下範例所示：

```csharp
private static Lazy<ConnectionMultiplexer>
    lazyConnection = new Lazy<ConnectionMultiplexer> (() =>
    {
        // Connect to a locally running instance of Redis to simulate
        // a local cache emulator experience.
        return ConnectionMultiplexer.Connect("127.0.0.1:6379");
    });

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

如有需要，您也可以依選擇設定 [redis.conf](https://redis.io/topics/config) 檔案，以更符合線上「Azure Redis 快取」的[預設快取設定](cache-configure.md#default-redis-server-configuration)。

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>如何執行 Redis 命令？
您可以使用 [Redis 命令](https://redis.io/commands#)中列出的任何命令，但不包含 [Azure Redis 快取中不支援的 Redis 命令](cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis)中列出的命令。 您有幾種方式可以執行 Redis 命令。

* 如果您有標準或進階快取，就可以使用 [Redis 主控台](cache-configure.md#redis-console)來執行 Redis 命令。 Redis 主控台提供安全的方式在 Azure 入口網站中執行 Redis 命令。
* 您也可以使用 Redis 命令列工具。 若要使用那些工具，請執行下列步驟：
* 下載 [Redis 命令列工具](https://github.com/MSOpenTech/redis/releases/)。
* 使用 `redis-cli.exe`連線至快取。 使用 -h 參數傳入快取端點，並使用 -a 傳入金鑰，如下列範例所示：
* `redis-cli -h <Azure Cache for Redis name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> Redis 命令列工具未使用 TLS 連接埠，但您可以遵循[如何使用 Redis 命令列工具搭配 Azure Cache for Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-redis-cli-tool)文章中的指示，使用公用程式 (例如 `stunnel`) 將工具安全地連線至 TLS 連接埠。
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Azure Redis 快取為什麼沒有像一些其他 Azure 服務有 MSDN 類別庫參考？
Microsoft Azure Cache for Redis 是以熱門的開放原始碼記憶體內部資料存放區 Redis 為基礎。 它可由各種不同的 [Redis 用戶端](https://redis.io/clients) 來存取，並適用許多程式設計語言。 每個用戶端都有自己的 API，可使用 [Redis 命令](https://redis.io/commands)對「Azure Redis 快取」執行個體發出呼叫。

因為每個用戶端都不同，所以 MSDN 上沒有一個集中式類別參考，每個用戶端都會維護其專屬的參考文件。 除了參考文件之外，還有數個教學課程，示範如何使用不同的語言和快取用戶端來開始使用「Azure Redis 快取」。 若要存取這些教學課程，請參閱[如何使用 Azure Cache for Redis](cache-dotnet-how-to-use-azure-redis-cache.md) 以及目錄中的同層級文章。

### <a name="can-i-use-azure-cache-for-redis-as-a-php-session-cache"></a>是否可以使用 Azure Redis 快取作為 PHP 工作階段快取？
是，若要使用「Azure Redis 快取」作為 PHP 工作階段快取，請在 `session.save_path` 中指定「Azure Redis 快取」執行個體的連接字串。

> [!IMPORTANT]
> 使用「Azure Redis 快取」作為 PHP 工作階段快取時，您必須將用來連線到快取的安全性金鑰進行 URL 編碼，如以下範例所示：
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> 如果金鑰未進行 URL 編碼，您可能會收到類似此訊息的例外狀況︰`Failed to parse session.save_path`
>
>

如需有關搭配 PhpRedis 用戶端使用「Azure Redis 快取」作為 PHP 工作階段快取的詳細資訊，請參閱 [PHP 工作階段處理常式](https://github.com/phpredis/phpredis#php-session-handler) \(英文\)。

### <a name="what-are-redis-databases"></a>什麼是 Redis 資料庫？

Redis 資料庫就是相同 Redis 執行個體內的資料邏輯分隔。 所有資料庫之間會共用快取記憶體，給定資料庫的實際記憶體耗用量取決於該資料庫中儲存的索引鍵/值。 例如，假設 C6 快取有 53 GB 的記憶體，而 P5 有 120 GB。 您可以選擇將 53 GB / 120 GB 全部放入一個資料庫，或分割給多個資料庫。 

> [!NOTE]
> 使用已啟用叢集功能的「進階 Azure Redis 快取」時，只有資料庫 0 可供使用。 這項限制是固有的 Redis 限制，並非特別針對「Azure Redis 快取」。 如需詳細資訊，請參閱 [我需要對用戶端應用程式進行任何變更才能使用叢集嗎？](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis"></a>何時應該啟用非 TLS/SSL 連接埠來連線至 Redis？
Redis 伺服器並未原生支援 SSL，但 Azure Cache for Redis 則可支援。 如果您是連線至 Azure Cache for Redis，且您的用戶端支援 TLS (例如 StackExchange.Redis)，則應該使用 TLS。

>[!NOTE]
>預設會停用新「Azure Cache for Redis」執行個體的非 TLS 連接埠。 如果您的用戶端不支援 TLS，則必須依照[在 Azure Cache for Redis 中設定快取](cache-configure.md)一文中[存取連接埠](cache-configure.md#access-ports)一節的指示來啟用非 TLS 連接埠。
>
>

Redis 工具 (例如 `redis-cli`) 未使用 TLS 連接埠，但您可以遵循[宣佈 Redis 預覽版本的 ASP.NET 工作階段狀態供應器](https://devblogs.microsoft.com/aspnet/announcing-asp-net-session-state-provider-for-redis-preview-release/)部落格文章中的指示，使用公用程式 (例如 `stunnel`) 將工具安全地連線至 TLS 連接埠。

如需下載 Redis 工具的指示，請參閱 [如何執行 Redis 命令？](#cache-commands) 小節。

### <a name="what-are-some-production-best-practices"></a>生產環境的最佳作法有哪些？
* [StackExchange.Redis 最佳作法](#stackexchangeredis-best-practices)
* [組態和概念](#configuration-and-concepts)
* [效能測試](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis 最佳作法
* 將 `AbortConnect` 設定為 false，然後讓 ConnectionMultiplexer 自動重新連線。 [參閱此處了解詳細資訊](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md)。
* 重複使用 ConnectionMultiplexer - 不要對每個要求建立一個新的。 建議使用[此處顯示](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)的 `Lazy<ConnectionMultiplexer>` 模式。
* Redis 在值越小時運作得最好，因此請考慮將較大的資料切分成多個金鑰。 在[此 Redis 討論](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)中，100kb 就算很大。 閱讀 [這篇文章](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) 以了解較大值所造成的範例問題。
* 設定您的 [執行緒集區設定](#important-details-about-threadpool-growth) 以避免逾時。
* 使用至少 5 秒的預設 connectTimeout。 此間隔可讓 StackExchange.Redis 在發生網路中斷的情況下，有足夠的時間重新建立連線。
* 請注意您執行不同作業的相關效能成本。 例如， `KEYS` 命令是一種 O(n) 作業，應該盡量避免。 [Redis.io 網站](https://redis.io/commands/) 有它支援的每項作業的時間複雜度詳細資訊。 按一下每個命令，可查看每項作業的複雜度。

#### <a name="configuration-and-concepts"></a>組態和概念
* 為生產環境系統使用標準或進階層。 基本層是一個單一節點的系統，沒有資料複寫也沒有 SLA。 此外，請至少使用 C1 快取。 C0 快取通常用於簡單的開發/測試案例。
* 請記住，Redis 是一種 **記憶體內部** 資料存放區。 閱讀 [這篇文章](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) ，您就能知道資料可能遺失的案例。
* 開發您的系統，讓系統可以處理因 [修補和容錯移轉](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)所造成的連線短暫中斷。

#### <a name="performance-testing"></a>效能測試
* 先使用 `redis-benchmark.exe` ，在撰寫您自己的效能測試之前了解可能的輸送量。 因為 `redis-benchmark` 不支援 TLS，您必須在執行測試之前，[透過 Azure 入口網站啟用非 TLS 連接埠 ](cache-configure.md#access-ports)。 例如，參閱 [如何效能評定和測試我快取的效能？](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* 用來進行測試的用戶端 VM 應該與您的「Azure Redis 快取」執行個體位於相同的區域。
* 我們建議為您的用戶端使用 Dv2 VM 系列，因為此系列有更好的硬體，能提供最佳的結果。
* 請確定您選擇的用戶端 VM 與您進行測試的快取至少有一樣多的運算和頻寬能力。
* 如果您是在 Windows 上，請在用戶端電腦上啟用 VRSS。 [參閱此處了解詳細資訊](https://technet.microsoft.com/library/dn383582.aspx)。
* 進階層 Redis 執行個體會有比較好的網路延遲和輸送量，因為是在比較好的硬體 (CPU 和網路) 上執行。

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>使用常見 Redis 命令時的一些考量為何？

* 除非您充分了解需要花很長時間才能完成特定 Redis 命令的影響，否則應避免執行這些命令。 例如，請勿在生產環境中執行 [KEYS](https://redis.io/commands/keys) 命令。 視金鑰的數目而定，系統可能需要很長的時間才會傳回。 Redis 是單一執行緒伺服器，並且一次處理一個命令。 如果您在 KEYS 之後發出其他命令，則除非 Redis 處理 KEYS 命令，否則不會處理它們。 [Redis.io 網站](https://redis.io/commands/) 有它支援的每項作業的時間複雜度詳細資訊。 按一下每個命令，可查看每項作業的複雜度。
* 索引鍵大小 - 應該使用較小的索引鍵/值還是較大的索引鍵/值？ 需視案例而定。 如果您的案例需要較大的金鑰，您可以調整 ConnectionTimeout，然後重試值並調整重試邏輯。 從 Redis 伺服器的觀點，較小的值即表示有較佳的效能。
* 這些考量不表示您無法在 Redis 中儲存較大的值；您必須注意下列考量。 延遲會比較高。 如果您有一個較大的資料集和一個較小的值，則可以使用多個 ConnectionMultiplexer 執行個體，而每個執行個體都會設定一組不同的逾時和重試值 (如先前的 [StackExchange.Redis 設定選項的作用為何](#cache-configuration) 小節所述)。

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>如何效能評定和測試我快取的效能？
* [啟用快取診斷](cache-how-to-monitor.md#enable-cache-diagnostics)，以[監視](cache-how-to-monitor.md)您快取的健全狀況。 您可以在 Azure 入口網站中檢視度量，也可以使用您選擇的工具 [下載並檢閱](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) 它們。
* 您可以使用 redis-benchmark.exe 對 Redis 伺服器進行負載測試。
* 確定負載測試用戶端與「Azure Redis 快取」位於相同的區域。
* 使用 redis-cli.exe，並使用 INFO 命令來監視快取。
* 如果您的負載造成記憶體分散嚴重，則應該擴大為較大的快取大小。
* 如需下載 Redis 工具的指示，請參閱 [如何執行 Redis 命令？](#cache-commands) 小節。

以下命令提供使用 redis-benchmark.exe 的範例。 如需精確的結果，請從與快取位於相同區域的 VM 執行這些命令。

* 使用 1k 承載測試進行管線處理的 SET 要求

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* 使用 1k 承載測試進行管線處理的 GET 要求。
  注意：請先執行上面所示的 SET 測試來填入快取

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>執行緒集區成長的重要詳細資料
CLR 執行緒集區有兩種類型的執行緒：「背景工作」和「I/O 完成連接埠」(IOCP) 執行緒。

* 背景工作執行緒是用於處理 `Task.Run(…)` 或 `ThreadPool.QueueUserWorkItem(…)` 方法之類的作業。 需要在背景執行緒上開啟工作時，CLR 中的各種元件也會使用這些執行緒。
* 發生非同步 IO (例如從網路讀取) 時，會使用 IOCP 執行緒。

執行緒集區可視需要提供新背景工作執行緒或 I/O 完成執行緒 (而不需要任何節流)，直到它到達每個類型執行緒的「最低」設定。 根據預設，執行緒的數目下限設為系統上的處理器數目。

一旦現有 (忙碌) 執行緒的數量達到執行緒集區的「最低」執行緒數目，執行緒集區會以每 500 毫秒將一個新執行緒插入執行緒的速率節流。 通常，如果您的系統需要 IOCP 執行緒的工作暴增，系統將可快速處理該工作。 不過，如果暴增的工作超過設定的「最低」設定，部份工作的處理會發生一些延遲，因為執行緒集區會等待一或兩個狀況發生。

1. 現有的執行緒變得可用來處理工作。
2. 有 500 毫秒沒有現有的執行緒變得可用，因此會建立一個新的執行緒。

基本上，這表示，當忙碌執行緒數目超過「最低」執行緒數量時，在應用程式處理網路流量之前，您可能要支付 500 毫秒延遲。 此外，務必注意，當現有的執行緒處於閒置的時間超過 15 秒 (根據我的記憶)，它將會被清除，而且這個循環的擴大和縮減可以重複。

如果我們查看來自 StackExchange.Redis (建置 1.0.450 或更新版本) 的範例錯誤訊息，您會看到它現在會列印執行緒集區統計資料 (請參閱以下的 IOCP 和背景工作詳細資料)。

```
System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
IOCP: (Busy=6,Free=994,Min=4,Max=1000),
WORKER: (Busy=3,Free=997,Min=4,Max=1000)
```

在上述範例中，您可以看到 IOCP 執行緒有六個忙碌執行緒，而系統設定成允許最低四個執行緒。 在此情況下，用戶端就可能會看到兩個 500 毫秒延遲，因為 6 > 4。

請注意，如果 IOCP 或背景工作執行緒的成長發生節流，StackExchange.Redis 可能達到逾時。

### <a name="recommendation"></a>建議

經由這項資訊，我們強烈建議客戶將 IOCP 和背景工作執行緒的「最低」組態值設定成大於預設值的數值。 對於這個值應該為何，我們無法提供一體適用的指引，因為某個應用程式的適用值對另一個應用程式來說有可能太高或太低。 這項設定也會影響複雜應用程式其他部分的效能，因此每位客戶需要微調此設定來滿足其特定需求。 200 或 300 是好的起點，那麼請測試並視需要調整。

如何設定這項設定：

* 建議使用 `global.asax.cs` 中的 [ThreadPool.SetMinThreads (...)](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_) 方法，以程式設計方式變更這項設定。 例如：

    ```csharp
    private readonly int minThreads = 200;
    void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on application startup
        AreaRegistration.RegisterAllAreas();
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ThreadPool.SetMinThreads(minThreads, minThreads);
    }
    ```

    > [!NOTE]
    > 此方法指定的值是全域設定，會影響整個 AppDomain。 舉例來說，如果您有一部 4 核心電腦，而且想要將「minWorkerThreads」 和「minIOThreads」設為執行階段期間每個 CPU 50 個，可以使用 **ThreadPool.SetMinThreads (200, 200)** 。

* 您也可以使用 `Machine.config` (通常位於 `%SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\`) 中 `<processModel>` 設定元素底下的 [*minIoThreads* 或 *minWorkerThreads* 組態設定](https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx)來指定最低執行緒設定。 **我們通常不建議以這種方式設定最低執行緒數，因為它是全系統的設定。**

  > [!NOTE]
  > 這個組態元素中指定的值是「每一核心」設定。 例如，如果您有 4 核心的電腦，並且想要在執行階段將 *minIOThreads* 設為 200，您會使用 `<processModel minIoThreads="50"/>`。
  >

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>使用 StackExchange.Redis 時啟用伺服器 GC 在用戶端上取得更多輸送量
啟用伺服器 GC 可以最佳化用戶端，並在使用 StackExchange.Redis 時提供較佳的效能和輸送量。 如需有關伺服器 GC，以及如何加以啟用的詳細資訊，請參閱下列文件：

* [啟用伺服器 GC](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Fundamentals of Garbage Collection (記憶體回收的基本概念)](/dotnet/standard/garbage-collection/fundamentals)
* [Garbage Collection and Performance (記憶體回收與效能)](/dotnet/standard/garbage-collection/performance)


### <a name="performance-considerations-around-connections"></a>連線相關的效能考量

每個定價層都有不同的用戶端連線、記憶體和頻寬的限制。 每個快取大小都可允許以某個數目為「上限」的連線數，而每個連到 Redis 的連線則都有相關的額外負荷。 因 TLS/SSL 加密而產生的 CPU 與記憶體使用量即是這類額外負荷的其中一例。 所指定快取大小的連線數上限是假設快取負載情況為輕度。 如果來自連線額外負荷的負載「加上」來自用戶端作業的負載超過系統的容量，則即使您尚未超出目前快取大小的連線限制，快取也會發生容量問題。

如需有關每個層級之不同連線限制的詳細資訊，請參閱 [Azure Redis 快取價格](https://azure.microsoft.com/pricing/details/cache/)。 如需有關連線及其他預設組態的詳細資訊，請參閱[預設 Redis 伺服器組態](cache-configure.md#default-redis-server-configuration)。

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>如何監視快取的健全狀況和效能？
您可以在 [Azure 入口網站](https://portal.azure.com)中監視「Microsoft Azure Redis 快取」執行個體。 您可以檢視度量、將度量圖表釘選到「開始面板」、自訂監視圖表的日期和時間範圍、新增和移除圖表中的度量，以及設定符合特定條件時的警示。 如需詳細資訊，請參閱[監視 Azure Redis 快取](cache-how-to-monitor.md)。

「Azure Redis 快取」**資源功能表**也包含數個工具，可對快取進行監控和疑難排解。

* **疑難排解和解決問題**提供常見問題的相關資訊，以及解決問題的策略。
* **資源健康狀態** 會監看您的資源，並告知您資源是否正如預期般執行。 如需 Azure 資源健康狀態服務的詳細資訊，請參閱 [Azure 資源健康狀態概觀](../resource-health/resource-health-overview.md)。
* **新增支援要求** 提供選項來提出快取的支援要求。

這些工具可讓您監視 Azure Cache for Redis 執行個體的健康情況，並協助您管理快取應用程式。 如需詳細資訊，請參閱[如何設定 Azure Redis 快取](cache-configure.md)的＜支援和疑難排解設定＞一節。

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>為什麼看到逾時？
用來與 Redis 溝通的用戶端發生逾時。 將命令傳送到 Redis 伺服器時，會將命令排入佇列，而且 Redis 伺服器最後會挑選並執行命令。 不過，用戶端可能會在此程序期間逾時，而且，如果是這樣，則會在呼叫端引發例外狀況。 如需為逾時問題疑難排解的詳細資訊，請參閱[用戶端疑難排解](cache-troubleshoot-client.md)和 [StackExchange.Redis 逾時例外狀況](cache-troubleshoot-timeouts.md#stackexchangeredis-timeout-exceptions)。

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>我的用戶端為什麼中斷與快取的連線？
下面是快取中斷連線的一些常見原因。

* 用戶端原因
  * 已重新部署用戶端應用程式。
  * 用戶端應用程式已執行調整作業。
    * 如果是雲端服務或 Web Apps，這可能是自動調整所造成。
  * 用戶端上的網路層已變更。
  * 在用戶端或用戶端與伺服器之間的網路節點中，發生暫時性錯誤。
  * 已達頻寬閾值限制。
  * CPU 繫結作業用了太多時間才完成。
* 伺服器端原因
  * 在標準快取供應專案上，Azure Cache for Redis 服務已起始從主要節點到複本節點的故障切換。
  * Azure 會修補已部署快取的執行個體
    * 這可能適用於 Redis 伺服器更新或一般 VM 維護。

### <a name="which-azure-cache-offerings-is-right-for-me"></a>哪一個 Azure 快取供應專案適合我？
> [!IMPORTANT]
> 根據 2016[公告](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)，azure 受控快取服務和 Azure 角色中快取服務已于2016年11月30日**淘汰**。 我們建議使用 [Azure Redis 快取](https://azure.microsoft.com/services/cache/)。 如需有關移轉的資訊，請參閱[從受控快取服務移轉至 Azure Redis 快取](cache-migrate-to-redis.md)。
>
>

### <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Azure Cache for Redis 已正式推出，大小最大可達 120 GB 且可用性 SLA 為 99.9%。 新的 [進階層](cache-premium-tier-intro.md) 以 99.9% SLA 提供高達 1.2 TB 的大小，並且支援叢集、VNET 和持續性。

「Azure Redis 快取」可讓您存取由 Microsoft 管理的安全、專用「Azure Redis 快取」。 使用這項供應項目，您可以利用 Redis 提供的豐富功能集和生態系統，並使用 Microsoft 提供的可靠託管及監控服務。

不同於僅處理金鑰-值組的傳統快取，Redis 受到歡迎是因為其高效能資料類型。 Redis 也支援在這些類型上執行不可部分完成的作業，例如附加至字串、在雜湊中遞增值、推入至清單、計算集合交集、聯集和差異，或取得已排序集合中最高排名的成員。 其他功能包括支援交易、發行/訂用、Lua 指令碼撰寫、存活時間有限的索引鍵及組態設定，可讓 Redis 的行為更像傳統快取。

Redis 成功的另一個重要層面是建置健全、有活力的開放原始碼生態系統。 這會反映在跨多種語言可用的各式各樣 Redis 用戶端。 此生態系統和廣泛的一系列用戶端可讓「Azure Redis 快取」可供幾乎任何您會在 Azure 內建置的工作負載使用。

如需有關如何開始使用「Azure Redis 快取」的詳細資訊，請參閱[如何使用 Azure Redis 快取](cache-dotnet-how-to-use-azure-redis-cache.md)和 [Azure Redis 快取文件](index.yml)。

### <a name="managed-cache-service"></a>受控快取服務
[受控快取服務已在 2016 年 11 月 30 日淘汰 (英文)。](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

若要檢視封存文件，請參閱[封存受控快取服務文件](/previous-versions/azure/azure-services/dn386094(v=azure.100))。

### <a name="in-role-cache"></a>角色中快取
[In-Role Cache 已在 2016 年 11 月 30 日淘汰 (英文)。](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

若要檢視封存文件，請參閱[封存 In-Role Cache 文件](/previous-versions/azure/azure-services/dn386103(v=azure.100))。

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
