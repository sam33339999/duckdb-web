---
github_repository: https://github.com/duckdb/duckdb-python
layout: docu
redirect_from:
- /docs/api/python
- /docs/api/python/
- /docs/api/python/overview
- /docs/api/python/overview/
- /docs/clients/python/overview
title: Python API
lang: zh-TW
---

> DuckDB Python 客戶端的最新穩定版本是 {{ site.current_duckdb_version }}。

## 安裝

DuckDB Python API 可以使用 [pip](https://pip.pypa.io) 安裝：`pip install duckdb`。詳細資訊請參閱[安裝頁面]({% link install/index.html %}?environment=python)。也可以使用 [conda](https://docs.conda.io) 安裝 DuckDB：`conda install python-duckdb -c conda-forge`。

**Python 版本：**
DuckDB 需要 Python 3.9 或更新版本。

## 基本 API 使用

使用 DuckDB 執行 SQL 查詢最直接的方式是使用 `duckdb.sql` 指令。

```python
import duckdb

duckdb.sql("SELECT 42").show()
```

這將使用儲存在 Python 模組內部的**記憶體資料庫**執行查詢。查詢的結果會以 **Relation** 的形式回傳。Relation 是查詢的符號表示。在獲取結果或要求列印到螢幕之前，查詢不會執行。

Relation 可以透過將它們儲存在變數中並用作資料表來在後續查詢中引用。這樣可以逐步建構查詢。

```python
import duckdb

r1 = duckdb.sql("SELECT 42 AS i")
duckdb.sql("SELECT i * 2 AS k FROM r1").show()
```

## 資料輸入

DuckDB 可以從各種格式擷取資料 - 無論是磁碟上的還是記憶體中的。更多資訊請參閱[資料擷取頁面]({% link docs/stable/clients/python/data_ingestion.md %})。

```python
import duckdb

duckdb.read_csv("example.csv")                # 將 CSV 檔案讀取為 Relation
duckdb.read_parquet("example.parquet")        # 將 Parquet 檔案讀取為 Relation
duckdb.read_json("example.json")              # 將 JSON 檔案讀取為 Relation

duckdb.sql("SELECT * FROM 'example.csv'")     # 直接查詢 CSV 檔案
duckdb.sql("SELECT * FROM 'example.parquet'") # 直接查詢 Parquet 檔案
duckdb.sql("SELECT * FROM 'example.json'")    # 直接查詢 JSON 檔案
```

### DataFrame

DuckDB 可以直接查詢 Pandas DataFrame、Polars DataFrame 和 Arrow 資料表。請注意，這些是唯讀的，即無法透過 [`INSERT`]({% link docs/stable/sql/statements/insert.md %}) 或 [`UPDATE` 陳述式]({% link docs/stable/sql/statements/update.md %})編輯這些資料表。

#### Pandas

要直接查詢 Pandas DataFrame，請執行：

```python
import duckdb
import pandas as pd

pandas_df = pd.DataFrame({"a": [42]})
duckdb.sql("SELECT * FROM pandas_df")
```

```text
┌───────┐
│   a   │
│ int64 │
├───────┤
│    42 │
└───────┘
```

#### Polars

要直接查詢 Polars DataFrame，請執行：

```python
import duckdb
import polars as pl

polars_df = pl.DataFrame({"a": [42]})
duckdb.sql("SELECT * FROM polars_df")
```

```text
┌───────┐
│   a   │
│ int64 │
├───────┤
│    42 │
└───────┘
```

#### PyArrow

要直接查詢 PyArrow 資料表，請執行：

```python
import duckdb
import pyarrow as pa

arrow_table = pa.Table.from_pydict({"a": [42]})
duckdb.sql("SELECT * FROM arrow_table")
```

```text
┌───────┐
│   a   │
│ int64 │
├───────┤
│    42 │
└───────┘
```

## 結果轉換

DuckDB 支援將查詢結果高效率地轉換為各種格式。更多資訊請參閱[結果轉換頁面]({% link docs/stable/clients/python/conversion.md %})。

```python
import duckdb

duckdb.sql("SELECT 42").fetchall()   # Python 物件
duckdb.sql("SELECT 42").df()         # Pandas DataFrame
duckdb.sql("SELECT 42").pl()         # Polars DataFrame
duckdb.sql("SELECT 42").arrow()      # Arrow 資料表
duckdb.sql("SELECT 42").fetchnumpy() # NumPy 陣列
```

## 將資料寫入磁碟

DuckDB 支援將 Relation 物件直接以各種格式寫入磁碟。[`COPY` 陳述式]({% link docs/stable/sql/statements/copy.md %})可以作為使用 SQL 將資料寫入磁碟的替代方案。

```python
import duckdb

duckdb.sql("SELECT 42").write_parquet("out.parquet") # 寫入 Parquet 檔案
duckdb.sql("SELECT 42").write_csv("out.csv")         # 寫入 CSV 檔案
duckdb.sql("COPY (SELECT 42) TO 'out.parquet'")      # 複製到 Parquet 檔案
```

## 連接選項

應用程式可以透過 `duckdb.connect()` 方法開啟新的 DuckDB 連接。

### 使用記憶體資料庫

透過 `duckdb.sql()` 使用 DuckDB 時，它在**記憶體**資料庫上運作，即不會將資料表持久化到磁碟。不帶參數呼叫 `duckdb.connect()` 方法會回傳一個連接，該連接也使用記憶體資料庫：

```python
import duckdb

con = duckdb.connect()
con.sql("SELECT 42 AS x").show()
```

### 持久化儲存

`duckdb.connect(dbname)` 建立與**持久化**資料庫的連接。寫入該連接的任何資料都將被持久化，並且可以透過重新連接到同一檔案來重新載入，無論是從 Python 還是從其他 DuckDB 客戶端。

```python
import duckdb

# 建立與名為 'file.db' 的檔案的連接
con = duckdb.connect("file.db")
# 建立資料表並將資料載入其中
con.sql("CREATE TABLE test (i INTEGER)")
con.sql("INSERT INTO test VALUES (42)")
# 查詢資料表
con.table("test").show()
# 明確關閉連接
con.close()
# 注意：當連接超出範圍時，它們也會隱式關閉
```

您也可以使用上下文管理器來確保連接被關閉：

```python
import duckdb

with duckdb.connect("file.db") as con:
    con.sql("CREATE TABLE test (i INTEGER)")
    con.sql("INSERT INTO test VALUES (42)")
    con.table("test").show()
    # 上下文管理器會自動關閉連接
```

### 配置

`duckdb.connect()` 接受 `config` 字典，可以在其中指定[配置選項]({% link docs/stable/configuration/overview.md %}#configuration-reference)。例如：

```python
import duckdb

con = duckdb.connect(config = {'threads': 1})
```

### 連接物件和模組

連接物件和 `duckdb` 模組可以互換使用 - 它們支援相同的方法。唯一的區別是，當使用 `duckdb` 模組時，會使用全域記憶體資料庫。

> 如果您正在開發供其他人使用的套件，並在套件中使用 DuckDB，建議您建立連接物件，而不是使用 `duckdb` 模組上的方法。這是因為 `duckdb` 模組使用共享的全域資料庫 - 如果從多個不同的套件中使用，可能會導致難以除錯的問題。

### 在平行 Python 程式中使用連接

`DuckDBPyConnection` 物件不是執行緒安全的。如果您想從多個執行緒寫入同一個資料庫，請使用 [`DuckDBPyConnection.cursor()` 方法]({% link docs/stable/clients/python/reference/index.md %}#duckdb.DuckDBPyConnection.cursor)為每個執行緒建立游標。

## 載入和安裝擴充套件

DuckDB 的 Python API 提供了安裝和載入[擴充套件]({% link docs/stable/extensions/overview.md %})的函式，它們分別執行與執行 `INSTALL` 和 `LOAD` SQL 指令相等的操作。安裝和載入 [`spatial` 擴充套件]({% link docs/stable/core_extensions/spatial/overview.md %})的範例如下：

```python
import duckdb

con = duckdb.connect()
con.install_extension("spatial")
con.load_extension("spatial")
```

### 社群擴充套件

要載入[社群擴充套件]({% link community_extensions/index.md %})，請在 `install_extension` 方法中使用 `repository="community"` 參數。

例如，安裝和載入 `h3` 社群擴充套件如下：

```python
import duckdb

con = duckdb.connect()
con.install_extension("h3", repository="community")
con.load_extension("h3")
```

### 未簽章擴充套件

要載入[未簽章擴充套件]({% link docs/stable/extensions/overview.md %}#unsigned-extensions)，請使用：

```python
con = duckdb.connect(config={"allow_unsigned_extensions": "true"})
```

> 警告：僅載入來自您信任的來源的未簽章擴充套件。避免透過 HTTP 載入未簽章擴充套件。請參閱[保護 DuckDB 頁面]({% link docs/stable/operations_manual/securing_duckdb/securing_extensions.md %})以獲取有關如何以安全方式設定 DuckDB 的指南。
