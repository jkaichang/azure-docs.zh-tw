---
title: 來自 Apache Spark 應用程式的 RequestBodyTooLarge 錯誤-Azure HDInsight
description: NativeAzureFileSystem ...RequestBodyTooLarge 會出現在 Azure HDInsight 的 Apache Spark 串流應用程式的記錄中
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 777d06670238a7625d190c92f78a55cd4794d226
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "75894391"
---
# <a name="nativeazurefilesystemrequestbodytoolarge-appear-in-apache-spark-streaming-app-log-in-hdinsight"></a>"NativeAzureFileSystem...RequestBodyTooLarge "會出現在 HDInsight 的 Apache Spark 串流應用程式記錄中

本文說明在 Azure HDInsight 叢集中使用 Apache Spark 元件時，疑難排解步驟和可能的解決方案。

## <a name="issue"></a>問題

錯誤： `NativeAzureFileSystem ... RequestBodyTooLarge` 會出現在 Apache Spark 串流應用程式的驅動程式記錄檔中。

## <a name="cause"></a>原因

您的 Spark 事件記錄檔可能達到 WASB 的檔案長度限制。

在 Spark 2.3 中，每個 Spark 應用程式都會產生一個 Spark 事件記錄檔。 當應用程式正在執行時，Spark 串流應用程式的 Spark 事件記錄檔會繼續成長。 現在，WASB 上的檔案具有50000區塊限制，而預設區塊大小為 4 MB。 因此，在預設設定中，檔案大小上限為 195 GB。 不過，Azure 儲存體已將最大區塊大小增加到 100 MB，這可有效地將單一檔案限制設為 4.75 TB。 如需詳細資訊，請參閱 [Blob 儲存體的延展性和效能目標](../../storage/blobs/scalability-targets.md)。

## <a name="resolution"></a>解決方案

此錯誤有三個可用的解決方案：

* 將區塊大小增加至最多 100 MB。 在 Ambari UI 中，修改 HDFS 設定屬性 `fs.azure.write.request.size` （或在區段中建立 `Custom core-site` ）。 將屬性設定為較大的值，例如：33554432。 儲存已更新的設定並重新啟動受影響的元件。

* 定期停止並重新提交 spark 串流作業。

* 使用 HDFS 儲存 Spark 事件記錄檔。 使用 HDFS 進行儲存體可能會導致在叢集調整或 Azure 升級期間遺失 Spark 事件資料。

    1. 透過 `spark.eventlog.dir` `spark.history.fs.logDirectory` Ambari UI 進行變更：

        ```
        spark.eventlog.dir = hdfs://mycluster/hdp/spark2-events
        spark.history.fs.logDirectory = "hdfs://mycluster/hdp/spark2-events"
        ```

    1. 在 HDFS 上建立目錄：

        ```
        hadoop fs -mkdir -p hdfs://mycluster/hdp/spark2-events
        hadoop fs -chown -R spark:hadoop hdfs://mycluster/hdp
        hadoop fs -chmod -R 777 hdfs://mycluster/hdp/spark2-events
        hadoop fs -chmod -R o+t hdfs://mycluster/hdp/spark2-events
        ```

    1. 透過 Ambari UI 重新開機所有受影響的服務。

## <a name="next-steps"></a>後續步驟

如果您沒有看到您的問題，或無法解決您的問題，請瀏覽下列其中一個管道以取得更多支援：

* 透過 [Azure 社群支援](https://azure.microsoft.com/support/community/)獲得由 Azure 專家所提供的解答。

* 與 [@AzureSupport](https://twitter.com/azuresupport) 聯繫 - 專為改善客戶體驗而設的官方 Microsoft Azure 帳戶，協助 Azure 社群連接至適當的資源，例如解答、支援及專家等。

* 如果需要更多協助，您可在 [Azure 入口網站](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/) 提交支援要求。 在功能表列選取 [支援] 或開啟 [說明 + 支援] 中心。 如需詳細資訊，請參閱[如何建立 Azure 支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) (機器翻譯)。 您可透過 Microsoft Azure 訂用帳戶來存取訂用帳戶管理和帳單支援，並透過其中一項 [Azure 支援方案](https://azure.microsoft.com/support/plans/)以取得技術支援。
