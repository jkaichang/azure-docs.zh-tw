---
title: Azure Cosmos DB 中的分頁
description: 瞭解分頁概念和接續權杖
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/29/2020
ms.openlocfilehash: 7f7f895b61e3c638cb347a2d73bb5ee458b31acd
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498815"
---
# <a name="pagination-in-azure-cosmos-db"></a>Azure Cosmos DB 中的分頁

在 Azure Cosmos DB 中，查詢可能會有多個頁面的結果。 本檔說明 Azure Cosmos DB 的查詢引擎用來決定是否要將查詢結果分割成多個頁面的準則。 您可以選擇性地使用接續權杖來管理跨越多個頁面的查詢結果。

## <a name="understanding-query-executions"></a>瞭解查詢執行

有時候查詢結果會分割成多個頁面。 每個頁面的結果都是由個別的查詢執行所產生。 當單一執行中無法傳回查詢結果時，Azure Cosmos DB 會自動將結果分割成多個頁面。

您可以藉由設定來指定查詢所傳回的最大專案數 `MaxItemCount` 。 `MaxItemCount`會針對每個要求指定，並保證查詢引擎會傳回該數目或更少的專案。 `MaxItemCount` `-1` 如果您不想限制每個查詢執行的結果數目，可以將設定為。

此外，其他原因是查詢引擎可能需要將查詢結果分割成多個頁面。 其中包括：

- 容器已進行節流處理，而且沒有可用的 ru 可傳回更多查詢結果
- 查詢執行的回應太大
- 查詢執行時間太長
- 查詢引擎更有效率地在其他執行中傳回結果

每個查詢執行所傳回的專案數一律會小於或等於 `MaxItemCount` 。 不過，其他條件可能會限制查詢可能傳回的結果數目。 如果您多次執行相同的查詢，則分頁的數目可能不是常數。 例如，如果查詢受到節流處理，每頁的可用結果可能較少，這表示查詢將會有額外的頁面。 在某些情況下，您的查詢也可能會傳回空的結果頁面。

## <a name="handling-multiple-pages-of-results"></a>處理多個結果頁面

為確保查詢結果正確，您應該在所有頁面上進行。 您應該繼續執行查詢，直到沒有其他頁面為止。

以下是一些從具有多個頁面的查詢來處理結果的範例：

- [.NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L280)
- [Java SDK](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/src/main/java/com/azure/cosmos/examples/documentcrud/sync/DocumentCRUDQuickstart.java#L162-L176)
- [Node.js SDK](https://github.com/Azure/azure-sdk-for-js/blob/83fcc44a23ad771128d6e0f49043656b3d1df990/sdk/cosmosdb/cosmos/samples/IndexManagement.ts#L128-L140)
- [Python SDK](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/examples.py#L89)

## <a name="continuation-tokens"></a>接續權杖

在 .NET SDK 和 JAVA SDK 中，您可以選擇性地使用接續權杖做為查詢進度的書簽。 Azure Cosmos DB 查詢執行在伺服器端是無狀態的，而且可以使用接續 token 隨時繼續執行。 Node.js SDK 或 Python SDK 不支援接續權杖。

以下是使用接續權杖的一些範例：

- [.NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/samples/code-samples/Queries/Program.cs#L699-L734)
- [Java SDK](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/src/main/java/com/azure/cosmos/examples/queries/sync/QueriesQuickstart.java#L216)

如果查詢傳回接續 token，則會有額外的查詢結果。

在 Azure Cosmos DB 的 REST API 中，您可以使用標頭來管理接續權杖 `x-ms-continuation` 。 如同使用 .NET 或 JAVA SDK 查詢，如果 `x-ms-continuation` 回應標頭不是空的，則表示查詢具有其他結果。

只要您使用相同的 SDK 版本，接續權杖永遠不會過期。 您可以選擇性地[限制接續 token 的大小](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions.responsecontinuationtokenlimitinkb?view=azure-dotnet#Microsoft_Azure_Documents_Client_FeedOptions_ResponseContinuationTokenLimitInKb)。 不論容器中的資料量或實體分割區數目，查詢都會傳回單一接續 token。

您不能將接續 token 用於具有[GROUP BY](sql-query-group-by.md)或[DISTINCT](sql-query-keywords.md#distinct)的查詢，因為這些查詢需要儲存大量的狀態。 針對具有的查詢 `DISTINCT` ，如果您將加入至查詢，則可以使用接續 token `ORDER BY` 。

以下是使用接續 token 的查詢範例 `DISTINCT` ：

```sql
SELECT DISTINCT VALUE c.name
FROM c
ORDER BY c.name
```

## <a name="next-steps"></a>後續步驟

- [Azure Cosmos DB 簡介](introduction.md)
- [Azure Cosmos DB .NET 範例](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [ORDER BY 子句](sql-query-order-by.md)