---
title: 對應資料流程
description: 在 Azure Data Factory 中對應資料流程的總覽
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/09/2020
ms.openlocfilehash: 850879675d4554329f24c86f2ac28660b303084c
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475561"
---
# <a name="what-are-mapping-data-flows"></a>什麼是對應資料流程？

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

對應資料流程會以視覺化方式設計 Azure Data Factory 中的資料轉換。 資料流程可讓資料工程師開發圖形化資料轉換邏輯，而不需要撰寫程式碼。 產生的資料流程會當做使用向外延展 Apache Spark 叢集 Azure Data Factory 管線內的活動來執行。 資料流程活動可以透過現有的 Data Factory 排程、控制、流程和監視功能來參與。

對應資料流程可提供完全視覺化的體驗，而不需要撰寫任何程式碼。 您的資料流程會在執行叢集上執行，以進行相應放大的資料處理。 Azure Data Factory 會處理所有的程式碼轉譯、路徑優化，以及資料流程作業的執行。

![架構](media/data-flow/adf-data-flows.png "架構")

## <a name="getting-started"></a>開始使用

若要建立資料流程，請選取 [ **Factory 資源**] 底下的加號，然後選取 **[資料流程]。** 

![新增資料流程](media/data-flow/newdataflow2.png "新增資料流程")

此動作會帶您前往 [資料流程] 畫布，您可以在其中建立轉換邏輯。 選取 [**新增來源**]，開始設定您的來源轉換。 如需詳細資訊，請參閱[來源轉換](data-flow-source.md)。

## <a name="data-flow-canvas"></a>資料流程畫布

資料流程畫布分成三個部分：頂端列、圖形和設定面板。 

![畫布](media/data-flow/canvas1.png "畫布")

### <a name="graph"></a>圖形

圖形會顯示轉換資料流程。 它會顯示來源資料流入一或多個接收時的歷程。 若要新增來源，請選取 [新增**來源**]。 若要加入新的轉換，請選取現有轉換右下方的加號。

![畫布](media/data-flow/canvas2.png "畫布")

### <a name="azure-integration-runtime-data-flow-properties"></a>Azure 整合執行時間資料流程屬性

![偵錯按鈕](media/data-flow/debugbutton.png "偵錯按鈕")

當您開始使用 ADF 中的資料流程時，您會想要針對瀏覽器 UI 上方的資料流程開啟「Debug」參數。 這會增加 Spark 叢集，以用於互動式的偵錯工具、資料預覽，以及管線的偵測執行。 您可以選擇自訂的[Azure Integration Runtime](concepts-integration-runtime.md)來設定使用的叢集大小。 在您上次進行資料預覽或上次執行的「偵測管線」之後，debug 會話會維持運作狀態最長達60分鐘。

當您使用「資料流程」活動來讓管線時，ADF 會使用與「執行于」屬性中的[活動](control-flow-execute-data-flow-activity.md)相關聯的 Azure Integration Runtime。

預設 Azure Integration Runtime 是一個小型的4核心單一背景工作節點叢集，可讓您預覽資料，並以最少的成本快速執行 debug 管線。 如果您要對大型資料集執行作業，請設定較大的 Azure IR 設定。

您可以藉由在 Azure IR 的資料流程屬性中設定 TTL，指示 ADF 維護叢集資源（Vm）的集區。 此動作會導致後續活動的作業執行速度更快。

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Azure 整合執行時間和資料流程策略

##### <a name="execute-data-flows-in-parallel"></a>平行執行資料流程

如果您以平行方式執行管線中的資料流程，ADF 會根據連接至每個活動的 Azure Integration Runtime 中的設定，為每個活動執行增加個別的 Spark 叢集。 若要在 ADF 管線中設計平行執行，請在 UI 中新增不含優先順序條件約束的資料流程活動。

在這三個選項中，此選項可能會在最短的時間內執行。 不過，每個平行資料流程會在不同的叢集上同時執行，因此事件的順序不具決定性。

如果您以平行方式在管線內執行資料流程活動，建議不要使用 TTL。 此動作是因為平行執行您的資料流程同時使用相同的 Azure Integration Runtime 會導致您的資料處理站有多個暖集區實例。

##### <a name="overload-single-data-flow"></a>多載單一資料流程

如果您將所有邏輯放在單一資料流程中，ADF 會在單一 Spark 叢集實例上執行相同的作業執行內容。

因為您的商務規則和商務邏輯可以混雜在一起，所以這個選項可能更具挑戰性並進行疑難排解。 此選項也無法提供許多重複用性。

##### <a name="execute-data-flows-sequentially"></a>循序執行資料流程

如果您在管線中依序執行資料流程活動，而且您已在 Azure IR 設定上設定 TTL，則 ADF 會重複使用計算資源（Vm），以加快後續的執行時間。 您仍然會在每次執行時收到新的 Spark 內容。

在這三個選項中，此動作可能會花費最長的時間來執行端對端。 但它確實提供了每個資料流程步驟中邏輯作業的清楚分隔。

### <a name="configuration-panel"></a>設定面板

[設定] 面板會顯示目前選取之轉換的特定設定。 如果未選取任何轉換，則會顯示資料流程。 在整體資料流程設定中，您可以編輯 [**一般**] 索引標籤底下的 [名稱] 和 [描述]，或透過 [**參數**] 索引標籤新增參數。如需詳細資訊，請參閱[對應資料流程參數](parameters-data-flow.md)。

每個轉換都包含至少四個設定索引標籤。

#### <a name="transformation-settings"></a>轉換設定

每個轉換的 [設定] 窗格中的第一個索引標籤包含該轉換的特定設定。 如需詳細資訊，請參閱該轉換的檔頁面。

![[來源設定] 索引標籤](media/data-flow/source1.png "[來源設定] 索引標籤")

#### <a name="optimize"></a>最佳化

[**優化**] 索引標籤包含用於設定資料分割配置的設定。 若要深入瞭解如何優化您的資料流程，請參閱[對應資料流程效能指南](concepts-data-flow-performance.md)。

![最佳化](media/data-flow/optimize.png "最佳化")

#### <a name="inspect"></a>檢查

[**檢查**] 索引標籤可讓您查看要轉換之資料流程的中繼資料。 您可以看到資料行計數、變更的資料行、加入的資料行、資料類型、資料行順序和資料行參考。 [**檢查**] 是您的中繼資料的唯讀視圖。 您不需要啟用 [偵測] 模式，即可在 [**檢查**] 窗格中查看中繼資料。

![檢查](media/data-flow/inspect1.png "檢查")

當您透過轉換來變更資料的圖形時，您會在 [**檢查**] 窗格中看到中繼資料變更流程。 如果您的來源轉換中沒有已定義的架構，則中繼資料將不會顯示在 [**檢查**] 窗格中。 缺乏中繼資料在架構漂移案例中很常見。

#### <a name="data-preview"></a>資料預覽

如果開啟 [偵錯工具] 模式，[**資料預覽**] 索引標籤會提供您每個轉換之資料的互動式快照集。 如需詳細資訊，請參閱[在偵錯工具模式中的資料預覽](concepts-data-flow-debug-mode.md#data-preview)。

### <a name="top-bar"></a>頂端列

頂端列包含會影響整個資料流程的動作，例如儲存和驗證。 您也可以使用 [**顯示圖形**] 和 [**隱藏圖形]** 按鈕，在圖表和設定模式之間切換。

![隱藏圖形](media/data-flow/hideg.png "隱藏圖形")

如果您隱藏圖形，可以透過 [**上一個**] 和 **[下一步**] 按鈕，橫向流覽您的轉換節點。

![上一個和下一個按鈕](media/data-flow/showhide.png "上一個和下一個按鈕")

## <a name="next-steps"></a>後續步驟

* 瞭解如何建立[來源轉換](data-flow-source.md)。
* 瞭解如何在「偵錯工具」[模式](concepts-data-flow-debug-mode.md)中建立資料流程。
