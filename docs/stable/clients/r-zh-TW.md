---
github_repository: https://github.com/duckdb/duckdb-r
layout: docu
redirect_from:
- /docs/api/r
- /docs/api/r/
- /docs/clients/r
title: R 客戶端
lang: zh-TW
---

> DuckDB R 客戶端的最新穩定版本是 {{ site.current_duckdb_r_version }}。

## 安裝

### `duckdb`：R 客戶端

DuckDB R 客戶端可以使用以下命令安裝：

```r
install.packages("duckdb")
```

詳情請參閱[安裝頁面]({% link install/index.html %}?environment=r)。

### `duckplyr`：dplyr 客戶端

DuckDB 透過 `duckplyr` 套件提供與 [dplyr](https://dplyr.tidyverse.org/) 相容的 API。它可以使用 `install.packages("duckplyr")` 安裝。詳情請參閱 [`duckplyr` 文件](https://tidyverse.github.io/duckplyr/)。

## 參考手冊

DuckDB R 客戶端的參考手冊可在 [r.duckdb.org](https://r.duckdb.org) 取得。

## 基本客戶端使用

標準 DuckDB R 客戶端為 R 實作 [DBI 介面](https://cran.r-project.org/package=DBI)。如果您還不熟悉 DBI，請參閱 [Using DBI 頁面](https://solutions.rstudio.com/db/r-packages/DBI/)以取得簡介。

### 啟動與關閉

若要使用 DuckDB，您必須先建立一個代表資料庫的連接物件。連接物件將資料庫檔案作為參數以進行讀取和寫入。如果資料庫檔案不存在，將會建立它（檔案副檔名可以是 `.db`、`.duckdb` 或任何其他副檔名）。特殊值 `:memory:`（預設值）可用於建立**記憶體資料庫**。請注意，對於記憶體資料庫，不會將資料持久化到磁碟（即，當您退出 R 行程時，所有資料都會遺失）。如果您想以唯讀模式連接到現有資料庫，請將 `read_only` 旗標設定為 `TRUE`。如果多個 R 行程想要同時存取同一個資料庫檔案，則需要唯讀模式。

```r
library("duckdb")
# to start an in-memory database
con <- dbConnect(duckdb())
# or
con <- dbConnect(duckdb(), dbdir = ":memory:")
# to use a database file (not shared between processes)
con <- dbConnect(duckdb(), dbdir = "my-db.duckdb", read_only = FALSE)
# to use a database file (shared between processes)
con <- dbConnect(duckdb(), dbdir = "my-db.duckdb", read_only = TRUE)
```

當連接超出範圍時或使用 `dbDisconnect()` 明確關閉時，連接會隱式關閉。若要關閉與連接相關聯的資料庫實例，請使用 `dbDisconnect(con, shutdown = TRUE)`

### 查詢

DuckDB 支援標準的 DBI 方法來傳送查詢和擷取結果集。`dbExecute()` 用於不期望結果的查詢，例如 `CREATE TABLE` 或 `UPDATE` 等。而 `dbGetQuery()` 用於產生結果的查詢（例如 `SELECT`）。以下是一個範例。

```r
# create a table
dbExecute(con, "CREATE TABLE items (item VARCHAR, value DECIMAL(10, 2), count INTEGER)")
# insert two items into the table
dbExecute(con, "INSERT INTO items VALUES ('jeans', 20.0, 1), ('hammer', 42.2, 2)")

# retrieve the items again
res <- dbGetQuery(con, "SELECT * FROM items")
print(res)
#     item value count
# 1  jeans  20.0     1
# 2 hammer  42.2     2
```

DuckDB 在 R 客戶端中也支援使用 `dbExecute` 和 `dbGetQuery` 方法的預備陳述式。以下是一個範例：

```r
# prepared statement parameters are given as a list
dbExecute(con, "INSERT INTO items VALUES (?, ?, ?)", list('laptop', 2000, 1))

# if you want to reuse a prepared statement multiple times, use dbSendStatement() and dbBind()
stmt <- dbSendStatement(con, "INSERT INTO items VALUES (?, ?, ?)")
dbBind(stmt, list('iphone', 300, 2))
dbBind(stmt, list('android', 3.5, 1))
dbClearResult(stmt)

# query the database using a prepared statement
res <- dbGetQuery(con, "SELECT item FROM items WHERE value > ?", list(400))
print(res)
#       item
# 1 laptop
```

> 警告：**不要**使用預備陳述式將大量資料插入 DuckDB。有關更好的選項，請參閱下文。

## 高效率傳輸

若要將 R 資料框寫入 DuckDB，請使用標準的 DBI 函式 `dbWriteTable()`。這會在 DuckDB 中建立一個資料表，並使用資料框內容填充它。例如：

```r
dbWriteTable(con, "iris_table", iris)
res <- dbGetQuery(con, "SELECT * FROM iris_table LIMIT 1")
print(res)
#   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
# 1          5.1         3.5          1.4         0.2  setosa
```

也可以將 R 資料框「註冊」為虛擬資料表，類似於 SQL `VIEW`。這*實際上還沒有將資料傳輸*到 DuckDB。以下是一個範例：

```r
duckdb_register(con, "iris_view", iris)
res <- dbGetQuery(con, "SELECT * FROM iris_view LIMIT 1")
print(res)
#   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
# 1          5.1         3.5          1.4         0.2  setosa
```

> DuckDB 在註冊後保留對 R 資料框的引用。這可以防止資料框被垃圾回收。當連接關閉時會清除引用，但也可以使用 `duckdb_unregister()` 方法手動清除。

另請參閱[資料匯入文件]({% link docs/stable/data/overview.md %})以取得更多有效匯入資料的選項。

## dbplyr

DuckDB 也可以很好地與 [dbplyr](https://CRAN.R-project.org/package=dbplyr) / [dplyr](https://dplyr.tidyverse.org) 套件配合使用，用於從 R 進行程式化查詢建構。以下是一個範例：

```r
library("duckdb")
library("dplyr")
con <- dbConnect(duckdb())
duckdb_register(con, "flights", nycflights13::flights)

tbl(con, "flights") |>
  group_by(dest) |>
  summarise(delay = mean(dep_time, na.rm = TRUE)) |>
  collect()
```

使用 dbplyr 時，可以使用 `dplyr::tbl` 函式讀取 CSV 和 Parquet 檔案。

```r
# Establish a CSV for the sake of this example
write.csv(mtcars, "mtcars.csv")

# Summarize the dataset in DuckDB to avoid reading the entire CSV into R's memory
tbl(con, "mtcars.csv") |>
  group_by(cyl) |>
  summarise(across(disp:wt, .fns = mean)) |>
  collect()
```

```r
# Establish a set of Parquet files
dbExecute(con, "COPY flights TO 'dataset' (FORMAT parquet, PARTITION_BY (year, month))")

# Summarize the dataset in DuckDB to avoid reading 12 Parquet files into R's memory
tbl(con, "read_parquet('dataset/**/*.parquet', hive_partitioning = true)") |>
  filter(month == "3") |>
  summarise(delay = mean(dep_time, na.rm = TRUE)) |>
  collect()
```

## 記憶體限制

您可以使用 [`memory_limit` 組態選項]({% link docs/stable/configuration/pragmas.md %})來限制 DuckDB 的記憶體使用，例如：

```sql
SET memory_limit = '2GB';
```

請注意，此限制僅適用於 DuckDB 使用的記憶體，並不影響其他 R 程式庫的記憶體使用。
因此，R 行程使用的總記憶體可能高於設定的 `memory_limit`。

## 疑難排解

### 在 macOS 上安裝時的警告

在 macOS 上安裝 DuckDB 可能會導致警告 `unable to load shared object '.../R_X11.so'`：

```console
Warning message:
In doTryCatch(return(expr), name, parentenv, handler) :
  unable to load shared object '/Library/Frameworks/R.framework/Resources/modules//R_X11.so':
  dlopen(/Library/Frameworks/R.framework/Resources/modules//R_X11.so, 0x0006): Library not loaded: /opt/X11/lib/libSM.6.dylib
  Referenced from: <31EADEB5-0A17-3546-9944-9B3747071FE8> /Library/Frameworks/R.framework/Versions/4.4-arm64/Resources/modules/R_X11.so
  Reason: tried: '/opt/X11/lib/libSM.6.dylib' (no such file) ...
> ')
```

請注意，這只是一個警告，因此最簡單的解決方案是忽略它。或者，您可以從 [R-universe](https://r-universe.dev/search) 安裝 DuckDB：

```R
install.packages("duckdb", repos = c("https://duckdb.r-universe.dev", "https://cloud.r-project.org"))
```

您也可以[透過 Homebrew 安裝可選的 `xquartz` 相依性](https://formulae.brew.sh/cask/xquartz)。
