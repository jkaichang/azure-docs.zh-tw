---
title: 為 Azure Cosmos DB MongoDB API 的資料庫和集合建立資源鎖定
description: 為 Azure Cosmos DB MongoDB API 的資料庫和集合建立資源鎖定
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 06/03/2020
ms.openlocfilehash: 3a8e1f31419d4ab14283418fbeb6391ae7167e5b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87048995"
---
# <a name="create-a-resource-lock-for-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>使用 Azure CLI 為 Azure Cosmos DB 的 MongoDB API 建立資源鎖定

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

如果您選擇在本機安裝和使用 CLI，本主題會要求您執行 Azure CLI 2.6.0 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli)。

> [!IMPORTANT]
> 除非先鎖定 Cosmos DB 帳戶並啟用 `disableKeyBasedMetadataWriteAccess` 屬性，否則資源鎖定不適用於使用任何 MongoDB SDK、Mongoshell、任何工具或 Azure 入口網站進行連線之使用者所做的變更。 若要深入了解如何啟用此屬性，請參閱[防止來自 SDK 的變更](../../../role-based-access-control.md#prevent-sdk-changes) (機器翻譯)。

## <a name="sample-script"></a>範例指令碼

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/lock.sh "Create a resource lock for an Azure Cosmos DB MongoDB API database and collection.")]

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az lock create](/cli/azure/lock#az-lock-create) | 建立鎖定。 |
| [az lock list](/cli/azure/lock#az-lock-list) | 列出鎖定資訊。 |
| [az lock show](/cli/azure/lock#az-lock-show) | 顯示鎖定的屬性。 |
| [az lock delete](/cli/azure/lock#az-lock-delete) | 刪除鎖定。 |

## <a name="next-steps"></a>後續步驟

-[鎖定資源以防止非預期的變更](../../../../azure-resource-manager/management/lock-resources.md)

-[Azure Cosmos DB CLI 文件](/cli/azure/cosmosdb)。

-[Azure Cosmos DB CLI GitHub 存放庫](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)。
