---
title: 在 IoT Edge 裝置上部署即時影片分析-Azure
description: 本文列出的步驟將協助您在 IoT Edge 裝置上部署即時影片分析。 例如，如果您有本機 Linux 電腦的存取權，以及（或）先前已建立 Azure 媒體服務帳戶，您就會這麼做。
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: ea7a1026f42cd3d8745559bc195a89b7fbcb69a0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074462"
---
# <a name="deploy-live-video-analytics-on-an-iot-edge-device"></a>在 IoT Edge 裝置上部署即時影片分析

本文列出的步驟將協助您在 IoT Edge 裝置上部署即時影片分析。 例如，如果您有本機 Linux 電腦的存取權，以及（或）先前已建立 Azure 媒體服務帳戶，您就會這麼做。


## <a name="prerequisites"></a>先決條件

* 符合適用于即時影片分析之 HW/SW 限制的 Linux 機器
* 您擁有擁有者[許可權](../../role-based-access-control/built-in-roles.md#owner)的 Azure 訂用帳戶
* [建立和設定 IoT 中樞](../../iot-hub/iot-hub-create-through-portal.md)
* [註冊 IoT Edge 裝置](../../iot-edge/how-to-register-device.md)
* [在以 Debian 為基礎的 Linux 系統上安裝 Azure IoT Edge 執行階段](../../iot-edge/how-to-install-iot-edge-linux.md)
* [建立 Azure 媒體服務帳戶](../latest/create-account-howto.md)
    * 使用下列其中一個區域：美國東部2、美國中部、美國中北部、日本東部、美國西部2、美國中西部、加拿大東部、英國南部、法國中部、法國南部、瑞士北部、瑞士西部和日本西部。
    * 建議使用一般用途 v2 （GPv2）儲存體帳戶

## <a name="configuring-azure-resources-for-using-live-video-analytics"></a>使用即時影片分析來設定 Azure 資源

### <a name="create-custom-azure-resource-manager-role"></a>建立自訂 Azure Resource Manager 角色

請參閱[建立自訂 Azure Resource Manager 角色](create-custom-azure-resource-manager-role-how-to.md)，並將它指派給服務主體，以供即時影片分析使用。

### <a name="set-up-a-premium-streaming-endpoint"></a>設定 premium 串流端點

如果您想要使用即時影片分析，將影片持續記錄到雲端，並在播放前先使用[查詢 api](playback-recordings-how-to.md#query-api) ，建議您將媒體服務更新為使用[premium 串流端點](../latest/streaming-endpoint-concept.md#types)。  

這是選擇性步驟。 您可以使用此 Azure CLI 命令來執行此動作：

```azure-cli
az ams streaming-endpoint scale --resource-group $RESOURCE_GROUP --account-name $AMS_ACCOUNT -n default --scale-units 1
```

您可以使用此命令來啟動串流端點 

> [!IMPORTANT]
> 您的訂用帳戶將于此時開始計費。

```azure-cli
az ams streaming-endpoint start --resource-group $RESOURCE_GROUP --account-name $AMS_ACCOUNT -n default --no-wait
```

請遵循這篇文章中的步驟，以取得存取媒體服務 Api 的認證：[存取媒體服務 api](../latest/access-api-howto.md#use-the-azure-portal)。

## <a name="create-and-use-local-user-account-for-deployment"></a>建立和使用本機使用者帳戶進行部署
若要在 IoT Edge 模組上執行即時影片分析，請建立一個具有盡可能少許可權的本機使用者帳戶。 例如，在您的 Linux 電腦上執行下列命令：

```
sudo groupadd -g 1010 localuser
sudo adduser --home /home/edgeuser --uid 1010 -gid 1010 edgeuser
```

## <a name="granting-permissions-to-device-storage"></a>授與許可權給裝置存放裝置

現在您已建立本機使用者帳戶， 

* 您將需要本機資料夾來儲存應用程式設定資料。 建立資料夾，並使用下列命令，將許可權授與 localuser 帳戶寫入該資料夾：

```
sudo mkdir /var/lib/azuremediaservices
sudo chown -R edgeuser /var/lib/azuremediaservices
```

* 您也需要一個可將影片[記錄到本機](event-based-video-recording-concept.md#video-recording-based-on-events-from-other-sources)檔案的資料夾。 使用下列命令來建立相同的本機資料夾：

```
sudo mkdir /var/media
sudo chown -R edgeuser /var/media
```

## <a name="deploy-live-video-analytics-edge-module"></a>部署即時影片分析 Edge 模組

<!-- (To JuliaKo: this is similar to https://docs.microsoft.com/azure/iot-edge/how-to-deploy-blob)-->
IoT Edge 上的即時影片分析會公開模組對應項設定[架構](module-twin-configuration-schema.md)中記載的模組對應項屬性。 

### <a name="deploy-using-the-azure-portal"></a>使用 Azure 入口網站部署

Azure 入口網站會引導您建立部署資訊清單，並將部署推送至 IoT Edge 裝置。
選取您的裝置

1. 登入 [Azure 入口網站](https://ms.portal.azure.com/)，然後瀏覽至 IoT 中樞。
1. 選取功能表中的 [IoT Edge]****。
1. 按一下裝置清單中目標裝置的識別碼。
1. 選取 [**設定模組**]。

#### <a name="configure-a-deployment-manifest"></a>設定部署資訊清單

部署資訊清單為 JSON 文件，說明應部署的模組、資料如何在模組之間流動，以及想要的模組對應項需要的屬性。 Azure 入口網站有一個可引導您建立部署資訊清單的 wizard。 其中有三個步驟會組織成索引標籤：**模組**、**路由**，以及 [**審核] + [建立**]。

#### <a name="add-modules"></a>新增模組

1. 在頁面的 [ **IoT Edge 模組**] 區段中，按一下 [**新增**] 下拉式清單，然後選取 [ **IoT Edge 模組**] 以顯示 [**新增 IoT Edge 模組**] 頁面。
1. 在 [**模組設定**] 索引標籤上，提供模組的名稱，然後指定容器映射 URI：   
    範例：
    
    * **IoT Edge 模組名稱**： lvaEdge
    * **映射 URI**： mcr.microsoft.com/media/live-video-analytics:1。0    
    
    ![新增](./media/deploy-iot-edge-device/add.png)
    
    > [!TIP]
    > 在 [**模組設定**]、[**容器建立選項**] 和 [模組對應項**設定**] 索引標籤上指定值之前，請不要選取 [**新增**]，如此程式中所述。
    
    > [!IMPORTANT]
    > 當您對模組進行呼叫時，Azure IoT Edge 會區分大小寫。 記下您用來做為模組名稱的確切字串。

1. 開啟 [**環境變數**] 索引標籤。
   
   複製下列 JSON 並貼到方塊中，以提供用來儲存應用程式資料和影片輸出的使用者識別碼和群組識別碼。
    ```   
   {
        "LOCAL_USER_ID": 
        {
            "value": "1010"
        },
        "LOCAL_GROUP_ID": {
            "value": "1010"
        }
    }
     ``` 

1. 開啟 [**容器建立選項**] 索引標籤。

    ![容器建立選項](./media/deploy-iot-edge-device/container-create-options.png)
 
    複製下列 JSON 並貼到方塊中，以限制模組所產生的記錄檔大小。
    
    ```    
    {
        "HostConfig": {
            "LogConfig": {
                "Type": "",
                "Config": {
                    "max-size": "10m",
                    "max-file": "10"
                }
            },
            "Binds": [
               "/var/lib/azuremediaservices:/var/lib/azuremediaservices",
               "/var/media:/var/media"
            ]
        }
    }
    ````
   
   JSON 中的「系結」區段有2個專案：
   1. "/var/lib/azuremediaservices：/var/lib/azuremediaservices"：這是用來系結容器中的持續性應用程式設定資料，並將它儲存在 edge 裝置上。
   1. "/var/media：/var/media"：這會系結 edge 裝置與容器之間的媒體檔案夾。 當您執行支援在 edge 裝置上儲存影片剪輯的 media graph 拓朴時，會使用此來儲存影片錄製。
   
1. 在 [**模組**對應項設定] 索引標籤上，複製下列 JSON 並貼到方塊中。
 
    ![對應項設定](./media/deploy-iot-edge-device/twin-settings.png)

    IoT Edge 上的即時影片分析需要一組強制對應項屬性，才能執行，如模組對應項設定[架構](module-twin-configuration-schema.md)中所列。  

    您需要在 [模組對應項設定] 編輯方塊中輸入的 JSON 如下所示：    
    ```
    {
        "applicationDataDirectory": "/var/lib/azuremediaservices",
        "azureMediaServicesArmId": "/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/microsoft.media/mediaservices/{AMS-account-name}",
        "aadTenantId": "{the-ID-of-your-tenant}",
        "aadServicePrincipalAppId": "{the-ID-of-the-service-principal-app-for-ams-account}",
        "aadServicePrincipalSecret": "{secret}"
    }
    ```
    這些是**必要**的屬性，而對於上述的 JSON，  
    * {subscriptionID}-這是您的 Azure 訂用帳戶識別碼
    * {resourceGroupName}-這是您的媒體服務帳戶所屬的資源群組
    * {AMS-帳戶-名稱}-這是您媒體服務帳戶的名稱
    
    若要取得其他值，請參閱[存取 AZURE 媒體服務 API](../latest/access-api-howto.md#use-the-azure-portal)。  
    * aadTenantId-這是您的租使用者識別碼，與上述連結中的 "AadTenantId" 相同。
    * aadServicePrincipalAppId-這是您媒體服務帳戶之服務主體的應用程式識別碼，與上述連結中的「AadClientId」相同。
    * aadServicePrincipalSecret-這是服務主體的密碼，與上述連結中的 "AadSecret" 相同。

    以下是一些其他**建議**的屬性，您可以將其新增至 JSON，並協助監視模組。 如需詳細資訊，請參閱[監視和記錄](monitoring-logging.md)：
    
    ```
    "diagnosticsEventsOutputName": "lvaEdgeDiagnostics",
    "OperationalEventsOutputName": "lvaEdgeOperational",
    "logLevel": "Information",
    "logCategories": "Application,Events"
    ```
    
    以下是您可以在 JSON 中新增的一些**選擇性**屬性：
    
    ```
    "aadEndpoint": "https://login.microsoftonline.com",
    "aadResourceId": "https://management.core.windows.net/",
    "armEndpoint": "https://management.azure.com/",
    "allowUnsecuredEndpoints": true
    ```
   [!Note]
   對應項屬性**allowUnsecuredEndpoints**會針對教學課程和快速入門的目的設定為 true。   
   在生產環境中執行時，您應該將此屬性設定為**false** 。 這可確保應用程式會封鎖所有不安全的端點，若要執行圖形拓撲，則需要有效的連接認證。  
   
    選取 [新增] 以新增模組對應項屬性。
1. 選取 **[下一步]： [路由]** 以繼續前往 [路由] 區段。
    指定路由。

保留預設路由，然後選取 **[下一步：查看 + 建立]** 繼續前往 [審核] 區段。

#### <a name="review-deployment"></a>檢閱部署

檢閱區段會顯示 JSON 部署資訊清單，該清單會根據您在前兩個區段中的選項而建立。 也有兩個宣告的模組未加入： $edgeAgent 和 $edgeHub。 這兩個模組組成 IoT Edge 執行階段，在每個部署中都是必要的預設值。

檢查您的部署資訊，然後選取 [建立]。

### <a name="verify-your-deployment"></a>驗證您的部署

建立部署之後，您會返回 IoT 中樞的 [IoT Edge] 頁面。

1.  選取您使用部署設定為目標的 IoT Edge 裝置，以開啟其詳細資料。
2.  在裝置詳細資料中，確認 Blob 儲存體模組列為 [指定於部署中]** 和 [由裝置回報]**。

模組在裝置上啟動然後向 IoT 中樞回報可能需要一點時間。 重新整理頁面來查看更新狀態。
狀態碼：200– OK 表示[IoT Edge 運行](../../iot-edge/iot-edge-runtime.md)時間狀況良好，且運作正常。

![狀態](./media/deploy-iot-edge-device/status.png)

#### <a name="invoke-a-direct-method"></a>叫用直接方法

接下來，讓我們藉由叫用直接方法來測試範例。 閱讀[IoT Edge 上即時影片分析的直接方法](direct-methods.md)，以瞭解我們的 lvaEdge 模組所提供的所有直接方法。

1. 按一下您建立的 edge 模組，將會帶您前往其 [設定] 頁面。  

    ![單元](./media/deploy-iot-edge-device/modules.png)
1. 按一下 [直接方法] 功能表選項。

    > [!NOTE] 
    > 您必須在 [連接字串] 區段中加入一個值，如您在目前的頁面上所見。 您不需要隱藏或變更 [**設定名稱**] 區段中的任何專案。 可以讓它成為公用。

    ![直接方法](./media/deploy-iot-edge-device/module-details.png)
1. 接下來，在 [方法名稱] 方塊中輸入 "GraphTopologyList"。
1. 接下來，在 [裝載] 方塊中複製並貼上下列 JSON 承載。
    
    ```
    {
        "@apiVersion" : "1.0"
    }
    ```
1. 按一下頁面頂端的 [叫用方法] 選項

    ![直接方法](./media/deploy-iot-edge-device/direct-method.png)
1. 您應該會在 [結果] 方塊中看到狀態200訊息

    ![狀態200訊息](./media/deploy-iot-edge-device/connection-timeout.png) 

## <a name="next-steps"></a>後續步驟

[快速入門：開始使用 - IoT Edge 上的 Live Video Analytics](get-started-detect-motion-emit-events-quickstart.md)
