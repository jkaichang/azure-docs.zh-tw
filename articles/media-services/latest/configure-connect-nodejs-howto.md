---
title: 連接到 Azure 媒體服務 v3 API-Node.js
description: 本文示範如何使用 Node.js 連接到媒體服務 v3 API。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.custom: devx-track-javascript
ms.openlocfilehash: 8e54fec584f8961dfc44a7c93f95772ea03e1259
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87424421"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>連接到媒體服務 v3 API-Node.js

本文說明如何使用服務主體登入方法來連線到 Azure 媒體服務 v3 node.js SDK。

## <a name="prerequisites"></a>必要條件

- 安裝 [Node.js](https://nodejs.org/en/download/)。
- [建立媒體服務帳戶](./create-account-howto.md)。 請務必記住資源組名和媒體服務帳戶名稱。

> [!IMPORTANT]
> 檢查[命名慣例](media-services-apis-overview.md#naming-conventions)。

## <a name="create-packagejson"></a>建立 package.js

1. 使用您慣用的編輯器，在檔案上建立 package.js。
1. 開啟檔案，並貼上下列程式碼：

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^4.1.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

應指定下列套件：

|Package|描述|
|---|---|
|`azure-arm-mediaservices`|Azure 媒體服務 SDK。 <br/>若要確定您使用的是最新的 Azure 媒體服務套件，請核取 [ [NPM 安裝] [Azure-arm-windowsazure.mediaservices.extensions](https://www.npmjs.com/package/azure-arm-mediaservices/)]。|
|`azure-storage`|儲存體 SDK。 將檔案上傳到資產時使用。|
|`ms-rest-azure`| 用來登入。|

您可以執行下列命令，以確定您使用的是最新的套件：

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>連接到 Node.js 用戶端

1. 使用您慣用的編輯器建立 .js 檔案。
1. 開啟該檔案，並貼上下列程式碼。
1. 將 "endpoint config" 區段中的值設定為您從[存取 api](./access-api-howto.md)取得的值。

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>執行應用程式

開啟命令提示字元。 流覽至範例的目錄，然後執行下列命令：

```
npm install 
node index.js
```

## <a name="see-also"></a>另請參閱

- [媒體服務概念](concepts-overview.md)
- [NPM 安裝 azure-arm-mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>後續步驟

探索媒體服務 [Node.js 參考](/javascript/api/overview/azure/mediaservices/management)文件，並查看[範例](https://github.com/Azure-Samples/media-services-v3-node-tutorials)示範如何搭配使用媒體服務 API 與 Node.js。
