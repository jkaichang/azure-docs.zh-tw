---
title: 延伸模組-超大規模資料庫 (Citus) -適用於 PostgreSQL 的 Azure 資料庫
description: '說明使用適用於 PostgreSQL 的 Azure 資料庫超大規模資料庫 (Citus 中的延伸模組，擴充資料庫功能的能力) '
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: de2579868ad72bdf4cf78c552e9553f289ecabd0
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259052"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql--hyperscale-citus"></a>適用於 PostgreSQL 的 Azure 資料庫中的于 postgresql 延伸模組–超大規模資料庫 (Citus) 

于 postgresql 提供使用延伸模組來擴充資料庫功能的功能。 擴充功能可在單一封裝中一併統合多個相關的 SQL 物件，其可使用單一命令從您的資料庫加以載入或移除。 載入資料庫之後，延伸模組的功能就像內建功能一樣。 如需于 postgresql 擴充功能的詳細資訊，請參閱 [將相關物件封裝成擴充](https://www.postgresql.org/docs/current/static/extend-extensions.html)功能。

## <a name="use-postgresql-extensions"></a>使用於 postgresql 擴充功能

您必須先在資料庫中安裝 PostgreSQL 擴充功能，然後才能使用它們。 若要安裝特定的擴充功能，請從 psql 工具執行 [CREATE extension](https://www.postgresql.org/docs/current/static/sql-createextension.html)   命令，以將封裝的物件載入至您的資料庫。

適用於 PostgreSQL 的 Azure 資料庫超大規模資料庫 (Citus) 目前支援金鑰延伸的子集，如這裡所列。 不支援所列的延伸模組。 您無法使用適用於 PostgreSQL 的 Azure 資料庫建立自己的延伸模組。

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>適用於 PostgreSQL 的 Azure 資料庫支援的擴充功能

下表列出適用於 PostgreSQL 的 Azure 資料庫目前支援的標準 PostgreSQL 擴充功能。 此資訊也可以藉由執行 `SELECT * FROM pg_available_extensions;` 來取得。

### <a name="data-types-extensions"></a>資料類型擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/current/static/citext.html) | 提供不區分大小寫的字元字串類型。 |
> | [cube](https://www.postgresql.org/docs/current/static/cube.html) | 提供多維度 Cube 的資料類型。 |
> | [hstore](https://www.postgresql.org/docs/current/static/hstore.html) | 提供用來儲存索引鍵/值組集合的資料類型。 |
> | [hll](https://github.com/citusdata/postgresql-hll) | 提供 HyperLogLog 資料結構。 |
> | [isn](https://www.postgresql.org/docs/current/static/isn.html) | 提供國際產品編號標準的資料類型。 |
> | [lo](https://www.postgresql.org/docs/current/lo.html) | 大型物件維護。 |
> | [ltree](https://www.postgresql.org/docs/current/static/ltree.html) | 提供階層式樹狀結構的資料類型。 |
> | [seg](https://www.postgresql.org/docs/current/seg.html) | 代表線段或浮點間隔的資料類型。 |
> | [topn](https://github.com/citusdata/postgresql-topn/) | 針對 top-n JSONB 的類型。 |

### <a name="full-text-search-extensions"></a>全文檢索搜尋擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/current/static/dict-int.html) | 提供整數的文字搜尋字典範本。 |
> | [dict \_ xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | 延伸同義文書處理的文字搜尋字典範本。 |
> | [unaccent](https://www.postgresql.org/docs/current/static/unaccent.html) | 文字搜尋字典，可從詞彙中移除重音符號 (變音符號)。 |

### <a name="functions-extensions"></a>函數擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [autoinc](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | 用於自動遞增欄位的函數。 |
> | [earthdistance](https://www.postgresql.org/docs/current/static/earthdistance.html) | 提供方法來計算地球表面上的大圓距離。 |
> | [fuzzystrmatch](https://www.postgresql.org/docs/current/static/fuzzystrmatch.html) | 提供數個函數來判斷字串之間的相似性與距離。 |
> | [插入使用者 \_ 名稱](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | 用於追蹤哪些人變更了資料表的函數。 |
> | [intagg](https://www.postgresql.org/docs/current/intagg.html) | 整數匯總工具和列舉值 (過時的) 。 |
> | [intarray](https://www.postgresql.org/docs/current/static/intarray.html) | 提供函數和運算子來操作無 null 的整數陣列。 |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | 用於追蹤上次修改時間的函式。 |
> | [pgcrypto](https://www.postgresql.org/docs/current/static/pgcrypto.html) | 提供密碼編譯函式。 |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | 依時間或識別碼管理分割的資料表。 |
> | [pg\_trgm](https://www.postgresql.org/docs/current/static/pgtrgm.html) | 提供可根據三併詞比對判斷英數文字相似度的函式和運算子。 |
> | [refint](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | 用來實現參考完整性 (過時) 的函式。 |
> | 會話 \_ 分析 | 用來查詢 hstore 陣列的函數。 |
> | [tablefunc](https://www.postgresql.org/docs/current/static/tablefunc.html) | 提供操縱整個資料表 (包括交叉資料表) 的函式。 |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | 已觸發變更通知。 |
> | [timetravel](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | 用於執行時間移動的函式。 |
> | [uuid-ossp](https://www.postgresql.org/docs/current/static/uuid-ossp.html) | 產生通用唯一識別碼 (UUID)。 |

### <a name="hyperscale-extensions"></a>超大規模資料庫擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Citus 分散式資料庫。 |
> | 分區 \_ rebalancer | 在節點新增或移除時，安全地重新平衡伺服器群組中的資料。 |

### <a name="index-types-extensions"></a>索引類型擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [bloom](https://www.postgresql.org/docs/current/bloom.html) | Bloom 存取方法-以檔案為基礎的簽章索引。 |
> | [btree\_gin](https://www.postgresql.org/docs/current/static/btree-gin.html) | 提供範例 GIN 運算子類別，可針對特定資料類型執行 B 型樹狀結構的行為。 |
> | [btree\_gist](https://www.postgresql.org/docs/current/static/btree-gist.html) | 提供可實作 B 型樹狀結構的 GiST 索引運算子類別。 |

### <a name="language-extensions"></a>語言延伸模組

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/current/static/plpgsql.html) | PL/pgSQL 可載入的程序性語言。 |

### <a name="miscellaneous-extensions"></a>其他擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [adminpack](https://www.postgresql.org/docs/current/adminpack.html) | 于 postgresql 的系統管理功能。 |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | 用來驗證關聯完整性的函數。 |
> | [檔案 \_ fdw](https://www.postgresql.org/docs/current/file-fdw.html) | 一般檔案存取的外部資料包裝函式。 |
> | [pageinspect](https://www.postgresql.org/docs/current/pageinspect.html) | 檢查低層級的資料庫頁面內容。 |
> | [pg\_buffercache](https://www.postgresql.org/docs/current/static/pgbuffercache.html) | 提供一種方法，即時檢查共用緩衝區快取中發生的狀況。 |
> | [pg \_ cron](https://github.com/citusdata/pg_cron) | 于 postgresql 的作業排程器。 |
> | [pg \_ freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | 檢查可用空間對應 (FSM) 。 |
> | [pg\_prewarm](https://www.postgresql.org/docs/current/static/pgprewarm.html) | 提供一種方式，來將關聯資料載入至緩衝區快取。 |
> | [pg\_stat\_statements](https://www.postgresql.org/docs/current/static/pgstatstatements.html) | 提供一種方法，來追蹤伺服器所執行之所有 SQL 陳述式的執行統計資料。 如需此延伸模組的相關資訊，請參閱「pg_stat_statements」一節。 |
> | [pg \_ 可見度](https://www.postgresql.org/docs/current/pgvisibility.html) | 檢查 (VM) 和頁面層級可見度資訊的可見度對應。 |
> | [pgrowlocks](https://www.postgresql.org/docs/current/static/pgrowlocks.html) | 提供方法來顯示資料列層級鎖定的資訊。 |
> | [pgstattuple](https://www.postgresql.org/docs/current/static/pgstattuple.html) | 提供方法來顯示 Tuple 層級統計資料。 |
> | [postgres\_fdw](https://www.postgresql.org/docs/current/static/postgres-fdw.html) | 可用來存取儲存於外部 PostgreSQL 伺服器之資料的外部資料包裝函式。 如需此延伸模組的相關資訊，請參閱「dblink 和 postgres_fdw」一節。|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | TLS/SSL 憑證的相關資訊。 |
> | [tsm \_ 系統資料 \_ 列](https://www.postgresql.org/docs/current/tsm-system-rows.html) | TABLESAMPLE 方法，可接受資料列數目做為限制。 |
> | [tsm \_ 系統 \_ 時間](https://www.postgresql.org/docs/current/tsm-system-time.html) | TABLESAMPLE 方法，其接受以毫秒為單位的時間做為限制。 |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | 提供方法建立不使用 CPU 或磁碟的假設索引。 |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | 支援從資料庫工作階段連線至其他 PostgreSQL 資料庫的模組。 如需此延伸模組的相關資訊，請參閱「dblink 和 postgres_fdw」一節。 |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | XPath 查詢和 XSLT。 |


### <a name="postgis-extensions"></a>PostGIS 擴充功能

> [!div class="mx-tableFixed"]
> | **延伸模組** | **描述** |
> |---|---|
> | [PostGIS](https://www.postgis.net/)、postgis\_topology、postgis\_tiger\_geocoder、postgis\_sfcgal | 適用於 PostgreSQL 的空間與地理物件。 |
> | address\_standardizer、address\_standardizer\_data\_us | 用來將位址剖析為組成項目。 用來支援對位址進行地理編碼的正規化步驟。 |
> | postgis \_ sfcgal | PostGIS SFCGAL 函數。 |
> | postgis \_ tiger \_ geocoder | PostGIS tiger geocoder 和 reverse geocoder。 |
> | postgis \_ 拓撲 | PostGIS 拓撲空間類型和函數。 |


## <a name="pg_stat_statements"></a>pg_stat_statements
[Pg \_ stat \_ 語句擴充](https://www.postgresql.org/docs/current/pgstatstatements.html)功能會在每個適用於 PostgreSQL 的 Azure 資料庫伺服器上預先載入，以提供您一種追蹤 SQL 語句執行統計資料的方法。

設定 `pg_stat_statements.track` 可控制延伸模組所計算的語句。 預設為 `top` ，這表示會追蹤用戶端直接發出的所有語句。 其他兩個會追蹤層級 `none` 和 `all`。 這項設定是可透過 [Azure 入口網站](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal)或 [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli) 設定的伺服器參數。

Pg_stat_statements 提供的查詢執行資訊與伺服器效能的影響，會在記錄每個 SQL 語句時受到取捨。 如果您不是使用 pg_stat_statements 延伸模組，建議您將設定 `pg_stat_statements.track` 為 `none` 。 某些協力廠商監視服務可能會依賴 pg_stat_statements 來傳遞查詢效能深入解析，因此請確認是否符合您的情況。

## <a name="dblink-and-postgres_fdw"></a>dblink 和 postgres_fdw

您可以使用 dblink 和 postgres \_ fdw，從一個于 postgresql 伺服器連線到另一個，或連接到相同伺服器中的另一個資料庫。  接收端伺服器必須允許來自傳送端伺服器通過其防火牆的連線。  若要使用這些擴充功能在適用於 PostgreSQL 的 Azure 資料庫伺服器或超大規模資料庫 (Citus) 伺服器群組之間進行連線，請將 [**允許 Azure 服務和資源存取此伺服器群組] (或 [伺服器) ** ] 設定為 [開啟]。  如果您想要使用延伸模組來回複至相同的伺服器，您也需要開啟此設定。
[**允許 Azure 服務和資源來存取此伺服器] 群組**設定，可以在 [**網路**] 下超大規模資料庫 (Citus) 伺服器群組的 [Azure 入口網站] 頁面中找到。  目前不支援來自適用於 PostgreSQL 的 Azure 資料庫單一伺服器和超大規模資料庫 (Citus) 的輸出連線，但與其他適用於 PostgreSQL 的 Azure 資料庫伺服器和超大規模資料庫 (Citus) 伺服器群組的連線除外。
