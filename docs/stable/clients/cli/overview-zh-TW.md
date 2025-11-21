---
layout: docu
redirect_from:
- /docs/api/cli
- /docs/api/cli/
- /docs/clients/cli
- /docs/clients/cli/
- /docs/api/cli/overview
- /docs/api/cli/overview/
- /docs/clients/cli/overview
title: 命令列用戶端
lang: zh-TW
---

> DuckDB 命令列用戶端的最新穩定版本是 {{ site.current_duckdb_version }}。

## 安裝

DuckDB CLI（命令列介面）是一個單一、無相依性的執行檔。它已針對 Windows、Mac 和 Linux 預編譯，包括穩定版本和 GitHub Actions 產生的 nightly 建置版本。請參閱 CLI 標籤下的[安裝頁面]({% link install/index.html %})以取得下載連結。

DuckDB CLI 基於 SQLite 命令列 shell，因此 CLI 用戶端特定的功能類似於 [SQLite 文件](https://www.sqlite.org/cli.html)中所描述的內容（儘管 DuckDB 的 SQL 語法遵循 PostgreSQL 慣例，[有一些例外]({% link docs/stable/sql/dialect/postgresql_compatibility.md %})）。

> DuckDB 有一個 [tldr 頁面](https://tldr.inbrowser.app/pages/common/duckdb)，總結了 CLI 用戶端最常見的用法。
> 如果您已安裝 [tldr](https://github.com/tldr-pages/tldr)，您可以執行 `tldr duckdb` 來顯示它。

## 入門

下載 CLI 執行檔後，將其解壓縮並儲存到任何目錄。
在終端機中瀏覽到該目錄並輸入命令 `duckdb` 來執行該執行檔。
如果在 PowerShell 或 POSIX shell 環境中，請改用命令 `./duckdb`。

## 使用方式

`duckdb` 命令的典型用法如下：

```batch
duckdb ⟨OPTIONS⟩ ⟨FILENAME⟩
```

### 選項

`⟨OPTIONS⟩`{:.language-sql .highlight} 部分編碼 [CLI 用戶端的參數]({% link docs/stable/clients/cli/arguments.md %})。常見選項包括：

* `-csv`：將輸出模式設定為 CSV
* `-json`：將輸出模式設定為 JSON
* `-readonly`：以唯讀模式開啟資料庫（請參閱 [DuckDB 中的並行處理]({% link docs/stable/connect/concurrency.md %}#handling-concurrency)）

有關選項的完整清單，請參閱[命令列參數頁面]({% link docs/stable/clients/cli/arguments.md %})。

### 記憶體資料庫與持久化資料庫

當未提供 `⟨FILENAME⟩`{:.language-sql .highlight} 參數時，DuckDB CLI 將開啟一個暫時的[記憶體資料庫]({% link docs/stable/connect/overview.md %}#in-memory-database)。
您將看到 DuckDB 的版本號、連接資訊以及以 `D` 開頭的提示符號。

```batch
duckdb
```

```text
DuckDB v{{ site.current_duckdb_version }} ({{ site.current_duckdb_codename }}) {{ site.current_duckdb_hash }}
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
D
```

若要開啟或建立[持久化資料庫]({% link docs/stable/connect/overview.md %}#persistent-database)，只需將路徑作為命令列參數包含即可：

```batch
duckdb my_database.duckdb
```

### 在 CLI 中執行 SQL 陳述式

開啟 CLI 後，輸入一個 SQL 陳述式，後面接著分號，然後按 Enter，它就會被執行。結果將以表格形式顯示在終端機中。如果省略分號，按 Enter 將允許輸入多行 SQL 陳述式。

```sql
SELECT 'quack' AS my_column;
```

| my_column |
|-----------|
| quack     |

CLI 支援 DuckDB 所有豐富的 [SQL 語法]({% link docs/stable/sql/introduction.md %})，包括 `SELECT`、`CREATE` 和 `ALTER` 陳述式。

### 編輯器功能

CLI 支援[自動完成]({% link docs/stable/clients/cli/autocomplete.md %})，並在某些平台上具有複雜的[編輯器功能]({% link docs/stable/clients/cli/editing.md %})和[語法突顯]({% link docs/stable/clients/cli/syntax_highlighting.md %})。

### 退出 CLI

若要退出 CLI，如果您的平台支援，請按 `Ctrl`+`D`。否則，請按 `Ctrl`+`C` 或使用 `.exit` 命令。如果您使用持久化資料庫，DuckDB 將自動檢查點（將最新編輯儲存到磁碟）並關閉。這將移除 `.wal` 檔案（[預寫式日誌](https://en.wikipedia.org/wiki/Write-ahead_logging)）並將所有資料合併到單一檔案資料庫中。

### 點命令

除了 SQL 語法之外，還可以在 CLI 用戶端中輸入特殊的[點命令]({% link docs/stable/clients/cli/dot_commands.md %})。若要使用其中一個命令，請在行首使用句點（`.`），緊接著您想執行的命令名稱。命令的其他參數在命令後以空格分隔輸入。如果參數必須包含空格，可以使用單引號或雙引號來包裝該參數。點命令必須在單行上輸入，且句點之前不能有空格。行末不需要分號。

經常使用的組態可以儲存在檔案 `~/.duckdbrc` 中，該檔案將在啟動 CLI 用戶端時載入。有關這些選項的詳細資訊，請參閱下面的[設定 CLI](#設定-cli) 區段。

> 提示：若要防止 DuckDB CLI 用戶端讀取 `~/.duckdbrc` 檔案，請如下啟動它：
> ```batch
> duckdb -init /dev/null
> ```

下面，我們總結了一些重要的點命令。若要查看所有可用命令，請參閱[點命令頁面]({% link docs/stable/clients/cli/dot_commands.md %})或使用 `.help` 命令。

#### 開啟資料庫檔案

除了在開啟 CLI 時連接到資料庫之外，還可以使用 `.open` 命令建立新的資料庫連接。如果沒有提供其他參數，將建立新的記憶體資料庫連接。當 CLI 連接關閉時，此資料庫將不會持久化。

```text
.open
```

`.open` 命令可選擇接受多個選項，但最後一個參數可用於指示持久化資料庫的路徑（或應建立的位置）。特殊字串 `:memory:` 也可用於開啟暫時記憶體資料庫。

```text
.open persistent.duckdb
```

> 警告：`.open` 會關閉目前的資料庫。
> 若要保留目前的資料庫，同時新增新資料庫，請使用 [`ATTACH` 陳述式]({% link docs/stable/sql/statements/attach.md %})。

`.open` 接受的一個重要選項是 `--readonly` 旗標。這將禁止對資料庫進行任何編輯。若要以唯讀模式開啟，資料庫必須已經存在。這也意味著無法以唯讀模式開啟新的記憶體資料庫，因為記憶體資料庫是在連接時建立的。

```text
.open --readonly preexisting.duckdb
```

#### 輸出格式

`.mode` [點命令]({% link docs/stable/clients/cli/dot_commands.md %}#mode)可用於變更終端機輸出中返回的表格的外觀。
這些包括預設的 `duckbox` 模式、用於其他工具擷取的 `csv` 和 `json` 模式、用於文件的 `markdown` 和 `latex`，以及用於產生 SQL 陳述式的 `insert` 模式。

#### 將結果寫入檔案

預設情況下，DuckDB CLI 將結果傳送到終端機的標準輸出。但是，可以使用 `.output` 或 `.once` 命令修改此設定。
詳情請參閱 [output 點命令文件]({% link docs/stable/clients/cli/dot_commands.md %}#output-writing-results-to-a-file)。

#### 從檔案讀取 SQL

DuckDB CLI 可以使用 `.read` 命令從外部檔案讀取 SQL 命令和點命令，而不是從終端機讀取。這允許按順序執行多個命令，並允許儲存和重複使用命令序列。

`.read` 命令只需要一個參數：包含要執行的 SQL 和/或命令的檔案路徑。執行檔案中的命令後，控制權將恢復到終端機。該檔案執行的輸出受先前討論的相同 `.output` 和 `.once` 命令控制。這允許輸出顯示回終端機，如下面的第一個範例，或輸出到另一個檔案，如第二個範例。

在此範例中，檔案 `select_example.sql` 位於與 duckdb.exe 相同的目錄中，並包含以下 SQL 陳述式：

```sql
SELECT *
FROM generate_series(5);
```

若要從 CLI 執行它，請使用 `.read` 命令。

```text
.read select_example.sql
```

預設情況下，以下輸出將返回到終端機。可以使用 `.output` 或 `.once` 命令調整表格的格式。

```text
| generate_series |
|----------------:|
| 0               |
| 1               |
| 2               |
| 3               |
| 4               |
| 5               |
```

多個命令，包括 SQL 和點命令，也可以在單一 `.read` 命令中執行。在此範例中，檔案 `write_markdown_to_file.sql` 位於與 duckdb.exe 相同的目錄中，並包含以下命令：

```sql
.mode markdown
.output series.md
SELECT *
FROM generate_series(5);
```

若要從 CLI 執行它，請如前所述使用 `.read` 命令。

```text
.read write_markdown_to_file.sql
```

在這種情況下，不會有輸出返回到終端機。相反，將建立檔案 `series.md`（如果已存在則替換），其中包含此處顯示的 markdown 格式結果：

```text
| generate_series |
|----------------:|
| 0               |
| 1               |
| 2               |
| 3               |
| 4               |
| 5               |
```

<!-- The edit function does not appear to work -->

## 設定 CLI

可以使用多個點命令來設定 CLI。
啟動時，CLI 會讀取並執行檔案 `~/.duckdbrc` 中的所有命令，包括點命令和 SQL 陳述式。
這允許您儲存 CLI 的組態狀態。
您也可以使用 `-init` 指向不同的初始化檔案。

### 設定自訂提示符號

例如，與 DuckDB CLI 位於同一目錄中名為 `prompt.sql` 的檔案將把 DuckDB 提示符號變更為鴨子頭並執行 SQL 陳述式。
請注意，鴨子頭是使用 Unicode 字元建立的，並不適用於所有終端機環境（例如，在 Windows 中，除非使用 WSL 並使用 Windows Terminal）。

```text
.prompt '⚫◗ '
```

若要在初始化時呼叫該檔案，請使用此命令：

```batch
duckdb -init prompt.sql
```

這會輸出：

```text
-- Loading resources from prompt.sql
v⟨version⟩ ⟨git_hash⟩
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
⚫◗
```

## 非互動式使用

若要讀取/處理檔案並立即退出，請將檔案內容重新導向到 `duckdb`：

```batch
duckdb < select_example.sql
```

若要執行直接從命令列傳入的 SQL 文字命令，請使用兩個參數呼叫 `duckdb`：資料庫位置（或 `:memory:`），以及包含要執行的 SQL 陳述式的字串。

```batch
duckdb :memory: "SELECT 42 AS the_answer"
```

## 載入擴充套件

若要載入擴充套件，請像使用其他 SQL 陳述式一樣使用 DuckDB 的 SQL `INSTALL` 和 `LOAD` 命令。

```sql
INSTALL fts;
LOAD fts;
```

詳情請參閱[擴充套件文件]({% link docs/stable/extensions/overview.md %})。

## 從 stdin 讀取和寫入 stdout

在 Unix 環境中，在多個命令之間傳輸資料可能很有用。
DuckDB 能夠從 stdin 讀取資料以及寫入 stdout，方法是在 SQL 命令中使用 stdin（`/dev/stdin`）和 stdout（`/dev/stdout`）的檔案位置，因為管道的行為與檔案控制代碼非常相似。

此命令將建立一個範例 CSV：

```sql
COPY (SELECT 42 AS woot UNION ALL SELECT 43 AS woot) TO 'test.csv' (HEADER);
```

首先，讀取一個檔案並將其傳輸到 `duckdb` CLI 執行檔。作為 DuckDB CLI 的參數，傳入要開啟的資料庫位置，在這種情況下是記憶體資料庫，以及一個使用 `/dev/stdin` 作為檔案位置的 SQL 命令。

```batch
cat test.csv | duckdb -c "SELECT * FROM read_csv('/dev/stdin')"
```

| woot |
|-----:|
| 42   |
| 43   |

若要寫回 stdout，可以使用 copy 命令與 `/dev/stdout` 檔案位置。

```batch
cat test.csv | \
    duckdb -c "COPY (SELECT * FROM read_csv('/dev/stdin')) TO '/dev/stdout' WITH (FORMAT csv, HEADER)"
```

```csv
woot
42
43
```

## 讀取環境變數

`getenv` 函式可以讀取環境變數。

### 範例

若要從 `HOME` 環境變數擷取家目錄的路徑，請使用：

```sql
SELECT getenv('HOME') AS home;
```

|       home       |
|------------------|
| /Users/user_name |

`getenv` 函式的輸出可用於設定[組態選項]({% link docs/stable/configuration/overview.md %})。例如，若要根據環境變數 `DEFAULT_NULL_ORDER` 設定 `NULL` 順序，請使用：

```sql
SET default_null_order = getenv('DEFAULT_NULL_ORDER');
```

### 讀取環境變數的限制

只有當 [`enable_external_access`]({% link docs/stable/configuration/overview.md %}#configuration-reference) 選項設定為 `true`（預設設定）時，才能執行 `getenv` 函式。
它僅在 CLI 用戶端中可用，在其他 DuckDB 用戶端中不受支援。

## 預備陳述式

DuckDB CLI 支援執行[預備陳述式]({% link docs/stable/sql/query_syntax/prepared_statements.md %})以及一般的 `SELECT` 陳述式。
若要在 CLI 用戶端中建立和執行預備陳述式，請使用 `PREPARE` 子句和 `EXECUTE` 陳述式。

## 查詢完成預估時間

DuckDB 的 CLI 現在為正在執行的查詢提供智慧的完成時間預估，並在完成時顯示總執行時間。

在 DuckDB CLI 中執行查詢時，進度列會顯示預估的剩餘完成時間。此功能採用進階統計建模（[卡爾曼濾波](https://en.wikipedia.org/wiki/Kalman_filter)）以提供比簡單線性外推更準確的預測。

### 運作方式

DuckDB 透過以下流程計算預估的完成時間：

1. 進度監控：DuckDB 的內部進度 API 回報正在執行的查詢的預估完成百分比
2. 統計濾波：卡爾曼濾波器平滑雜訊進度測量並考慮執行變異性
3. 持續改進：系統在新進度資料可用時持續更新預測完成時間，在整個執行過程中提高準確性

卡爾曼濾波器適應不斷變化的執行條件，例如記憶體壓力、I/O 瓶頸或網路延遲。這種自適應方法意味著預估完成時間可能並不總是線性減少——當查詢執行變得不太可預測時，預估可能會增加。

### 影響查詢完成預估時間準確性的因素

在以下條件下，完成時間預估可能不太可靠：

系統資源約束：

* 導致磁碟交換的記憶體壓力
* 來自競爭行程的高 CPU 負載
* 磁碟 I/O 瓶頸

查詢執行特性：

* 可變執行階段（初始設定與主要處理）
* 具有不一致延遲的網路相依操作
* 具有不可預測分支邏輯的查詢
* 對遠端資料來源的操作
* 外部函式呼叫
* 高度偏斜的資料分佈
