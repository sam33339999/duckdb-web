---
layout: docu
redirect_from:
- /docs/extensions
- /docs/extensions/
- /docs/extensions/overview
title: 擴充套件
lang: zh-TW
---

DuckDB 具有靈活的擴充套件機制，允許動態載入擴充套件。
擴充套件可以透過提供對其他檔案格式的支援、引入新類型以及特定領域的功能來增強 DuckDB 的功能。

> 擴充套件可在所有用戶端（例如 Python 和 R）上載入。
> 透過核心和社群儲存庫分發的擴充套件在 macOS、Windows 和 Linux 上建置和測試。所有作業系統都支援 AMD64 和 ARM64 架構。

## 列出擴充套件

若要取得擴充套件清單，請使用 `duckdb_extensions` 函式：

```sql
SELECT extension_name, installed, description
FROM duckdb_extensions();
```

| extension_name    | installed | description                                                  |
|-------------------|-----------|--------------------------------------------------------------|
| arrow             | false     | A zero-copy data integration between Apache Arrow and DuckDB |
| autocomplete      | false     | Adds support for autocomplete in the shell                   |
| ...               | ...       | ...                                                          |

此清單將顯示哪些擴充套件可用、哪些擴充套件已安裝、版本為何、安裝位置等。
此清單包含大部分但不是所有可用的核心擴充套件。完整清單請參閱[核心擴充套件清單]({% link docs/stable/core_extensions/overview.md %})。

## 內建擴充套件

DuckDB 的二進位分發版標準配備了幾個內建擴充套件。它們靜態連結到二進位檔案中，可以直接使用。
例如，使用內建的 [`json` 擴充套件]({% link docs/stable/data/json/overview.md %})讀取 JSON 檔案：

```sql
SELECT *
FROM 'test.json';
```

為了使 DuckDB 分發版輕量化，只有少數必要的擴充套件是內建的，每個分發版略有不同。哪個擴充套件在哪個平台上是內建的記錄在[核心擴充套件清單]({% link docs/stable/core_extensions/overview.md %}#default-extensions)中。

## 安裝更多擴充套件

若要讓非內建的擴充套件在 DuckDB 中可用，需要執行兩個步驟：

1. **擴充套件安裝**是下載擴充套件二進位檔案並驗證其中繼資料的過程。在安裝期間，DuckDB 會將下載的擴充套件和一些中繼資料儲存在本機目錄中。DuckDB 可以從此目錄在需要時載入擴充套件。這意味著安裝只需要進行一次。

2. **擴充套件載入**是將二進位檔案動態載入到 DuckDB 實例中的過程。DuckDB 將在本機擴充套件目錄中搜尋已安裝的擴充套件，然後載入它以使其功能可用。這意味著每次重新啟動 DuckDB 時，所有使用的擴充套件都需要（重新）載入

> 擴充套件安裝和載入受到一些[限制]({% link docs/stable/extensions/installing_extensions.md %}#limitations)。

有兩種主要方法可以讓 DuckDB 對可安裝的擴充套件執行**安裝**和**載入**步驟：**明確**和**自動載入**。

### 明確的 `INSTALL` 和 `LOAD`

在 DuckDB 中，擴充套件也可以明確安裝和載入。非自動載入和自動載入的擴充套件都可以透過這種方式安裝。
若要明確安裝和載入擴充套件，DuckDB 有專用的 SQL 陳述式 `LOAD` 和 `INSTALL`。例如，
若要安裝和載入 [`spatial` 擴充套件]({% link docs/stable/core_extensions/spatial/overview.md %})，請執行：

```sql
INSTALL spatial;
LOAD spatial;
```

使用這些陳述式，DuckDB 將確保已安裝 spatial 擴充套件（如果已安裝則忽略 `INSTALL` 陳述式），然後繼續
`LOAD` spatial 擴充套件（如果已載入則再次忽略該陳述式）。

#### 擴充套件儲存庫

可選擇性地提供一個儲存庫，指定應從何處安裝擴充套件，方法是在 `INSTALL` / `FORCE INSTALL` 命令後附加 `FROM ⟨repository⟩`{:.language-sql .highlight}。
此儲存庫可以是別名，例如 [`community`]({% link community_extensions/index.md %})，也可以是直接 URL，以單引號字串提供。

安裝/載入擴充套件後，可以使用 [`duckdb_extensions` 函式](#列出擴充套件)來取得更多資訊。

### 自動載入擴充套件

對於許多 DuckDB 的核心擴充套件，不需要明確載入和安裝擴充套件。DuckDB 包含一個自動載入機制，
可以在查詢中使用核心擴充套件時立即安裝和載入它們。例如，當執行：

```sql
SELECT *
FROM 'https://raw.githubusercontent.com/duckdb/duckdb-web/main/data/weather.csv';
```

DuckDB 將自動安裝和載入 [`httpfs`]({% link docs/stable/core_extensions/httpfs/overview.md %}) 擴充套件。不需要明確的 `INSTALL` 或 `LOAD` 陳述式。

並非所有擴充套件都可以自動載入。這可能有各種原因：有些擴充套件會對正在執行的 DuckDB 實例進行多次變更，使得自動載入在技術上（尚未）可行。對於其他擴充套件，由於它們修改 DuckDB 中行為的方式，更傾向於讓使用者在使用前明確選擇擴充套件。

若要查看哪些擴充套件可以自動載入，請查看[核心擴充套件清單]({% link docs/stable/core_extensions/overview.md %})。

### 社群擴充套件

DuckDB 支援安裝第三方[社群擴充套件]({% link community_extensions/index.md %})。例如，您可以透過以下方式安裝 [`avro` 社群擴充套件]({% link community_extensions/extensions/avro.md %})：

```sql
INSTALL avro FROM community;
```

社群擴充套件由社群成員貢獻，但它們是在集中式儲存庫中建置、[簽署]({% link docs/stable/extensions/extension_distribution.md %}#signed-extensions)和分發的。

## 更新擴充套件

雖然內建擴充套件由於其內建到 DuckDB 二進位檔案中的性質而與 DuckDB 版本綁定，但可安裝的擴充套件
可以而且確實會收到更新。若要確保所有目前已安裝的擴充套件都是最新版本，請呼叫：

```sql
UPDATE EXTENSIONS;
```

有關擴充套件版本的更多詳細資訊，請參閱[擴充套件版本控制頁面]({% link docs/stable/extensions/versioning_of_extensions.md %})。

## 開發擴充套件

核心擴充套件使用的相同 API 可用於開發擴充套件。這允許使用者擴展 DuckDB 的功能，使其最適合他們的領域。
[`extension-template` 儲存庫](https://github.com/duckdb/extension-template/)中提供了用於建立擴充套件的範本。此範本還包含有關如何開始建立自己的擴充套件的一些文件。

## 使用擴充套件

請參閱[安裝說明]({% link docs/stable/extensions/installing_extensions.md %})和[進階安裝方法頁面]({% link docs/stable/extensions/advanced_installation_methods.md %})。
