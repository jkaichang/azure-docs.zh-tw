---
title: Include 檔案
description: 包含檔案
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 95c553d26a3e79b53106b933c629c5884c3e004c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84466828"
---
| 資源 | 目標 | 固定限制 |
|----------|--------------|------------|
| 每個區域的儲存體同步服務數目 | 100儲存體同步服務 | 是 |
| 每個儲存體同步服務的同步群組 | 200同步群組 | 是 |
| 每個儲存體同步服務的已註冊伺服器 | 99 部伺服器 | 是 |
| 每個同步群組的雲端端點 | 1 個雲端端點 | 是 |
| 每個同步群組的伺服器端點 | 50 個伺服器端點 | No |
| 每部伺服器的伺服器端點 | 30 個伺服器端點 | 是 |
| 每個同步群組的檔案系統物件 (目錄和檔案) | 100000000物件 | No |
| 目錄中的檔案系統物件 (目錄和檔案) 數目上限 | 5000000物件 | 是 |
| 物件 (目錄和檔案) 安全性描述元大小上限 | 64 KiB | 是 |
| 檔案大小 | 100 GiB | No |
| 要分層之檔案的檔案大小下限 | V9：以檔案系統叢集大小（雙重檔案系統叢集大小）為基礎。 例如，如果檔案系統叢集大小為 4 kb，則檔案大小下限會是 8 kb。<br> V8 和更舊版本： 64 KiB  | 是 |

> [!Note]  
> Azure 檔案同步端點可以相應增加至 Azure 檔案共用的大小。 如果達到 Azure 檔案共用大小限制，同步處理將無法運作。
