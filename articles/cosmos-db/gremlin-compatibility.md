---
title: Azure Cosmos DB Gremlin 與 TinkerPop 功能的相容性
description: 參考檔 Graph 引擎相容性問題
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 7257246f618e3028534f3ebd60eaf6f94a3a4720
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092502"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin 相容性
Azure Cosmos DB 圖形引擎會密切遵循[Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)的「遍歷」步驟規格，但在執行方面，特定的 Azure Cosmos DB 有差異。 若要瞭解支援的 Gremlin 步驟清單，請參閱[GREMLIN API 線路通訊協定支援](gremlin-support.md)一文。

## <a name="behavior-differences"></a>行為差異

* Azure Cosmos DB 圖形引擎會執行***廣度優先的***回合，而 TinkerPop Gremlin 是深度優先。 此行為可在水準可擴充的系統（例如 Cosmos DB）中達到更佳的效能。 

## <a name="unsupported-features"></a>不支援的功能

* ***[Gremlin 位元組程式碼](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** 是不限程式設計語言的圖形周遊規格。 Cosmos DB Graph 尚不支援。 使用 `GremlinClient.SubmitAsync()` 並將遍歷傳遞為文字字串。

* ***`property(set, 'xyz', 1)`*** 目前不支援 set 基數。 請改用 `property(list, 'xyz', 1)`。 若要深入瞭解，請參閱[具有 TinkerPop 的頂點屬性](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties)。

* 此*** `match()` 步驟***目前無法使用。 此步驟提供宣告式查詢功能。

* 不支援***以物件作為***頂點或邊緣的屬性。 屬性只能是基本類型或陣列。

* ***依陣列屬性排序*** `order().by(<array property>)`不支援。 僅支援依據基本類型排序。

* 不支援***非基本的 JSON 類型***。 使用 `string` 、 `number` 或 `true` / `false` 類型。 `null`不支援值。 

* 目前不支援***GraphSONv3***序列化程式。 使用 `GraphSONv2` 連接設定中的序列化程式、讀取器和寫入器類別。 Azure Cosmos DB Gremlin API 傳回的結果與 GraphSON 格式的格式不同。 

* 目前不支援**Lambda 運算式和函數**。 這包括 `.map{<expression>}` 、 `.by{<expression>}` 和 `.filter{<expression>}` 函數。 若要深入瞭解，並瞭解如何使用 Gremlin 步驟來重寫它們，請參閱[lambda 上的附注](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)。

* 因為系統的分散式本質，所以不支援***交易***。  在 Gremlin 帳戶上設定適當的一致性模型，以「讀取您自己的寫入」，並使用開放式平行存取來解決衝突的寫入。

## <a name="known-limitations"></a>已知限制

* **使用中間遍歷 `.V()` 步驟 Gremlin 查詢的索引使用率**：目前，只有第一次呼叫時，才 `.V()` 會使用索引來解析附加的任何篩選或述詞。 後續的呼叫將不會查閱索引，這可能會增加查詢的延遲和成本。
    
    假設是預設的索引編制，從步驟開始的一般 read Gremlin 查詢 `.V()` 會使用其附加篩選步驟中的參數，例如 `.has()` 或， `.where()` 以優化查詢的成本和效能。 例如:

    ```java
    g.V().has('category', 'A')
    ```

    不過，如果 `.V()` Gremlin 查詢中包含一個以上的步驟，則查詢的資料解析可能不是最佳的。 以下列查詢做為範例：

    ```java
    g.V().has('category', 'A').as('a').V().has('category', 'B').as('b').select('a', 'b')
    ```

    此查詢會根據名為的屬性，傳回兩個頂點群組 `category` 。 在此情況下，只有第一個呼叫 `g.V().has('category', 'A')` 會使用索引來根據其屬性的值來解析頂點。

    此查詢的因應措施是使用 subtraversal 步驟（例如 `.map()` 和） `union()` 。 這一例如下：

    ```java
    // Query workaround using .map()
    g.V().has('category', 'A').as('a').map(__.V().has('category', 'B')).as('b').select('a','b')

    // Query workaround using .union()
    g.V().has('category', 'A').fold().union(unfold(), __.V().has('category', 'B'))
    ```

    您可以使用[Gremlin `executionProfile()` 步驟](graph-execution-profile.md)來檢查查詢的效能。

## <a name="next-steps"></a>後續步驟
* 請造訪[Cosmos DB user voice](https://feedback.azure.com/forums/263030-azure-cosmos-db)頁面來分享意見反應，並協助團隊專注于對您很重要的功能。
