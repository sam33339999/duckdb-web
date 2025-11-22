---
layout: default
title: 開發路線圖
body_class: roadmap blog_typography post
redirect_from:
- /docs/stable/dev/roadmap
- /docs/stable/dev/roadmap/
- /docs/preview/dev/roadmap
- /docs/preview/dev/roadmap/
max_page_width: small
toc: false
---

<div class="wrap pagetitle">
  <h1>開發路線圖</h1>
</div>

_（最後更新：2025 年 10 月）_

DuckDB 專案由[非營利的 DuckDB 基金會]({% link foundation/index.html %})管理。
基金會和 [DuckDB Labs](https://duckdblabs.com) 並未接受外部投資者（例如創投）的資金。
相反地，基金會由其[成員]({% link foundation/index.html %}#supporters)的貢獻資助，
而 DuckDB Labs 的收入則基於[商業支援和功能優先開發服務](https://duckdblabs.com/#support)。

此清單由 DuckDB 維護者編制，基於 DuckDB 專案的長期策略願景，以及與開源社群使用者的一般互動（GitHub Issues 和 Discussions、社群媒體等）。
有關如何在 DuckDB 中請求功能的詳細資訊，請參閱常見問題項目[「我希望 DuckDB 實作功能 X」]({% link faq.md %}#i-would-like-feature-x-to-be-implemented-in-duckdb-how-do-i-proceed)。

## 計劃功能

本節列出 DuckDB 團隊計劃在**未來一年內**開發的功能。

* 遷移和文件化到 [C 客戶端 API]({% link docs/stable/clients/c/overview.md %}) 和 [C 擴展 API](https://github.com/duckdb/extension-template-c)
* 擴展的 Rust 支援
* lakehouse 格式的改進
    * 透過 [`iceberg` 擴展]({% link docs/stable/core_extensions/iceberg/overview.md %})改進對 Iceberg 格式的支援。這在 [v1.4.0]({% post_url 2025-09-16-announcing-duckdb-140 %}) 中部分實作，可以寫入到 Iceberg 表格。
    * 透過 [`delta` 擴展]({% link docs/stable/core_extensions/delta.md %})改進對 Delta Lake 的支援。
    * 2025 年 5 月，我們發布了 [DuckLake](https://ducklake.select/)，一個新的 lakehouse 格式。我們想要強調的是，我們仍然致力於開發 `iceberg` 和 `delta` 擴展。我們也努力[提供互通性]({% post_url 2025-09-17-ducklake-03 %}#interoperability-with-iceberg)，在 DuckLake 和其他 lakehouse 格式之間。
* 用於模式匹配的 [`MATCH_RECOGNIZE`](https://github.com/duckdb/duckdb/discussions/3994)
* [`GEOMETRY` 類型](https://github.com/duckdb/duckdb/pull/19136)
* Windows ARM64 擴展的發布
* [非同步 I/O 支援](https://github.com/duckdb/duckdb/discussions/3560)
* [並行 Python UDF](https://github.com/duckdb/duckdb/issues/14817)

請注意，**不保證**特定功能會在明年內發布。本頁面上的所有內容都可能在不另行通知的情況下變更。

## 未來工作 / 尋求資助

有幾個項目我們計劃在未來某個時間點實作。
如果您希望加速這些功能的開發，請[與 DuckDB Labs 聯繫](https://duckdblabs.com/contact/)。

* 擴展的 Go 支援
* musl libc 二進位檔案的發布
* 時間序列最佳化
* 分區感知最佳化
* 排序感知最佳化
* 使用自動維護的表格樣本來改進篩選基數估計
* [`ALTER TABLE` 支援新增外鍵](https://github.com/duckdb/duckdb/discussions/4204)
* 查詢分析的改進（特別是針對並行執行的查詢）
* [具體化視圖](https://github.com/duckdb/duckdb/discussions/3638)
* [PL/SQL 預存程序支援](https://github.com/duckdb/duckdb/discussions/8104)
* [通用 ODBC 目錄](https://github.com/duckdb/duckdb/discussions/6645)，類似於現有的 PostgreSQL / MySQL / SQLite 整合
* [XML 讀取支援](https://github.com/duckdb/duckdb/discussions/9547)
* 保證[資料庫加密]({% link docs/stable/sql/statements/attach.md %}#database-encryption)的 [FIPS](https://en.wikipedia.org/wiki/FIPS_140-2) 合規性
