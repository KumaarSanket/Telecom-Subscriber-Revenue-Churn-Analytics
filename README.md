# Telecom-Subscriber-Revenue-Churn-Analytics

# 📡 Telecom Customer Intelligence Dashboard
## Revenue, Service Bundle & Churn Analytics

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Records](https://img.shields.io/badge/Records-7%2C043-0284C7?style=for-the-badge)
![Churn](https://img.shields.io/badge/Churn_Rate-26.54%25-DC2626?style=for-the-badge)

> Data Analytics Portfolio · Tools: Python + MySQL + Power BI Desktop

---

## 📌 Project Overview

Executed a 3-phase end-to-end analytics pipeline on IBM's extended Telco Customer Churn dataset — 7,043 subscribers across 33 raw columns — selecting the richer XLSX source (geography, CLTV, predictive Churn Score, free-text exit reasons) over the standard 21-column CSV variant. • Phase 1 (Python): fixed a text-formatted currency field, dropped 4 constant/redundant columns, engineered 4 derived features, and generated 10 EDA visualizations. • Phase 2 (MySQL): built a 29-column table, 8 indexes, 8 validated analytical queries, and a 13-dimension VIEW — diagnosing and fixing a hidden carriage-return bug along the way. • Phase 3 (Power BI): engineered 15 DAX measures and built a 28-visual dashboard including a true geographic map, surfacing that Month-to-month customers churn **15× more** than Two-year customers, with **$139,131/month** in revenue at risk.

---

## 🎯 Problem Statement

A telecom provider maintained 7,043 subscriber records spanning demographics, 9 service subscriptions, contract terms, and churn outcomes with no structured reporting layer. Retention teams had no segmented view of which contract types or service bundles drove the 26.54% churn rate, no quantified revenue-at-risk figure, and — despite having both a predictive Churn Score and free-text exit reasons for every departure — no mechanism to validate the score or translate raw reasons into a prioritized retention strategy.

---

## 🎯 Objectives

- **Obj 1:** Execute a structured Python EDA pipeline — clean the 33-column source, fix currency formatting, engineer 4 derived features
- **Obj 2:** Build a normalized MySQL database with inline conversion, 8 indexes, and 8 validated analytical queries
- **Obj 3:** Create a 13-dimension VIEW enabling multi-factor Power BI segmentation
- **Obj 4:** Engineer DAX measures for churn rate, revenue-at-risk, and weighted VIEW averages
- **Obj 5:** Build a 28-visual dashboard including a true geographic map from subscriber lat/long
- **Obj 6:** Categorize 20 raw churn reasons into 6 business categories and validate IBM's predictive Churn Score

---

## 📁 Dataset

| Attribute | Detail |
|---|---|
| **Name** | IBM Telco Customer Churn (Extended) |
| **Source** | Kaggle / IBM Sample Data Sets — `Telco_customer_churn.xlsx` |
| **Format** | Excel Workbook (.xlsx) — richer than the standard 21-column CSV |
| **Records** | 7,043 customers |
| **Columns** | 33 raw → 29 kept (4 constant/redundant dropped) |
| **Null Values** | Zero, except Churn Reason (5,174 blank — correct, only churners have one) |
| **Geographic Scope** | 1,129 unique cities, all California |
| **Data Quality Issue** | Total Charges stored as text, 11 blanks (all Tenure=0 customers) |

### Why XLSX Over CSV?

| Feature | Standard CSV (21 cols) | Extended XLSX (33 cols) |
|---|---|---|
| Geography (city, lat/long) | ❌ | ✅ |
| Customer Lifetime Value | ❌ | ✅ |
| Predictive Churn Score | ❌ | ✅ |
| Free-text Churn Reason | ❌ | ✅ |

### Key Column Definitions

| Column | Type | Description |
|---|---|---|
| customer_id | Text (PK) | Unique subscriber identifier |
| city, latitude, longitude | Geo | Enables true map visuals |
| senior_citizen | 1/0 (converted) | Converted from Yes/No at import |
| tenure_months | Whole Number | 0–72 months as subscriber |
| contract | Text | Month-to-month / One year / Two year |
| monthly_charges, total_charges | Decimal | Billing amounts (USD) |
| churn_value, churn_score, cltv, churn_reason | Mixed | Actual outcome, predictive score, lifetime value, exit reason |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python 3.11** | Phase 1 — EDA, XLSX ingestion, feature engineering, 10 charts, CSV export |
| **pandas / openpyxl** | `read_excel()`, boundary-safe `pd.cut()`, groupby aggregation |
| **matplotlib / seaborn** | Bar, donut, scatter, box plot, correlation heatmap |
| **MySQL 8.0 + Workbench** | Table creation, `LOAD DATA INFILE`, 8 indexes, 8 queries, VIEW |
| **Power BI Desktop** | MySQL connection, DAX modelling, 28-visual dashboard, geographic map |
| **DAX** | 15 measures — 12 core + 3 weighted-average correction measures |
| **JSON Theme File** | Telecom Signal Intelligence — Network Blue custom theme |

---

## ⚙️ Python EDA Process (10 Steps)

```
Step 01 → pd.read_excel('Telco_customer_churn.xlsx') — Shape (7043, 33) confirmed
Step 02 → Compared against standard CSV (21 cols) — selected XLSX as strictly richer source
Step 03 → Dropped Count, Country, State, Lat Long — all constant/redundant → 29 columns
Step 04 → Fixed Total Charges (text→numeric, 11 blanks→0, all confirmed Tenure=0)
Step 05 → Engineered TenureBucket, CLTVBucket, ChargesBucket, ChurnReasonCategory
Step 06 → Charts 1-3 — Overview, Contract, Tenure: confirmed 26.54% churn, 15× contract gap
Step 07 → Charts 4-6 — Internet, Payment, Reason Category
Step 08 → Charts 7-9 — Scatter, Service Add-Ons, Churn Score Validation
Step 09 → Chart 10 — Correlation Heatmap (fixed a pie-chart unpacking bug along the way)
Step 10 → df_export.to_csv('telecom_cleaned.csv') — 7,043 rows × 29 columns exported
```

---

## 🗄️ MySQL Process (13 Steps)

```
Step 01 → CREATE TABLE telecom_churn (29 columns, customer_id PK)
Step 02 → LOAD DATA INFILE with SET senior_citizen = IF(@raw='Yes',1,0)
           7,043 rows loaded · 0 skipped · 0 warnings
Step 03 → Verified counts — 7,043 total, 1,869 churned (matches Python exactly)
Step 04 → Created 8 indexes (churn, contract, internet, payment, tenure, senior, city, tech support)
Step 05 → Query 1 — Overall KPIs: 26.54% churn, $139,130.85 revenue at risk
Step 06 → Query 2 — Churn by Contract: Month-to-month 42.71% → Two-year 2.83%
Step 07 → Query 3 — Internet × Payment: Fiber+Electronic Check = 53.23% (worst combo)
Step 08 → Query 4 — Churn by Tenure: 0-6mo 52.94% → 49-72mo 9.51%
Step 09 → Query 5 — Service Add-Ons: Tech Support cuts churn from 41.64% to 15.17%
Step 10 → Query 6 — Top Cities: Santa Rosa highest at 45.83%
Step 11 → Query 7 — Churn Reason Categories: fixed a hidden \r carriage-return bug
           (see Challenges) — corrected to 621/478/366/213/103/88 across 6 categories
Step 12 → CREATE VIEW vw_telecom_summary — 13-dimension aggregation for Power BI
Step 13 → Connected Power BI, renamed both tables in Power Query
```

---

## 📐 DAX Measures Created

```dax
Total Customers = COUNTROWS(telecom_churn)
-- Result: 7,043

Churn Rate % = DIVIDE([Total Churned], [Total Customers]) * 100
-- Result: 26.54%

Revenue At Risk = CALCULATE([Total Monthly Revenue], telecom_churn[churn_value] = 1)
-- Result: $139,130.85/month

Month to Month Churn Rate = CALCULATE([Churn Rate %], telecom_churn[contract] = "Month-to-month")
-- Result: 42.71%
```

**Weighted-average correction measure** (proactively built — see Challenges):

```dax
VIEW Weighted Churn Rate =
DIVIDE(
    SUMX(vw_telecom_summary, vw_telecom_summary[churn_rate_pct] * vw_telecom_summary[total_customers]),
    SUM(vw_telecom_summary[total_customers])
)
```

---

## 📊 Dashboard Visuals — 28 Visuals Across 3 Pages

### Page 1 — Churn Overview
6 KPI cards · Donut (churn split) · Bar (churn by contract) · **Geographic map** (lat/long across CA) · Internet Service + Payment Method slicers.

### Page 2 — Churn Drivers
Tenure bucket churn · Internet service churn · Payment method churn · Service add-on impact · Churn Reason Category donut · Churn Score validation · Contract + Tenure Bucket slicers.

### Page 3 — Customer Segmentation
4 KPI cards · Senior citizen churn · Dependents churn · Top cities table · Senior Citizen + Dependents slicers.

---

## 📈 Key Insights & Results

### A. Contract Type — The Dominant Retention Lever
- Month-to-month customers churn at **42.71%** vs 11.27% (One-year) vs **2.83%** (Two-year) — a **15×** gap.
- **Fiber Optic + Electronic Check** is the single worst combination at **53.23%** churn (849 of 1,595) — risk factors compound.

### B. Tenure — The First 6 Months Are Critical
- 0–6 month customers churn at **52.94%** — over half leave before establishing any relationship — falling to **9.51%** by 49–72 months.

### C. Service Bundle Impact
- Every protective add-on nearly halves churn: Online Security (41.77%→14.61%), Tech Support (41.64%→15.17%), Online Backup (39.93%→21.53%), Device Protection (39.13%→22.50%).

### D. Why Customers Actually Leave
- **Competitor Loss** leads at **33.2%** of all churn (621), followed by **Product/Price Dissatisfaction** at 25.6% (478) and **Support/Attitude Problems** at 19.6% (366).

### E. Predictive Score Validation & Demographics
- IBM's Churn Score validates strongly: churned avg **82.51** vs retained avg **50.10** (r=**0.665**) — the strongest correlate in the dataset.
- Having dependents is the strongest demographic protective factor: **6.52%** churn vs 32.55% without — a **5×** gap.

---

## 📊 KPI Summary Table

| KPI | Value | KPI | Value |
|---|---|---|---|
| Total Customers | **7,043** | Total Churned | **1,869 (26.54%)** |
| Month-to-Month Churn | **42.71%** | Two-Year Churn | 2.83% (15× gap) |
| 0-6 Month Tenure Churn | **52.94%** | 49-72 Month Churn | 9.51% |
| Fiber Optic Churn | 41.89% | DSL Churn | 18.96% |
| Electronic Check Churn | **45.29%** | Credit Card (Auto) Churn | 15.24% |
| Worst Combination | Fiber + E-Check — **53.23%** | Monthly Revenue at Risk | **$139,130.85** |
| Annualized Revenue Loss | **$1,669,570.20** | Top Churn Reason | Competitor Loss — 33.2% |
| Churn Score — Churned | 82.51 | Churn Score — Retained | 50.10 |
| Has Dependents Churn | **6.52%** | No Dependents Churn | 32.55% (5× gap) |
| Correlation — Churn Score | **0.665** | Correlation — Tenure | -0.352 |

---

## ⚡ Challenges & Solutions

**Challenge 1 — Missing openpyxl Dependency**
`pd.read_excel()` raised `ImportError: Missing optional dependency 'openpyxl'` — the first project in this portfolio reading `.xlsx` instead of `.csv`. • Resolved via `python -m pip install openpyxl`.

**Challenge 2 — Matplotlib Pie Chart Return-Value Mismatch**
`ValueError: not enough values to unpack (expected 3, got 2)` — `ax.pie()` only returns `autotexts` as a third value when `autopct` is supplied; this chart used custom labels instead. • Fixed by unpacking only `wedges, texts = ax.pie(...)`.

**Challenge 3 — Hidden Carriage Return Broke Text Categorization**
A churn-reason categorization query returned 100% of rows in one catch-all bucket. Root cause: Windows `\r\n` line endings + `LOAD DATA INFILE` configured with `LINES TERMINATED BY '\n'` left a trailing `\r` on `churn_reason` (the last column per row) — invisible, and untouched by MySQL's space-only `TRIM()`. • Diagnosed via `LENGTH()` comparison, fixed with `UPDATE telecom_churn SET churn_reason = TRIM(TRAILING '\r' FROM churn_reason)` — corrected all 1,869 rows without re-import.

**Challenge 4 — VIEW Aggregation Risk (Proactively Addressed)**
Based on an identical bug in a prior project, any visual built on `vw_telecom_summary`'s pre-aggregated columns with default SUM would show inflated values. • Built `VIEW Weighted Churn Rate` proactively — before any visual was built — preventing the bug from recurring.

---

## 🎓 Skills Learned

- **Multi-Format Source Evaluation** — compared CSV vs XLSX versions of the same dataset and selected the strictly richer source before starting analysis
- **Invisible-Character Debugging** — diagnosed a silent carriage-return bug with no error message, using a suspiciously uniform 100%-in-one-bucket result as the diagnostic signal
- **Cross-Tool Verification** — confirmed Python's independent churn-reason categorization matched MySQL's post-fix output exactly
- **Validating External Predictive Scores** — quantified IBM's Churn Score against actual outcomes (0.665 correlation) rather than assuming vendor-supplied scores are automatically trustworthy
- **Proactive Bug Prevention** — applied a lesson from a prior project's VIEW-aggregation bug before it could recur here

---

## 🎨 Custom Theme

| Attribute | Detail |
|---|---|
| **Theme Name** | Telecom Signal Intelligence — Network Blue |
| **Style** | 🌐 Dark — Deep Indigo-Navy + Electric Blue Primary |
| **Primary Accent** | #3B82F6 (Electric Blue) — signal/network association |
| **Risk Color** | #EF4444 (Red) — churned/high-risk indicators |
| **Safe Color** | #22C55E (Green) — retained/loyal customers |
| **Neutral Color** | #F59E0B (Amber) — medium-risk segments |
| **Background** | #0D1526 (Deep Indigo-Navy) |
| **Best For** | Telecom, ISP, subscription/SaaS churn dashboards |

**To apply:** Power BI → View → Themes → Browse for themes → select `Telecom_Signal_Intelligence_Network_Blue_Theme.json`

---

## 📂 Repository Structure

```
telecom-customer-revenue-churn-analytics/
│
├── 📊 Telecom_Customer_Intelligence_Dashboard.pbix   # Power BI dashboard file
├── 📁 Dataset/
│   ├── Telco_customer_churn.xlsx                     # Raw source (33 cols, primary)
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv           # Standard variant (21 cols, reference)
├── 📁 Python/
│   ├── telecom_churn_eda.py                          # Python EDA + visualization script
│   └── telecom_cleaned.csv                           # Cleaned export for MySQL
├── 📁 MySQL/
│   └── telecom_queries.sql                           # All SQL: CREATE, IMPORT, 8 queries, VIEW
├── 📁 Charts/
│   └── chart1-10_*.png                                # 10 Python EDA charts
├── 📁 Theme/
│   └── Telecom_Signal_Intelligence_Network_Blue_Theme.json
├── 📄 Telecom_Customer_Intelligence_Documentation.pdf # Full portfolio documentation
└── 📄 README.md
```

---

*Data Analytics · Telecom Customer Intelligence · Python + MySQL + Power BI · K.S. · Dataset: IBM Sample Data Sets (Kaggle)*
