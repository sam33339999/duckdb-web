---
layout: docu
redirect_from:
- /docs/guides
- /docs/guides/
- /docs/guides/index
- /docs/guides/index/
- /docs/guides/overview
title: 指南
---

指南部分包含專注於實現單一目標的簡潔操作指南。
有關 API 參考和範例，請參閱文件的其他部分。

請注意，有許多使用 DuckDB 的工具未包含在官方指南中。
要找到這些工具的列表，請查看 [Awesome DuckDB 儲存庫](https://github.com/davidgasquez/awesome-duckdb)。

> Tip 若需簡短的入門教學，請參閱[「分析荷蘭的鐵路交通」]({% post_url 2024-05-31-analyzing-railway-traffic-in-the-netherlands %})教學。

## 資料匯入和匯出

* [資料匯入概述]({% link docs/stable/guides/file_formats/overview.md %})
* [使用 `file:` 協定存取檔案]({% link docs/stable/guides/file_formats/file_access.md %})

### CSV 檔案

* [如何將 CSV 檔案載入到表格]({% link docs/stable/guides/file_formats/csv_import.md %})
* [如何將表格匯出為 CSV 檔案]({% link docs/stable/guides/file_formats/csv_export.md %})

### Parquet 檔案

* [如何將 Parquet 檔案載入到表格]({% link docs/stable/guides/file_formats/parquet_import.md %})
* [如何將表格匯出為 Parquet 檔案]({% link docs/stable/guides/file_formats/parquet_export.md %})
* [如何直接在 Parquet 檔案上執行查詢]({% link docs/stable/guides/file_formats/query_parquet.md %})

### HTTP(S)、S3 和 GCP

* [如何直接從 HTTP(S) 載入 Parquet 檔案]({% link docs/stable/guides/network_cloud_storage/http_import.md %})
* [如何直接從 S3 載入 Parquet 檔案]({% link docs/stable/guides/network_cloud_storage/s3_import.md %})
* [如何將 Parquet 檔案匯出到 S3]({% link docs/stable/guides/network_cloud_storage/s3_export.md %})
* [如何從 S3 Express One 載入 Parquet 檔案]({% link docs/stable/guides/network_cloud_storage/s3_express_one.md %})
* [如何直接從 GCS 載入 Parquet 檔案]({% link docs/stable/guides/network_cloud_storage/gcs_import.md %})
* [如何直接從 Cloudflare R2 載入 Parquet 檔案]({% link docs/stable/guides/network_cloud_storage/cloudflare_r2_import.md %})
* [如何直接從 S3 載入 Iceberg 表格]({% link docs/stable/guides/network_cloud_storage/s3_iceberg_import.md %})

### JSON 檔案

* [如何將 JSON 檔案載入到表格]({% link docs/stable/guides/file_formats/json_import.md %})
* [如何將表格匯出為 JSON 檔案]({% link docs/stable/guides/file_formats/json_export.md %})

### 使用 Spatial 擴展的 Excel 檔案

* [如何將 Excel 檔案載入到表格]({% link docs/stable/guides/file_formats/excel_import.md %})
* [如何將表格匯出為 Excel 檔案]({% link docs/stable/guides/file_formats/excel_export.md %})

### 查詢其他資料庫系統

* [如何直接查詢 MySQL 資料庫]({% link docs/stable/guides/database_integration/mysql.md %})
* [如何直接查詢 PostgreSQL 資料庫]({% link docs/stable/guides/database_integration/postgres.md %})
* [如何直接查詢 SQLite 資料庫]({% link docs/stable/guides/database_integration/sqlite.md %})

### 直接讀取檔案

* [如何直接讀取二進位檔案]({% link docs/stable/guides/file_formats/read_file.md %}#read_blob)
* [如何直接讀取文字檔案]({% link docs/stable/guides/file_formats/read_file.md %}#read_text)

## 效能

* [我的工作負載很慢（疑難排解指南）]({% link docs/stable/guides/performance/my_workload_is_slow.md %})
* [如何設計架構以獲得最佳效能]({% link docs/stable/guides/performance/schema.md %})
* [DuckDB 的理想硬體環境是什麼]({% link docs/stable/guides/performance/environment.md %})
* [Parquet 檔案和（壓縮的）CSV 檔案有什麼效能影響]({% link docs/stable/guides/performance/file_formats.md %})
* [如何調整工作負載]({% link docs/stable/guides/performance/how_to_tune_workloads.md %})
* [基準測試]({% link docs/stable/guides/performance/benchmarks.md %})

## 元查詢

* [如何列出所有表格]({% link docs/stable/guides/meta/list_tables.md %})
* [如何查看查詢結果的架構]({% link docs/stable/guides/meta/describe.md %})
* [如何使用 summarize 快速了解資料集]({% link docs/stable/guides/meta/summarize.md %})
* [如何查看查詢的查詢計劃]({% link docs/stable/guides/meta/explain.md %})
* [如何分析查詢]({% link docs/stable/guides/meta/explain_analyze.md %})

## ODBC

* [如何設定 ODBC 應用程式（等等！）]({% link docs/stable/guides/odbc/general.md %})

## Python 客戶端

* [如何安裝 Python 客戶端]({% link docs/stable/guides/python/install.md %})
* [如何執行 SQL 查詢]({% link docs/stable/guides/python/execute_sql.md %})
* [如何在 Jupyter Notebooks 中輕鬆查詢 DuckDB]({% link docs/stable/guides/python/jupyter.md %})
* [如何在 marimo Notebooks 中輕鬆查詢 DuckDB]({% link docs/stable/guides/python/marimo.md %})
* [如何在 DuckDB 中使用多個 Python 執行緒]({% link docs/stable/guides/python/multiple_threads.md %})
* [如何在 DuckDB 中使用 fsspec 檔案系統]({% link docs/stable/guides/python/filesystems.md %})

### Pandas

* [如何在 Pandas DataFrame 上執行 SQL]({% link docs/stable/guides/python/sql_on_pandas.md %})
* [如何從 Pandas DataFrame 建立表格]({% link docs/stable/guides/python/import_pandas.md %})
* [如何將資料匯出到 Pandas DataFrame]({% link docs/stable/guides/python/export_pandas.md %})

### Apache Arrow

* [如何在 Apache Arrow 上執行 SQL]({% link docs/stable/guides/python/sql_on_arrow.md %})
* [如何從 Apache Arrow 建立 DuckDB 表格]({% link docs/stable/guides/python/import_arrow.md %})
* [如何將資料匯出到 Apache Arrow]({% link docs/stable/guides/python/export_arrow.md %})

### 關聯式 API

* [如何使用關聯式 API 查詢 Pandas DataFrames]({% link docs/stable/guides/python/relational_api_pandas.md %})

### Python 函式庫整合

* [如何使用 Ibis 以或不以 SQL 查詢 DuckDB]({% link docs/stable/guides/python/ibis.md %})
* [如何透過 Apache Arrow 將 DuckDB 與 Polars DataFrames 一起使用]({% link docs/stable/guides/python/polars.md %})

## SQL 功能

* [Friendly SQL]({% link docs/stable/sql/dialect/friendly_sql.md %})
* [As-of join]({% link docs/stable/guides/sql_features/asof_join.md %})
* [全文搜尋]({% link docs/stable/guides/sql_features/full_text_search.md %})
* [`query` 和 `query_table` 函數]({% link docs/stable/guides/sql_features/query_and_query_table_functions.md %})

## SQL 編輯器和 IDE

* [如何設定 DBeaver SQL IDE]({% link docs/stable/guides/sql_editors/dbeaver.md %})

## 資料檢視器

* [如何使用 Tableau 視覺化 DuckDB 資料庫]({% link docs/stable/guides/data_viewers/tableau.md %})
* [如何使用 DuckDB 和 YouPlot 繪製命令列圖表]({% link docs/stable/guides/data_viewers/youplot.md %})
