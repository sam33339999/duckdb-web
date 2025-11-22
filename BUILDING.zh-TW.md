# 建置 DuckDB 文件網站

## 目錄

* [建置 DuckDB 文件網站](#建置-duckdb-文件網站)
  * [目錄](#目錄)
  * [使用本機 Jekyll 安裝](#使用本機-jekyll-安裝)
    * [前置需求](#前置需求)
    * [使用本機 Jekyll 執行網站](#使用本機-jekyll-執行網站)
  * [使用 Docker](#使用-docker)
    * [前置需求](#前置需求-1)
    * [從 Docker 執行網站](#從-docker-執行網站)
  * [使用開發容器](#使用開發容器)
  * [產生搜尋索引](#產生搜尋索引)
  * [更新發布日曆](#更新發布日曆)
  * [語法高亮器](#語法高亮器)
  * [疑難排解](#疑難排解)
    * [Jekyll 在 Windows 上無法運作](#jekyll-在-windows-上無法運作)
    * [無法安裝相依套件](#無法安裝相依套件)
    * [Jekyll 執行失敗](#jekyll-執行失敗)
    * [Bundle 更新失敗](#bundle-更新失敗)

本網站使用 GitHub Pages 所採用的 [Jekyll](https://jekyllrb.com/) 建置系統來建置。

## 使用本機 Jekyll 安裝

### 前置需求

1. 本網站使用 [Jekyll](https://jekyllrb.com/) 建置。關於設定 Jekyll 所需的 Ruby 環境，請參考 [Jekyll 安裝頁面](https://jekyllrb.com/docs/installation/macos/)。若要安裝特定的 Ruby 版本，我們建議使用 [chruby](https://jekyllrb.com/docs/installation/macos/#step-2-install-chruby-and-the-latest-ruby-with-ruby-install)。

2. 使用 Bundler 安裝 Jekyll 和其他必要的 Ruby 相依套件：

    ```batch
    bundle install
    ```

    關於設定 Jekyll 的更多細節，請參考 [GitHub 的說明文件](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)。

3. 建立 Python 虛擬環境並安裝必要套件：

    ```batch
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    ```

### 使用本機 Jekyll 執行網站

若要執行本網站，請執行：

```batch
scripts/serve-latest.sh
```

造訪 <http://localhost:4000/docs/> 以瀏覽網站。

請注意，為了節省建置時間，`serve-latest.sh` 腳本只會部署最新的穩定版本，並排除歷史封存版本。若要執行包含舊版本的完整網站，請執行：

```batch
scripts/serve.sh
```

## 使用 Docker

### 前置需求

請先安裝 [Docker](https://docs.docker.com/get-docker/)。

### 從 Docker 執行網站

為了方便移植，我們提供了 [Docker 映像檔](Dockerfile)。

首先，使用以下指令建置映像檔：

```batch
scripts/docker-build.sh
```

執行網站（僅最新版本，排除歷史封存）：

```batch
scripts/docker-serve-latest.sh
```

造訪 <http://localhost:4000/docs/> 以瀏覽網站。

執行完整網站（包含所有版本）：

```batch
scripts/docker-serve.sh
```

若要停止容器，請執行：

```batch
scripts/docker-stop.sh
```

## 使用開發容器

如果您正在使用 [開發容器](https://code.visualstudio.com/docs/devcontainers/containers)，請點選右上方的綠色 Code 按鈕來開啟一個已初始化此儲存庫的新程式碼空間（codespace）。

## 產生搜尋索引

若要產生搜尋索引，請執行：

```batch
scripts/install-dependencies.sh
scripts/generate-search-index.sh
```

## 更新發布日曆

發布日曆會由 [CI](.github/workflows/jekyll.yml) 自動更新。若要手動更新發布日曆，請執行：

```batch
python scripts/get_calendar.py
```

## 語法高亮器

我們使用 [Rouge 語法高亮器的分支版本](https://github.com/duckdb/rouge/blob/duckdb/lib/rouge/lexers/sql.rb)，該版本擴充了標準 SQL 中沒有的關鍵字（例如 `RETURNING`、`ASOF`）。這會由 Bundler 自動安裝。

若要在本機測試 `rouge` 的更新，請編輯 `Gemfile`：

```diff
-gem "rouge", git: "https://github.com/duckdb/rouge.git", branch: "duckdb"
+gem "rouge", git: "/path/to/your/local/rouge/directory"
```

然後執行：

```batch
bundle install
```

## 疑難排解

### Jekyll 在 Windows 上無法運作

如果您使用 Windows 系統，請執行以下兩個指令以確保 Jekyll 正常運作：

```batch
gem uninstall eventmachine --force
gem install eventmachine --platform ruby
```

### 無法安裝相依套件

若出現以下錯誤：

```console
posix-spawn.c:226:27: error: incompatible function pointer types passing 'int (VALUE, VALUE, posix_spawn_file_actions_t *)' (aka 'int (unsigned long, unsigned long,
```

解決方法是執行以下 `bundle` 指令：

```batch
bundle config set --global build.posix-spawn "--with-cflags=-Wno-error=incompatible-function-pointer-types"
```

### Jekyll 執行失敗

升級 Ruby 後，Jekyll 出現以下錯誤訊息：

```console
/opt/homebrew/opt/ruby/bin/bundler:25:in `load': cannot load such file -- /opt/homebrew/lib/ruby/gems/3.3.0/gems/bundler-2.5.4/exe/bundler (LoadError)
	from /opt/homebrew/opt/ruby/bin/bundler:25:in `<main>'
```

解決方法是在儲存庫中執行以下指令：

```batch
gem install bundler
bundle install
```

如果此方法仍無法解決，您可能需要升級 Bundler 版本。請執行：

```batch
rm Gemfile.lock
bundle install
```

### Jekyll 內容無法正確呈現

動態頁面重新產生可能無法套用到 includes、鐵路圖（railroad diagrams）或其他內容。若要強制重新產生頁面或 include 檔案，使其在本機瀏覽器中正確呈現，請執行以下步驟：

1. 停止執行 Jekyll 建置指令的終端機會話，例如 `scripts/serve-latest.sh`。
2. 對您的特定頁面執行 `git add myfile.md`。
3. 停止 Jekyll 並執行 `rm -rf _site` 來清理先前產生的檔案。
4. 如果網站仍未重新產生，請執行 `git clean` 來清理儲存庫，但請小心不要遺失任何未暫存或未提交的變更。
5. 再次從終端機執行 `scripts/serve-latest.sh`。
6. 如果您正在編輯的頁面已在瀏覽器中開啟，它應該會自動重新整理。如果沒有，請手動重新整理。現在應該能看到本機分支的變更。

### Bundle 更新失敗

執行 Bundle 更新時出現以下錯誤訊息：

```batch
bundle update
```

```console
Git error: command `git fetch --force --quiet
/opt/homebrew/lib/ruby/gems/3.3.0/cache/bundler/git/minima-4abf4ea566b1c7c640342d1bbff5586f3c10dd05 --depth 1
1d5286cf9a1aae34078420d183d560dd673d98b5` in directory /opt/homebrew/lib/ruby/gems/3.3.0/bundler/gems/minima-1d5286cf9a1a has failed.
fatal: '/opt/homebrew/lib/ruby/gems/3.3.0/cache/bundler/git/minima-4abf4ea566b1c7c640342d1bbff5586f3c10dd05' does not appear to be a git
repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

若要解決此問題，請清理 Jekyll gem 快取：

```batch
rm -rf /opt/homebrew/lib/ruby/gems/3.3.0/cache/
```

### Bundle 安裝失敗

執行 Bundle 安裝時出現以下錯誤訊息：

```batch
bundle install
```

```console
The running version of Bundler (2.5.11) does not match the version of the specification installed for it (2.5.18). This can be caused by
reinstalling Ruby without removing previous installation, leaving around an upgraded default version of Bundler. Reinstalling Ruby from
scratch should fix the problem.
```

根據 [Stack Overflow 解答](https://stackoverflow.com/a/63761800)，解決方法是執行：

```batch
gem update --system
```

### `ERROR bad URI`

當嘗試透過 HTTPS（`https://localhost:4000/`）存取本機建置的網站時，會出現以下錯誤：

```console
[2024-09-17 12:15:36] ERROR bad URI `����\x12`�\x17\x03�L\x00\x00\x14�'.
[2024-09-17 12:15:36] ERROR bad Request-Line ...
```

這經常發生在瀏覽器試圖強制使用 HTTPS 連線時，例如 Safari。解決方法是使用 HTTP 連線（`http://localhost:4000`），並可選擇嘗試其他瀏覽器（例如 Chrome 或 Firefox）。
