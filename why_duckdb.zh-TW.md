---
layout: default
title: 為什麼選擇 DuckDB
body_class: why-duckdb blog_typography
toc: true
---

<div class="wrap pagetitle">
  <div class="pagetitle-heading" role="heading" aria-level="1">為什麼選擇 DuckDB</div>
</div>

市面上有許多資料庫管理系統（DBMS）。但[沒有一種萬用的資料庫系統](https://blobs.duckdb.org/papers/stonebraker-centintemel-one-size-fits-all-icde-2015.pdf)。所有系統都在不同方面做出取捨，以更好地適應特定使用情境。DuckDB 也不例外。在此，我們試圖解釋 DuckDB 的目標，以及我們為何及如何透過技術手段實現這些目標。首先，DuckDB 是一個[關聯式（表格導向）資料庫管理系統](https://en.wikipedia.org/wiki/Relational_database)，支援[結構化查詢語言（SQL）](https://en.wikipedia.org/wiki/SQL)。

## DuckDB 的主要特性

### 簡單易用

SQLite 是[全球部署最廣泛的資料庫管理系統](https://www.sqlite.org/mostdeployed.html)。安裝簡單和嵌入式程序內運作是其成功的關鍵。DuckDB 採用了這些簡單性和嵌入式運作的理念。

DuckDB **沒有外部相依性**，無論是編譯時還是執行時都不需要。對於發布版本，DuckDB 的整個原始碼樹會被編譯成兩個檔案：一個標頭檔和一個實作檔案，也就是所謂的「amalgamation」。這大大簡化了部署和整合到其他建置流程中。建置時，只需要一個可用的 C++11 編譯器即可建置 DuckDB。

使用 DuckDB 不需要安裝、更新和維護資料庫伺服器軟體。DuckDB 不會作為獨立程序執行，而是**完全嵌入在宿主程序中**。對於 DuckDB 針對的分析使用情境，這還有**高速資料傳輸**往來資料庫的額外優勢。在某些情況下，DuckDB 可以在不複製的情況下處理外部資料。例如，DuckDB Python 套件可以直接在 Pandas 資料上執行查詢，而無需匯入或複製任何資料。

### 可移植性

由於沒有相依性，DuckDB 具有極高的可移植性。它可以在所有主要作業系統（Linux、macOS、Windows）和 CPU 架構（x86、ARM）上編譯。它可以從小型、資源受限的邊緣裝置部署到具有 100 多個 CPU 核心的大型多 TB 記憶體伺服器。使用 [DuckDB-Wasm]({% link docs/stable/clients/wasm/overview.md %})，DuckDB 甚至可以在網頁瀏覽器和行動電話上執行。

DuckDB 提供 [Java、C、C++、Go、Node.js 和其他語言的 API]({% link docs/stable/clients/overview.md %})。

### 功能豐富

DuckDB 提供嚴謹的資料管理功能。它廣泛支援 SQL 中的**複雜查詢**，擁有大量函數庫、視窗函數等。DuckDB 透過我們自訂的、針對批量最佳化的[多版本並行控制（MVCC）](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)提供**交易保證**（ACID 特性）。資料可以儲存在持久化的**單檔案資料庫**中。DuckDB 支援次要索引以加速嘗試尋找單一表格項目的查詢。

DuckDB 深度整合於 Python 和 R 中，以實現高效的互動式資料分析。

### 快速

DuckDB 旨在支援**分析查詢工作負載**，也稱為[線上分析處理（OLAP）](https://en.wikipedia.org/wiki/Online_analytical_processing)。這些工作負載的特點是複雜、相對長時間執行的查詢，會處理儲存資料集的大部分，例如對整個表格的彙總或多個大型表格之間的連接。對資料的變更預期也是相當大規模的，會同時附加多列，或同時變更或新增表格的大部分。

為了有效支援這種工作負載，關鍵是減少每個個別值所耗費的 CPU 週期數量。實現此目標的最新技術是[向量化或即時編譯查詢執行引擎](https://www.vldb.org/pvldb/vol11/p2209-kersten.pdf)。DuckDB 使用**列式向量化查詢執行引擎**，其中查詢仍然是被解釋的，但一次處理一大批值（一個「向量」）。這大大減少了傳統系統（如 PostgreSQL、MySQL 或 SQLite）中存在的開銷，這些系統會依序處理每一列。向量化查詢執行在 OLAP 查詢中帶來更好的效能。

### 可擴展性

DuckDB 提供[靈活的擴展機制]({% link docs/stable/core_extensions/overview.md %})，允許定義新的資料類型、函數、檔案格式和新的 SQL 語法。事實上，DuckDB 的許多關鍵功能，例如對 [Parquet 檔案格式]({% link docs/stable/data/parquet/overview.md %})、[JSON]({% link docs/stable/data/json/overview.md %})、[時區]({% link docs/stable/core_extensions/icu.md %})的支援，以及對 [HTTP(S) 和 S3 協定]({% link docs/stable/core_extensions/httpfs/overview.md %})的支援，都是作為擴展實作的。擴展也[可以在 DuckDB Wasm 中運作]({% post_url 2023-12-18-duckdb-extensions-in-wasm %})。
使用者貢獻可作為[社群擴展]({% link community_extensions/index.md %})提供。

### 免費

DuckDB 的開發始於主要開發者在荷蘭擔任公職人員期間。我們認為將我們工作的成果免費提供給荷蘭或其他地方的任何人是我們對社會的責任和義務。這就是為什麼 DuckDB 以非常寬鬆的 [MIT 授權](https://en.wikipedia.org/wiki/MIT_License)發布，並且專案的智慧財產權由 [DuckDB 基金會]({% link foundation/index.html %})持有。我們邀請任何人的貢獻，只要他們遵守我們的[行為準則]({% link code_of_conduct.md %})。

### 經過充分測試

雖然 DuckDB 最初是由研究小組創建的，但它從未打算成為研究原型。相反，它旨在成為一個穩定且成熟的資料庫系統。為了促進這種穩定性，DuckDB 使用[持續整合](https://github.com/duckdb/duckdb/actions)進行密集測試。DuckDB 的測試套件目前包含數百萬個查詢，包括改編自 SQLite、PostgreSQL 和 MonetDB 測試套件的查詢。測試在各種平台和編譯器上重複執行。每個拉取請求都會針對完整的測試設定進行檢查，只有通過測試才會合併。

除了這個測試套件，我們還執行各種在高負載下壓力測試 DuckDB 的測試。我們執行 [TPC-H]({% link docs/stable/core_extensions/tpch.md %}) 和 [TPC-DS]({% link docs/stable/core_extensions/tpcds.md %}) 基準測試，並執行各種測試，其中 DuckDB 被許多客戶端並行使用。

## 同行評審的論文和學位論文

* [Runtime-Extensible Parsers]({% link pdf/CIDR2025-muehleisen-raasveldt-extensible-parsers.pdf %}) (CIDR 2025)
* [Robust External Hash Aggregation in the Solid State Age]({% link pdf/ICDE2024-kuiper-boncz-muehleisen-out-of-core.pdf %}) (ICDE 2024)
* [These Rows Are Made for Sorting and That's Just What We'll Do]({% link pdf/ICDE2023-kuiper-muehleisen-sorting.pdf %}) (ICDE 2023)
* [Join Order Optimization with (Almost) No Statistics](https://blobs.duckdb.org/papers/tom-ebergen-msc-thesis-join-order-optimization-with-almost-no-statistics.pdf)（碩士論文，2022）
* [DuckDB-Wasm: Fast Analytical Processing for the Web]({% link pdf/VLDB2022-kohn-duckdb-wasm.pdf %}) (VLDB 2022 Demo)
* [Data Management for Data Science - Towards Embedded Analytics]({% link pdf/CIDR2020-raasveldt-muehleisen-duckdb.pdf %}) (CIDR 2020)
* [DuckDB: an Embeddable Analytical Database]({% link pdf/SIGMOD2019-demo-duckdb.pdf %}) (SIGMOD 2019 Demo)

## 使用/為 DuckDB 建置的專案

若要了解使用 DuckDB 的專案，請造訪 [Awesome DuckDB 儲存庫](https://github.com/davidgasquez/awesome-duckdb)。

## 站在巨人的肩膀上

DuckDB 使用來自各種開源專案的一些元件，並從科學出版物中汲取靈感。我們對此非常感激。以下是概述：

* **執行引擎**：向量化執行引擎受到 Peter Boncz、Marcin Zukowski 和 Niels Nes 的論文 [MonetDB/X100: Hyper-Pipelining Query Execution](http://cidrdb.org/cidr2005/papers/P19.pdf) 的啟發。MonetDB/X100 後來成為 [Vectorwise (Actian Vector) 資料庫系統](https://ir.cwi.nl/pub/19958/19958B.pdf)。
* **最佳化器**：DuckDB 的最佳化器受到 Guido Moerkotte 和 Thomas Neumann 的論文 [Dynamic programming strikes back](https://15721.courses.cs.cmu.edu/spring2020/papers/20-optimizer2/p539-moerkotte.pdf) 以及 Thomas Neumann 和 Alfons Kemper 的 [Unnesting Arbitrary Queries](http://www.btw-2015.de/res/proceedings/Hauptband/Wiss/Neumann-Unnesting_Arbitrary_Querie.pdf) 的啟發。
* **並行控制**：我們的 MVCC 實作受到 Thomas Neumann、Tobias Mühlbauer 和 Alfons Kemper 的論文 [Fast Serializable Multi-Version Concurrency Control for Main-Memory Database Systems](https://db.in.tum.de/~muehlbau/papers/mvcc.pdf) 的啟發。
* **次要索引**：DuckDB 支援基於 Viktor Leis、Alfons Kemper 和 Thomas Neumann 的論文 [The Adaptive Radix Tree: ARTful Indexing for Main-Memory Databases](https://db.in.tum.de/~leis/papers/ART.pdf) 的次要索引。
* **SQL 視窗函數**：DuckDB 的視窗函數實作使用 Viktor Leis、Kan Kundhikanjana、Alfons Kemper 和 Thomas Neumann 的論文 [Efficient Processing of Window Functions in Analytical SQL Queries](https://www.vldb.org/pvldb/vol8/p1058-leis.pdf) 中描述的分段樹聚合。
* **SQL 不等連接**：DuckDB 的不等連接實作使用 Zuhair Khayyat、William Lucia、Meghna Singh、Mourad Ouzzani、Paolo Papotti、Jorge-Arnulfo Quiané-Ruiz、Nan Tang 和 Panos Kalnis 的論文 [Lightning Fast and Space Efficient Inequality Joins](https://vldb.org/pvldb/vol8/p2074-khayyat.pdf) 中描述的 IEJoin 演算法。
* **浮點數值壓縮**：DuckDB 支援多種壓縮浮點數值的演算法：
    * Panagiotis Liakos、Katia Papakonstantinopoulou 和 Yannis Kotidi 的 [Chimp]({% link _science/2024-03-25-fp-compression.md %})
    * [Patas](https://github.com/duckdb/duckdb/pull/5044)，內部開發
    * Azim Afroozeh、Leonard Kuffo 和 Peter Boncz 的 [ALP（自適應無損浮點壓縮）]({% link _science/2024-06-09-alp.md %})，他們也[貢獻了其實作](https://github.com/duckdb/duckdb/pull/9635)
* **SQL 解析器**：我們使用被[重新打包為獨立函式庫](https://github.com/lfittl/libpg_query)的 PostgreSQL 解析器。轉換到我們自己的解析樹的靈感來自 [Peloton](https://db.cs.cmu.edu/peloton/)。
* **命令列介面**：我們使用 [SQLite shell](https://sqlite.org/cli.html) 來操作 DuckDB。
* **正規表達式**：DuckDB 使用 Google 的 [RE2](https://github.com/google/re2) 正規表達式引擎。
* **字串格式化**：DuckDB 使用 [fmt](https://github.com/fmtlib/fmt) 字串格式化函式庫。
* **UTF 處理**：DuckDB 使用 [utf8proc](https://juliastrings.github.io/utf8proc/) 函式庫來檢查和正規化 UTF8。
* **排序和時間**：DuckDB 使用 [ICU](https://unicode-org.github.io/icu/) 函式庫來支援排序、時區和日曆。
* **測試框架**：DuckDB 使用 [Catch2](https://github.com/catchorg/Catch2) 單元測試框架。
* **測試案例**：我們使用 [SQLite 的 SQL Logic Tests](https://www.sqlite.org/sqllogictest/doc/trunk/about.wiki) 來測試 DuckDB。
* **結果驗證**：[Manuel Rigger](https://www.manuelrigger.at) 使用他優秀的 [SQLancer](https://github.com/sqlancer/sqlancer) 工具來驗證 DuckDB 結果的正確性。
* **查詢模糊測試**：我們透過 [`sqlsmith` 擴展]({% link docs/stable/core_extensions/sqlsmith.md %})使用 [SQLsmith](https://github.com/anse1/sqlsmith) 來產生隨機查詢進行額外測試。
* **JSON 解析器**：我們使用 [yyjson](https://github.com/ibireme/yyjson)，一個用 ANSI C 編寫的高效能 JSON 函式庫，在 DuckDB 的 [JSON 擴展]({% link docs/stable/data/json/overview.md %})中解析 JSON。
* **遞迴 CTE 中的 `USING KEY`**：來自圖賓根大學的創新想法,允許將遞迴共同表表達式中的中間結果視為鍵值字典，從而顯著提升效能和記憶體使用。請參閱論文 ["How DuckDB is `USING KEY` to Unlock Recursive Query Performance"]({% link _science/2025-06-22-bamberg-using-key-sigmod.md %})。
