---
layout: docu
redirect_from:
- /docs/sql/introduction
title: SQL 簡介
lang: zh-TW
---

這裡我們提供如何在 SQL 中執行簡單操作的概覽。本教學僅旨在為您提供入門介紹，絕非完整的 SQL 教學。本教學改編自 [PostgreSQL 教學](https://www.postgresql.org/docs/current/tutorial-sql-intro.html)。

> DuckDB 的 SQL 方言緊密遵循 PostgreSQL 方言的慣例。
> 少數例外情況列於 [PostgreSQL 相容性頁面]({% link docs/stable/sql/dialect/postgresql_compatibility.md %})。

在下面的範例中，我們假設您已安裝 DuckDB 命令列介面 (CLI) Shell。有關如何安裝 CLI 的資訊，請參閱[安裝頁面]({% link install/index.html %}?environment=cli)。

## 概念

DuckDB 是一個關聯式資料庫管理系統 (RDBMS)。這意味著它是一個用於管理儲存在關聯中的資料的系統。關聯本質上是資料表的數學術語。

每個資料表都是一個命名的資料列集合。給定資料表的每一列都具有相同的一組命名欄位，而每個欄位都屬於特定的資料類型。資料表本身儲存在結構描述中，而結構描述的集合構成您可以存取的整個資料庫。

## 建立新資料表

您可以透過指定資料表名稱以及所有欄位名稱和它們的類型來建立新資料表：

```sql
CREATE TABLE weather (
    city    VARCHAR,
    temp_lo INTEGER, -- 一天的最低溫度
    temp_hi INTEGER, -- 一天的最高溫度
    prcp    FLOAT,
    date    DATE
);
```

您可以在 shell 中輸入此內容，包含換行符號。指令在分號之前不會終止。

空白字元（即空格、Tab 和換行）可以在 SQL 指令中自由使用。這意味著您可以與上面不同地對齊輸入指令，甚至全部在一行上。兩個破折號字元 (`--`) 引入註解。在它們之後的任何內容都會被忽略，直到行尾。SQL 對關鍵字和識別符號不區分大小寫。當回傳識別符號時，[會保留其原始大小寫]({% link docs/stable/sql/dialect/keywords_and_identifiers.md %}#rules-for-case-sensitivity)。

在 SQL 指令中，我們首先指定我們想要執行的指令類型：`CREATE TABLE`。之後是指令的參數。首先給出資料表名稱 `weather`。然後是欄位名稱和欄位類型。

`city VARCHAR` 指定資料表有一個名為 `city` 的欄位，類型為 `VARCHAR`。`VARCHAR` 指定一種可以儲存任意長度文字的資料類型。溫度欄位儲存在 `INTEGER` 類型中，該類型儲存整數（即沒有小數點的整數）。`FLOAT` 欄位儲存單精度浮點數（即帶小數點的數字）。`DATE` 儲存日期（即年、月、日組合）。`DATE` 僅儲存特定日期，而不儲存與該日期關聯的時間。

DuckDB 支援標準 SQL 類型 `INTEGER`、`SMALLINT`、`FLOAT`、`DOUBLE`、`DECIMAL`、`CHAR(n)`、`VARCHAR(n)`、`DATE`、`TIME` 和 `TIMESTAMP`。

第二個範例將儲存城市及其相關的地理位置：

```sql
CREATE TABLE cities (
    name VARCHAR,
    lat  DECIMAL,
    lon  DECIMAL
);
```

最後，應該提到的是，如果您不再需要資料表或想以不同方式重新建立它，可以使用以下指令將其移除：

```sql
DROP TABLE ⟨tablename⟩;
```

## 用資料列填充資料表

insert 陳述式用於用資料列填充資料表：

```sql
INSERT INTO weather
VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
```

非數值的常數（例如文字和日期）必須用單引號 (`''`) 括起來，如範例所示。日期類型的輸入日期必須格式化為 `'YYYY-MM-DD'`。

我們可以用相同的方式插入 `cities` 資料表。

```sql
INSERT INTO cities
VALUES ('San Francisco', -194.0, 53.0);
```

到目前為止使用的語法要求您記住欄位的順序。另一種語法允許您明確列出欄位：

```sql
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

如果您願意，可以以不同的順序列出欄位，甚至省略某些欄位，例如，如果 `prcp` 未知：

```sql
INSERT INTO weather (date, city, temp_hi, temp_lo)
VALUES ('1994-11-29', 'Hayward', 54, 37);
```

> 提示：許多開發人員認為明確列出欄位比隱式依賴順序是更好的風格。

請輸入上面顯示的所有指令，以便在以下部分中有一些資料可以使用。

或者，您可以使用 `COPY` 陳述式。這對於大量資料來說更快，因為 `COPY` 指令針對批量載入進行了最佳化，同時靈活性低於 `INSERT`。使用 [`weather.csv`]({% link data/weather.csv %}) 的範例如下：

```sql
COPY weather
FROM 'weather.csv';
```

其中來源檔案的檔案名稱必須在執行行程的機器上可用。還有許多其他方式可以將資料載入 DuckDB，請參閱[相應的文件部分]({% link docs/stable/data/overview.md %})以獲取更多資訊。

## 查詢資料表

要從資料表中檢索資料，需要查詢資料表。使用 SQL `SELECT` 陳述式來執行此操作。該陳述式分為選擇列表（列出要回傳的欄位的部分）、資料表列表（列出要從中檢索資料的資料表的部分）和可選的限定條件（指定任何限制的部分）。例如，要檢索資料表 weather 的所有資料列，請輸入：

```sql
SELECT *
FROM weather;
```

這裡 `*` 是「所有欄位」的簡寫。因此，使用以下指令也會得到相同的結果：

```sql
SELECT city, temp_lo, temp_hi, prcp, date
FROM weather;
```

輸出應該是：

|     city      | temp_lo | temp_hi | prcp |    date    |
|---------------|--------:|--------:|-----:|------------|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 |
| Hayward       | 37      | 54      | NULL | 1994-11-29 |

您可以在選擇列表中編寫運算式，而不僅僅是簡單的欄位參考。例如，您可以這樣做：

```sql
SELECT city, (temp_hi + temp_lo) / 2 AS temp_avg, date
FROM weather;
```

這應該會得到：

|     city      | temp_avg |    date    |
|---------------|---------:|------------|
| San Francisco | 48.0     | 1994-11-27 |
| San Francisco | 50.0     | 1994-11-29 |
| Hayward       | 45.5     | 1994-11-29 |

注意 `AS` 子句如何用於重新標記輸出欄位。（`AS` 子句是可選的。）

查詢可以透過新增指定所需資料列的 `WHERE` 子句來「限定」。`WHERE` 子句包含一個布林（真值）運算式，僅回傳布林運算式為真的資料列。在限定條件中允許使用通常的布林運算子（`AND`、`OR` 和 `NOT`）。例如，以下內容檢索 San Francisco 在雨天的天氣：

```sql
SELECT *
FROM weather
WHERE city = 'San Francisco'
  AND prcp > 0.0;
```

結果：

|     city      | temp_lo | temp_hi | prcp |    date    |
|---------------|--------:|--------:|-----:|------------|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 |

您可以要求以排序順序回傳查詢的結果：

```sql
SELECT *
FROM weather
ORDER BY city;
```

|     city      | temp_lo | temp_hi | prcp |    date    |
|---------------|--------:|--------:|-----:|------------|
| Hayward       | 37      | 54      | NULL | 1994-11-29 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 |
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 |

在此範例中，排序順序未完全指定，因此您可能以任一順序獲得 San Francisco 資料列。但是，如果您執行以下操作，您總是會得到上面顯示的結果：

```sql
SELECT *
FROM weather
ORDER BY city, temp_lo;
```

您可以要求從查詢結果中移除重複的資料列：

```sql
SELECT DISTINCT city
FROM weather;
```

|     city      |
|---------------|
| San Francisco |
| Hayward       |

同樣，結果資料列順序可能會有所不同。您可以透過同時使用 `DISTINCT` 和 `ORDER BY` 來確保一致的結果：

```sql
SELECT DISTINCT city
FROM weather
ORDER BY city;
```

## 資料表之間的連接

到目前為止，我們的查詢一次只存取一個資料表。查詢可以一次存取多個資料表，或以一次處理資料表的多個資料列的方式存取同一個資料表。一次存取同一個或不同資料表的多個資料列的查詢稱為連接查詢。例如，假設您希望列出所有天氣記錄以及相關城市的位置。為此，我們需要將 `weather` 資料表的每一列的 city 欄位與 `cities` 資料表中所有資料列的 name 欄位進行比較，並選擇這些值相符的資料列對。

這將透過以下查詢來完成：

```sql
SELECT *
FROM weather, cities
WHERE city = name;
```

|     city      | temp_lo | temp_hi | prcp |    date    |     name      |   lat    |  lon   |
|---------------|--------:|--------:|-----:|------------|---------------|---------:|-------:|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 | San Francisco | -194.000 | 53.000 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 | San Francisco | -194.000 | 53.000 |

觀察結果集的兩件事：

* Hayward 城市沒有結果資料列。這是因為 `cities` 資料表中沒有 Hayward 的相符項目，因此連接會忽略 `weather` 資料表中不相符的資料列。我們很快就會看到如何解決這個問題。
* 有兩個包含城市名稱的欄位。這是正確的，因為 `weather` 和 `cities` 資料表的欄位列表被串聯起來。但實際上這是不理想的，因此您可能希望明確列出輸出欄位，而不是使用 `*`：

```sql
SELECT city, temp_lo, temp_hi, prcp, date, lon, lat
FROM weather, cities
WHERE city = name;
```

|     city      | temp_lo | temp_hi | prcp |    date    |  lon   |   lat    |
|---------------|--------:|--------:|-----:|------------|-------:|---------:|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 | 53.000 | -194.000 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 | 53.000 | -194.000 |

由於所有欄位都有不同的名稱，解析器會自動找到它們所屬的資料表。如果兩個資料表中有重複的欄位名稱，您需要限定欄位名稱以顯示您的意思，如下所示：

```sql
SELECT weather.city, weather.temp_lo, weather.temp_hi,
       weather.prcp, weather.date, cities.lon, cities.lat
FROM weather, cities
WHERE cities.name = weather.city;
```

廣泛認為在連接查詢中限定所有欄位名稱是良好的風格，這樣如果稍後將重複的欄位名稱新增到其中一個資料表中，查詢就不會失敗。

到目前為止看到的連接查詢也可以用這種替代形式編寫：

```sql
SELECT *
FROM weather
INNER JOIN cities ON weather.city = cities.name;
```

此語法不像上面那樣常用，但我們在此處顯示它以幫助您理解以下主題。

現在我們將弄清楚如何將 Hayward 記錄取回。我們希望查詢做的是掃描 `weather` 資料表，對於每一列找到相符的 cities 資料列。如果找不到相符的資料列，我們希望為 `cities` 資料表的欄位替換一些「空值」。這種查詢稱為外部連接。（到目前為止我們看到的連接是內部連接。）指令如下所示：

```sql
SELECT *
FROM weather
LEFT OUTER JOIN cities ON weather.city = cities.name;
```

|     city      | temp_lo | temp_hi | prcp |    date    |     name      |   lat    |  lon   |
|---------------|--------:|--------:|-----:|------------|---------------|---------:|-------:|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 | San Francisco | -194.000 | 53.000 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 | San Francisco | -194.000 | 53.000 |
| Hayward       | 37      | 54      | NULL | 1994-11-29 | NULL          | NULL     | NULL   |

此查詢稱為左外部連接，因為連接運算子左側提到的資料表將在輸出中至少出現一次，而右側的資料表只會輸出與左側資料表的某些資料列相符的資料列。當輸出沒有右側資料表相符的左側資料表資料列時，會為右側資料表欄位替換空 (null) 值。

## 聚合函式

與大多數其他關聯式資料庫產品一樣，DuckDB 支援聚合函式。聚合函式從多個輸入資料列計算單一結果。例如，有聚合函式可以計算一組資料列的 `count`、`sum`、`avg`（平均值）、`max`（最大值）和 `min`（最小值）。

例如，我們可以找到任何地方的最高最低溫度讀數：

```sql
SELECT max(temp_lo)
FROM weather;
```

| max(temp_lo) |
|-------------:|
| 46           |

如果我們想知道該讀數發生在哪個城市（或哪些城市），我們可能會嘗試：

```sql
SELECT city
FROM weather
WHERE temp_lo = max(temp_lo);
```

但這不會起作用，因為聚合 max 不能在 `WHERE` 子句中使用：

```console
Binder Error:
WHERE clause cannot contain aggregates!
```

存在此限制是因為 `WHERE` 子句決定哪些資料列將包含在聚合計算中；因此顯然必須在計算聚合函式之前對其進行評估。然而，通常情況下，可以重新表述查詢以實現所需的結果，這裡透過使用子查詢：

```sql
SELECT city
FROM weather
WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

|     city      |
|---------------|
| San Francisco |

這是可以的，因為子查詢是一個獨立的計算，它與外部查詢中發生的事情分開計算自己的聚合。

聚合與 `GROUP BY` 子句結合使用也非常有用。例如，我們可以獲得每個城市觀察到的最高最低溫度：

```sql
SELECT city, max(temp_lo)
FROM weather
GROUP BY city;
```

|     city      | max(temp_lo) |
|---------------|--------------|
| San Francisco | 46           |
| Hayward       | 37           |

這為我們提供了每個城市一個輸出資料列。每個聚合結果都是針對與該城市相符的資料表資料列計算的。我們可以使用 `HAVING` 過濾這些分組的資料列：

```sql
SELECT city, max(temp_lo)
FROM weather
GROUP BY city
HAVING max(temp_lo) < 40;
```

|  city   | max(temp_lo) |
|---------|-------------:|
| Hayward | 37           |

這為我們提供了僅針對所有 `temp_lo` 值低於 40 的城市的相同結果。最後，如果我們只關心名稱以 `S` 開頭的城市，我們可以使用 `LIKE` 運算子：

```sql
SELECT city, max(temp_lo)
FROM weather
WHERE city LIKE 'S%'            -- (1)
GROUP BY city
HAVING max(temp_lo) < 40;
```

有關 `LIKE` 運算子的更多資訊，請參閱[模式比對頁面]({% link docs/stable/sql/functions/pattern_matching.md %})。

了解聚合與 SQL 的 `WHERE` 和 `HAVING` 子句之間的互動非常重要。`WHERE` 和 `HAVING` 之間的根本區別是：`WHERE` 在計算群組和聚合之前選擇輸入資料列（因此，它控制哪些資料列進入聚合計算），而 `HAVING` 在計算群組和聚合之後選擇群組資料列。因此，`WHERE` 子句不得包含聚合函式；嘗試使用聚合來確定哪些資料列將成為聚合的輸入是沒有意義的。另一方面，`HAVING` 子句總是包含聚合函式。

在前面的範例中，我們可以在 `WHERE` 中應用城市名稱限制，因為它不需要聚合。這比將限制新增到 `HAVING` 更有效率，因為我們避免對所有未通過 `WHERE` 檢查的資料列執行分組和聚合計算。

## 更新

您可以使用 `UPDATE` 指令更新現有資料列。假設您發現在 11 月 28 日之後所有溫度讀數都偏差了 2 度。您可以按如下方式更正資料：

```sql
UPDATE weather
SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
WHERE date > '1994-11-28';
```

查看資料的新狀態：

```sql
SELECT *
FROM weather;
```

|     city      | temp_lo | temp_hi | prcp |    date    |
|---------------|--------:|--------:|-----:|------------|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 |
| San Francisco | 41      | 55      | 0.0  | 1994-11-29 |
| Hayward       | 35      | 52      | NULL | 1994-11-29 |

## 刪除

可以使用 `DELETE` 指令從資料表中移除資料列。假設您不再對 Hayward 的天氣感興趣。那麼您可以執行以下操作從資料表中刪除這些資料列：

```sql
DELETE FROM weather
WHERE city = 'Hayward';
```

所有屬於 Hayward 的天氣記錄都將被移除。

```sql
SELECT *
FROM weather;
```

|     city      | temp_lo | temp_hi | prcp |    date    |
|---------------|--------:|--------:|-----:|------------|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 |
| San Francisco | 41      | 55      | 0.0  | 1994-11-29 |

在發出以下形式的陳述式時應謹慎：

```sql
DELETE FROM ⟨table_name⟩;
```

> 警告：如果沒有限定條件，`DELETE` 將從給定資料表中移除所有資料列，使其為空。系統不會在執行此操作之前請求確認。
