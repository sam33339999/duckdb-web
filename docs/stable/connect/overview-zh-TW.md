---
layout: docu
redirect_from:
- /docs/connect
- /docs/connect/
- /docs/connect/overview
title: 連接
lang: zh-TW
---

## 連接或建立資料庫

要使用 DuckDB，您必須先建立與資料庫的連接。確切的語法因[客戶端 API]({% link docs/stable/clients/overview.md %}) 而異，但通常涉及傳遞參數來配置持久化。

## 持久化

DuckDB 可以在持久化模式（資料儲存到磁碟）和記憶體模式（整個資料集儲存在主記憶體中）下運作。

> 提示：持久化資料庫和記憶體資料庫都使用磁碟溢出來處理超出記憶體容量的工作負載（即核外處理）。

### 持久化資料庫

要建立或開啟持久化資料庫，請在建立連接時設定資料庫檔案的路徑，例如 `my_database.duckdb`。此路徑可以指向現有資料庫或尚不存在的檔案，DuckDB 將根據需要在該位置開啟或建立資料庫。檔案可以有任意副檔名，但 `.db` 或 `.duckdb` 是兩個常見的選擇，有時也會使用 `.ddb`。

從 v0.10 開始，DuckDB 的儲存格式具有[向後相容性]({% link docs/stable/internals/storage.md %}#backward-compatibility)，即 DuckDB 能夠讀取舊版本 DuckDB 產生的資料庫檔案。例如，DuckDB v0.10 可以讀取和操作由先前的 DuckDB 版本 v0.9 建立的檔案。有關 DuckDB 儲存格式的更多詳細資訊，請參閱[儲存頁面]({% link docs/stable/internals/storage.md %})。

### 記憶體資料庫

DuckDB 可以在記憶體模式下運作。在大多數客戶端中，可以透過傳遞特殊值 `:memory:` 作為資料庫檔案或省略資料庫檔案參數來啟動此模式。在記憶體模式下，資料不會持久化到磁碟，因此當行程結束時，所有資料都會遺失。
