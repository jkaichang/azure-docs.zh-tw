---
title: 教學課程：適用於 SQL API 的 Azure Cosmos DB 全域散發教學課程
description: 教學課程：了解如何使用 SQL API 搭配 .NET、Java、Python 和其他各種 SDK 來設定 Azure Cosmos DB 全域散發
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 11/05/2019
ms.reviewer: sngun
ms.custom: tracking-python
ms.openlocfilehash: 8d33756e1c28247c48d0b4c0a734e604c4e9eab0
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87280812"
---
# <a name="tutorial-set-up-azure-cosmos-db-global-distribution-using-the-sql-api"></a>教學課程：使用 SQL API 來設定 Azure Cosmos DB 全域散發

在本文中，我們會說明如何使用 Azure 入口網站來設定 Azure Cosmos DB 全域散發，然後使用 SQL API 來進行連線。

本文涵蓋下列工作： 

> [!div class="checklist"]
> * 使用 Azure 入口網站來設定全域散發
> * [使用 SQL API 來設定全域散發](sql-api-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a><a id="preferred-locations"></a> 使用 SQL API 來連線到慣用的區域

為了充分運用 [全球發佈](distribute-data-globally.md)，用戶端應用程式可以指定已排序的區域喜好設定清單，以用來執行文件作業。 SQL SDK 將會根據 Azure Cosmos DB 帳戶組態、目前的區域可用性及所指定的喜好設定清單，選擇最適合的端點來執行寫入和讀取作業。

這份喜好設定清單是在使用 SQL SDK 將連線初始化時即已指定。 SDK 會接受選擇性參數 `PreferredLocations`，也就是已排序的 Azure 區域清單。

SDK 會自動將所有寫入傳送至目前的寫入區域。 所有讀取都將傳送至慣用位置清單中的第一個可用區域。 如果要求失敗，用戶端將無法往下到清單中的下一個區域。

SDK 只會嘗試從慣用位置中指定的區域讀取。 因此，比方說，如果 Azure Cosmos 帳戶可供四個區域使用，但用戶端只針對 `PreferredLocations` 指定其中兩個讀取 (非寫入) 區域，則將不會在 `PreferredLocations` 中未指定的讀取區域以外地方提供讀取服務。 如果 `PreferredLocations` 清單中指定的讀取區域無法使用，就會在寫入區域以外地方提供讀取服務。

應用程式可以藉由檢查兩個屬性 (`WriteEndpoint` 和 `ReadEndpoint`，適用於 SDK 1.8 版和以上版本) 來確認 SDK 目前所選擇的寫入端點和讀取端點。 如果未設定 `PreferredLocations` 屬性，將會從目前的寫入區域為所有要求提供服務。

如果您未指定慣用位置但使用 `setCurrentLocation` 方法，SDK 會根據目前正在執行用戶端的區域，自動填入慣用的位置。 SDK 會根據區域與目前區域的鄰近性來排序區域。

## <a name="net-sdk"></a>.NET SDK

您不需變更任何程式碼即可使用 SDK。 在此情況下，SDK 會自動將讀取和寫入導向至目前寫入區域。

在 .NET SDK 的 1.8 版和更新版本中，適用於 DocumentClient 建構函式的 ConnectionPolicy 參數會有一個名為 Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations 的屬性。 這個屬性的類型是 Collection `<string>` ，而且應包含區域名稱的清單。 字串值會按照 [Azure 區域][regions]頁面的 [區域名稱] 資料行格式化，且在第一個字元之前和最後一個字元之後沒有空格。

目前的寫入和讀取端點分別適用於 DocumentClient.WriteEndpoint 和 DocumentClient.ReadEndpoint。

> [!NOTE]
> 不應將端點的 URI 視為長時間執行的常數。 服務可能會隨時更新這些項目。 SDK 會自動處理此變更。
>

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

如果您使用 .NET V2 SDK，請使用 `PreferredLocations` 屬性來設定慣用的區域。

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

或者，您可以使用 `SetCurrentLocation` 屬性，並讓 SDK 根據鄰近性選擇慣用的位置。

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

connectionPolicy.SetCurrentLocation("West US 2"); /

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

如果您使用 .NET V3 SDK，請使用 `ApplicationPreferredRegions` 屬性來設定慣用的區域。

```csharp

CosmosClientOptions options = new CosmosClientOptions();
options.ApplicationName = "MyApp";
options.ApplicationPreferredRegions = new List<string> {Regions.WestUS, Regions.WestUS2};

CosmosClient client = new CosmosClient(connectionString, options);

```

或者，您可以使用 `ApplicationRegion` 屬性，並讓 SDK 根據鄰近性選擇慣用的位置。

```csharp
CosmosClientOptions options = new CosmosClientOptions();
options.ApplicationName = "MyApp";
// If the application is running in West US
options.ApplicationRegion = Regions.WestUS;

CosmosClient client = new CosmosClient(connectionString, options);
```

---

## <a name="nodejsjavascript"></a>Node.js/JavaScript

> [!NOTE]
> 不應將端點的 URI 視為長時間執行的常數。 服務可能會隨時更新這些項目。 SDK 將會自動處理此變更。
>
>

以下是 Node.js/Javascript 的程式碼範例。

```JavaScript
// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
const preferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
const client = new CosmosClient{ endpoint, key, connectionPolicy: { preferredLocations } });
```

## <a name="python-sdk"></a>Python SDK

下列程式碼說明如何使用 Python SDK 來設定慣用位置：

```python
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe']
client = cosmos_client.CosmosClient(ENDPOINT, {'masterKey': MASTER_KEY}, connectionPolicy)

```

## <a name="java-v4-sdk"></a><a id="java4-preferred-locations"></a> Java V4 SDK

下列程式碼說明如何使用 Java SDK 來設定慣用位置：

# <a name="async"></a>[非同步](#tab/api-async)

   [Java SDK V4](sql-api-sdk-java-v4.md) (Maven [com.azure::azure-cosmos](https://mvnrepository.com/artifact/com.azure/azure-cosmos)) 非同步 API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=TutorialGlobalDistributionPreferredLocationAsync)]

# <a name="sync"></a>[同步處理](#tab/api-sync)

   [Java SDK V4](sql-api-sdk-java-v4.md) (Maven [com.azure::azure-cosmos](https://mvnrepository.com/artifact/com.azure/azure-cosmos)) 同步 API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/sync/SampleDocumentationSnippets.java?name=TutorialGlobalDistributionPreferredLocationSync)]

--- 

## <a name="rest"></a>REST

一旦資料庫帳戶可供多個區域使用之後，用戶端就可藉由在此 URI `https://{databaseaccount}.documents.azure.com/` 上執行 GET 要求來查詢其可用性

服務將會針對複本傳回區域清單及其對應的 Azure Cosmos DB 端點 URI。 回應中將會指出目前的寫入區域。 用戶端接著可針對所有未來的 REST API 要求選取適當的端點，如下所示。

範例回應

```json
{
    "_dbs": "//dbs/",
    "media": "//media/",
    "writableLocations": [
        {
            "Name": "West US",
            "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
        }
    ],
    "readableLocations": [
        {
            "Name": "East US",
            "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
        }
    ],
    "MaxMediaStorageUsageInMB": 2048,
    "MediaStorageUsageInMB": 0,
    "ConsistencyPolicy": {
        "defaultConsistencyLevel": "Session",
        "maxStalenessPrefix": 100,
        "maxIntervalInSeconds": 5
    },
    "addresses": "//addresses/",
    "id": "globaldbexample",
    "_rid": "globaldbexample.documents.azure.com",
    "_self": "",
    "_ts": 0,
    "_etag": null
}
```

* 所有的 PUT、POST 和 DELETE 要求都必須移至指定的寫入 URI
* 所有的 GET 和其他唯讀要求 (例如查詢) 可能會移至用戶端選擇的任何端點

將要求寫入至唯讀區域將會失敗，並產生 HTTP 錯誤碼 403 (「禁止」)。

如果寫入區域在用戶端初始探索階段之後變更，則寫入至上一個寫入區域的後續作業將會失敗，並產生 HTTP 錯誤碼 403 (「禁止」)。 用戶端接著應再次 GET 區域清單，以取得更新的寫入區域。

就這麼簡單，這樣便已完成本教學課程。 您可以透過閱讀 [Azure Cosmos DB 中的一致性層級](consistency-levels.md)，來了解如何管理全域複寫帳戶的一致性。 如需有關 Azure Cosmos DB 中全域資料庫複寫運作方式的詳細資訊，請參閱[使用 Azure Cosmos DB 來全域散發資料](distribute-data-globally.md)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已完成下列操作：

> [!div class="checklist"]
> * 使用 Azure 入口網站來設定全域散發
> * 使用 SQL API 來設定全域散發

您現在可以繼續進行到下一個教學課程，以了解如何使用 Azure Cosmos DB 本機模擬器在本機進行開發。

> [!div class="nextstepaction"]
> [使用模擬器在本機進行開發](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

