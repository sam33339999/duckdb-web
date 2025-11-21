---
layout: default
title: 常見問題
body_class: faq
toc: false
lang: zh-TW
---

<!-- ################################################################################# -->
<!-- ################################################################################# -->
<!-- ################################################################################# -->

<div class="wrap pagetitle">
  <h1>常見問題</h1>
</div>

## 概述

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 誰開發了 DuckDB？

<div class="answer" markdown="1">

DuckDB 由 [Dr. Mark Raasveldt](https://mytherin.github.io) 和 [Prof. Dr. Hannes Mühleisen](https://hannes.muehleisen.org) 以及來自世界各地的[許多貢獻者](https://github.com/duckdb/duckdb/graphs/contributors)共同維護。Mark 和 Hannes 成立了 [DuckDB 基金會](https://duckdb.org/foundation/)，負責募集捐款並資助 DuckDB 的開發和維護。Mark 和 Hannes 也是 [DuckDB Labs](https://www.duckdblabs.com) 的共同創辦人，該公司提供圍繞 DuckDB 的商業服務。其他幾位 DuckDB 貢獻者也隸屬於 DuckDB Labs。

DuckDB 的初期開發工作是在荷蘭阿姆斯特丹的 [Centrum Wiskunde & Informatica (CWI)](https://www.cwi.nl) 的[資料庫架構研究組](https://www.cwi.nl/research/groups/database-architectures)進行的。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 為什麼叫做 DuckDB？

<div class="answer" markdown="1">

鴨子是神奇的動物。牠們可以飛、可以走、也可以游泳。牠們幾乎可以靠任何東西維生。牠們對環境挑戰具有相當的適應力。鴨子的叫聲能讓人起死回生，並且[啟發了資料庫研究](/images/wilbur.jpg)。因此，牠們是多功能且具韌性的資料管理系統的完美吉祥物。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 是開源的嗎？

<div class="answer" markdown="1">

DuckDB 完全採用 MIT 授權條款開源，其開發工作在 [GitHub 的 `duckdb/duckdb` 儲存庫](https://github.com/duckdb/duckdb)中進行。DuckDB 的所有元件在免費版本中均採用此授權條款：沒有「企業版」的 DuckDB。

DuckDB 的大部分智慧財產權已刻意轉移給非營利實體，以將專案的授權與商業公司 DuckDB Labs 分開。DuckDB 基金會的[章程]({% link pdf/deed-of-incorporation-stichting-duckdb-foundation.pdf %})也確保 DuckDB 永久保持 MIT 授權條款的開源狀態。[CWI (Centrum Wiskunde & Informatica)](https://cwi.nl/) 在 DuckDB 基金會董事會中擁有一個席位，而對 DuckDB 基金會的捐款將直接資助 DuckDB 的開發。

有關 DuckDB 周邊組織的更多資訊，請參閱下一個問答。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB、DuckDB 基金會、DuckDB Labs 和 MotherDuck 之間有什麼關係？

<div class="answer" markdown="1">

[**DuckDB**](https://duckdb.org/) 是採用 MIT 授權條款的開源專案名稱。

[**DuckDB 基金會**]({% link foundation/index.html %})是一個非營利組織，持有 DuckDB 專案的智慧財產權。DuckDB 基金會的[章程]({% link pdf/deed-of-incorporation-stichting-duckdb-foundation.pdf %})確保 DuckDB 永久保持 MIT 授權條款的開源狀態。

[**DuckDB Labs**](https://duckdblabs.com/) 是一家總部位於阿姆斯特丹的公司，為 DuckDB 提供商業支援服務。DuckDB Labs 雇用了 DuckDB 專案的核心貢獻者。

[**MotherDuck**](https://motherduck.com/) 是一家獲得創投支持的公司，使用 DuckDB 建立混合雲端/本機平台。MotherDuck 與 DuckDB Labs 簽約提供開發服務，而 DuckDB Labs 擁有 MotherDuck 的一部分股權。[詳情請參閱合作公告](https://duckdblabs.com/news/2022/11/15/motherduck-partnership)。若要了解更多關於 MotherDuck 的資訊，請參閱 [CIDR 2024 關於 MotherDuck 的論文](https://www.cidrdb.org/cidr2024/papers/p46-atwal.pdf)和 [MotherDuck 文件](https://motherduck.com/docs)。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我在哪裡可以找到 DuckDB 標誌和設計指南？

<div class="answer" markdown="1">

請前往[設計與品牌資產頁面]({% link design/index.html %})。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我在哪裡可以找到 DuckDB 商標使用指南？

<div class="answer" markdown="1">

請參閱 [DuckDB™ 商標指南]({% link trademark_guidelines.md %})。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我發現一個名稱中包含「duck」的專案。它是否正式隸屬於 DuckDB？

<div class="answer" markdown="1">

以下專案正式隸屬於 DuckDB：

* [DuckDB 專案](https://github.com/duckdb/duckdb)
* 所有主要的[客戶端函式庫]({% link docs/stable/clients/overview.md %})
* 所有[核心 DuckDB 擴充套件]({% link docs/stable/core_extensions/overview.md %})
* [DuckDB UI](https://github.com/duckdb/duckdb-ui)
* [MotherDuck](https://motherduck.com)
* [`dbt-duckdb`](https://github.com/duckdb/dbt-duckdb)
* [`pg_duckdb`](https://github.com/duckdb/pg_duckdb)

其他專案可能_不隸屬於_ DuckDB 專案。請查看它們的網站、README 和授權條款以獲取更多詳細資訊。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我希望在 DuckDB 中實作功能 X。我該如何進行？

<div class="answer" markdown="1">

DuckDB 中的功能可以透過不同方式實作：在主 DuckDB 專案中、作為[核心擴充套件]({% link docs/stable/core_extensions/overview.md %})或[社群擴充套件]({% link community_extensions/index.md %})。如果您有功能請求，請遵循以下指南：

* 如果您有功能想法，請在 [DuckDB 的 GitHub Discussions 的「Ideas」區塊](https://github.com/duckdb/duckdb/discussions/categories/ideas)中提出議題。DuckDB 團隊會監控這些想法，並隨著時間推移實作經常被請求的功能。例如，我們最近發布了 [Avro 社群擴充套件]({% link community_extensions/extensions/avro.md %})以支援讀取 Avro 檔案，這是議題追蹤器中最常被請求的功能。
* 如果您想在主 DuckDB 專案中實作功能，請在 GitHub Discussions 或[我們的 Discord 伺服器](https://discord.duckdb.org/)上與 DuckDB 團隊討論。團隊可以驗證該想法和提議的實作是否符合專案的長期願景。
* 如果您想將功能實作為擴充套件，請考慮將其提交至[社群擴充套件儲存庫]({% link community_extensions/index.md %})。

請注意，雇用主要 DuckDB 貢獻者的公司 DuckDB Labs 提供 [DuckDB 的諮詢服務](https://duckdblabs.com/support/)，其中可以包括在 DuckDB 或作為 DuckDB 擴充套件實作功能。

</div>

</div>

<!-- ################################################################################# -->
<!-- ################################################################################# -->
<!-- ################################################################################# -->

## 使用 DuckDB

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 可以將資料儲存到磁碟嗎？

<div class="answer" markdown="1">

DuckDB 支援[持久化儲存]({% link docs/stable/connect/overview.md %}#persistent-database)，並將資料庫儲存為單一檔案，其中包含資料庫中的所有資料表、視圖、索引、巨集等。DuckDB 的[儲存格式]({% link docs/stable/internals/storage.md %})使用壓縮的列式表示法，既緊湊又允許高效的批量更新。DuckDB 也可以在[記憶體模式]({% link docs/stable/connect/overview.md %}#in-memory-database)下執行，此時不會將資料持久化到磁碟。DuckDB 還可以透過 [`ducklake` 擴充套件]({% link docs/stable/core_extensions/ducklake.md %})以 [DuckLake 格式](http://ducklake.select/)儲存資料。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我應該在什麼類型的儲存裝置上執行 DuckDB（例如本機磁碟、網路附加儲存）？

<div class="answer" markdown="1">

用於執行 DuckDB 的儲存裝置類型對[效能有顯著影響]({% link docs/stable/guides/performance/environment.md %}#disk)。一般來說，使用 SSD（SATA 或 NVMe SSD）相較於 HDD 會帶來更優越的效能。

儲存裝置的位置因工作負載而異：

* _對於唯讀工作負載_，DuckDB 資料庫可以儲存在本機磁碟和遠端端點上，例如 [HTTPS]({% link docs/stable/core_extensions/httpfs/https.md %}) 和雲端物件儲存，如 [AWS S3]({% link docs/stable/core_extensions/httpfs/s3api.md %}) 及類似的提供商。
* _對於讀寫工作負載_，將資料庫儲存在實例附加儲存上可獲得最佳效能。網路附加雲端儲存（如 [AWS EBS](https://aws.amazon.com/ebs/)）也可以使用，其效能可以透過保證 IOPS 設定進行微調。根據我們的經驗，我們**強烈建議不要在[網路附加儲存 (NAS)](https://en.wikipedia.org/wiki/Network-attached_storage) 上執行 DuckDB（或任何其他資料庫管理系統）進行讀寫工作負載**。這些設定通常速度較慢，並且會導致難以排查的偽隨機故障。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 是記憶體內資料庫嗎？

<div class="answer" markdown="1">

DuckDB 是記憶體內資料庫是一個常見的誤解。雖然 DuckDB _可以_在記憶體中工作，但它**不是記憶體內資料庫**。DuckDB 可以利用可用記憶體進行快取，但它也完全支援基於磁碟的持久化以及[將超出記憶體容量的操作卸載到磁碟]({% link docs/stable/guides/performance/how_to_tune_workloads.md %}#larger-than-memory-workloads-out-of-core-processing)。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 是基於 Arrow 建置的嗎？

<div class="answer" markdown="1">

DuckDB 內部不使用 [Apache Arrow 格式](https://arrow.apache.org/)。然而，DuckDB 支援使用 [`arrow` 社群擴充套件]({% link community_extensions/extensions/arrow.md %})從 Arrow 讀取和寫入 Arrow。它還可以使用 [`pyarrow`]({% link docs/stable/guides/python/sql_on_arrow.md %}) 直接在 Arrow 上執行 SQL 查詢。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 的資料庫檔案是否可以在不同的 DuckDB 版本和客戶端之間移植？

<div class="answer" markdown="1">

自 0.10.0 版本（2024 年 2 月發布）起，DuckDB 在讀取資料庫檔案時具有向後相容性，即較新版本的 DuckDB 始終能夠讀取使用較舊版本 DuckDB 建立的資料庫檔案。DuckDB 也盡力提供部分向前相容性。詳情請參閱[儲存頁面]({% link docs/stable/internals/storage.md %})。不同 DuckDB 客戶端（例如 Python 和 R）之間也保證相容性：使用一個客戶端建立的資料庫檔案可以用其他客戶端讀取。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 如何處理並行？多個行程可以寫入 DuckDB 嗎？

<div class="answer" markdown="1">

請參閱關於[處理並行]({% link docs/stable/connect/concurrency.md %}#handling-concurrency)的文件以及[「從多個行程寫入 DuckDB」]({% link docs/stable/connect/concurrency.md %}#writing-to-duckdb-from-multiple-processes)的章節。

若要使用多個 DuckDB 客戶端處理同一資料集，請考慮透過 [`ducklake` 擴充套件]({% link docs/stable/core_extensions/ducklake.md %})使用 [DuckLake 格式](http://ducklake.select/)。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 是否有官方的 DuckDB Docker 映像檔可用？

<div class="answer" markdown="1">

您可以使用官方 [DuckDB Docker 映像檔]({% link docs/stable/operations_manual/duckdb_docker.md %})執行 DuckDB 命令列客戶端。

請注意，在大多數情況下，您不需要容器來執行 DuckDB：您可以簡單地在客戶端應用程式中[以行程內方式]({% link why_duckdb.md %}#simple)部署它，或作為獨立的命令列二進位檔案。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 如何在同一台電腦上使用多個 DuckDB 客戶端？

<div class="answer" markdown="1">

您可以在同一台電腦上安裝多個 DuckDB 客戶端。這些客戶端是獨立安裝的，可以有不同的 DuckDB 版本。例如，您可以在 R 中使用 DuckDB 1.3.2 套件，使用 DuckDB 1.4.0 作為 CLI 客戶端，並在 Python 中使用預覽版本。

如果您不確定行程中使用的 DuckDB 版本，請執行 `PRAGMA version` 查詢，它會列印 DuckDB 的版本。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我可以在哪裡了解更多關於 DuckDB 的資訊？

<div class="answer" markdown="1">

DuckDB 有官方的[文件]({% link docs/stable/index.md %})、[部落格]({% link news/index.html %})和[媒體收藏]({% link media/index.html %})。同時，還有一些第三方資源可以幫助您了解更多關於 DuckDB 的資訊：

* 若要發現使用 DuckDB 的專案，我們建議訪問 [`awesome-duckdb` 儲存庫](https://github.com/davidgasquez/awesome-duckdb)。
* 有許多 [DuckDB 書籍](https://www.goodreads.com/search?utf8=%E2%9C%93&q=duckdb&search_type=books)可供閱讀。
* [tldr pages](https://tldr.sh/) 計畫有一個 [DuckDB 條目](https://tldr.inbrowser.app/pages/common/duckdb)。

</div>

</div>

<!-- ################################################################################# -->
<!-- ################################################################################# -->
<!-- ################################################################################# -->

## 效能

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 使用 SIMD 嗎？

<div class="answer" markdown="1">

DuckDB 不使用*顯式 SIMD*（單指令多資料）指令，因為它們會大大增加可攜性和編譯的複雜性。相反，DuckDB 使用*隱式 SIMD*，我們竭盡全力以這樣的方式編寫 C++ 程式碼，使編譯器可以為特定硬體*自動生成 SIMD 指令*。舉個例子說明為什麼這是個好主意：將 DuckDB 移植到 Apple Silicon 架構只花了 10 分鐘。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 中的可擴充性如何運作？

<div class="answer" markdown="1">

DuckDB 是一個單節點資料庫系統，因此它利用_垂直擴充性_，即利用更多資源（CPU、記憶體和磁碟）來支援更大的資料集。DuckDB 已在擁有 100+ CPU 核心和數 TB 記憶體的機器上測試過。

DuckDB 的原生資料庫格式也可擴充到數 TB 的資料，但這需要一些規劃 - 請參閱[「使用大型資料庫」頁面]({% link docs/stable/guides/performance/working_with_huge_databases.md %})。

若要處理大規模資料集和/或在同一資料集上協作，請考慮使用 [DuckLake](https://ducklake.select/) 湖倉格式。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 我想將 DuckDB 與另一個系統進行基準測試。我該如何進行？

<div class="answer" markdown="1">

我們歡迎將 DuckDB 的效能與其他系統進行比較的實驗。為確保公平比較，我們有一些建議。首先，嘗試使用[預覽版本]({% link docs/preview/index.md %})，它通常比最新穩定版本有顯著的效能改進。其次，請參考我們的 DBTest 2018 論文 [_Fair Benchmarking Considered Difficult: Common Pitfalls In Database Performance Testing_](https://hannes.muehleisen.org/publications/DBTEST2018-performance-testing.pdf)，了解如何避免基準測試中的常見問題。第三，研讀 DuckDB [效能指南]({% link docs/stable/guides/performance/overview.md %})，其中包含確保最佳效能的最佳實踐。最後，請報告 DuckDB 版本（對於穩定版本為版本號，對於每日建置版本為提交雜湊值）。

</div>

</div>

<!-- ################################################################################# -->
<!-- ################################################################################# -->
<!-- ################################################################################# -->

## DuckDB 的使用案例

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 是為資料科學還是資料工程工作負載而設計的？

<div class="answer" markdown="1">

DuckDB 在設計時考慮了資料科學和資料工程工作負載。因此，您可以根據需求使用 DuckDB 的 SQL 語法，使其非常靈活或非常精確。

對於經常以互動方式執行查詢的資料科學使用者，DuckDB 提供了多種機制來快速探索資料集。例如，可以透過[自動推斷其結構描述]({% link docs/stable/data/csv/auto_detection.md %})使用 `CREATE TABLE tbl AS FROM 'input.csv'` 來載入 CSV 檔案。此外，還有許多被稱為[「友善 SQL」]({% link docs/stable/sql/dialect/friendly_sql.md %})的 SQL 簡寫，用於更簡潔的表達式，例如 [`GROUP BY ALL` 子句]({% link docs/stable/sql/query_syntax/groupby.md %}#group-by-all)。

對於資料工程使用案例，DuckDB 允許完全控制載入過程，因此可以使用 `CREATE TABLE tbl ⟨schema⟩`{:.language-sql .highlight} 陳述式定義精確的結構描述，並使用指定 CSV 方言（分隔符號、引號等）的 [`COPY` 陳述式]({% link docs/stable/sql/statements/copy.md %})填充它。大多數友善 SQL 擴充功能都可以簡單地重寫為與 PostgreSQL 完全相容的 SQL 查詢。例如，`GROUP BY ALL` 子句可以替換為 `GROUP BY` 子句和明確的欄位列表。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### DuckDB 的典型使用案例有哪些？

<div class="answer" markdown="1">

DuckDB 的使用案例大致可以分為<a href="https://blobs.duckdb.org/events/duckcon5/hannes-muhleisen-mark-raasveldt-introduction-and-state-of-project.pdf#page=8">三大類</a>。也就是說，DuckDB 可以用於使用者的互動式資料分析（「資料科學」）和作為自動化資料處理的管線元件（「資料工程」）。DuckDB 也可以部署在新穎的架構中，在這些架構中，傳統上無法執行分析型資料庫管理系統，但由於其可攜性，DuckDB 可以使用。這些架構包括在瀏覽器中執行 DuckDB（使用 <a href="{% link docs/stable/clients/wasm/overview.md %}">WebAssembly 客戶端</a>）和在智慧型手機上執行。此外，DuckDB 的擴充套件解鎖了諸如<a href="{% link docs/stable/core_extensions/spatial/overview.md %}">地理空間分析</a>和與<a href="{% link docs/stable/core_extensions/mysql.md %}">其他</a><a href="{% link docs/stable/core_extensions/postgres.md %}">資料庫</a><a href="{% link docs/stable/core_extensions/sqlite.md %}">系統</a>的深度整合等使用案例。最後，在某些情況下，DuckDB <a href="https://www.nikolasgoebel.com/2024/05/28/duckdb-doesnt-need-data">甚至不需要資料也能成為資料庫</a>。

</div>

</div>

<!-- ################################################################################# -->
<!-- ################################################################################# -->
<!-- ################################################################################# -->

## 發布與開發

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 哪些 DuckDB 客戶端和版本獲得官方支援？

<div class="answer" markdown="1">

官方支援涵蓋最新 LTS 版本（目前為 1.4.x）和最新穩定版本（目前也是 1.4.x）的[主要客戶端]({% link docs/stable/clients/overview.md %})。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 新的 DuckDB 版本多久發布一次？

<div class="answer" markdown="1">

新功能版本（例如 v1.2.0）每 3-5 個月發布一次。錯誤修復版本（例如 v1.1.3）在功能版本發布後每 2-4 週發布一次。您可以在[發布行事曆]({% link release_calendar.md %})中找到最近的發布版本。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 下一個版本何時發布？我可以期待哪些功能？

<div class="answer" markdown="1">

請查看[發布行事曆]({% link release_calendar.md %})以了解下一個 DuckDB 穩定版本的計劃發布日期，以及[開發路線圖]({% link roadmap.md %})以了解未來一年計劃的功能。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 如何為 DuckDB 文件做出貢獻？

<div class="answer" markdown="1">

DuckDB 網站由 GitHub Pages 託管，並從 [`duckdb/duckdb-web`](https://github.com/duckdb/duckdb-web) 儲存庫部署。當從桌面電腦瀏覽文件時，每個頁面頂部都有一個「頁面來源」按鈕，可將您導航到其 Markdown 原始檔案。我們非常歡迎提交 Pull Request 來修復問題或擴充 DuckDB 功能的文件部分。在開啟 Pull Request 之前，請參閱我們的[貢獻者指南](https://github.com/duckdb/duckdb-web/blob/main/CONTRIBUTING.md)。

</div>

</div>

<!-- ----- ----- ----- ----- ----- ----- Q&A entry ----- ----- ----- ----- ----- ----- -->

<div class="qa-wrap" markdown="1">

### 哪些是 DuckDB 的官方資訊來源？

<div class="answer" markdown="1">

以下列出了 DuckDB 和 DuckLake 專案的官方權威資訊來源。使用其他來源時請謹慎。從其他來源下載二進位檔案和安裝腳本時應特別小心。

網站：

* [`duckdb.org`](https://duckdb.org/)：DuckDB
* [`ducklake.select`](https://ducklake.select/)：DuckLake
* [`duckdblabs.com`](https://duckdblabs.com/)：DuckDB Labs

社群媒體：

* [Bluesky](https://bsky.app/profile/duckdb.org)
* [LinkedIn](https://www.linkedin.com/company/duckdb/)
* [Twitter (X)](https://x.com/duckdb)

</div>

</div>
