# DuckDB 文件網站繁體中文翻譯指南

## 目錄

* [簡介](#簡介)
* [翻譯原則](#翻譯原則)
* [專案結構](#專案結構)
* [翻譯流程](#翻譯流程)
* [技術規範](#技術規範)
* [常見術語對照表](#常見術語對照表)
* [建置與預覽](#建置與預覽)
* [提交與審查](#提交與審查)

## 簡介

本指南旨在協助貢獻者將 DuckDB 文件網站翻譯成繁體中文。DuckDB 網站使用 Jekyll 建置，包含 Markdown 和 HTML 混合的內容。

### 翻譯目標

- **準確性**：確保技術內容翻譯正確無誤
- **易讀性**：使用簡單清晰、連貫的繁體中文語句
- **一致性**：統一技術術語和風格
- **完整性**：保留所有原始格式、連結和程式碼

## 翻譯原則

### 語言風格

1. **使用繁體中文詞彙**
   - 優先使用台灣、香港地區常用的繁體中文詞彙
   - 避免使用簡體中文的用語習慣

2. **簡單易懂**
   - 使用簡潔明瞭的語句
   - 避免過於艱深的專業術語，必要時提供說明
   - 保持語句連貫性，讓讀者容易理解

3. **保持專業性**
   - 技術術語保持一致
   - 對於特定的技術名詞，第一次出現時可以中英文並列

### 翻譯範圍

#### 需要翻譯的內容

- 文字內容（標題、段落、列表項目等）
- 圖片替代文字（alt text）
- 錨點連結文字
- 註解（必要時）

#### 不需要翻譯的內容

- 程式碼區塊中的程式碼
- 命令列指令
- 檔案路徑和檔案名稱
- URL 和網址
- 變數名稱和函數名稱
- SQL 關鍵字和語法
- 專有名詞（如 DuckDB、Jekyll、Docker 等）
- YAML front matter 中的 key（但 value 可能需要翻譯）

## 專案結構

```
duckdb-web/
├── _config.yml              # Jekyll 主要配置
├── _data/                   # 資料檔案（選單、作者等）
├── _includes/               # 可重用的模板片段
├── _layouts/                # 頁面佈局模板
├── _posts/                  # 部落格文章
├── docs/                    # 文件內容（主要翻譯目標）
│   ├── stable/             # 穩定版文件
│   └── preview/            # 預覽版文件
├── images/                  # 圖片資源
├── css/                     # 樣式表
├── js/                      # JavaScript 檔案
├── scripts/                 # 建置腳本
├── BUILDING.md             # 建置說明（英文）
├── BUILDING.zh-TW.md       # 建置說明（繁體中文）
├── CONTRIBUTING.md         # 貢獻指南（英文）
├── CONTRIBUTING.zh-TW.md   # 貢獻指南（繁體中文）
├── README.md               # 專案說明（英文）
├── README.zh-TW.md         # 專案說明（繁體中文）
├── faq.md                  # 常見問題（英文）
└── faq.zh-TW.md            # 常見問題（繁體中文）
```

### 翻譯檔案命名規則

- 繁體中文翻譯檔案加上 `.zh-TW` 後綴
- 範例：
  - `BUILDING.md` → `BUILDING.zh-TW.md`
  - `README.md` → `README.zh-TW.md`
  - `faq.md` → `faq.zh-TW.md`

## 翻譯流程

### 1. 準備工作

```bash
# 複製儲存庫
git clone https://github.com/duckdb/duckdb-web.git
cd duckdb-web

# 建立翻譯分支
git checkout -b translation/zh-tw-page-name
```

### 2. 選擇要翻譯的檔案

建議的翻譯優先順序：

1. **建置說明文檔**（BUILDING.md）✅ 已完成
2. **核心頁面**（README.md, CONTRIBUTING.md, faq.md）✅ 已完成
3. **主要文檔索引頁面**（docs/stable/index.md 等）
4. **常用指南**（guides/ 目錄下的檔案）
5. **API 文件**（docs/stable/ 下的其他內容）
6. **部落格文章**（_posts/ 目錄）

### 3. 翻譯檔案

1. 複製原始英文檔案
2. 重新命名為 `.zh-TW.md`
3. 開始翻譯，遵循[技術規範](#技術規範)

### 4. 預覽檢查

```bash
# 使用本機 Jekyll 預覽
scripts/serve-latest.sh

# 或使用 Docker 預覽
scripts/docker-serve-latest.sh
```

在瀏覽器中訪問 <http://localhost:4000/docs/> 檢查翻譯效果。

### 5. 品質檢查

- 檢查是否有遺漏的翻譯
- 確認所有連結正常運作
- 驗證程式碼區塊格式正確
- 檢查術語使用是否一致

## 技術規範

### Markdown 格式

#### 標題翻譯

```markdown
<!-- 原文 -->
## Building the Site

<!-- 繁體中文 -->
## 建置網站
```

#### 程式碼區塊

程式碼區塊**完全不翻譯**，但可以翻譯說明文字：

```markdown
<!-- 原文 -->
To build the site, run:

\`\`\`bash
bundle install
\`\`\`

<!-- 繁體中文 -->
若要建置網站，請執行：

\`\`\`bash
bundle install
\`\`\`
```

#### 連結

連結文字需要翻譯，但 URL 保持不變：

```markdown
<!-- 原文 -->
See the [Installation Guide]({% link docs/stable/installation.md %})

<!-- 繁體中文 -->
請參閱[安裝指南]({% link docs/stable/installation.md %})
```

#### 列表

```markdown
<!-- 原文 -->
* First item
* Second item

<!-- 繁體中文 -->
* 第一項
* 第二項
```

### HTML 內容翻譯

Jekyll 檔案經常混合 HTML 和 Markdown：

```html
<!-- 原文 -->
<div class="qa-wrap" markdown="1">
### Who makes DuckDB?
<div class="answer" markdown="1">
DuckDB is maintained by researchers...
</div>
</div>

<!-- 繁體中文 -->
<div class="qa-wrap" markdown="1">
### 誰開發 DuckDB？
<div class="answer" markdown="1">
DuckDB 由研究人員維護...
</div>
</div>
```

**重要**：保留所有 HTML 標籤、類別名稱和屬性。

### YAML Front Matter

```yaml
---
layout: default
title: Frequently Asked Questions  # 需要翻譯
body_class: faq                    # 不翻譯
toc: false                         # 不翻譯
---
```

Front matter 中：
- `title` 通常需要翻譯
- `layout`、`body_class` 等技術性 key-value 保持不變

### Jekyll 語法

保留所有 Jekyll 標記：

```liquid
{% link docs/stable/index.md %}
{% post_url 2022-10-28-lightweight-compression %}
{{ site.baseurl }}
```

## 常見術語對照表

### 技術術語

| 英文 | 繁體中文 | 說明 |
|------|---------|------|
| Database | 資料庫 | |
| Query | 查詢 | |
| Table | 表格 | |
| Column | 欄位 | 也可譯為「列」，但「欄位」較常用 |
| Row | 列 | 也可譯為「行」 |
| Index | 索引 | |
| Extension | 擴展 | 也可譯為「擴充功能」 |
| Client | 客戶端 | |
| Repository | 儲存庫 | GitHub 相關 |
| Pull Request | 拉取請求 | GitHub 相關 |
| Commit | 提交 | Git 相關 |
| Build | 建置 | |
| Deploy | 部署 | |
| Docker | Docker | 專有名詞，不翻譯 |
| Container | 容器 | Docker 相關 |

### DuckDB 專業術語

| 英文 | 繁體中文 | 說明 |
|------|---------|------|
| DuckDB | DuckDB | 不翻譯 |
| In-memory database | 記憶體資料庫 | |
| Persistent storage | 持久化儲存 | |
| SIMD | SIMD | 保留英文縮寫 |
| Columnar storage | 列式儲存 | |
| OLAP | OLAP | 線上分析處理，通常保留英文 |
| SQL | SQL | 不翻譯 |

### 文件相關術語

| 英文 | 繁體中文 |
|------|---------|
| Documentation | 文件 |
| Guide | 指南 |
| Tutorial | 教學 |
| Example | 範例 |
| Installation | 安裝 |
| Configuration | 配置 |
| Troubleshooting | 疑難排解 |

### Jekyll 相關術語

| 英文 | 繁體中文 |
|------|---------|
| Static site generator | 靜態網站產生器 |
| Layout | 佈局 |
| Template | 模板 |
| Front matter | 前言 |
| Markdown | Markdown（不翻譯） |
| Liquid | Liquid（不翻譯） |

## 建置與預覽

### 本機建置

詳細說明請參閱 [BUILDING.zh-TW.md](BUILDING.zh-TW.md)。

快速開始：

```bash
# 安裝依賴
bundle install

# 建立 Python 虛擬環境
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 執行網站（僅最新版本）
scripts/serve-latest.sh
```

### Docker 建置

```bash
# 建置 Docker 映像
scripts/docker-build.sh

# 執行網站
scripts/docker-serve-latest.sh

# 停止容器
scripts/docker-stop.sh
```

### 預覽檢查清單

在提交翻譯之前，請確認：

- [ ] 頁面正確顯示，沒有格式錯誤
- [ ] 所有連結可以正常點擊
- [ ] 程式碼區塊正確呈現
- [ ] 圖片正常載入
- [ ] 表格格式正確
- [ ] 導覽選單正常運作（如果有修改選單檔案）

## 提交與審查

### 提交變更

```bash
# 檢視變更
git status
git diff

# 加入變更
git add BUILDING.zh-TW.md README.zh-TW.md

# 提交
git commit -m "翻譯：新增 BUILDING.md 和 README.md 的繁體中文版本"

# 推送到遠端
git push origin translation/zh-tw-page-name
```

### 建立 Pull Request

1. 前往 GitHub 儲存庫
2. 點選「New Pull Request」
3. 選擇您的翻譯分支
4. 填寫 PR 標題和描述：

```markdown
## 翻譯：[頁面名稱] 繁體中文版本

### 變更說明
- 新增 `BUILDING.zh-TW.md`：建置說明的繁體中文翻譯
- 新增 `README.zh-TW.md`：專案說明的繁體中文翻譯

### 檢查清單
- [x] 已在本機預覽網站
- [x] 確認所有連結正常運作
- [x] 保留所有程式碼和格式
- [x] 術語使用一致
```

### 審查標準

翻譯審查將檢查：

1. **內容準確性**：翻譯是否正確傳達原意
2. **語言品質**：是否使用自然、流暢的繁體中文
3. **技術規範**：是否遵守格式和技術要求
4. **一致性**：術語使用是否與其他頁面一致

## 常見問題

### Q: 遇到不確定如何翻譯的術語怎麼辦？

A:
1. 先搜尋是否有現有的繁體中文翻譯參考
2. 查看[常見術語對照表](#常見術語對照表)
3. 可以在 PR 中提出討論
4. 必要時可保留英文並加上括號說明

### Q: 是否需要翻譯程式碼註解？

A: 程式碼區塊中的內容通常不翻譯，包括註解。但如果註解對理解很重要，可以在程式碼區塊外用中文說明。

### Q: 如何處理連結到英文頁面的情況？

A: 保持連結指向原始英文頁面。如果該頁面有繁體中文版本，可以更新連結指向中文版本。

### Q: Front matter 中的內容是否需要翻譯？

A: 通常只有 `title`、`description` 等顯示給使用者的欄位需要翻譯。技術性的欄位如 `layout`、`permalink` 等保持不變。

## 貢獻者

感謝所有為 DuckDB 文件繁體中文翻譯做出貢獻的人！

## 授權

本翻譯指南與 DuckDB 文件網站採用相同的授權條款。

---

如有任何問題或建議，歡迎在 [duckdb-web 儲存庫](https://github.com/duckdb/duckdb-web)開啟 issue 討論。
