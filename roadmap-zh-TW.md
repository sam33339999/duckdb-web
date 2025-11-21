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
lang: zh-TW
---

<div class="wrap pagetitle">
  <h1>開發路線圖</h1>
</div>

_（最後更新：2025 年 10 月）_

DuckDB 專案由[非營利的 DuckDB 基金會]({% link foundation/index.html %})管理。該基金會和 [DuckDB Labs](https://duckdblabs.com) 並非由外部投資者（例如創投）資助。相反，基金會由其[成員]({% link foundation/index.html %}#supporters)的捐款資助，而 DuckDB Labs 的收入則基於[商業支援和功能優先開發服務](https://duckdblabs.com/#support)。

此清單由 DuckDB 維護者編製，基於 DuckDB 專案的長期策略願景以及與開源社群使用者的一般互動（GitHub Issues 和 Discussions、社群媒體等）。有關如何在 DuckDB 中請求功能的詳細資訊，請參閱常見問題項目[「我希望在 DuckDB 中實作功能 X」]({% link faq.md %}#i-would-like-feature-x-to-be-implemented-in-duckdb-how-do-i-proceed)。

## 計劃中的功能

本節列出了 DuckDB 團隊計劃**在未來一年內**開發的功能。

* 遷移和文件化到 [C 客戶端 API]({% link docs/stable/clients/c/overview.md %}) 和 [C 擴充 API](https://github.com/duckdb/extension-template-c)
* 擴充套件的 Rust 支援
* 湖倉格式改進
    * 透過 [`iceberg` 擴充套件]({% link docs/stable/core_extensions/iceberg/overview.md %})改進對 Iceberg 格式的支援。這在 [v1.4.0]({% post_url 2025-09-16-announcing-duckdb-140 %}) 中部分實作，該版本可以寫入 Iceberg 資料表。
    * 透過 [`delta` 擴充套件]({% link docs/stable/core_extensions/delta.md %})改進對 Delta Lake 的支援。
    * 2025 年 5 月，我們發布了 [DuckLake](https://ducklake.select/)，一種新的湖倉格式。我們想強調的是，我們仍然致力於開發 `iceberg` 和 `delta` 擴充套件。我們也努力[提供互操作性]({% post_url 2025-09-17-ducklake-03 %}#interoperability-with-iceberg)，使 DuckLake 與其他湖倉格式之間能夠互通。
* 用於模式比對的 [`MATCH_RECOGNIZE`](https://github.com/duckdb/duckdb/discussions/3994)
* [`GEOMETRY` 類型](https://github.com/duckdb/duckdb/pull/19136)
* 分發 Windows ARM64 擴充套件
* [非同步 I/O 支援](https://github.com/duckdb/duckdb/discussions/3560)
* [平行 Python UDF](https://github.com/duckdb/duckdb/issues/14817)

請注意，**不保證**特定功能將在明年內發布。本頁面上的所有內容如有變更，恕不另行通知。

## 未來工作 / 尋求資助

有幾個項目我們計劃在未來某個時間點實作。如果您想加快這些功能的開發，請[與 DuckDB Labs 聯繫](https://duckdblabs.com/contact/)。

* 擴充套件的 Go 支援
* 分發 musl libc 二進位檔案
* 時間序列最佳化
* 分區感知最佳化
* 排序感知最佳化
* 使用自動維護的資料表樣本進行更好的過濾器基數估計
* [`ALTER TABLE` 支援新增外鍵](https://github.com/duckdb/duckdb/discussions/4204)
* 查詢效能分析改進（特別是針對並行執行的查詢）
* [物化視圖](https://github.com/duckdb/duckdb/discussions/3638)
* [PL/SQL 預存程序支援](https://github.com/duckdb/duckdb/discussions/8104)
* [通用 ODBC 目錄](https://github.com/duckdb/duckdb/discussions/6645)，類似於現有的 PostgreSQL / MySQL / SQLite 整合
* [XML 讀取支援](https://github.com/duckdb/duckdb/discussions/9547)
* 保證[資料庫加密]({% link docs/stable/sql/statements/attach.md %}#database-encryption)的 [FIPS](https://en.wikipedia.org/wiki/FIPS_140-2) 合規性
