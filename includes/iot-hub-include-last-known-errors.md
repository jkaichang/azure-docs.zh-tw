---
title: 包含檔案
description: 包含檔案
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793289"
---
REST API 中的 [[取得端點健全狀況](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth)] 會提供端點的健康狀態，以及最後一個已知的錯誤，以識別端點狀況不良的原因。 下表列出最常見的錯誤。

|上次已知錯誤|描述/發生時機|可能的緩和措施|
|-----|-----|-----|
|暫時性|發生暫時性錯誤，IoT 中樞將會重試作業。|觀察路由[診斷記錄](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)。|
|InternalError|將訊息傳遞至端點時發生錯誤。|這是內部例外狀況，但也會觀察路由[診斷記錄](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes)。|
|未經授權|IoT 中樞未獲授權，無法將訊息傳送至指定的端點。|驗證端點的連接字串為最新狀態。 如果已變更，請考慮 IoT 中樞的更新。 如果端點使用受控識別，請檢查 IoT 中樞主體是否具有目標的必要許可權。|
|調整執行速度|將訊息寫入至端點時，IoT 中樞正在進行節流處理。|檢查受影響端點的節流限制。 視需要修改端點的設定以進行相應增加。|
|逾時|作業逾時。|重試作業。|
|找不到|目標資源不存在。|請確定目標資源存在。|
|找不到容器|儲存體容器不存在。|請確定儲存體容器存在。|
|已停用容器|儲存體容器已停用。|請確定已啟用儲存體容器。|
|MaxMessageSizeExceeded|訊息路由的訊息大小限制為 256 Kb。要路由傳送的訊息大小超過此限制。|使用較少的應用程式屬性或較少的訊息擴充，檢查是否可以減少訊息大小。|
|PartitioningAndDuplicateDetectionNotSupported|服務匯流排可能未啟用重複偵測。|停用服務匯流排的重複偵測，或考慮使用沒有重複偵測的實體。|
|SessionfulEntityNotSupported|服務匯流排可能未啟用會話。|從服務匯流排停用會話，或考慮使用沒有會話的實體。|
|NoMatchingSubscriptionsForMessage|在服務匯流排主題上沒有可寫入訊息的訂用帳戶。|建立要路由傳送 IoT 中樞訊息的訂用帳戶。|
|EndpointExternallyDisabled|端點不是處於使用中狀態，因此 IoT 中樞可以將訊息傳送給它。|啟用端點使其恢復為作用中狀態。|
|DeviceMaximumQueueDepthExceeded|已達到服務匯流排大小限制。|請考慮從目標事件中樞中移除訊息，以允許將新訊息內嵌到事件中樞中。|