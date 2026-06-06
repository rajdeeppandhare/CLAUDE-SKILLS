# 🕷️ Web Scraper Analyst — Claude Skill

> A browser automation and data science skill for Claude that turns any URL into a clean CSV — then analyses it like a senior data scientist, complete with ML patterns, forecasting, and hypothesis testing.

---

## ✨ What It Does

Give the Web Scraper Analyst a URL and a goal — it will:

- 🌐 **Scrape** any website using Playwright — static pages, JS-heavy SPAs, e-commerce, news feeds, dashboards
- 📄 **Follow pagination** automatically — next buttons, URL params, infinite scroll, load-more
- 📦 **Output a clean CSV** — typed columns, deduped rows, ISO dates, metadata included
- 📊 **Analyse like a data scientist** — descriptive stats, distributions, correlations, outlier detection
- 🤖 **Find ML patterns** — clustering, feature importance, multivariate anomaly detection
- 📅 **Forecast trends** — time series analysis, linear extrapolation, confidence intervals
- 🧪 **Test hypotheses** — auto-selects the right statistical test (t-test, Mann-Whitney, ANOVA, chi-square)
- 🐍 **Give you runnable code** — complete Playwright scraper + pandas analysis scripts, ready to execute

---

## 🗂️ What It Can Scrape

| Site Type | Examples | Strategy |
|---|---|---|
| 📋 Static HTML Tables | Wikipedia, financial data, gov data, sports stats | Direct DOM extraction |
| 🛒 E-Commerce Listings | Product grids, prices, ratings, stock status | Card scraper + pagination |
| 📰 News & Article Feeds | Headlines, authors, dates, summaries, URLs | Feed scraper |
| ⚡ JS-Heavy SPAs | React/Vue/Angular apps, dashboards | Network idle + API interception |
| 🔄 Infinite Scroll | Social feeds, Pinterest-style layouts | Scroll automation loop |
| 📑 Multi-Page Results | Search results, directories, catalogues | Auto-pagination (3 modes) |

---

## 📊 Analysis Depth

| Layer | What's Included |
|---|---|
| 📈 Descriptive | Mean, median, std, percentiles, value counts, cardinality |
| 📉 Distributions | Skewness, kurtosis, normality tests, histogram description |
| 🔗 Correlations | Pearson + Spearman, top correlated pairs, interpretation |
| 🚨 Outliers | IQR, Z-score, modified Z-score, Isolation Forest (multivariate) |
| 🤖 ML Patterns | K-Means clustering, silhouette scoring, feature importance |
| 📅 Forecasting | Trend direction, volatility, linear extrapolation + 95% CI |
| 🧪 Hypothesis Tests | Auto-selected test, effect size (Cohen's d), p-value interpretation |
| ⚠️ Data Quality | Missing patterns, type mismatches, duplicate detection, constant cols |

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `web-scraper-analyst.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `web-scraper-analyst/` folder
3. Rename to `web-scraper-analyst.skill`
4. Install via Settings → Skills

### Option 3 — Install Playwright (to run the generated scripts)
```bash
pip install playwright pandas numpy scipy scikit-learn matplotlib seaborn
playwright install chromium
```

---

## 💬 Example Usage

**E-Commerce Scrape + Analysis**
```
Scrape https://books.toscrape.com and give me a CSV of all books
with title, price, rating, and availability. Then analyse it —
what price ranges are most common? Any correlation between rating and price?
```

**Wikipedia Table Extraction**
```
Extract the table from https://en.wikipedia.org/wiki/List_of_countries_by_GDP
and analyse it — distribution of GDP, top/bottom 10, regional patterns.
```

**News Feed Analysis**
```
Scrape the latest 100 articles from https://techcrunch.com and analyse —
which categories are most covered? Any patterns in publishing times?
```

**Multi-Page Product Scrape**
```
Scrape all pages of search results from this URL, follow pagination automatically,
and give me a CSV. Then tell me — what's the price distribution? Any outliers?
```

**Time Series from a Dashboard**
```
This URL has a table of monthly sales data going back 3 years.
Extract it, build the CSV, then forecast the next 6 months.
```

---

## 📤 Example Output

```
🌐 Target: https://books.toscrape.com
📦 Extraction Strategy: Static HTML — paginated product grid
📄 Pages: 50 pages followed automatically
📊 Rows Extracted: 1,000 books
📁 Output: scraped_data.csv

## 📊 Dataset Overview
1,000 rows × 6 columns | 0 missing values | 0 duplicates

## 📈 Descriptive Statistics
Price (GBP): mean=£35.07 | median=£34.25 | std=£14.86 | min=£10.00 | max=£59.99
Rating: mean=3.0 | median=3.0 | std=1.4 | range=1–5

## 🔗 Correlations
Price × Rating: r=0.021 (Spearman) — negligible, price and rating are independent

## 🚨 Outliers
No significant price outliers detected (IQR method)
Isolation Forest: 50 multivariate anomalies flagged (5% contamination)

## 🤖 ML Patterns
Optimal clusters: k=3 (silhouette=0.41)
  Cluster 0 — Budget books: avg £15.20, mixed ratings
  Cluster 1 — Mid-range: avg £35.50, avg rating 3.2
  Cluster 2 — Premium: avg £52.10, avg rating 2.9

## 💡 Key Insights
1. Price is uniformly distributed — no natural price tiers despite 50 categories
2. Rating has zero correlation with price — expensive ≠ better reviewed
3. 3 natural clusters by price point; premium books rated slightly lower on average
4. 'One' and 'Two' star books are underrepresented (18% combined)
5. Average price £35 with low skew — dataset appears curated, not real market data

⚠️ Data Quality: source_url and scraped_at metadata columns included in CSV
```

---

## 📁 Skill Structure

```
web-scraper-analyst/
├── SKILL.md                          # Core skill logic & routing
├── README.md                         # This file
├── web-scraper-analyst.skill         # Installable skill package
└── references/
    ├── playwright-patterns.md        # Scraper code patterns by site type
    └── data-science-playbook.md      # Statistical methods & ML techniques
```

---

## ⚙️ Configuration (in generated scripts)

| Variable | Default | Description |
|---|---|---|
| `MAX_PAGES` | 50 | Safety cap on pagination depth |
| `SCROLL_LIMIT` | 20 | Max scroll attempts for infinite scroll |
| `HEADLESS` | True | Run browser without UI |
| `WAIT_STRATEGY` | networkidle | Page load wait (`networkidle` / `domcontentloaded`) |

---

## ⚠️ Responsible Scraping

- Always checks `robots.txt` and flags disallowed targets
- Adds 1–2s polite delay between page requests by default
- Flags Cloudflare / heavy anti-bot protection
- Never scrapes login-gated content without user-provided auth
- MAX_PAGES capped at 50 by default — must be explicitly raised

---

## 🔗 Part of the Claude Skills Portfolio

This skill is part of a growing portfolio of domain-specific Claude skills — structured, installable packages that give Claude deep expertise in specialised areas.

**Other skills in this portfolio:**
- 🔍 [Cybersecurity Excavator](../cybersecurity-excavator/) — Vulnerability scanning & security audit
- 🧾 [Global Accountant](../global-accountant/) — Tax, GST/VAT, financial statements across 40+ jurisdictions
- 🧠 [Context Memory](../context-memory/) — Preserve conversation context across session breaks

---

*Built with the Claude Skills framework · Powered by Playwright + pandas + scikit-learn ·*
