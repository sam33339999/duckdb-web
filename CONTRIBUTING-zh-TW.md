# 貢獻

* [貢獻](#貢獻)
  * [行為準則](#行為準則)
  * [貢獻至 DuckDB 文件](#貢獻至-duckdb-文件)
  * [資格要求](#資格要求)
  * [新增頁面](#新增頁面)
  * [風格指南](#風格指南)
    * [格式化](#格式化)
    * [標題](#標題)
    * [SQL 風格](#sql-風格)
    * [Python 風格](#python-風格)
    * [拼字](#拼字)
  * [範例程式碼片段](#範例程式碼片段)
  * [交叉引用](#交叉引用)
  * [預覽、穩定版和版本化頁面](#預覽穩定版和版本化頁面)
  * [自動生成頁面](#自動生成頁面)
    * [自動生成的 SQL 函式清單](#自動生成的-sql-函式清單)
  * [注意事項](#注意事項)

## 行為準則

本專案及所有參與者均受到[行為準則](code_of_conduct.md)的約束。參與時，您應遵守此準則。請將不可接受的行為回報至 [quack@duckdb.org](mailto:quack@duckdb.org)。

## 貢獻至 DuckDB 文件

歡迎為 [DuckDB 文件](https://duckdb.org/)做出貢獻。若要提交貢獻，請在 [`duckdb/duckdb-web`](https://github.com/duckdb/duckdb-web) 儲存庫中開啟 [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)。

## 資格要求

在提交貢獻之前，請先檢查您的貢獻是否符合資格。

1. 在建立新頁面之前，請先搜尋現有文件是否有類似頁面。
2. 一般而言，使用 DuckDB 的第三方工具指南不應包含在 DuckDB 文件中。這些工具及其文件應收錄在 [Awesome DuckDB 社群儲存庫](https://github.com/davidgasquez/awesome-duckdb)中。

## 新增頁面

感謝您為 DuckDB 文件做出貢獻！

每個新頁面至少需要 2 處編輯：

* 建立新的 Markdown 檔案（使用 `snake_case` 命名慣例）。請遵循 `docs/stable` 資料夾中其他 `.md` 檔案的格式。
* 在 `_data/menu_docs_stable.json` 中新增指向新頁面的連結。這會填充側邊選單。

新增指南需要額外的一處編輯：

* 在指南登陸頁面 `docs/guides/overview.md` 中新增指向新頁面的連結。

在建立 pull request 之前，請執行以下步驟：

* 使用[網站建置指南](BUILDING.md)在瀏覽器中預覽您的變更。
* 執行 `scripts/lint.sh` 來顯示潛在問題，並執行 `scripts/lint.sh -f` 來執行 markdownlint 的修正。

建立 PR 時，請勾選「Allow edits from maintainers」方塊。這允許維護者在合併 pull request 之前進行小幅調整。

## 風格指南

提交 pull request 時，請遵守以下風格指南。

此風格指南的部分內容已透過 GitHub Actions 自動化，但您也可以執行 [`scripts/lint.sh`](scripts/lint.sh) 在本機執行它們。

### 格式化

* 使用 [GitHub 的 Markdown 語法](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)進行格式化。
* 不要在文字區塊中硬換行。
* 使用適當的語言格式化程式碼區塊（例如，\`\`\`sql CODE\`\`\`）。
* 若要讓 SQL 程式碼區塊以 DuckDB 的 `D` 提示開始，請使用 `plsql` 語言標籤（例如，\`\`\`plsql CODE\`\`\`）。這兩者使用相同的語法突顯工具，唯一的差別是 `D` 提示的存在。
* 使用 `batch` 語言標籤的程式碼區塊在渲染時會自動獲得 `$` 提示。若要排版沒有提示的 Bash 程式碼區塊，請使用 `bash` 語言標籤（例如，\`\`\`batch CODE\`\`\`）。這兩者使用相同的語法突顯工具，唯一的差別是提示的缺失。
* 若要顯示沒有語言的文字區塊（例如，腳本的輸出），請使用 \`\`\`text OUTPUT\`\`\`。
* 若要顯示錯誤訊息，請使用 \`\`\`console ERROR MESSAGE\`\`\`。
* 引用區塊（以 `>` 開頭的行）會渲染為[有顏色的方塊](https://duckdb.org/docs/stable/data/insert)。可用的方塊類型如下：`Note`（預設）、`Warning`、`Tip`、`Bestpractice`、`Deprecated`。
* 務必將 SQL 程式碼、變數名稱、函式名稱等格式化為程式碼。例如，在談論 `CREATE TABLE` 陳述式時，關鍵字應格式化為程式碼。
* 當呈現 SQL 陳述式時，不要包含 DuckDB 提示（`D `）。
* SQL 陳述式應以分號（`;`）結尾，以便讀者可以快速貼到 SQL 控制台中。
* 主要包含程式碼輸出的資料表（例如，`DESCRIBE` 陳述式的結果）應該在前面加上一個具有 `monospace_table` 類別的空 div：`<div class="monospace_table"></div>`。
* 標題應該置中對齊的資料表（相對於預設的左對齊）應該在前面加上一個具有 `center_aligned_header_table` 類別的空 div：`<div class="center_aligned_header_table"></div>`。
* 如果可能，不要引入硬換行。因此，避免使用 `<br/>` HTML 標籤，並避免[在 Markdown 行末尾使用雙空格](https://spec.commonmark.org/0.28/#hard-line-breaks)。
* 單引號和雙引號字元（`'` 和 `"`）不會自動轉換為智慧引號。若要插入這些，請使用 `"` `"` 和 `'` `'`。
* 當引用其他文章時，請將其標題放在引號中，例如 `請參閱 ["Lightweight Compression in DuckDB" 部落格文章]({% post_url 2022-10-28-lightweight-compression %})`。
* 對於無序清單，請使用 `*`。如果清單有多個層級，請使用 **4 個空格**進行縮排。

> [!TIP]
> 在 VS Code 中，您可以設定 [Markdown All in One 擴充套件](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)，在為頁面生成目錄時使用星號作為預設標記，方法是在 `settings.json` 中使用以下設定：
> `"markdown.extension.toc.unorderedList.marker": "*"`

### 標題

* 頁面標題應編碼在 front matter 的 `title` 屬性中。頁面主體不應以此標題的重複開始。
* 在頁面主體中，限制標題使用以下層級：h2（`##`）、h3（`###`）和 h4（`####`）。
* 使用[芝加哥格式手冊](https://capitalizemytitle.com/style/Chicago/)中定義的標題大小寫。

### SQL 風格

* 使用 **4 個空格**進行縮排。
* 使用大寫 SQL 關鍵字，例如 `SELECT 42 AS x, 'hello world' AS y FROM ...;`。
* 使用小寫函式名稱，例如 `SELECT cos(pi()), date_part('year', DATE '1992-09-20');`。
* 使用蛇形命名法（小寫加底線分隔）作為資料表和欄位名稱，例如 `SELECT departure_time FROM train_services;`
* 在逗號和運算子周圍加上空格，例如 `SELECT FROM tbl WHERE x > 42;`。
* 在每個 SQL 陳述式的末尾加上分號，例如 `SELECT 42 AS x;`。
* 逗號應放在每一行的末尾。
* _不要_新增純粹用於對齊行的子句或表達式。例如，避免新增 `WHERE 1 = 1` 和 `WHERE true`。
* _不要_包含 DuckDB 提示。例如，避免以下內容：`D SELECT 42;`。
* 允許使用 DuckDB 的語法擴充，例如 [`FROM-first` 語法](https://duckdb.org/docs/sql/query_syntax/from)和 [`GROUP BY ALL`](https://duckdb.org/docs/sql/query_syntax/groupby#group-by-all)，但在引入新功能時應謹慎使用。
* 較大的返回資料表應使用 DuckDB CLI 的 markdown 模式（`.mode markdown`）進行格式化，並將 NULL 值渲染為 `NULL`（`.nullvalue NULL`）。對於較小的資料表，使用預設的 `.mode duckbox` 是可以接受的。
* 在系統控制台上列印的輸出應使用 `text` 語言標籤格式化為程式碼。例如：
   ````
   ```text
   DuckDB v1.3.1 (Ossivalis) 2063dda3e6
   ```
   ````
* 錯誤訊息應使用 `console` 語言標籤格式化為程式碼。例如：
   ````
   ```console
   Error: Constraint Error: Duplicate key "i: 1" violates primary key constraint.
   ```
   ````
* 若要指定佔位符（或模板樣式程式碼），請使用左尖括號和右尖括號字元 `⟨` 和 `⟩`，也稱為角括號或尖括號。這些將以紅色突顯並以等寬粗體斜體排版以引起讀者的注意。
     * 例如，您可以這樣寫：若要從 Parquet 檔案建立資料表，請執行：`CREATE TABLE ⟨your_table_name⟩ AS FROM '⟨your_filename⟩.parquet'`。
     * 從此處複製字元：`⟨⟩`。
     * 這些字元在 LaTeX 程式碼中稱為 `\langle` 和 `\rangle`。
     * 包含佔位符的行內程式碼片段應突顯為 SQL 程式碼。這可以透過在程式碼片段後附加 `{:.language-sql .highlight}` 來實現（大括號前不需要空格）。
     * *避免*為此目的使用算術比較字元 `<` 和 `>`、方括號 `[` 和 `]`、大括號 `{` 和 `}`。

### Python 風格

* 使用 **4 個空格**進行縮排。
* 預設使用雙引號（`"`）作為字串。

### 拼字

* 使用[美式英語 (en-US) 拼字](https://en.wikipedia.org/wiki/Oxford_spelling#Language_tag_comparison)。
* 避免使用牛津逗號。

## 範例程式碼片段

* 非常歡迎說明功能使用的範例。在適用的情況下，請考慮在頁面開頭放幾個簡單的範例，展示所描述功能的最常見用途。
* 如果可能，範例應該是獨立且可重現的。例如，範例中使用的資料表必須作為範例程式碼片段的一部分建立。

## 交叉引用

* 在適用的情況下，新增指向文件中其他相關頁面的交叉引用。
* 使用 Jekyll 的 [link 標籤](https://jekyllrb.com/docs/liquid/tags/#link)來連結到頁面。
    * 例如，若要連結到 `SELECT` 陳述式頁面上的 Example 區段，請使用 `{% link docs/stable/sql/statements/select.md %}#examples`。
    * Link 標籤可確保只有在連結指向現有頁面時才會編譯和部署文件。
    * 請注意，路徑必須包含正確的副檔名（通常是 `.md`），並且必須相對於儲存庫根目錄。
    * :x: ```請參閱 [the `SELECT` statement](../../sql/statements/select)```
    * :white_check_mark: ```請參閱 [the `SELECT` statement]({% link docs/stable/sql/statements/select.md %})```
* 避免在連結中使用「這裡」一詞。有關理由，請參閱[為什麼您的連結不應該說「點擊這裡」的詳細說明](https://uxmovement.com/content/why-your-links-should-never-say-click-here/)。
    * :x: `請參閱[這裡]({% link docs/stable/sql/statements/copy.md %}#copy-from)`
    * :white_check_mark: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy-from)```
* 盡可能引用特定區段：
    * :x: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %})```
    * :white_check_mark: ```請參閱 [`COPY ... FROM` 陳述式]({% link docs/stable/sql/statements/copy.md %}#copy-from)```
* 在大多數情況下，不建議連結相關的 GitHub issue/discussion。這允許文件自成一體。

## 預覽、穩定版和版本化頁面

* <https://duckdb.org/docs/preview/> 下的 `preview` 頁面包含 DuckDB 最新預覽（nightly）版本的文件。
* <https://duckdb.org/docs/stable/> 下的 `stable` 頁面包含 DuckDB 最新穩定版本的文件（例如 v1.2）。**大多數 pull request 應針對這些頁面。**
* 版本化頁面（例如 <https://duckdb.org/docs/v1.0/>）包含 DuckDB 舊穩定版本的文件。我們通常只接受對最新穩定版本的貢獻。舊頁面僅在包含嚴重錯誤時才會維護。

## 自動生成頁面

文件的許多頁面都是自動生成的。在編輯之前，請檢查 [`scripts/generate_all_docs.sh`](scripts/generate_all_docs.sh) 腳本。避免直接編輯生成的內容，而是編輯原始檔案（通常在 [`duckdb` 儲存庫](https://github.com/duckdb/duckdb)中找到），並執行生成器腳本。

> [!TIP]
> 動態頁面重新生成可能不適用於 include、railroad 圖表或其他內容。
> 若要強制重新生成頁面或 include，以便在本機瀏覽器中渲染，請執行 `BUILDING.md` 指南的「Jekyll Content Not Rendering Properly」區段中描述的步驟。

### 自動生成的 SQL 函式清單

目錄 `docs/stable/sql/functions/` 中有關 SQL 函式的文件部分由腳本 [`scripts/generate_sql_function_docs.py`](scripts/generate_sql_function_docs.py) 自動生成。
函式文件頁面中的特定區段將列出可用函式及其描述和標準範例。這些區段將透過直接查詢 DuckDB 目錄生成。因此，更新函式描述和範例應在 [`duckdb` 儲存庫](https://github.com/duckdb/duckdb)中完成（搜尋名為 `functions.json` 的檔案）。

若要在文件頁面上插入函式清單，請新增以下兩行：

```md
<!-- Start of section generated by scripts/generate_sql_function_docs.py; categories: [text_similarity] -->
<!-- End of section generated by scripts/generate_sql_function_docs.py -->
```

函式清單將新增在這些行之間，並在每次生成文件時與 DuckDB 目錄同步。
請注意，要包含在清單中的函式類別需要在第一行的末尾設定，例如：`categories: [string,regex]`。
若要檢查目錄中函式的類別標籤，可以使用以下查詢：

```sql
SELECT DISTINCT function_name, categories
FROM duckdb_functions()
WHERE categories != [];
```

所有資料（例如，參數名稱、描述、範例）都來自 `duckdb_functions()` 的輸出。任何偏差（排除、新增或覆蓋）都需要在腳本 [`generate_sql_function_docs.py`](scripts/generate_sql_function_docs.py) 中透過變數 `OVERRIDES` 和 `EXCLUDES` 進行硬編碼。

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

我們保留對是否合併 pull request 的完全和最終決定權。遵守這些指南並不保證您的 pull request 會被合併。
