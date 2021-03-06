---
title: Azure 時間序列深入解析 Gen2 中的 long 資料類型支援 |Microsoft Docs
description: Azure 時間序列深入解析 Gen2 中的 long 資料類型支援。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: dpalled
ms.openlocfilehash: 34cf770a8ac75c2516480ec3136e61da15f4e4ff
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446639"
---
# <a name="adding-support-for-long-data-type-in-azure-time-series-insights-gen2"></a>在 Azure 時間序列深入解析 Gen2 中新增 long 資料類型的支援

新增 long 資料類型的支援，只會影響我們在 Azure 時間序列深入解析 Gen2 環境中儲存和索引數值資料的方式。 如果您有 Gen1 環境，可以忽略這些變更。

從2020年6月29日或6月30日開始，視您的地區而定，您的資料將會以**Long**和**Double**編制索引。  如果您有任何關於這種變更的問題或疑慮，請透過 Azure 入口網站提交支援票證，並提及此通訊。

如果您受到下列任何一種情況的影響，請進行建議的變更：

- **案例 1**：您目前使用時間序列模型變數，並只傳送遙測資料中的整數資料類型。
- **案例 2**：您目前使用時間序列模型變數，並在您的遙測資料中傳送整數和非整數資料類型。
- **案例 3**：您可以使用類別變數，將整數值對應至分類。
- **案例 4**：您可以使用 JavaScript SDK 來建立自訂的前端應用程式。
- **案例 5**：您已接近暖存放區中的 1000-屬性名稱限制，並同時傳送整數和非整數資料。 在[Azure 入口網站](https://portal.azure.com/)中，可以將屬性計數視為度量。

如果有任何案例適用于您，請對您的模型進行變更。 以建議的變更，更新您變數定義中的時間序列運算式（TSX）。 更新兩者：

- Azure 時間序列深入解析 Gen2 explorer
- 使用我們的 Api 的任何自訂用戶端

根據您的 IoT 解決方案和條件約束，您可能無法看到傳送至 Azure 時間序列深入解析 Gen2 環境的資料。 如果您不確定您的資料是唯一的，或是整數和非整數，您有幾個選項：

- 您可以等候釋放該功能。 然後，在 explorer UI 中探索您的原始事件，以瞭解哪些屬性會儲存在兩個不同的資料行中。
- 您可以事先對所有數值標記進行建議的變更。
- 您可以暫時將事件的子集路由至儲存體，以進一步瞭解並探索您的架構。

若要儲存事件，請開啟 Azure 事件中樞的[事件捕捉](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)，或從您的 IoT 中樞[路由](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c#azure-storage)至 Azure Blob 儲存體。

您也可以透過[事件中樞瀏覽器](https://marketplace.visualstudio.com/items?itemName=Summer.azure-event-hub-explorer)或使用[事件處理器主機](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send#receive-events)來觀察資料。

如果您使用 IoT 中樞，請移至[從內建端點讀取裝置到雲端訊息](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin)，以瞭解如何存取內建端點。

> [!NOTE]
> 如果您未進行建議的變更，您可能會遇到中斷的情況。 例如，透過查詢 Api 或時間序列深入解析 explorer 存取的受影響時間序列深入解析變數會傳回**null** （也就是在 explorer 中未顯示任何資料）。

## <a name="recommended-changes"></a>建議變更

### <a name="case-1-using-time-series-model-variables-and-sending-only-integral-data-types-in-telemetry-data"></a>案例1：使用時間序列模型變數，並只傳送遙測資料中的整數資料類型

案例1的建議變更等同于案例2。 依照案例2的一節中的指示進行。

### <a name="case-2-using-time-series-model-variables-and-sending-both-integral-and-nonintegral-types-in-telemetry-data"></a>案例2：使用時間序列模型變數，並在遙測資料中傳送整數和非整數類型

如果您目前傳送整數遙測資料，您的資料將會分成兩個數據行：

- **propertyValue_double**
- **propertyValue_long**

您的整數資料會寫入**propertyValue_long**。 先前在**propertyValue_double**中內嵌（和未來的內嵌）數值資料不會複製到。

如果您想要針對**propertyValue**屬性查詢這兩個數據行中的資料，您必須在 TSX 中使用**聯合（）** 純量函數。 函數接受相同**資料類型**的引數，並傳回引數清單中的第一個非 null 值。 如需詳細資訊，請參閱[Azure 時間序列深入解析 Gen2 資料存取概念](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions)。

#### <a name="variable-definition-in-tsx---numeric"></a>TSX 中的變數定義-數值

*先前的變數定義：*

[![先前的變數定義](media/time-series-insights-long-data-type/var-def-previous.png)](media/time-series-insights-long-data-type/var-def-previous.png#lightbox)

*新增變數定義：*

[![新增變數定義](media/time-series-insights-long-data-type/var-def.png)](media/time-series-insights-long-data-type/var-def.png#lightbox)

您也可以使用**聯合（$Event toDouble （$event）** 做為自訂[時間序列運算式](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)。

#### <a name="inline-variable-definition-using-tsx-query-apis---numeric"></a>使用 TSX 查詢 Api 的內嵌變數定義-數值

*先前的變數定義：*

```JSON
"PropertyValueVariable": {

    "kind": "numeric",

    "value": {

        "tsx": "$event.propertyValue.Double"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

*新增變數定義：*

```JSON
"PropertyValueVariable ": {

    "kind": "numeric",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, toLong($event.propertyValue.Double))"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

您也可以使用**聯合（$Event toDouble （$event）** 做為自訂[時間序列運算式](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)。

> [!NOTE]
> 我們建議您在所有可能使用的位置更新這些變數。 這些位置包含時間序列模型、已儲存的查詢，以及 Power BI 的連接器查詢。

### <a name="case-3-using-categorical-variables-to-map-integer-values-to-categories"></a>案例3：使用類別變數將整數值對應至分類

如果您目前使用將整數值對應至分類的類別變數，您很可能會使用**toLong**函數，將**Double**類型的資料轉換成**Long**類型。 就像案例1和2，您需要合併**Double**和**Long** **DataType 資料**行。

#### <a name="variable-definition-in-time-series-explorer---categorical"></a>時間序列瀏覽器中的變數定義-類別

*先前的變數定義：*

[![先前的變數定義](media/time-series-insights-long-data-type/var-def-cat-previous.png)](media/time-series-insights-long-data-type/var-def-cat-previous.png#lightbox)

*新增變數定義：*

[![新增變數定義](media/time-series-insights-long-data-type/var-def-cat.png)](media/time-series-insights-long-data-type/var-def-cat.png#lightbox)

您也可以使用**聯合（$Event toDouble （$event）** 做為自訂[時間序列運算式](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)。

類別變數仍然需要值為整數類型。 在自訂[時間序列運算式](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)中，**聯合（）** 中所有引數的**資料**類型必須是**Long**類型。

#### <a name="inline-variable-definition-using-tsx-query-apis---categorical"></a>使用 TSX 查詢 Api 的內嵌變數定義-類別

*先前的變數定義：*

```JSON
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "tolong($event.propertyValue.Double)"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }
}
```

*新增變數定義：*

```JSON
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, tolong($event.propertyValue.Double))"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }
}
```

類別變數仍然需要值為整數類型。 在自訂[時間序列運算式](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)中，**聯合（）** 中所有引數的**資料**類型必須是**Long**類型。

> [!NOTE]
> 我們建議您在所有可能使用的位置更新這些變數。 這些位置包含時間序列模型、已儲存的查詢，以及 Power BI 的連接器查詢。

### <a name="case-4-using-the-javascript-sdk-to-build-a-custom-front-end-application"></a>案例4：使用 JavaScript SDK 來建立自訂前端應用程式

如果您受到案例1到3的影響，並建立自訂應用程式，您必須更新查詢以使用**聯合（）** 函數，如先前範例所示。

### <a name="case-5-nearing-warm-store-1000-property-limit"></a>案例5：接近暖存放區1000屬性限制

如果您是具有大量屬性的暖存放區使用者，並認為這項變更會將您的環境推送至1000暖存放區屬性名稱限制，請透過 Azure 入口網站提交支援票證，並提及這項通訊。

## <a name="next-steps"></a>後續步驟

- 請參閱[支援的資料類型](concepts-supported-data-types.md)完整清單。
