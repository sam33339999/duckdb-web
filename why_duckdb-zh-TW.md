---
layout: default
title: 為什麼選擇 DuckDB
body_class: why-duckdb blog_typography
toc: true
lang: zh-TW
---

<div class="wrap pagetitle">
  <div class="pagetitle-heading" role="heading" aria-level="1">為什麼選擇 DuckDB</div>
</div>

市面上有許多資料庫管理系統（DBMS）。但是[沒有一種資料庫系統適合所有場景](https://blobs.duckdb.org/papers/stonebraker-centintemel-one-size-fits-all-icde-2015.pdf)。每種系統都在不同方面做出取捨，以更好地適應特定的使用案例。DuckDB 也不例外。在此，我們試圖解釋 DuckDB 的目標是什麼，以及我們為何以及如何透過技術手段來實現這些目標。首先，DuckDB 是一個支援[結構化查詢語言（SQL）](https://en.wikipedia.org/wiki/SQL)的[關聯式（表格導向）DBMS](https://en.wikipedia.org/wiki/Relational_database)。

## DuckDB 的關鍵特性

### 簡單易用

SQLite 是[全球部署最廣泛的 DBMS](https://www.sqlite.org/mostdeployed.html)。安裝簡單和嵌入式行程內操作是其成功的核心。DuckDB 採用了這些簡單性和嵌入式操作的理念。

DuckDB **沒有外部相依性**，無論是編譯還是執行時都不需要。對於發布版本，DuckDB 的整個原始碼樹會被編譯成兩個檔案：一個標頭檔和一個實作檔案，即所謂的「amalgamation」。這大大簡化了部署和與其他建置過程的整合。要建置 DuckDB，只需要一個可運作的 C++11 編譯器。

對於 DuckDB，無需安裝、更新和維護 DBMS 伺服器軟體。DuckDB 不作為獨立行程執行，而是完全**嵌入在主機行程中**。對於 DuckDB 所針對的分析型使用案例，這還具有**高速資料傳輸**往返資料庫的額外優勢。在某些情況下，DuckDB 可以在不複製的情況下處理外部資料。例如，DuckDB Python 套件可以直接在 Pandas 資料上執行查詢，而無需匯入或複製任何資料。

### 跨平台可攜

由於沒有相依性，DuckDB 極具可攜性。它可以針對所有主流作業系統（Linux、macOS、Windows）和 CPU 架構（x86、ARM）進行編譯。它可以從小型、資源受限的邊緣裝置部署到擁有 100 多個 CPU 核心的大型數 TB 記憶體伺服器。使用 [DuckDB-Wasm]({% link docs/stable/clients/wasm/overview.md %})，DuckDB 甚至可以在網頁瀏覽器和行動電話上執行。

DuckDB 提供 [Java、C、C++、Go、Node.js 和其他語言的 API]({% link docs/stable/clients/overview.md %})。

### 功能豐富

DuckDB 提供強大的資料管理功能。它廣泛支援 SQL 中的**複雜查詢**，擁有大型函式庫、視窗函式等。DuckDB 透過我們自訂的、針對批量作業最佳化的[多版本並行控制（MVCC）](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)提供**交易保證**（ACID 屬性）。資料可以儲存在持久化的**單檔資料庫**中。DuckDB 支援次要索引以加快嘗試尋找單個資料表項目的查詢。

DuckDB 深度整合到 Python 和 R 中，以實現高效的互動式資料分析。

### 速度極快

DuckDB 旨在支援**分析型查詢工作負載**，也稱為[線上分析處理（OLAP）](https://en.wikipedia.org/wiki/Online_analytical_processing)。這些工作負載的特徵是複雜、執行時間相對較長的查詢，處理儲存資料集的大部分內容，例如對整個資料表的聚合或多個大型資料表之間的連接。對資料的變更預期也是大規模的，例如同時附加多行，或同時變更或新增資料表的大部分內容。

為了有效支援這種工作負載，關鍵是減少每個單獨值所消耗的 CPU 週期數。實現這一目標的資料管理最新技術是[向量化或即時查詢執行引擎](https://www.vldb.org/pvldb/vol11/p2209-kersten.pdf)。DuckDB 使用**列式向量化查詢執行引擎**，其中查詢仍然是解釋性的，但一大批值（一個「向量」）在一次操作中被處理。這大大減少了傳統系統（如 PostgreSQL、MySQL 或 SQLite）中按順序處理每一行所存在的開銷。向量化查詢執行在 OLAP 查詢中帶來了更好的效能。

### 可擴充性

DuckDB 提供[靈活的擴充機制]({% link docs/stable/core_extensions/overview.md %})，允許定義新的資料類型、函式、檔案格式和新的 SQL 語法。事實上，DuckDB 的許多關鍵功能，例如對 [Parquet 檔案格式]({% link docs/stable/data/parquet/overview.md %})、[JSON]({% link docs/stable/data/json/overview.md %})、[時區]({% link docs/stable/core_extensions/icu.md %})的支援，以及對 [HTTP(S) 和 S3 協定]({% link docs/stable/core_extensions/httpfs/overview.md %})的支援都是以擴充套件的形式實作的。擴充套件也[可以在 DuckDB Wasm 中運作]({% post_url 2023-12-18-duckdb-extensions-in-wasm %})。使用者貢獻可作為[社群擴充套件]({% link community_extensions/index.md %})取得。

### 免費開源

DuckDB 的開發始於主要開發者在荷蘭擔任公職人員期間。我們認為將工作成果免費提供給荷蘭或其他地方的任何人是我們對社會的責任和義務。這就是為什麼 DuckDB 採用非常寬鬆的 [MIT 授權條款](https://en.wikipedia.org/wiki/MIT_License)發布，並且專案的智慧財產權由 [DuckDB 基金會]({% link foundation/index.html %})持有。我們邀請任何人貢獻，只要他們遵守我們的[行為準則]({% link code_of_conduct.md %})。

### 經過徹底測試

雖然 DuckDB 最初是由一個研究小組建立的，但它從未打算成為一個研究原型。相反，它旨在成為一個穩定且成熟的資料庫系統。為了促進這種穩定性，DuckDB 使用[持續整合](https://github.com/duckdb/duckdb/actions)進行密集測試。DuckDB 的測試套件目前包含數百萬個查詢，包括改編自 SQLite、PostgreSQL 和 MonetDB 測試套件的查詢。測試在各種平台和編譯器上重複進行。每個 Pull Request 都會針對完整的測試設定進行檢查，只有在通過後才會合併。

除了這個測試套件外，我們還執行各種測試，在高負載下測試 DuckDB。我們執行 [TPC-H]({% link docs/stable/core_extensions/tpch.md %}) 和 [TPC-DS]({% link docs/stable/core_extensions/tpcds.md %}) 基準測試，並執行各種測試，其中 DuckDB 由許多客戶端平行使用。

## 經同儕審查的論文和論文作品

* [Runtime-Extensible Parsers]({% link pdf/CIDR2025-muehleisen-raasveldt-extensible-parsers.pdf %})（CIDR 2025）
* [Robust External Hash Aggregation in the Solid State Age]({% link pdf/ICDE2024-kuiper-boncz-muehleisen-out-of-core.pdf %})（ICDE 2024）
* [These Rows Are Made for Sorting and That's Just What We'll Do]({% link pdf/ICDE2023-kuiper-muehleisen-sorting.pdf %})（ICDE 2023）
* [Join Order Optimization with (Almost) No Statistics](https://blobs.duckdb.org/papers/tom-ebergen-msc-thesis-join-order-optimization-with-almost-no-statistics.pdf)（碩士論文，2022）
* [DuckDB-Wasm: Fast Analytical Processing for the Web]({% link pdf/VLDB2022-kohn-duckdb-wasm.pdf %})（VLDB 2022 Demo）
* [Data Management for Data Science - Towards Embedded Analytics]({% link pdf/CIDR2020-raasveldt-muehleisen-duckdb.pdf %})（CIDR 2020）
* [DuckDB: an Embeddable Analytical Database]({% link pdf/SIGMOD2019-demo-duckdb.pdf %})（SIGMOD 2019 Demo）

## 使用/為 DuckDB 建置的專案

若要了解使用 DuckDB 的專案，請造訪 [Awesome DuckDB 儲存庫](https://github.com/davidgasquez/awesome-duckdb)。

## 站在巨人的肩膀上

DuckDB 使用了來自各種開源專案的一些元件，並從科學出版物中汲取靈感。我們對此深表感激。以下是概覽：

* **執行引擎：** 向量化執行引擎的靈感來自 Peter Boncz、Marcin Zukowski 和 Niels Nes 的論文 [MonetDB/X100: Hyper-Pipelining Query Execution](http://cidrdb.org/cidr2005/papers/P19.pdf)。MonetDB/X100 後來成為 [Vectorwise（Actian Vector）資料庫系統](https://ir.cwi.nl/pub/19958/19958B.pdf)。
* **最佳化器：** DuckDB 的最佳化器從 Guido Moerkotte 和 Thomas Neumann 的論文 [Dynamic programming strikes back](https://15721.courses.cs.cmu.edu/spring2020/papers/20-optimizer2/p539-moerkotte.pdf) 以及 Thomas Neumann 和 Alfons Kemper 的論文 [Unnesting Arbitrary Queries](http://www.btw-2015.de/res/proceedings/Hauptband/Wiss/Neumann-Unnesting_Arbitrary_Querie.pdf) 中汲取靈感。
* **並行控制：** 我們的 MVCC 實作靈感來自 Thomas Neumann、Tobias Mühlbauer 和 Alfons Kemper 的論文 [Fast Serializable Multi-Version Concurrency Control for Main-Memory Database Systems](https://db.in.tum.de/~muehlbau/papers/mvcc.pdf)。
* **次要索引：** DuckDB 支援基於 Viktor Leis、Alfons Kemper 和 Thomas Neumann 的論文 [The Adaptive Radix Tree: ARTful Indexing for Main-Memory Databases](https://db.in.tum.de/~leis/papers/ART.pdf) 的次要索引。
* **SQL 視窗函式：** DuckDB 的視窗函式實作使用了 Viktor Leis、Kan Kundhikanjana、Alfons Kemper 和 Thomas Neumann 的論文 [Efficient Processing of Window Functions in Analytical SQL Queries](https://www.vldb.org/pvldb/vol8/p1058-leis.pdf) 中描述的 Segment Tree Aggregation。
* **SQL 不等式連接：** DuckDB 的不等式連接實作使用了 Zuhair Khayyat、William Lucia、Meghna Singh、Mourad Ouzzani、Paolo Papotti、Jorge-Arnulfo Quiané-Ruiz、Nan Tang 和 Panos Kalnis 的論文 [Lightning Fast and Space Efficient Inequality Joins](https://vldb.org/pvldb/vol8/p2074-khayyat.pdf) 中描述的 IEJoin 演算法。
* **浮點數值壓縮：** DuckDB 支援多種壓縮浮點數值的演算法：
    * Panagiotis Liakos、Katia Papakonstantinopoulou 和 Yannis Kotidi 的 [Chimp]({% link _science/2024-03-25-fp-compression.md %})
    * [Patas](https://github.com/duckdb/duckdb/pull/5044)，內部開發
    * Azim Afroozeh、Leonard Kuffo 和 Peter Boncz 的 [ALP（自適應無損浮點壓縮）]({% link _science/2024-06-09-alp.md %})，他們也[貢獻了他們的實作](https://github.com/duckdb/duckdb/pull/9635)
* **SQL 解析器：** 我們使用[重新封裝為獨立函式庫](https://github.com/lfittl/libpg_query)的 PostgreSQL 解析器。對我們自己的解析樹的轉換靈感來自 [Peloton](https://db.cs.cmu.edu/peloton/)。
* **Shell：** 我們使用 [SQLite shell](https://sqlite.org/cli.html) 來使用 DuckDB。
* **正規表示式：** DuckDB 使用 Google 的 [RE2](https://github.com/google/re2) 正規表示式引擎。
* **字串格式化：** DuckDB 使用 [fmt](https://github.com/fmtlib/fmt) 字串格式化函式庫。
* **UTF 處理：** DuckDB 使用 [utf8proc](https://juliastrings.github.io/utf8proc/) 函式庫來檢查和規範化 UTF8。
* **排序規則和時間：** DuckDB 使用 [ICU](https://unicode-org.github.io/icu/) 函式庫進行排序規則、時區和日曆支援。
* **測試框架：** DuckDB 使用 [Catch2](https://github.com/catchorg/Catch2) 單元測試框架。
* **測試案例：** 我們使用來自 [SQLite 的 SQL Logic Tests](https://www.sqlite.org/sqllogictest/doc/trunk/about.wiki) 來測試 DuckDB。
* **結果驗證：** [Manuel Rigger](https://www.manuelrigger.at) 使用他出色的 [SQLancer](https://github.com/sqlancer/sqlancer) 工具來驗證 DuckDB 結果的正確性。
* **查詢模糊測試：** 我們透過 [`sqlsmith` 擴充套件]({% link docs/stable/core_extensions/sqlsmith.md %})使用 [SQLsmith](https://github.com/anse1/sqlsmith) 來產生隨機查詢進行額外測試。
* **JSON 解析器：** 我們使用 [yyjson](https://github.com/ibireme/yyjson)，一個用 ANSI C 編寫的高效能 JSON 函式庫，來解析 DuckDB 的 [JSON 擴充套件]({% link docs/stable/data/json/overview.md %})中的 JSON。
* **遞迴 CTE 中的 `USING KEY`：** 來自圖賓根大學的創新想法，允許將遞迴公用資料表表示式中的中間結果視為鍵控字典，從而顯著改善效能和記憶體使用。請參閱論文 ["How DuckDB is `USING KEY` to Unlock Recursive Query Performance"]({% link _science/2025-06-22-bamberg-using-key-sigmod.md %})。
