---
layout: default
redirect_from:
- /cal
- /release-calendar
- /dev/release-dates
- /dev/release-dates/
- /dev/release-calendar
- /dev/release-calendar/
- /docs/dev/release_calendar
- /docs/dev/release_calendar/
- /docs/stable/dev/release_calendar
- /docs/stable/dev/release_calendar/
- /docs/preview/dev/release_calendar
- /docs/preview/dev/release_calendar/
title: 發布行事曆
body_class: release-calendar blog_typography post
max_page_width: medium
toc: false
lang: zh-TW
---

<div class="wrap pagetitle">
  <h1>發布行事曆</h1>
</div>

DuckDB 遵循[語意化版本控制](https://semver.org/spec/v2.0.0.html)。
較大的新功能在次要版本中引入，
而修補版本主要包含錯誤修正。

## 即將發布的版本

即將發布的 DuckDB 版本的計劃日期如下所示。
**請注意，這些日期是暫定的**，DuckDB 維護者可能會決定推遲發布日期，以確保版本的穩定性和品質。

<!-- markdownlint-disable MD055 MD056 MD058 -->

{% if site.data.upcoming_releases.size > 0 %}
| 日期 | 版本 |
|:-----|--------:|
{%- for release in site.data.upcoming_releases %}
| {{ release.start_date }} | {{ release.title }} |
{%- endfor %}
{% else %}
_目前沒有宣布即將發布的版本。請稍後再回來查看。_
{% endif %}

<!-- markdownlint-enable MD055 MD056 MD058 -->

有關計劃的新功能，請參閱 [DuckDB 開發路線圖]({% link roadmap.md %})。

### LTS 版本

從 v1.4.0 開始，每隔一個 DuckDB 版本將成為長期支援（LTS）版本。
對於 LTS DuckDB 版本，[社群支援](https://duckdblabs.com/community_support_policy/)的支援期目前為發布後一年。
[DuckDB Labs](https://duckdblabs.com/) 在社群支援到期後為較舊的 LTS 版本提供支援。

<img src="/images/blog/lts-support-light.svg" alt="DuckDB LTS support" width="900" class="lightmode-img">
<img src="/images/blog/lts-support-dark.svg" alt="DuckDB LTS support" width="900" class="darkmode-img">

有關終止支援資訊的概覽，請參閱 [`endoflife.date` 上的 DuckDB 條目](https://endoflife.date/duckdb)。

## 過往版本

以下列出 DuckDB 的過往版本及其代號（如適用）。
在 0.2.2 和 0.3.3 版本之間，所有版本（包括修補版本）都會獲得代號。
自 0.4.0 版本以來，只有主要版本和次要版本獲得代號。

<!-- markdownlint-disable MD034 MD055 MD056 MD058 -->

|      | 日期 | 版本 | 代號 | 命名來源 | 終止支援      |
|------|:-----|--------:|----------|-------------|------------------|
{% for row in site.data.past_releases %}
  {%- capture logo_filename %}images/release-icons/{{ row.version_number }}.svg{% endcapture -%}
  {%- capture logo_exists %}{% file_exists {{ logo_filename }} %}{% endcapture -%}
  | {% if logo_exists == "true" %}![Logo of version {{ row.version_number }}](/{{ logo_filename }}){% endif %} | {{ row.release_date }} | [{{ row.version_number }}{% if row.lts == "true" %} LTS{% endif %}](https://github.com/duckdb/duckdb/releases/tag/v{{ row.version_number }}) | {% if row.blog_post %}[{{ row.codename }}]({{ row.blog_post }}){% else %}{{ row.codename | default: "–" }}{% endif %} | {% if row.duck_wikipage %}<a href="{{ row.duck_wikipage }}">{% endif %}{{ row.duck_species_primary | default: "–" }}{% if row.duck_wikipage %}</a>{% endif %} {% if row.duck_species_secondary != nil %}_({{ row.duck_species_secondary }})_{% endif %} | {% if row.end_of_life %}{{ row.end_of_life }}{% endif %} |
{% endfor %}

<!-- markdownlint-enable MD034 MD055 MD056 MD058 -->

## CSV 檔案格式的發布行事曆

您可以取得[包含過往 DuckDB 版本的 CSV 檔案](/data/duckdb-releases.csv)，並使用 DuckDB 的 [CSV 讀取器]({% link docs/stable/data/csv/overview.md %})進行分析。
例如，您可以使用 [`lag` 視窗函式]({% link docs/stable/sql/functions/window_functions.md %}#lagexpr-offset-default-order-by-ordering-ignore-nulls)計算版本之間的平均天數：

```sql
SELECT avg(diff) AS average_days_between_releases
FROM (
    SELECT release_date - lag(release_date) OVER (ORDER BY release_date) AS diff
    FROM 'https://duckdb.org/data/duckdb-releases.csv'
);
```
