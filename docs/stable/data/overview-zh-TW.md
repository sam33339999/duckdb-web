---
layout: docu
redirect_from:
- /docs/data/overview
title: 匯入資料
lang: zh-TW
---

使用資料庫系統的第一步是將資料插入該系統。
DuckDB 可以直接連接到[許多熱門的資料來源]({% link docs/stable/data/data_sources.md %})，並提供多種資料匯入方法，讓您能夠輕鬆且有效率地填充資料庫。
在此頁面上，我們提供這些方法的概覽，以便您選擇最適合您使用案例的方法。

## `INSERT` 陳述式

`INSERT` 陳述式是將資料載入資料庫系統的標準方式。它們適合快速原型設計，但應避免用於批次載入，因為它們有顯著的每列開銷。

```sql
INSERT INTO people VALUES (1, 'Mark');
```

如需更詳細的說明，請參閱 [`INSERT` 陳述式頁面]({% link docs/stable/data/insert.md %})。

## 檔案載入：相對路徑

使用組態選項 [`file_search_path`]({% link docs/stable/configuration/overview.md %}#local-configuration-options) 來設定相對路徑要擴展到哪些「根目錄」。
如果未設定 `file_search_path`，則使用工作目錄作為相對路徑的基礎。

## 檔案格式

### CSV 載入

資料可以使用多種方法從 CSV 檔案有效率地載入。最簡單的方法是使用 CSV 檔案的名稱：

```sql
SELECT * FROM 'test.csv';
```

或者，使用 [`read_csv` 函式]({% link docs/stable/data/csv/overview.md %})來傳遞選項：

```sql
SELECT * FROM read_csv('test.csv', header = false);
```

或使用 [`COPY` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy--from)：

```sql
COPY tbl FROM 'test.csv' (HEADER false);
```

也可以直接從**壓縮的 CSV 檔案**讀取資料（例如，使用 [gzip](https://www.gzip.org/) 壓縮的檔案）：

```sql
SELECT * FROM 'test.csv.gz';
```

DuckDB 可以使用 [`CREATE TABLE ... AS SELECT` 陳述式]({% link docs/stable/sql/statements/create_table.md %}#create-table--as-select-ctas)從載入的資料建立資料表：

```sql
CREATE TABLE test AS
    SELECT * FROM 'test.csv';
```

如需更多詳細資訊，請參閱 [CSV 載入頁面]({% link docs/stable/data/csv/overview.md %})。

### Parquet 載入

Parquet 檔案可以使用其檔案名稱有效率地載入和查詢：

```sql
SELECT * FROM 'test.parquet';
```

或者，使用 [`read_parquet` 函式]({% link docs/stable/data/parquet/overview.md %})：

```sql
SELECT * FROM read_parquet('test.parquet');
```

或使用 [`COPY` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy--from)：

```sql
COPY tbl FROM 'test.parquet';
```

如需更多詳細資訊，請參閱 [Parquet 載入頁面]({% link docs/stable/data/parquet/overview.md %})。

### JSON 載入

JSON 檔案可以使用其檔案名稱有效率地載入和查詢：

```sql
SELECT * FROM 'test.json';
```

或者，使用 [`read_json_auto` 函式]({% link docs/stable/data/json/overview.md %})：

```sql
SELECT * FROM read_json_auto('test.json');
```

或使用 [`COPY` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy--from)：

```sql
COPY tbl FROM 'test.json';
```

如需更多詳細資訊，請參閱 [JSON 載入頁面]({% link docs/stable/data/json/overview.md %})。

### 返回檔案名稱

從 DuckDB v1.3.0 開始，CSV、JSON 和 Parquet 讀取器支援 `filename` 虛擬欄位：

```sql
COPY (FROM (VALUES (42), (43)) t(x)) TO 'test.parquet';
SELECT *, filename FROM 'test.parquet';
```

## Appender

在多個 API（C、C++、Go、Java 和 Rust）中，[Appender]({% link docs/stable/data/appender.md %}) 可以作為批次資料載入的替代方案。
這個類別可用於有效率地將列新增到資料庫系統，而無需使用 SQL 陳述式。
