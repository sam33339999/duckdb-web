# 貢獻指南

* [貢獻指南](#貢獻指南)
  * [行為準則](#行為準則)
  * [為 DuckDB 文件做出貢獻](#為-duckdb-文件做出貢獻)
  * [適用範圍](#適用範圍)
  * [新增頁面](#新增頁面)
  * [風格指南](#風格指南)
    * [格式化](#格式化)
    * [標題](#標題)
    * [SQL 風格](#sql-風格)
    * [Python 風格](#python-風格)
    * [拼寫](#拼寫)
  * [範例程式碼片段](#範例程式碼片段)
  * [交叉引用](#交叉引用)
  * [預覽版、穩定版和版本化頁面](#預覽版穩定版和版本化頁面)
  * [自動產生的頁面](#自動產生的頁面)
    * [自動產生的 SQL 函數列表](#自動產生的-sql-函數列表)
  * [注意事項](#注意事項)

## 行為準則

本專案及參與其中的所有人均受[行為準則](code_of_conduct.md)約束。參與本專案即表示您同意遵守此準則。請將不當行為回報至 [quack@duckdb.org](mailto:quack@duckdb.org)。

## 為 DuckDB 文件做出貢獻

我們歡迎為 [DuckDB 文件](https://duckdb.org/)做出貢獻。若要提交貢獻，請在 [`duckdb/duckdb-web`](https://github.com/duckdb/duckdb-web) 儲存庫中開啟[拉取請求（Pull Request）](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)。

## 適用範圍

在提交貢獻前，請先確認您的貢獻是否符合以下條件：

1. 在建立新頁面之前，請先搜尋現有文件中是否有類似的頁面。
2. 一般而言，使用 DuckDB 的第三方工具指南不應包含在 DuckDB 文件中。相反地，這些工具及其文件應該收錄在 [Awesome DuckDB 社群儲存庫](https://github.com/davidgasquez/awesome-duckdb)中。

## 新增頁面

感謝您為 DuckDB 文件做出貢獻！

每個新頁面至少需要進行 2 項編輯：

* 建立新的 Markdown 檔案（使用 `snake_case` 命名慣例）。請參照 `docs/stable` 資料夾中其他 `.md` 檔案的格式。
* 在 `_data/menu_docs_stable.json` 中加入新頁面的連結。這會填充側邊選單。

新增指南還需要額外的一項編輯：

* 在指南登陸頁面中加入新頁面的連結：`docs/guides/overview.md`

在建立拉取請求之前，請執行以下步驟：

* 使用[網站建置指南](BUILDING.md)在瀏覽器中預覽您的變更。
* 執行 `scripts/lint.sh` 來檢查潛在問題，並執行 `scripts/lint.sh -f` 來自動修正 markdownlint 的問題。

在建立 PR 時，請勾選「允許維護者編輯」的選項。這樣維護者可以在合併拉取請求之前進行小幅調整。

## 風格指南

提交拉取請求時，請遵守以下風格指南。

這些風格指南中的部分內容已透過 GitHub Actions 自動化，但您也可以執行 [`scripts/lint.sh`](scripts/lint.sh) 在本機執行檢查。

### 格式化

* 使用 [GitHub 的 Markdown 語法](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)進行格式化。
* 不要在文字區塊中強制換行。
* 使用適當的語言標記格式化程式碼區塊（例如 \`\`\`sql CODE\`\`\`）。
* 若要讓 SQL 程式碼區塊以 DuckDB 的 `D` 提示符號開始，請使用 `plsql` 語言標記（例如 \`\`\`plsql CODE\`\`\`）。兩者使用相同的語法高亮器，唯一的差異是是否顯示 `D` 提示符號。
* 使用 `batch` 語言標記的程式碼區塊在呈現時會自動加上 `$` 提示符號。若要顯示不含提示符號的 Bash 程式碼區塊，請使用 `bash` 語言標記（例如 \`\`\`batch CODE\`\`\`）。兩者使用相同的語法高亮器，唯一的差異是是否顯示提示符號。
* 若要顯示不帶語言標記的文字區塊（例如腳本的輸出），請使用 \`\`\`text OUTPUT\`\`\`。
* 若要顯示錯誤訊息，請使用 \`\`\`console ERROR MESSAGE\`\`\`。
* 引用區塊（以 `>` 開頭的行）會呈現為[彩色方框](https://duckdb.org/docs/stable/data/insert)。可使用的方框類型包括：`Note`（預設）、`Warning`、`Tip`、`Bestpractice`、`Deprecated`。
* 在提及 SQL 程式碼、變數名稱、函數名稱等時，請始終使用程式碼格式。例如，在討論 `CREATE TABLE` 陳述式時，關鍵字應該格式化為程式碼。
* 在呈現 SQL 陳述式時，不要包含 DuckDB 提示符號（`D `）。
* SQL 陳述式應以分號（`;`）結尾，以便讀者快速將其貼到 SQL 控制台中。
* 主要包含程式碼輸出的表格（例如 `DESCRIBE` 陳述式的結果）應在前面加上一個具有 `monospace_table` 類別的空 div：`<div class="monospace_table"></div>`。
* 標題應置中對齊（而非預設的靠左對齊）的表格應在前面加上一個具有 `center_aligned_header_table` 類別的空 div：`<div class="center_aligned_header_table"></div>`。
* 盡可能避免引入強制換行。因此，避免使用 `<br/>` HTML 標記，並避免[在 Markdown 中於行尾使用雙空格](https://spec.commonmark.org/0.28/#hard-line-breaks)。
* 單引號和雙引號字元（`'` 和 `"`）不會自動轉換為智慧引號。若要插入這些字元，請使用 `"` `"` 和 `'` `'`。
* 在引用其他文章時，將標題用引號括起來，例如 `請參閱 ["Lightweight Compression in DuckDB" 部落格文章]({% post_url 2022-10-28-lightweight-compression %})`。
* 對於無序列表，使用 `*`。如果列表有多個層級，請使用 **4 個空格**進行縮排。

> [!TIP]
> 在 VS Code 中，您可以設定 [Markdown All in One 擴展](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)來使用星號作為產生頁面目錄時的預設標記，方法是在 `settings.json` 中加入以下設定：
> `"markdown.extension.toc.unorderedList.marker": "*"`

### 標題

* 頁面的標題應編碼在前言（front matter）的 `title` 屬性中。頁面主體不應以重複此標題開始。
* 在頁面主體中，僅使用以下層級的標題：h2（`##`）、h3（`###`）和 h4（`####`）。
* 使用[芝加哥格式手冊](https://capitalizemytitle.com/style/Chicago/)中定義的標題大小寫規則。

### SQL 風格

* 使用 **4 個空格**進行縮排。
* 使用大寫 SQL 關鍵字，例如 `SELECT 42 AS x, 'hello world' AS y FROM ...;`。
* 使用小寫函數名稱，例如 `SELECT cos(pi()), date_part('year', DATE '1992-09-20');`。
* 表格和欄位名稱使用蛇形命名法（小寫加底線分隔），例如 `SELECT departure_time FROM train_services;`。
* 在逗號和運算子周圍加上空格，例如 `SELECT FROM tbl WHERE x > 42;`。
* 在每個 SQL 陳述式的結尾加上分號，例如 `SELECT 42 AS x;`。
* 逗號應放在每行的結尾。
* _不要_僅為了對齊行而加入子句或運算式。例如，避免加入 `WHERE 1 = 1` 和 `WHERE true`。
* _不要_包含 DuckDB 提示符號。例如，避免以下形式：`D SELECT 42;`。
* 允許使用 DuckDB 的語法擴充功能，例如 [`FROM-first` 語法](https://duckdb.org/docs/sql/query_syntax/from)和 [`GROUP BY ALL`](https://duckdb.org/docs/sql/query_syntax/groupby#group-by-all)，但在介紹新功能時請謹慎使用。
* 較大的返回表格應使用 DuckDB CLI 的 markdown 模式（`.mode markdown`）格式化，並將 NULL 值呈現為 `NULL`（`.nullvalue NULL`）。對於較小的表格，使用預設的 `.mode duckbox` 是可以接受的。
* 系統控制台上列印的輸出應使用 `text` 語言標記格式化為程式碼。例如：
   ````
   ```text
   DuckDB v1.3.1 (Ossivalis) 2063dda3e6
   ```
   ````
* 錯誤訊息應使用 `console` 語言標記格式化為程式碼。例如：
   ````
   ```console
   Error: Constraint Error: Duplicate key "i: 1" violates primary key constraint.
   ```
   ````
* 若要指定占位符（或範本樣式的程式碼），請使用左角括號和右角括號字元 `⟨` 和 `⟩`，也稱為角括號（chevrons）或尖括號（angle brackets）。這些字元會以紅色高亮顯示，並以等寬粗斜體排版，以引起讀者的注意。
     * 例如，您可以這樣寫：若要從 Parquet 檔案建立表格，請執行：`CREATE TABLE ⟨your_table_name⟩ AS FROM '⟨your_filename⟩.parquet'`。
     * 從這裡複製字元：`⟨⟩`。
     * 這些字元在 LaTeX 程式碼中稱為 `\langle` 和 `\rangle`。
     * 包含占位符的行內程式碼片段應高亮顯示為 SQL 程式碼。可以透過在程式碼片段後附加 `{:.language-sql .highlight}` 來實現（大括號前不需要空格）。
     * *避免*為此目的使用算術比較字元 `<` 和 `>`、方括號 `[` 和 `]`、大括號 `{` 和 `}`。

### Python 風格

* 使用 **4 個空格**進行縮排。
* 預設使用雙引號（`"`）表示字串。

### 拼寫

* 使用[美式英語（en-US）拼寫](https://en.wikipedia.org/wiki/Oxford_spelling#Language_tag_comparison)。
* 避免使用牛津逗號。

## 範例程式碼片段

* 非常歡迎說明功能使用的範例。在適用的情況下，請考慮在頁面開頭放置一些簡單的範例，以展示所描述功能的最常見用法。
* 如果可能，範例應該是獨立且可重現的。例如，範例中使用的表格必須作為範例程式碼片段的一部分建立。

## 交叉引用

* 在適用的情況下，加入指向文件中其他相關頁面的交叉引用。
* 使用 Jekyll 的[連結標記](https://jekyllrb.com/docs/liquid/tags/#link)來連結到頁面。
    * 例如，要連結到 `SELECT` 陳述式頁面上的範例部分，請使用 `{% link docs/stable/sql/statements/select.md %}#examples`。
    * 連結標記可確保只有在連結指向現有頁面時才會編譯和部署文件。
    * 請注意，路徑必須包含正確的副檔名（最常見的是 `.md`），並且路徑必須相對於儲存庫根目錄。
    * :x: ```請參閱 [the `SELECT` statement](../../sql/statements/select)```
    * :white_check_mark: ```請參閱 [the `SELECT` statement]({% link docs/stable/sql/statements/select.md %})```
* 避免在連結中使用「這裡」一詞。有關原因，請參閱[為什麼您的連結不應該寫「點擊這裡」的詳細解釋](https://uxmovement.com/content/why-your-links-should-never-say-click-here/)。
    * :x: `請參閱[這裡]({% link docs/stable/sql/statements/copy.md %}#copy-from)`
    * :white_check_mark: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy-from)```
* 盡可能引用特定部分：
    * :x: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %})```
    * :white_check_mark: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy-from)```
* 在大多數情況下，不建議連結相關的 GitHub issues/discussions。這使得文件可以自成一體。

## 預覽版、穩定版和版本化頁面

* <https://duckdb.org/docs/preview/> 下的 `preview` 頁面包含 DuckDB 最新預覽版（nightly）的文件。
* <https://duckdb.org/docs/stable/> 下的 `stable` 頁面包含 DuckDB 最新穩定版（例如 v1.2）的文件。**大多數拉取請求應針對這些頁面。**
* 版本化頁面（例如 <https://duckdb.org/docs/v1.0/>）包含 DuckDB 舊穩定版本的文件。我們通常只接受對最新穩定版本的貢獻。只有在包含嚴重錯誤的情況下，我們才會維護舊頁面。

## 自動產生的頁面

文件中的許多頁面都是自動產生的。在編輯之前,請檢查 [`scripts/generate_all_docs.sh`](scripts/generate_all_docs.sh) 腳本。避免直接編輯產生的內容，而是編輯來源檔案（通常位於 [`duckdb` 儲存庫](https://github.com/duckdb/duckdb)中），然後執行產生器腳本。

> [!TIP]
> 動態頁面重新產生可能不適用於 includes、鐵路圖（railroad diagrams）或其他內容。
> 若要強制重新產生頁面或 include 檔案，使其在本機瀏覽器中呈現，請執行 `BUILDING.md` 指南中「Jekyll 內容無法正確呈現」部分所述的步驟。

### 自動產生的 SQL 函數列表

`docs/stable/sql/functions/` 目錄中有關 SQL 函數的文件部分是由腳本 [`scripts/generate_sql_function_docs.py`](scripts/generate_sql_function_docs.py) 自動產生的。
函數文件頁面中的特定部分會列出可用的函數及其描述和標準範例。這些部分將透過直接查詢 DuckDB 目錄來產生。因此，更新函數描述和範例應在 [`duckdb` 儲存庫](https://github.com/duckdb/duckdb)中進行（搜尋名為 `functions.json` 的檔案）。

若要在文件頁面上插入函數列表，請加入以下兩行：

```md
<!-- Start of section generated by scripts/generate_sql_function_docs.py; categories: [text_similarity] -->
<!-- End of section generated by scripts/generate_sql_function_docs.py -->
```

函數列表將加在這兩行之間，並且每次產生文件時都會與 DuckDB 目錄同步。
請注意，需要在第一行的結尾設定要包含在列表中的函數類別，例如：`categories: [string,regex]`。
若要檢查目錄中函數的類別標籤，可以使用以下查詢：

```sql
SELECT DISTINCT function_name, categories
FROM duckdb_functions()
WHERE categories != [];
```

所有資料（例如參數名稱、描述、範例）均來自 `duckdb_functions()` 的輸出。任何偏差（排除、新增或覆寫）都需要在腳本 [`generate_sql_function_docs.py`](scripts/generate_sql_function_docs.py) 中透過變數 `OVERRIDES` 和 `EXCLUDES` 進行硬編碼。

範例：`blob.md`

```md
---
layout: docu
title: Blob Functions
---

<!-- markdownlint-disable MD001 -->

This section describes functions and operators for examining and manipulating [`BLOB` values]({% link docs/preview/sql/data_types/blob.md %}).

<!-- Start of section generated by scripts/generate_sql_function_docs.py; categories: [blob] -->
<!-- End of section generated by scripts/generate_sql_function_docs.py -->
```

## 注意事項

我們保留對是否合併拉取請求的完全且最終決定權。遵守這些指南並不保證您的拉取請求會被合併。
