---
title: 雙類別決策樹系：模組參考
titleSuffix: Azure Machine Learning
description: 瞭解如何在 Azure Machine Learning 中使用 [雙類別決策樹系] 模組，根據決策樹系演算法建立機器學習模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: c98935781699510d84247f80367d5c57cb388f6b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "82137632"
---
# <a name="two-class-decision-forest-module"></a>雙類別決策樹系模組

本文說明 Azure Machine Learning 設計工具 (預覽) 中的模組。

使用此模組來根據決策樹系演算法建立機器學習模型。  

決策樹系是快速且受監督的集團模型。 如果您想要預測最多兩個結果的目標，此模組是不錯的選擇。 

## <a name="understanding-decision-forests"></a>瞭解決策樹系

此決策樹系演算法是適用于分類工作的集團學習方法。 集團方法是以一般原則為基礎，而不是依賴單一模型，您可以建立多個相關模型並以某種方式加以結合，以取得更好的結果和更一般化的模型。 一般而言，集團模型比單一決策樹的涵蓋範圍更廣、精確度更高。 

有許多方法可以建立個別模型，並將它們結合在集團中。 這個特定的決策樹系的執行方式是建立多個決策樹，然後針對最受歡迎的輸出類別**投票**。 投票是在集團模型中產生結果的其中一個較佳的方法。 

+ 許多個別的分類樹狀結構都是使用整個資料集，但不同（通常是隨機化）起點來建立。 這不同于隨機樹系方法，因此個別決策樹可能只會使用部分資料或功能的隨機部分。
+ 決策樹系樹狀目錄中的每個樹狀結構都會輸出標籤的非正規化頻率長條圖。 
+ 匯總程式會加總這些長條圖並正規化結果，以取得每個標籤的「機率」。 
+ 具有高預測信賴度的樹狀結構在集團的最終決策中會有較高的權數。

一般決策樹有許多分類工作的優點：
  
- 他們可以捕捉非線性決策界限。
- 您可以對許多資料進行定型和預測，因為它們在計算和記憶體使用量上是有效率的。
- 功能選取已整合在訓練和分類流程中。  
- 樹狀結構可以容納雜訊資料和許多功能。  
- 它們是非參數化模型，這表示它們可以使用各種散發來處理資料。 

不過，簡單的決策樹可以過度學習資料，而且比起樹狀整體的可歸納性更低。

如需詳細資訊，請參閱[判定](https://go.microsoft.com/fwlink/?LinkId=403677)樹系。  

## <a name="how-to-configure"></a>如何設定
  
1.  在 Azure Machine Learning 中，將 [**雙類別決策樹**系] 模組新增至您的管線，然後開啟模組的 [**屬性**] 窗格。 

    您可以在 [ **Machine Learning**] 下找到此模組。 依序展開 [**初始化**] 和 [**分類**]。  
  
2.  針對 [重新**取樣方法**]，選擇用來建立個別樹狀結構的方法。  您可以選擇 [**封袋**]**或 [** 複寫]。  
  
    -   **封袋**：封袋也稱為「*啟動*程式匯總」。 在此方法中，每個樹狀結構都會在新的範例中成長，而這是透過使用取代隨機取樣原始資料集來建立的，直到您擁有資料集的原始大小為止。  
  
         模型的輸出會藉由*投票*來結合，這是一種匯總形式。 分類決策樹系中的每個樹狀結構都會輸出標籤的非標準化頻率長條圖。 匯總是要加總這些長條圖並加以正規化，以取得每個標籤的「機率」。 如此一來，具有高預測信賴度的樹狀結構在集團的最終決策中會有較高的權數。  
  
         如需詳細資訊，請參閱適用于啟動程式匯總的維琪百科專案。  
  
    -   複寫 **：在**複寫中，每個樹狀結構都是以完全相同的輸入資料進行定型。 判斷每個樹狀節點所使用的分割述詞會維持隨機，而且樹狀結構會不同。   
  
3.  藉由設定 [**建立定型模式]** 選項，指定您要如何訓練模型。  
  
    -   **單一參數**：如果您知道要如何設定模型，可以提供一組特定值做為引數。

    -   **參數範圍**：如果您不確定最佳參數，您可以使用[微調模型超參數](tune-model-hyperparameters.md)模組來尋找最佳參數。 您會提供某個範圍的值，而講師會逐一查看設定的多個組合，以判斷產生最佳結果的值組合。
  
4.  針對 [**決策樹數目**]，輸入可在集團中建立的決策樹數目上限。 藉由建立更多決策樹，您可能會獲得更好的涵蓋範圍，但是定型時間也會增加。  
  
    > [!NOTE]
    >  這個值也會控制視覺化定型模型時所顯示的樹狀結構數目。 如果您想要查看或列印單一樹狀結構，您可以將值設定為1。 不過，只能產生一個樹狀結構（具有初始參數集的樹狀結構），而且不會執行任何進一步的反覆運算。
  
5.  如需**決策樹的最大深度**，請輸入數位來限制任何決策樹的最大深度。 增加樹狀結構的深度可增加有效位數，但可能會有過度配適及定型時間增加的風險。
  
6.  針對 [**每個節點的隨機分割數目**]，輸入建立樹狀結構的每個節點時所要使用的分割數目。 「*分割*」（split）表示每個樹狀結構層級（節點）的功能會隨機分割。
  
7.  針對 [**每個分葉節點的最小樣本數**]，表示在樹狀結構中建立任何終端節點（分葉）所需的最小案例數目。
  
     藉由增加此值，您會增加建立新規則的臨界值。 例如，若預設值是 1，即使單一案例可能會造成新規則的建立。 如果您將此值增加為 5，則定型資料必須至少包含五個符合相同條件的案例。  
  
8.  選取 [**允許分類功能的未知值**] 選項，在定型或驗證集中建立未知值的群組。 已知值的模型可能較不精確，但它可以為新的（未知）值提供更佳的預測。 

     如果您取消選取此選項，模型只能接受定型資料中包含的值。
  
9. 附加已加上標籤的資料集，並將模型定型：

    + 如果您將 [**建立定型模式**] 設定為 [**單一參數**]，請連接已標記的資料集和 [[訓練模型](train-model.md)] 模組。  
  
    + 如果您將 [**建立定型模式**] 設定為 [**參數範圍**]，請連接已加上標籤的資料集，並使用[微調模型超參數](tune-model-hyperparameters.md)來定型模型。  
  
    > [!NOTE]
    > 
    > 如果您將參數範圍傳遞至[定型模型](train-model.md)，它只會使用單一參數清單中的預設值。  
    > 
    > 如果您將一組參數值傳遞至[微調模型超參數](tune-model-hyperparameters.md)模組，當它預期每個參數的設定範圍時，就會忽略這些值，並使用學習模組的預設值。  
    > 
    > 如果您選取 [**參數範圍**] 選項，並輸入任何參數的單一值，則會在整個清除中使用您指定的單一值，即使其他參數在某個範圍的值之間變更也是如此。  
    
## <a name="results"></a>結果

定型完成後：

+ 若要儲存定型模型的快照集，請選取 [**訓練模型**] 模組右面板中的 [**輸出**] 索引標籤。 選取 [**註冊資料集**] 圖示，將模型儲存為可重複使用的模組。

+ 若要使用模型進行評分，請將 [**評分模型**] 模組新增至管線。

## <a name="next-steps"></a>後續步驟

請參閱 Azure Machine Learning 的[可用模組集](module-reference.md)。 