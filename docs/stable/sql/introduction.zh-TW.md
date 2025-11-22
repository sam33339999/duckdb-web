---
layout: docu
redirect_from:
- /docs/sql/introduction
title: SQL 簡介
---

在此我們提供如何在 SQL 中執行簡單操作的概述。
本教學僅旨在為您提供簡介，絕不是 SQL 的完整教學。
本教學改編自 [PostgreSQL 教學](https://www.postgresql.org/docs/current/tutorial-sql-intro.html)。

> DuckDB 的 SQL 方言緊密遵循 PostgreSQL 方言的慣例。
> 少數例外情況列在 [PostgreSQL 相容性頁面]({% link docs/stable/sql/dialect/postgresql_compatibility.md %})上。

在以下範例中，我們假設您已安裝 DuckDB 命令列介面（CLI）shell。有關如何安裝 CLI 的資訊，請參閱[安裝頁面]({% link install/index.html %}?environment=cli)。

## 概念

DuckDB 是關聯式資料庫管理系統（RDBMS）。這意味著它是一個用於管理儲存在關聯中的資料的系統。關聯本質上是表格的數學術語。

每個表格都是一個命名的列集合。給定表格的每一列都有相同的命名欄位集，每個欄位都有特定的資料類型。表格本身儲存在架構中，架構的集合構成您可以存取的整個資料庫。

## 建立新表格

您可以透過指定表格名稱以及所有欄位名稱及其類型來建立新表格：

```sql
CREATE TABLE weather (
    city    VARCHAR,
    temp_lo INTEGER, -- 一天的最低溫度
    temp_hi INTEGER, -- 一天的最高溫度
    prcp    FLOAT,
    date    DATE
);
```

您可以在 shell 中輸入此內容，包含換行符號。命令不會在分號之前終止。

空白字元（即空格、製表符和換行符號）可以在 SQL 命令中自由使用。這意味著您可以將命令對齊方式與上面不同，甚至全部在一行上。兩個破折號字元（`--`）引入註解。其後的任何內容都會被忽略，直到行尾。SQL 對關鍵字和識別符不區分大小寫。在返回識別符時，[會保留其原始大小寫]({% link docs/stable/sql/dialect/keywords_and_identifiers.md %}#rules-for-case-sensitivity)。

在 SQL 命令中，我們首先指定要執行的命令類型：`CREATE TABLE`。之後是命令的參數。首先給出表格名稱 `weather`。然後是欄位名稱和欄位類型。

`city VARCHAR` 指定表格有一個名為 `city` 的欄位，類型為 `VARCHAR`。`VARCHAR` 指定可以儲存任意長度文字的資料類型。溫度欄位儲存在 `INTEGER` 類型中，該類型儲存整數（即沒有小數點的整數）。`FLOAT` 欄位儲存單精度浮點數（即帶小數點的數字）。`DATE` 儲存日期（即年、月、日組合）。`DATE` 僅儲存特定日期，不儲存與該日期相關的時間。

DuckDB 支援標準 SQL 類型 `INTEGER`、`SMALLINT`、`FLOAT`、`DOUBLE`、`DECIMAL`、`CHAR(n)`、`VARCHAR(n)`、`DATE`、`TIME` 和 `TIMESTAMP`。

第二個範例將儲存城市及其相關的地理位置：

```sql
CREATE TABLE cities (
    name VARCHAR,
    lat  DECIMAL,
    lon  DECIMAL
);
```

最後，應該提到的是，如果您不再需要表格或想要以不同方式重新建立它，可以使用以下命令將其刪除：

```sql
DROP TABLE ⟨tablename⟩;
```

## 在表格中填充列

插入陳述式用於在表格中填充列：

```sql
INSERT INTO weather
VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
```

非數值的常數（例如文字和日期）必須用單引號（`''`）括起來，如範例所示。日期類型的輸入日期必須格式化為 `'YYYY-MM-DD'`。

我們可以以相同的方式插入到 `cities` 表格中。

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

> Tip 許多開發人員認為明確列出欄位比隱式依賴順序更好的風格。

請輸入上面顯示的所有命令，以便您在以下部分中有一些資料可以使用。

或者，您可以使用 `COPY` 陳述式。這對於大量資料來說更快，因為 `COPY` 命令針對批量載入進行最佳化，同時允許比 `INSERT` 更少的靈活性。使用 [`weather.csv`]({% link data/weather.csv %}) 的範例如下：

```sql
COPY weather
FROM 'weather.csv';
```

其中來源檔案的檔案名稱必須在執行程序的機器上可用。還有許多其他方法可以將資料載入到 DuckDB 中，請參閱[相應的文件部分]({% link docs/stable/data/overview.md %})以獲取更多資訊。

## 查詢表格

要從表格中檢索資料，需要查詢表格。SQL `SELECT` 陳述式用於執行此操作。該陳述式分為選擇清單（列出要返回的欄位的部分）、表格清單（列出要從中檢索資料的表格的部分）和可選的限定條件（指定任何限制的部分）。例如，要檢索表格 weather 的所有列，請輸入：

```sql
SELECT *
FROM weather;
```

這裡 `*` 是「所有欄位」的簡寫。因此，可以得到相同的結果：

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

您可以在選擇清單中編寫運算式，而不僅僅是簡單的欄位引用。例如，您可以這樣做：

```sql
SELECT city, (temp_hi + temp_lo) / 2 AS temp_avg, date
FROM weather;
```

這應該會給出：

|     city      | temp_avg |    date    |
|---------------|---------:|------------|
| San Francisco | 48.0     | 1994-11-27 |
| San Francisco | 50.0     | 1994-11-29 |
| Hayward       | 45.5     | 1994-11-29 |

注意如何使用 `AS` 子句重新標記輸出欄位。（`AS` 子句是可選的。）

可以透過新增 `WHERE` 子句來「限定」查詢，該子句指定需要哪些列。`WHERE` 子句包含布林（真值）運算式，只有布林運算式為真的列才會被返回。在限定條件中允許使用常見的布林運算子（`AND`、`OR` 和 `NOT`）。例如，以下檢索舊金山下雨天的天氣：

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

您可以請求查詢的結果以排序順序返回：

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

在這個範例中，排序順序沒有完全指定，因此您可能以任一順序獲得舊金山的列。但是，如果您這樣做，您總是會得到上面顯示的結果：

```sql
SELECT *
FROM weather
ORDER BY city, temp_lo;
```

您可以請求從查詢結果中刪除重複的列：

```sql
SELECT DISTINCT city
FROM weather;
```

|     city      |
|---------------|
| San Francisco |
| Hayward       |

在這裡，結果列的順序可能會有所不同。您可以透過同時使用 `DISTINCT` 和 `ORDER BY` 來確保結果一致：

```sql
SELECT DISTINCT city
FROM weather
ORDER BY city;
```

## 表格之間的連接

到目前為止，我們的查詢一次只存取一個表格。查詢可以一次存取多個表格，或以一次處理表格的多個列的方式存取同一表格。一次存取同一個或不同表格的多個列的查詢稱為連接查詢。例如，假設您希望將所有天氣記錄與相關城市的位置一起列出。為此，我們需要比較 `weather` 表格的每一列的 city 欄位與 `cities` 表格中所有列的 name 欄位，並選擇這些值匹配的列對。

這可以透過以下查詢來完成：

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

* 沒有 Hayward 城市的結果列。這是因為 `cities` 表格中沒有 Hayward 的匹配項目，因此連接會忽略 `weather` 表格中不匹配的列。我們稍後會看到如何解決這個問題。
* 有兩個欄位包含城市名稱。這是正確的，因為 `weather` 和 `cities` 表格的欄位清單被串聯在一起。但實際上這是不可取的，因此您可能希望明確列出輸出欄位，而不是使用 `*`：

```sql
SELECT city, temp_lo, temp_hi, prcp, date, lon, lat
FROM weather, cities
WHERE city = name;
```

|     city      | temp_lo | temp_hi | prcp |    date    |  lon   |   lat    |
|---------------|--------:|--------:|-----:|------------|-------:|---------:|
| San Francisco | 46      | 50      | 0.25 | 1994-11-27 | 53.000 | -194.000 |
| San Francisco | 43      | 57      | 0.0  | 1994-11-29 | 53.000 | -194.000 |

由於所有欄位都有不同的名稱，解析器會自動找到它們屬於哪個表格。如果兩個表格中有重複的欄位名稱，您需要限定欄位名稱以顯示您的意思，如：

```sql
SELECT weather.city, weather.temp_lo, weather.temp_hi,
       weather.prcp, weather.date, cities.lon, cities.lat
FROM weather, cities
WHERE cities.name = weather.city;
```

通常認為在連接查詢中限定所有欄位名稱是良好的風格，這樣如果稍後將重複的欄位名稱新增到其中一個表格中，查詢就不會失敗。

到目前為止看到的這種連接查詢也可以用這種替代形式編寫：

```sql
SELECT *
FROM weather
INNER JOIN cities ON weather.city = cities.name;
```

這種語法不像上面的那樣常用，但我們在這裡展示它以幫助您理解以下主題。

現在我們將弄清楚如何將 Hayward 記錄找回來。我們希望查詢做的是掃描 `weather` 表格，並為每一列找到匹配的 cities 列。如果找不到匹配的列，我們希望為 `cities` 表格的欄位替換一些「空值」。這種查詢稱為外連接。（我們到目前為止看到的連接是內連接。）命令看起來像這樣：

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

這個查詢稱為左外連接，因為在連接運算子左側提到的表格將在輸出中至少有一次其每一列，而右側的表格將只有那些與左側表格某一列匹配的列輸出。當輸出左側表格的列而右側表格沒有匹配時，右側表格的欄位會替換為空（null）值。

## 彙總函數

像大多數其他關聯式資料庫產品一樣，DuckDB 支援彙總函數。彙總函數從多個輸入列計算單一結果。例如，有彙總函數可以計算一組列的 `count`、`sum`、`avg`（平均）、`max`（最大值）和 `min`（最小值）。

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

但這不起作用，因為彙總 max 不能在 `WHERE` 子句中使用：

```console
Binder Error:
WHERE clause cannot contain aggregates!
```

存在此限制是因為 `WHERE` 子句確定哪些列將包含在彙總計算中；因此顯然它必須在計算彙總函數之前進行評估。
但是，通常情況下，查詢可以重新陳述以完成所需的結果，這裡透過使用子查詢：

```sql
SELECT city
FROM weather
WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

|     city      |
|---------------|
| San Francisco |

這是可以的，因為子查詢是一個獨立的計算，它與外部查詢中發生的事情分開計算自己的彙總。

彙總與 `GROUP BY` 子句結合使用也非常有用。例如，我們可以獲得每個城市觀察到的最高最低溫度：

```sql
SELECT city, max(temp_lo)
FROM weather
GROUP BY city;
```

|     city      | max(temp_lo) |
|---------------|--------------|
| San Francisco | 46           |
| Hayward       | 37           |

這為我們提供了每個城市一個輸出列。每個彙總結果都是在與該城市匹配的表格列上計算的。我們可以使用 `HAVING` 來過濾這些分組的列：

```sql
SELECT city, max(temp_lo)
FROM weather
GROUP BY city
HAVING max(temp_lo) < 40;
```

|  city   | max(temp_lo) |
|---------|-------------:|
| Hayward | 37           |

這為我們提供了相同的結果，但僅適用於所有 `temp_lo` 值都低於 40 的城市。最後，如果我們只關心名稱以 `S` 開頭的城市，我們可以使用 `LIKE` 運算子：

```sql
SELECT city, max(temp_lo)
FROM weather
WHERE city LIKE 'S%'            -- (1)
GROUP BY city
HAVING max(temp_lo) < 40;
```

有關 `LIKE` 運算子的更多資訊可以在[模式匹配頁面]({% link docs/stable/sql/functions/pattern_matching.md %})中找到。

理解彙總和 SQL 的 `WHERE` 和 `HAVING` 子句之間的互動非常重要。`WHERE` 和 `HAVING` 之間的根本區別是：`WHERE` 在計算群組和彙總之前選擇輸入列（因此，它控制哪些列進入彙總計算），而 `HAVING` 在計算群組和彙總之後選擇群組列。因此，`WHERE` 子句不能包含彙總函數；嘗試使用彙總來確定哪些列將成為彙總的輸入是沒有意義的。另一方面，`HAVING` 子句總是包含彙總函數。

在前面的範例中，我們可以在 `WHERE` 中應用城市名稱限制，因為它不需要彙總。這比將限制新增到 `HAVING` 更有效，因為我們避免對所有未通過 `WHERE` 檢查的列進行分組和彙總計算。

## 更新

您可以使用 `UPDATE` 命令更新現有的列。假設您發現 11 月 28 日之後的溫度讀數都偏離了 2 度。您可以按如下方式更正資料：

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

可以使用 `DELETE` 命令從表格中刪除列。假設您不再對 Hayward 的天氣感興趣。然後您可以執行以下操作來從表格中刪除這些列：

```sql
DELETE FROM weather
WHERE city = 'Hayward';
```

所有屬於 Hayward 的天氣記錄都被刪除。

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

> Warning 沒有限定條件，`DELETE` 將從給定的表格中刪除所有列，使其為空。系統不會在執行此操作之前請求確認。
