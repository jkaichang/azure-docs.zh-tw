---
title: 快速入門：產生縮圖 - REST、Node.js
titleSuffix: Azure Cognitive Services
description: 在本快速入門中，您將搭配使用電腦視覺 API 與 Node.js，從影像產生縮圖。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: c28b3e648dbb451535c182b28b672e670d749629
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2020
ms.locfileid: "84985952"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-nodejs"></a>快速入門：使用電腦視覺 REST API 和 Node.js 產生縮圖

在此快速入門中，您會使用電腦視覺 REST API 從影像產生縮圖。 您可以使用[取得縮圖](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)方法產生影像的縮圖。 您可指定高度和寬度，可以與輸入影像的外觀比例不同。 「電腦視覺」會使用智慧型裁剪，以智慧方式識別關注區域，並根據該區域產生裁剪座標。

## <a name="prerequisites"></a>必要條件

* Azure 訂用帳戶 - [建立免費帳戶](https://azure.microsoft.com/free/cognitive-services/)
* [Node.js](https://nodejs.org) 4.x 或更新版本 
* [npm](https://www.npmjs.com/) 
* 擁有 Azure 訂用帳戶之後，在 Azure 入口網站中<a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="建立電腦視覺資源"  target="_blank">建立電腦視覺資源<span class="docon docon-navigate-external x-hidden-focus"></span></a>，以取得您的金鑰和端點。 在其部署後，按一下 [前往資源]。
    * 您需要來自所建立資源的金鑰和端點，以將應用程式連線至 電腦視覺服務。 您稍後會在快速入門中將金鑰和端點貼到下列程式碼中。
    * 您可以使用免費定價層 (`F0`) 來試用服務，之後可升級至付費層以用於實際執行環境。
* 為金鑰和端點 URL (分別名為 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`)，[建立環境變數](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)。

## <a name="create-and-run-the-sample"></a>建立並執行範例

若要建立並執行範例，請執行下列步驟：

1. 安裝 npm [`request`](https://www.npmjs.com/package/request) 套件。
   1. 以系統管理員身分開啟 [命令提示字元] 視窗。
   1. 執行以下命令：

      ```console
      npm install request
      ```

   1. 成功安裝套件之後，關閉命令提示字元視窗。

1. 將下列程式碼複製到文字編輯器。
1. (選擇性) 將 `imageUrl` 的值取代為您要分析之不同影像的 URL。
1. 將程式碼儲存為副檔名為 `.js` 的檔案。 例如： `get-thumbnail.js` 。
1. 開啟 [命令提示字元] 視窗。
1. 出現提示時，使用 `node` 命令來執行該檔案。 例如： `node get-thumbnail.js` 。

```javascript
'use strict';

const fs = require('fs');
const request = require('request').defaults({ encoding: null });

let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']

var uriBase = endpoint + 'vision/v3.0/generateThumbnail';

const imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
};

// Construct the request
const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
}

// Post the request and get the response (an image stream)
request.post(options, (error, response, body) => {
    // Write the stream to file
    var buf = Buffer.from(body, 'base64');
    fs.writeFile('thumbnail.png', buf, function (err) {
        if (err) throw err;
    });

    console.log('Image saved')
});
```

## <a name="examine-the-response"></a>檢查回應

縮圖影像的快顯隨即顯示。
成功的回應會以二進位資料的形式傳回，代表縮圖的影像資料。 如果要求失敗，則回應會顯示在主控台視窗中。 如果要求失敗，回應會包含錯誤碼和訊息以協助判斷問題所在。

## <a name="next-steps"></a>後續步驟

接下來，探索用來分析影像、偵測名人與地標、建立縮圖，以及擷取印刷與手寫文字的電腦視覺 API。

> [!div class="nextstepaction"]
> [探索電腦視覺 API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
