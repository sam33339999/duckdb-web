---
github_repository: https://github.com/duckdb/duckdb-java
layout: docu
redirect_from:
- /docs/api/java
- /docs/api/java/
- /docs/api/scala
- /docs/api/scala/
- /docs/clients/java
title: Java JDBC 客戶端
lang: zh-TW
---

> DuckDB Java (JDBC) 客戶端的最新穩定版本是 {{ site.current_duckdb_java_short_version }}。

## 安裝

DuckDB Java JDBC API 可以從 [Maven Central](https://search.maven.org/artifact/org.duckdb/duckdb_jdbc) 安裝。詳情請參閱[安裝頁面]({% link install/index.html %}?environment=java)。

## 基本 API 使用

DuckDB 的 JDBC API 實作了標準 Java Database Connectivity (JDBC) API 的主要部分，版本 4.1。描述 JDBC 超出本頁的範圍，詳情請參閱[官方文件](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)。下面我們專注於 DuckDB 特定的部分。

有關我們對 JDBC 規範的擴充或下面的 [Arrow 方法](#arrow-方法)的更多資訊，請參閱外部託管的 [API 參考](https://javadoc.io/doc/org.duckdb/duckdb_jdbc)。

### 啟動與關閉

在 JDBC 中，資料庫連接是透過標準的 `java.sql.DriverManager` 類別建立的。
驅動程式應該在 `DriverManager` 中自動註冊，如果由於某些原因無法運作，您可以使用以下陳述式強制註冊：

```java
Class.forName("org.duckdb.DuckDBDriver");
```

若要建立 DuckDB 連接，請使用 `jdbc:duckdb:` JDBC URL 前綴呼叫 `DriverManager`，如下所示：

```java
import java.sql.Connection;
import java.sql.DriverManager;

Connection conn = DriverManager.getConnection("jdbc:duckdb:");
```

若要使用 DuckDB 特定功能（例如 [Appender](#appender)），請將物件轉換為 `DuckDBConnection`：

```java
import java.sql.DriverManager;
import org.duckdb.DuckDBConnection;

DuckDBConnection conn = (DuckDBConnection) DriverManager.getConnection("jdbc:duckdb:");
```

當單獨使用 `jdbc:duckdb:` URL 時，會建立**記憶體資料庫**。請注意，對於記憶體資料庫，不會將資料持久化到磁碟（即，當您退出 Java 程式時，所有資料都會遺失）。如果您想要存取或建立持久化資料庫，請在路徑後附加其檔案名稱。例如，如果您的資料庫儲存在 `/tmp/my_database`，請使用 JDBC URL `jdbc:duckdb:/tmp/my_database` 來建立與它的連接。

可以以**唯讀**模式開啟 DuckDB 資料庫檔案。例如，當多個 Java 行程想要同時讀取同一個資料庫檔案時，這很有用。若要以唯讀模式開啟現有資料庫檔案，請如下設定連接屬性 `duckdb.read_only`：

```java
Properties readOnlyProperty = new Properties();
readOnlyProperty.setProperty("duckdb.read_only", "true");
Connection conn = DriverManager.getConnection("jdbc:duckdb:/tmp/my_database", readOnlyProperty);
```

可以使用 `DriverManager` 建立其他連接。更有效率的機制是呼叫 `DuckDBConnection#duplicate()` 方法：

```java
Connection conn2 = ((DuckDBConnection) conn).duplicate();
```

允許多個連接，但不支援混合讀寫和唯讀連接。

### 設定連接

可以提供組態選項來變更資料庫系統的不同設定。請注意，這些設定中的許多也可以稍後使用 [`PRAGMA` 陳述式]({% link docs/stable/configuration/pragmas.md %})進行變更。

```java
Properties connectionProperties = new Properties();
connectionProperties.setProperty("temp_directory", "/path/to/temp/dir/");
Connection conn = DriverManager.getConnection("jdbc:duckdb:/tmp/my_database", connectionProperties);
```

### 查詢

DuckDB 支援標準的 JDBC 方法來傳送查詢和擷取結果集。首先必須從 `Connection` 建立 `Statement` 物件，然後可以使用此物件透過 `execute` 和 `executeQuery` 傳送查詢。`execute()` 用於不期望結果的查詢，例如 `CREATE TABLE` 或 `UPDATE` 等。而 `executeQuery()` 用於產生結果的查詢（例如 `SELECT`）。以下是兩個範例。另請參閱 JDBC [`Statement`](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html) 和 [`ResultSet`](https://docs.oracle.com/javase/7/docs/api/java/sql/ResultSet.html) 文件。

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

Connection conn = DriverManager.getConnection("jdbc:duckdb:");

// create a table
Statement stmt = conn.createStatement();
stmt.execute("CREATE TABLE items (item VARCHAR, value DECIMAL(10, 2), count INTEGER)");
// insert two items into the table
stmt.execute("INSERT INTO items VALUES ('jeans', 20.0, 1), ('hammer', 42.2, 2)");

try (ResultSet rs = stmt.executeQuery("SELECT * FROM items")) {
    while (rs.next()) {
        System.out.println(rs.getString(1));
        System.out.println(rs.getInt(3));
    }
}
stmt.close();
```

```text
jeans
1
hammer
2
```

DuckDB 也支援符合 JDBC API 的預備陳述式：

```java
import java.sql.PreparedStatement;

try (PreparedStatement stmt = conn.prepareStatement("INSERT INTO items VALUES (?, ?, ?);")) {
    stmt.setString(1, "chainsaw");
    stmt.setDouble(2, 500.0);
    stmt.setInt(3, 42);
    stmt.execute();
    // more calls to execute() possible
}
```

> 警告：*不要*使用預備陳述式將大量資料插入 DuckDB。有關更好的選項，請參閱[資料匯入文件]({% link docs/stable/data/overview.md %})。

### Arrow 方法

有關類型簽章，請參閱 [API 參考](https://javadoc.io/doc/org.duckdb/duckdb_jdbc/latest/org/duckdb/DuckDBResultSet.html#arrowExportStream(java.lang.Object,long))

#### Arrow 匯出

以下示範如何匯出 Arrow 串流並使用 Java Arrow 綁定使用它

```java
import org.apache.arrow.memory.RootAllocator;
import org.apache.arrow.vector.ipc.ArrowReader;
import org.duckdb.DuckDBResultSet;

try (var conn = DriverManager.getConnection("jdbc:duckdb:");
    var stmt = conn.prepareStatement("SELECT * FROM generate_series(2000)");
    var resultset = (DuckDBResultSet) stmt.executeQuery();
    var allocator = new RootAllocator()) {
    try (var reader = (ArrowReader) resultset.arrowExportStream(allocator, 256)) {
        while (reader.loadNextBatch()) {
            System.out.println(reader.getVectorSchemaRoot().getVector("generate_series"));
        }
    }
    stmt.close();
}
```

#### Arrow 匯入

以下示範如何從 Java Arrow 綁定使用 Arrow 串流。

```java
import org.apache.arrow.memory.RootAllocator;
import org.apache.arrow.vector.ipc.ArrowReader;
import org.duckdb.DuckDBConnection;

// Arrow binding
try (var allocator = new RootAllocator();
     ArrowStreamReader reader = null; // should not be null of course
     var arrow_array_stream = ArrowArrayStream.allocateNew(allocator)) {
    Data.exportArrayStream(allocator, reader, arrow_array_stream);

    // DuckDB setup
    try (var conn = (DuckDBConnection) DriverManager.getConnection("jdbc:duckdb:")) {
        conn.registerArrowStream("asdf", arrow_array_stream);

        // run a query
        try (var stmt = conn.createStatement();
             var rs = (DuckDBResultSet) stmt.executeQuery("SELECT count(*) FROM asdf")) {
            while (rs.next()) {
                System.out.println(rs.getInt(1));
            }
        }
    }
}
```

### 串流結果

在 JDBC 驅動程式中，結果串流是選擇性的——透過在執行查詢之前將 `jdbc_stream_results` 組態設定為 `true`。最簡單的方法是在 `Properties` 物件中傳遞它。

```java
Properties props = new Properties();
props.setProperty(DuckDBDriver.JDBC_STREAM_RESULTS, String.valueOf(true));

Connection conn = DriverManager.getConnection("jdbc:duckdb:", props);
```

### Appender

[Appender]({% link docs/stable/data/appender.md %}) 透過 `org.duckdb.DuckDBAppender` 類別在 DuckDB JDBC 驅動程式中可用。
該類別的建構函式需要套用它的結構描述名稱和資料表名稱。
當呼叫 `close()` 方法時，Appender 會被刷新。

範例：

```java
import java.sql.DriverManager;
import java.sql.Statement;
import org.duckdb.DuckDBConnection;

DuckDBConnection conn = (DuckDBConnection) DriverManager.getConnection("jdbc:duckdb:");
try (var stmt = conn.createStatement()) {
    stmt.execute("CREATE TABLE tbl (x BIGINT, y FLOAT, s VARCHAR)"
);

// using try-with-resources to automatically close the appender at the end of the scope
try (var appender = conn.createAppender(DuckDBConnection.DEFAULT_SCHEMA, "tbl")) {
    appender.beginRow();
    appender.append(10);
    appender.append(3.2);
    appender.append("hello");
    appender.endRow();
    appender.beginRow();
    appender.append(20);
    appender.append(-8.1);
    appender.append("world");
    appender.endRow();
}
```

### 批次寫入器

DuckDB JDBC 驅動程式提供批次寫入功能。
批次寫入器支援預備陳述式以減輕查詢解析的開銷。

> 由於其更高的效能，批次插入的首選方法是使用 [Appender](#appender)。
> 但是，當無法使用 Appender 時，批次寫入器可作為替代方案。

#### 使用預備陳述式的批次寫入器

```java
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import org.duckdb.DuckDBConnection;

DuckDBConnection conn = (DuckDBConnection) DriverManager.getConnection("jdbc:duckdb:");
PreparedStatement stmt = conn.prepareStatement("INSERT INTO test (x, y, z) VALUES (?, ?, ?);");

stmt.setObject(1, 1);
stmt.setObject(2, 2);
stmt.setObject(3, 3);
stmt.addBatch();

stmt.setObject(1, 4);
stmt.setObject(2, 5);
stmt.setObject(3, 6);
stmt.addBatch();

stmt.executeBatch();
stmt.close();
```

#### 使用一般陳述式的批次寫入器

批次寫入器也支援一般 SQL 陳述式：

```java
import java.sql.DriverManager;
import java.sql.Statement;
import org.duckdb.DuckDBConnection;

DuckDBConnection conn = (DuckDBConnection) DriverManager.getConnection("jdbc:duckdb:");
Statement stmt = conn.createStatement();

stmt.execute("CREATE TABLE test (x INTEGER, y INTEGER, z INTEGER)");

stmt.addBatch("INSERT INTO test (x, y, z) VALUES (1, 2, 3);");
stmt.addBatch("INSERT INTO test (x, y, z) VALUES (4, 5, 6);");

stmt.executeBatch();
stmt.close();
```

## 疑難排解

### 找不到驅動程式類別

如果 Java 應用程式無法找到 DuckDB，它可能會拋出以下錯誤：

```console
Exception in thread "main" java.sql.SQLException: No suitable driver found for jdbc:duckdb:
    at java.sql/java.sql.DriverManager.getConnection(DriverManager.java:706)
    at java.sql/java.sql.DriverManager.getConnection(DriverManager.java:252)
    ...
```

當嘗試手動載入類別時，可能會導致此錯誤：

```console
Exception in thread "main" java.lang.ClassNotFoundException: org.duckdb.DuckDBDriver
    at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:520)
    at java.base/java.lang.Class.forName0(Native Method)
    at java.base/java.lang.Class.forName(Class.java:375)
    ...
```

這些錯誤源於未檢測到 DuckDB Maven/Gradle 相依性。若要確保檢測到它，請在您的 IDE 中強制重新整理 Maven 組態。
