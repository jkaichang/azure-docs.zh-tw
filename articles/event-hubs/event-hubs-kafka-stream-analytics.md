---
title: Azure 事件中樞 - 處理 Apache Kafka 事件
description: 教學課程：本文說明如何使用 Azure 串流分析，處理透過事件中樞擷取的 Kafka 事件。
ms.topic: tutorial
ms.date: 06/23/2020
ms.openlocfilehash: 6dfca5b476dff1ee3367b5365c15869c8cff819b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074911"
---
# <a name="tutorial-process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>教學課程：使用串流分析處理適用於事件中樞的 Apache Kafka 
本文說明如何將資料串流至事件中樞，以及使用 Azure 串流分析處理該資料。 本文將逐步引導您完成下列步驟： 

1. 建立事件中樞命名空間。
2. 建立 Kafka 用戶端，以將訊息傳送到事件中樞。
3. 建立 Stream Analytics 作業，以將事件中樞的資料複製到 Azure blob 儲存體。 

當您使用事件中樞所公開的 Kafka 端點時，您不需要變更您的通訊協定用戶端或執行自己的叢集。 Azure 事件中樞支援 [Apache Kafka 1.0 版](https://kafka.apache.org/10/documentation.html)。 和更新版本。 


## <a name="prerequisites"></a>Prerequisites

若要完成本快速入門，請確定您具備下列必要條件︰

* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)。
* [下載](https://maven.apache.org/download.cgi)及[安裝](https://maven.apache.org/install.html) Maven 二進位封存檔。
* [Git](https://www.git-scm.com/)
* **Azure 儲存體帳戶**。 如果您沒有 Azure 儲存體帳戶，請先[建立一個](../storage/common/storage-account-create.md)，再繼續執行。 本逐步解說中的作業作業會在 Azure blob 儲存體中儲存輸出資料。 


## <a name="create-an-event-hubs-namespace"></a>建立事件中樞命名空間
當您建立**標準**層事件中樞命名空間時，命名空間的 Kafka 端點會自動啟用。 您可以將事件從使用 Kafka 通訊協定的應用程式串流到標準層事件中樞。 遵循[使用 Azure 入口網站建立事件中樞](event-hubs-create.md)中的逐步指示來建立**標準**層事件中樞命名空間。 

> [!NOTE]
> Kafka 的事件中樞只能在**標準**和**專用**層使用。 **基本**層不支援在事件中樞上使用 Kafka。

## <a name="send-messages-with-kafka-in-event-hubs"></a>使用事件中樞內的 Kafka 傳送訊息

1. 將[適用於 Kafka 的 Azure 事件中樞存放庫](https://github.com/Azure/azure-event-hubs-for-kafka)複製到電腦。
2. 瀏覽到 `azure-event-hubs-for-kafka/quickstart/java/producer` 資料夾。 
4. 在 `src/main/resources/producer.config` 中更新產生器的組態詳細資料。 指定**事件中樞命名空間**的**名稱**和**連接字串**。 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. 瀏覽至 `azure-event-hubs-for-kafka/quickstart/java/producer/src/main/java/`，然後在您選擇的編輯器中開啟 **TestDataReporter.java** 檔案。 
6. 註解化下列程式碼行：

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. 新增下列程式碼行，代替加上註解的程式碼： 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    此程式碼會以 **JSON** 格式傳送事件資料。 當您設定串流分析作業的輸入時，您可指定 JSON 作為輸入資料的格式。 
7. **執行產生器**並串流至事件中樞。 在 Windows 電腦上，使用 **Node.js 命令提示字元**時，先切換至 `azure-event-hubs-for-kafka/quickstart/java/producer` 資料夾，再執行這些命令。 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>確認事件中樞接收資料

1. 選取 [實體]  之下的 [事件中樞]  。 確認您看到名為 **test** 的事件中樞。 

    ![事件中樞 - test](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. 確認您看到訊息傳入事件中樞。 

    ![事件中樞 - 訊息](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>使用串流分析作業處理事件資料
在這一節中，您會建立 Azure 串流分析作業。 Kafka 用戶端會將事件傳送到事件中樞。 您會建立串流分析作業，以將事件資料當作輸入並將它輸出至 Azure blob 儲存體。 如果您沒有 **Azure 儲存體帳戶**，請[建立一個](../storage/common/storage-account-create.md)。

串流分析作業中的查詢會通過資料，而不執行任何分析。 您可以建立可轉換輸入資料的查詢，以產生不同格式或有所見解的輸出資料。  

### <a name="create-a-stream-analytics-job"></a>建立串流分析作業 

1. 在 [Azure 入口網站](https://portal.azure.com)中選取 [+ 建立資源]  。
2. 在 **Azure Marketplace** 功能表中選取 [分析]  ，然後選取 [串流分析作業]  。 
3. 在 [新增串流分析]  頁面上，執行下列動作： 
    1. 輸入作業的**名稱**。 
    2. 選取您的 **訂用帳戶**。
    3. 針對 [資源群組]  選取 [建立新的]  ，然後輸入名稱。 您也可以 [使用現有的]  資源群組。 
    4. 選取作業的 [位置]  。
    5. 選取 [建立]  來建立作業。 

        ![新增串流分析作業](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>設定作業輸入

1. 在通知訊息中，選取 [前往資源]  以查看 [串流分析作業]  頁面。 
2. 在功能表的 [作業拓撲]  區段中選取 [輸入]  。
3. 選取 [新增資料流輸入]  並選取 [事件中樞]  。 

    ![新增事件中樞作為輸入](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. 在 [事件中樞輸入]  設定頁面上，執行下列動作： 

    1. 指定輸入的**別名**。 
    2. 選取您的 **Azure 訂用帳戶**。
    3. 選取您稍早建立的**事件中樞命名空間**。 
    4. 針對**事件中樞**選取 **test**。 
    5. 選取 [儲存]  。 

        ![事件中樞輸入組態](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>設定作業輸出 

1. 在功能表的 [作業拓撲]  區段中選取 [輸出]  。 
2. 選取工具列上的 [+ 新增]  ，然後選取 [Blob 儲存體]  。
3. 在 Blob 儲存體輸出設定頁面上，執行下列動作： 
    1. 指定輸出的**別名**。 
    2. 選取您的 Azure **訂用帳戶**。 
    3. 選取您的 **Azure 儲存體帳戶**。 
    4. 輸入**容器名稱**，該容器會儲存串流分析查詢的輸出資料。
    5. 選取 [儲存]  。

        ![Blob 儲存體輸出組態](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>定義查詢
在串流分析作業安裝程式讀取傳入資料流之後，下一個步驟是建立轉換以即時分析資料。 您可使用[串流分析查詢語言](/stream-analytics-query/stream-analytics-query-language-reference)來定義轉換查詢。 在本逐步解說中，您會定義可通過資料的查詢，而不需執行任何轉換。

1. 選取 [查詢]  。
2. 在查詢視窗中，以您稍早建立的輸出別名取代 `[YourOutputAlias]`。
3. 以您稍早建立的輸入別名取代 `[YourInputAlias]`。 
4. 在工具列上選取 [儲存]  。 

    ![查詢](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>執行串流分析作業

1. 選取左側功能表上的 [概觀]  。 
2. 選取 [開始]  。 

    ![開始功能表](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. 在 [啟動作業]  頁面中，選取 [啟動]  。 

    ![啟動作業頁面](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. 等到作業的狀態從 [啟動中]  變成 [執行中]  為止。 

    ![工作狀態 - 執行中](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>測試案例
1. 再次執行 **Kafka 產生器**，將事件傳送至事件中樞。 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. 確認您看到 **Azure blob 儲存體**中產生了**輸出資料**。 您會在容器中看到包含 100 個資料列的 JSON 檔案，如下列範例資料列所示： 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Azure 串流分析作業收到來自事件中樞的輸入資料，並將它儲存在此案例中的 Azure blob 儲存體。 



## <a name="next-steps"></a>後續步驟
在本文中，您已了解如何串流至事件中樞，而不需要變更通訊協定用戶端或執行您自己的叢集。 若要深入了解適用於 Apache Kafka 的事件中樞，請參閱 [適用於 Azure 事件中樞的 Apache Kafka 開發人員指南](apache-kafka-developer-guide.md)。 
