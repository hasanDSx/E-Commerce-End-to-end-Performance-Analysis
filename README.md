# 🏪 Toy Store E-Commerce — Analytics Report

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![DuckDB](https://img.shields.io/badge/DuckDB-0.9%2B-yellow?style=for-the-badge&logo=duckdb&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-5.0%2B-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)

**An end-to-end analytics report for a toy e-commerce store,  
covering data quality, revenue performance, product analysis, traffic, and refund risk.**

</div>

---

## 📋 Table of Contents

- [The Business Context](#the-business-context)
- [Dataset](#dataset)
- [Data Quality & EDA](#data-quality--eda)
- [Macro-Level Analysis](#macro-level-analysis)
- [Micro-Level Analysis](#micro-level-analysis)
- [Key Findings](#key-findings)
- [Installation & Usage](#installation--usage)
- [Project Structure](#project-structure)

---

## 🔍 The Business Context <a id="the-business-context"></a>

This report analyses ~3 years of transactional data (March 2012 – March 2015) for a toy
e-commerce store. The goal is to move beyond surface-level KPIs and answer **14 specific
business questions** across six analytical dimensions:

- Revenue & Profitability
- Product Performance
- Traffic & Conversion
- Device Behaviour (Mobile vs Desktop)
- Refund Risk
- Growth Anomalies & Peak Events

The pipeline starts with raw data extraction from **PostgreSQL**, loads it into **DuckDB**
for in-process SQL analysis, and produces both static (matplotlib/seaborn) and interactive
(Plotly) visualisations.

---

## 📊 Dataset <a id="dataset"></a>

| Property | Value |
|---|---|
| Source | PostgreSQL → DuckDB ETL |
| Time Span | March 2012 – March 2015 (≈ 3 years) |
| Total Transactions | 32,313 orders / 40,025 items |
| Gross Revenue | $1,938,510 |
| Fraud / Refund Rate | ~4.4% |

### Tables Overview

| Table | Rows | Columns |
|---|---|---|
| `website_pageviews` | 1,188,124 | 4 |
| `website_sessions` | 472,871 | 9 |
| `order_items` | 40,025 | 7 |
| `orders` | 32,313 | 8 |
| `order_item_refunds` | 1,731 | 5 |
| `products` | Small | — |

---

## 🔬 Data Quality & EDA <a id="data-quality--eda"></a>

### Completeness & Integrity Checks

Before any analysis, a full data quality audit was run across all six tables:

| Check | Result |
|---|---|
| ✅ Null Values | All tables clean — zero missing values |
| ✅ Duplicate Records | Zero duplicates across all tables |
| ✅ Negative Prices | None found |
| ✅ Refund > Purchase Price | None found |
| ✅ Orphan Refunds | None found |
| ✅ Future Dates | None found |
| ✅ Refund Before Purchase | None found |
| ✅ Zero Price Transactions | None found |

> The dataset is exceptionally clean — all 8 business rule validations passed with zero issues.

---

### Outlier Analysis (IQR Method)

Using the **IQR method**, outliers were identified in numeric columns across all tables.
The standout finding: `price_usd` and `cogs_usd` in `order_items` show **~39% outlier rate**
— expected behaviour given the diverse product price range in the catalogue.

A bar chart visualises outlier percentages per column with a 25% threshold reference line.

---

### Temporal Coverage

| Table | Start Date | End Date |
|---|---|---|
| `order_items` | 2012-03-19 | 2015-03-19 |
| `orders` | 2012-03-19 | 2015-03-19 |
| `order_item_refunds` | 2012-04-06 | 2015-04-01 |

---

## 📈 Macro-Level Analysis <a id="macro-level-analysis"></a>

### 💰 All-Time KPIs

| Metric | Value |
|---|---|
| Total Orders | 32,313 |
| Total Items Sold | 40,025 |
| Gross Revenue | $1,938,510 |
| Total COGS | $722,370 |
| Gross Profit | $1,216,140 |
| Total Refunds | $85,339 |
| **Net Revenue** | **$1,853,171** |
| Profit Margin % | ~62.7% |
| AOV (Avg Order Value) | $63.80 |
| Items Per Order | ~1.24 |
| Refund Rate | ~4.4% |

---

### 📅 Yearly Performance

| Year | Orders | Revenue | YoY Growth |
|---|---|---|---|
| 2012 | 2,586 | $129,274 | — |
| 2013 | 7,447 | $393,248 | **+204%** |
| 2014 | 16,860 | $1,075,612 | **+173%** |
| 2015 | 5,420 | $340,375 (Q1 only) | N/A* |

> *2015 data only covers Q1 — see Q13 for the corrected YoY comparison.

The Plotly chart shows grouped bars (Revenue + Gross Profit) with a secondary axis
displaying the YoY Growth % line.

---

### 📆 Monthly Revenue Trend

Monthly revenue is visualised as a filled area chart (2012–2015), with a secondary
bar chart showing **Month-over-Month (MoM) growth** in green (positive) and red (negative).

---

### 🧸 Top Products by Revenue & Margin

| Product | Items Sold | Revenue | Gross Margin % |
|---|---|---|---|
| The Original Mr. Fuzzy | 24,226 | $1,211,058 | ~61% |
| The Forever Love Bear | 5,796 | $347,702 | ~62.5% |
| **The Birthday Sugar Panda** | 4,985 | $229,260 | **~68.5% 🏆** |
| The Hudson River Mini Bear | — | — | — |

> **Sugar Panda** is the margin champion despite ranking 3rd in revenue volume.

---

### 🌐 Traffic Source Efficiency

| Source / Campaign | Sessions | Conversion Rate |
|---|---|---|
| gsearch / nonbrand | 282,706 | 6.66% |
| **direct / direct** | 83,328 | **7.34% 🏆** |
| bsearch / nonbrand | 54,909 | 6.95% |

Direct traffic converts best — suggesting strong brand recognition among returning visitors.

---

### 📱 Device Performance Gap

| Device | Sessions | Orders | Conversion Rate |
|---|---|---|---|
| **Desktop** | 327,027 | 27,805 | **8.50%** |
| Mobile | 145,844 | 4,508 | 3.09% |

> **The gap is 5.41 percentage points.** Mobile represents a significant revenue opportunity
> with a focused UX improvement effort.

---

## 🔎 Micro-Level Analysis <a id="micro-level-analysis"></a>

### Group 1 — Revenue & Profitability Deep Dive

**Q1 — Peak Season (Nov–Dec 2014): Mr. Fuzzy vs Sugar Panda**

Comparing the two products during peak season, Sugar Panda delivered a **higher Net Margin %**
after accounting for refunds, despite Mr. Fuzzy having higher gross revenue. A 3-panel
Plotly chart shows Gross Revenue, Total Refunds, and Net Margin side by side.

---

**Q2 — Post-Growth Spike Refund Risk (Jul–Sep 2013)**

Following the 204% revenue spike in 2013, Mr. Fuzzy showed a **disproportionately high
refund rate** relative to its sales volume. A dual-axis chart compares items sold (bars)
vs refund rate % (line) with the average 4.4% threshold marked.

---

**Q3 — 2014's Top 3 Profit Months**

The three highest gross profit months in 2014 (highlighted in red) all occurred in **Q4**.
All 12 months are compared against the yearly AOV benchmark of $63.80 — most months exceeded
it, confirming a healthy pricing and demand dynamic.

---

### Group 2 — Product Performance

**Q4 — Weekly Sales: Mini Bear vs Forever Love Bear (2014)**

While Mini Bear ranks 4th overall, there were specific weeks in 2014 where it **outsold
Forever Love Bear**. A Plotly line chart highlights the weeks of Mini Bear dominance
with a green fill area.

---

**Q5 — Birthday Sugar Panda Launch Velocity**

After launch in early 2014, the product's **first 100 orders** were tracked via a
cumulative area chart, with vertical lines marking the launch date and the 100th order date,
showing how many days it took to reach the milestone.

---

**Q6 — Q4 2014 Cross-Sell Rate**

A pie chart reveals the **cross-sell adoption rate** (orders with 2+ items) in Q4 2014,
alongside a horizontal bar chart showing the **top 5 most common product combinations**.

---

### Group 3 — Traffic & Conversion

**Q7 — gsearch nonbrand Seasonal Performance (Oct–Nov 2014)**

The monthly conversion rate for gsearch nonbrand throughout 2014 is plotted against the
all-time average of 6.66%. October and November are highlighted in red to reveal
whether peak season drove a seasonal lift or decline.

---

**Q8 — Desktop vs Mobile Gap (Q1 2013, gsearch nonbrand)**

Two Plotly charts show the conversion rate by device type for Jan–Mar 2013, and the
**gap in percentage points** between desktop and mobile — colour-coded red where the
gap exceeded 5pp.

---

### Group 4 — Device & Session Behaviour

**Q9 — How Wide Was the Device Gap in 2012?**

The yearly desktop vs mobile conversion rates are compared from 2012–2015, with a
trend line showing whether the gap widened or narrowed over time relative to the
all-time average of 5.41pp.

---

**Q10 — Best Day of Week for Desktop (2014)**

A two-panel chart shows which **day of the week** had the highest desktop session volume
and whether that same day also had the highest conversion rate — or if volume and
conversion quality peak on different days.

---

### Group 5 — Refunds & Risk

**Q11 — 2014 Monthly Refund Spike Detection**

A stacked Plotly chart tracks monthly refund volume (top panel) and refund rate %
(bottom panel), with a dashed line at the 4.4% all-time average.
Months exceeding the threshold are highlighted in red — flagging specific risk windows.

---

**Q12 — Mr. Fuzzy vs Sugar Panda: Full Profitability Audit**

A 3-panel chart directly compares the two flagship products on:
- Refund Rate %
- Gross Margin %
- Net Margin %

This reveals whether Mr. Fuzzy's volume advantage survives a quality-adjusted comparison.

---

### Group 6 — Growth & Anomalies

**Q13 — Correcting the Misleading 2015 YoY Drop**

The raw YoY comparison shows a −68% drop for 2015 — but 2015 only has Q1 data.
The corrected **Q1-to-Q1 comparison** (2013 vs 2014 vs 2015) shows the true growth
trajectory and avoids a false alarm.

---

**Q14 — Top 5 Revenue Days in History**

The 5 highest-grossing individual days across all years are identified and plotted
as star markers on the full daily revenue timeline. A ranked horizontal bar chart
shows their exact values — revealing whether peak days cluster in the same seasonal
window or are spread throughout the year.

---

## 🏆 Key Findings <a id="key-findings"></a>

| # | Finding | Impact |
|---|---|---|
| 1 | Revenue grew **+204%** in 2013 and **+173%** in 2014 — explosive and sustained growth | High |
| 2 | **Sugar Panda** has the highest margin (68.5%) despite ranking 3rd in revenue | High |
| 3 | Mobile conversion (3.09%) is **2.75× lower** than desktop (8.50%) | High |
| 4 | **Direct traffic** converts best (7.34%) — strong organic brand pull | Medium |
| 5 | Fraud spikes in **Mr. Fuzzy** post-2013 growth surge — quality risk at scale | Medium |
| 6 | Q4 is consistently the strongest quarter across revenue, profit, and AOV | Medium |
| 7 | Early morning hours (1–4 AM) show elevated refund/fraud activity | Medium |
| 8 | 2015 data covers Q1 only — raw YoY comparisons are misleading without correction | Low |

---

## ⚙️ Installation & Usage <a id="installation--usage"></a>

```bash
# Clone the repo
git clone https://github.com/hasanDSx/Toy-Store-E-Commerce.git
cd Toy-Store-E-Commerce

# Create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install pandas matplotlib seaborn plotly duckdb
```

> Ensure PostgreSQL is running with the `toys_store_ecommerce` database, or provide
> an existing DuckDB file at the path configured in the notebook.

### Run the full pipeline

Open and run `Analysis_Report.ipynb` in Jupyter:

```bash
jupyter notebook Analysis_Report.ipynb
```

This will:
1. Connect to PostgreSQL and load all tables into DuckDB
2. Run the full data quality audit (nulls, duplicates, business rules)
3. Generate all EDA and macro-level visualisations
4. Execute all 14 micro-level analytical questions
5. Output interactive Plotly charts for each section

---

## 📁 Project Structure <a id="project-structure"></a>

```
toy-store-analytics/
│
├── Analysis_Report.ipynb       # Full Jupyter notebook (88 cells)
├── my_analytics.db             # DuckDB database file (generated after ETL)
└── README.md
```

---

<div align="center">
Made by Hasan Muhammad
</div>
