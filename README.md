

<div align="center">

# 💳 Credit Card Financial & Customer Analytics Dashboard

### *Transforming 10,293 transaction records into executive-grade financial intelligence with SQL + Power BI*

![Power BI](https://img.shields.io/badge/Power_BI-Latest-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Measures-000000?style=for-the-badge&logo=powerbi&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-Data_Source-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

</div>

---

<div align="center">

### 🖼️ Hero Dashboard — Credit Card Transaction Report

![Credit Card Transaction Dashboard](Screenshots/cc_transaction_dashboard.png)

*A 2-page Power BI executive dashboard built on a PostgreSQL star-schema model with 10K+ transactional rows.*

</div>

---

## 📌 Project Overview

This project delivers an end-to-end **credit card financial analytics solution** that converts raw weekly transaction data into strategic business intelligence. It bridges the gap between operational data and executive decision-making by combining a PostgreSQL data warehouse, a curated Power BI semantic model, and DAX-driven KPIs. The dashboard empowers product managers, risk teams, and marketing leaders to monitor revenue performance, customer demographics, expense behavior, and delinquency risk in a single, interactive view.

The pipeline ingests 52 weeks of transactional data plus an incremental Week-53 batch, validates it through SQL constraints, models it into a single-direction star schema, and surfaces it through two report pages containing 39 visuals — bar charts, KPI cards, treemaps, slicers, and a combo chart. Every visual is bound to a real business question: *Where is revenue coming from? Who are our customers? Where is risk concentrating?*

### 🎯 Business Problem

A retail bank's credit card division was tracking performance through disconnected Excel exports — one file for transactions, one for customers, and a separate one for weekly incremental loads. Leadership had no single source of truth to answer critical questions: *Which card category drives the most revenue? Which customer segment is most likely to default? Are chip-based transactions growing? Which expense type dominates spend?* Without these answers, marketing budgets were spent blindly, risk signals were caught late, and quarterly reviews took days of manual reconciliation.

### 💡 Solution

Built a centralized PostgreSQL data warehouse (`ccdb`) that consolidates transactional and customer data into two normalized tables. Created a Power BI semantic model with explicit relationships, calculated columns for `AgeGroup` and `IncomeGroup`, and DAX measures for Revenue, Interest Earned, Transaction Volume, and Delinquency Rate. Delivered a two-page interactive dashboard with cross-filtering slicers (Quarter, Card Category, Gender, Income Group) that lets executives drill from macro KPIs down to individual customer segments in under three clicks.

### 📊 Key Results

> 💰 **$53.5M+** total revenue analyzed across 4 quarters (transaction amount + interest earned)
>
> 🧾 **667,234** transactions processed across **10,293** unique customer accounts
>
> 📈 **Blue Card** drives **70.6%** of total revenue ($37.8M) — clear volume leader
>
> 💳 **70.3%** of revenue comes from Swipe transactions; Chip usage at 23.9% — opportunity for contactless migration
>
> ⚠️ **6.06%** delinquency rate (624 accounts) — flagged for risk-team review
>
> 🎯 **57.5%** activation-within-30-days rate — measurable onboarding improvement target

---

## 🖥️ Dashboard Preview

The Power BI report contains two pages, each tuned for a different audience: the **Transaction** page for finance and product teams, and the **Customer** page for marketing and customer-success teams.

### 📊 Page 1: CC Transaction Report

![CC Transaction Dashboard](Screenshots/cc_transaction_dashboard.png)

> *Quarterly revenue, transaction volume, card-category breakdown, expense-type distribution, chip-vs-swipe behavior, and delinquency signals — all on one executive view.*

**Visuals on this page (20 visuals):**
- 4 KPI cards (Revenue, Transaction Amount, Transaction Count, Interest Earned)
- Quarter slicer + Card Category slicer
- Stacked column & line combo chart — Revenue & Transaction Count by Quarter
- Bar charts — Revenue by Card Category, Revenue by Expense Type, Revenue by Use Chip
- Treemaps — Revenue distribution by Expense Type and Card Category
- Detailed transaction table

### 👥 Page 2: CC Customer Report

![CC Customer Dashboard](Screenshots/cc_customer_dashboard.png)

> *Customer demographics, income & age groups, satisfaction score, job distribution, and geographic spread — for targeted marketing and retention.*

**Visuals on this page (19 visuals):**
- 4 KPI cards (Total Customers, Avg Income, Avg Satisfaction Score, Avg Age)
- Slicers — Gender, Income Group, Age Group
- Bar charts — Customers by Gender, Education, Marital Status, Job, State
- Treemaps — Customer distribution by Job and Education
- Customer detail table with satisfaction scores

> 🔗 **Live Dashboard:** *(Publish to Power BI Service and link here)*

---

## 🛠️ Tech Stack

| Tool | Purpose | Version |
|------|---------|---------|
| **Power BI Desktop** | Dashboard design, DAX modeling, interactive visualization | Feb 2026 Release |
| **PostgreSQL** | Relational data warehouse, schema design, batch ingestion | 14+ |
| **DAX** | Calculated measures, dynamic KPIs, time intelligence | Native |
| **Power Query (M)** | Data profiling, type detection, incremental load | Native |
| **CSV / Excel** | Raw data source & weekly incremental feeds | — |
| **Git & GitHub** | Version control, portfolio hosting | — |

---

## 🗄️ Data Architecture

The pipeline follows a classic **ETL → Semantic Model → Dashboard** flow, optimized for analytical query patterns rather than transactional writes.

```
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│  Raw CSV     │───▶│  PostgreSQL      │───▶│  Power BI        │───▶│  Interactive     │
│  (4 files)   │    │  Schema (ccdb)   │    │  Semantic Model  │    │  Dashboard       │
└──────────────┘    └──────────────────┘    └──────────────────┘    └──────────────────┘
   10,293 rows         2 tables              Star schema             2 pages / 39 visuals
   + Week-53           + COPY import         + DAX measures          + slicers / drill
```

### Data Sources

| Source File | Description | Rows | Role |
|-------------|-------------|------|------|
| `credit_card.csv` | Primary 52-week transaction feed | 10,108 | Transactional fact (base) |
| `cc_add.csv` | Week-53 incremental batch (tab-separated) | 185 | Incremental transactional fact |
| `customer.csv` | Customer demographic master | 10,108 | Customer dimension (base) |
| `cust_add.csv` | Week-53 customer incremental batch | 185 | Incremental customer dimension |

### Data Model

A **single-direction star schema** with two related tables connected through the shared `Client_Num` surrogate key. The relationship is filtered *from* customer *to* transaction (one customer → many transactions), which is the optimal pattern for this analytical workload — slicers on customer attributes correctly filter transactional measures, while the reverse is blocked to prevent unintended cross-filtering.

```
                    ┌─────────────────────────────┐
                    │       cust_detail (Dim)     │
                    │  ─────────────────────────  │
                    │  Client_Num (PK)            │
                    │  Customer_Age               │
                    │  Gender  ·  Education       │
                    │  Income ·  IncomeGroup      │
                    │  AgeGroup ·  Customer_Job   │
                    │  State_cd ·  Marital_Status │
                    └──────────────┬──────────────┘
                                   │  1
                                   │
                                   │  ∞
                    ┌──────────────▼──────────────┐
                    │      cc_detail (Fact)       │
                    │  ─────────────────────────  │
                    │  Client_Num (FK)            │
                    │  Card_Category              │
                    │  Total_Trans_Amt            │
                    │  Total_Trans_Ct             │
                    │  Interest_Earned            │
                    │  Revenue (calculated)       │
                    │  Exp_Type ·  Use_Chip       │
                    │  Qtr ·  Week_Start_Date     │
                    │  Delinquent_Acc             │
                    └─────────────────────────────┘
```

### Key Tables

| Table Name | Rows | Key Columns | Purpose |
|------------|------|-------------|---------|
| `cc_detail` | 10,293 | `Client_Num`, `Card_Category`, `Total_Trans_Amt`, `Interest_Earned`, `Qtr`, `Exp_Type`, `Use_Chip`, `Delinquent_Acc` | Transactional fact table — every row is one customer's weekly activity |
| `cust_detail` | 10,293 | `Client_Num`, `Customer_Age`, `Gender`, `Income`, `Education_Level`, `Customer_Job`, `State_cd`, `Cust_Satisfaction_Score` | Customer dimension — one row per unique customer with demographics |

---

## 📊 Key Analyses Performed

### Analysis 1: Revenue Performance by Quarter

**Business Question:** *Is the credit card portfolio growing, stagnating, or declining across the year?*
**Method:** Aggregated `Total_Trans_Amt + Interest_Earned` by `Qtr` and rendered as a stacked-column-and-line combo chart with transaction count on the secondary axis.
**Finding:** Revenue shows a steady upward trend — Q1: $11.25M → Q2: $11.14M → Q3: $11.45M → Q4: $11.70M — a **3.97% quarter-over-quarter** improvement from Q1 to Q4, indicating healthy portfolio momentum.
**Business Impact:** Marketing and product teams can confidently increase Q1 acquisition spend knowing the back-half trend supports payback; finance can adjust revenue forecasts upward.

---

### Analysis 2: Card Category Concentration Risk

**Business Question:** *How dependent is revenue on a single card tier, and what's the upside in premium tiers?*
**Method:** Grouped revenue by `Card_Category` (Blue, Silver, Gold, Platinum) and visualized as a bar chart with percentage labels.
**Finding:** The **Blue card alone generates 70.6% of total revenue ($37.8M)**, while Platinum contributes only 1.8% ($953K). Silver sits at 8.7% and Gold at 3.9%.
**Business Impact:** There's a clear premium-tier upgrade opportunity. Targeted campaigns to migrate high-income Blue customers to Silver/Gold could lift per-account revenue significantly — and reduce concentration risk on a single product.

---

### Analysis 3: Expense Type Spending Patterns

**Business Question:** *Where do cardholders spend the most, and which categories should we incentivize?*
**Method:** Aggregated `Total_Trans_Amt` by `Exp_Type` and rendered as a treemap for proportional visibility.
**Finding:** **Bills dominate** with $11.17M (24.5%), followed by Entertainment ($7.82M), Fuel ($7.71M), Grocery ($7.03M), Food ($6.81M), and Travel ($5.00M). The top three categories account for **58.5% of all spend**.
**Business Impact:** Reward structures should be revised — the current 1× flat rewards model under-monetizes Bills. A tiered cashback on Bills, Entertainment, and Fuel would directly drive transaction volume in the highest-yield categories.

---

### Analysis 4: Transaction Channel Behavior (Chip vs Swipe vs Online)

**Business Question:** *Are we migrating customers to more secure chip-based transactions?*
**Method:** Grouped revenue and transaction count by `Use_Chip` field, visualized as a horizontal bar chart.
**Finding:** **Swipe transactions still account for 70.3% of revenue ($28.5M)**, Chip 23.9% ($14.2M), and Online only 5.8% ($2.8M).
**Business Impact:** Chip adoption is lagging industry benchmarks. A security-first messaging campaign plus a one-time cashback incentive on Chip transactions could meaningfully reduce swipe-fraud exposure. Online channel is also underpenetrated — a digital-onboarding push could unlock a younger customer segment.

---

### Analysis 5: Customer Segmentation by Income & Age

**Business Question:** *Which demographic segments drive the most value, and where should acquisition focus?*
**Method:** Created DAX-calculated `IncomeGroup` and `AgeGroup` columns in the customer table, then cross-tabbed against customer count and average satisfaction.
**Finding:** Average customer age is **46 years** with average income of **$57,087** (range: $1,250 to $515,324). Married customers (5,218) outnumber Singles (4,310), and Graduates form the largest education segment (4,208). Customer satisfaction averages **3.19 / 5** — a clear improvement opportunity.
**Business Impact:** Premium card campaigns should target the high-income, graduate, married segment (highest LTV profile). The sub-3.5 satisfaction score is a churn risk signal — customer success teams should prioritize outreach to the lowest-quartile satisfaction cohort.

---

### Analysis 6: Delinquency & Risk Concentration

**Business Question:** *What share of the portfolio is delinquent, and is the rate acceptable?*
**Method:** Counted `Delinquent_Acc = 1` records against total, computed percentage, and cross-filtered by card category and expense type through slicers.
**Finding:** **624 accounts (6.06%)** are flagged delinquent. Combined with the 42.5% of accounts that did *not* activate within 30 days, this points to a high-risk onboarding funnel.
**Business Impact:** Risk team should review the delinquent cohort for common attributes (card category, expense pattern, income band) and tighten underwriting criteria for similar profiles. The 57.5% 30-day activation rate needs an onboarding workflow overhaul.

---

## 🔍 SQL Highlights

The SQL pipeline does the heavy lifting on data ingestion and validation. Below are the most important queries — full source in [`SQL/credit_card_schema.sql`](SQL/credit_card_schema.sql).

### Query 1: Database & Schema Initialization

```sql
-- Purpose: Create the analytical database and define two normalized tables
--          with explicit types, lengths, and decimal precision for financial accuracy.

CREATE DATABASE ccdb;

-- Transactional fact table — one row per customer per week
CREATE TABLE cc_detail (
    Client_Num            INT,
    Card_Category         VARCHAR(20),
    Annual_Fees           INT,
    Activation_30_Days    INT,
    Customer_Acq_Cost     INT,
    Week_Start_Date       DATE,
    Week_Num              VARCHAR(20),
    Qtr                   VARCHAR(10),
    current_year          INT,
    Credit_Limit          DECIMAL(10,2),
    Total_Revolving_Bal   INT,
    Total_Trans_Amt       INT,
    Total_Trans_Ct        INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip              VARCHAR(10),
    Exp_Type              VARCHAR(50),
    Interest_Earned       DECIMAL(10,3),
    Delinquent_Acc        VARCHAR(5)
);

-- Customer dimension table — one row per unique customer
CREATE TABLE cust_detail (
    Client_Num              INT,
    Customer_Age            INT,
    Gender                  VARCHAR(5),
    Dependent_Count         INT,
    Education_Level         VARCHAR(50),
    Marital_Status          VARCHAR(20),
    State_cd                VARCHAR(50),
    Zipcode                 VARCHAR(20),
    Car_Owner               VARCHAR(5),
    House_Owner             VARCHAR(5),
    Personal_Loan           VARCHAR(5),
    Contact                 VARCHAR(50),
    Customer_Job            VARCHAR(50),
    Income                  INT,
    Cust_Satisfaction_Score INT
);
```
> **Why this matters:** DECIMAL(10,2) on `Credit_Limit` and DECIMAL(10,3) on `Interest_Earned` preserve financial precision that FLOAT would silently corrupt. VARCHAR lengths are sized to actual data — no over-allocation, no truncation risk.

---

### Query 2: Bulk CSV Ingestion with Header Validation

```sql
-- Purpose: Bulk-load CSV data into PostgreSQL using the COPY command.
--          HEADER flag skips the first row (column names) automatically.

COPY cc_detail
FROM 'D:\credit_card.csv'
DELIMITER ','
CSV HEADER;

COPY cust_detail
FROM 'D:\customer.csv'
DELIMITER ','
CSV HEADER;
```
> **Why this matters:** `COPY` is ~10–100× faster than row-by-row INSERT and runs in a single transaction, ensuring either complete load or clean rollback — critical for reproducible analytics pipelines.

---

### Query 3: Incremental Week-53 Load (Append Pattern)

```sql
-- Purpose: Append the Week-53 incremental batch to existing tables.
--          Note: cc_add.csv is TAB-delimited (different from base files).

COPY cc_detail
FROM 'D:\cc_add.csv'
DELIMITER E'\t'      -- tab delimiter for incremental file
CSV HEADER;

COPY cust_detail
FROM 'D:\cust_add.csv'
DELIMITER ','
CSV HEADER;
```
> **Why this matters:** Incremental loading is the backbone of any real-world warehouse. Mixing delimiters in source files is common — using `E'\t'` escape syntax handles it cleanly without external preprocessing.

---

### Query 4: Data Quality & Revenue Validation

```sql
-- Purpose: Validate load completeness and compute headline revenue figures
--          used to cross-check Power BI KPI cards.

SELECT
    COUNT(*)                                   AS total_records,
    COUNT(DISTINCT Client_Num)                 AS unique_customers,
    SUM(Total_Trans_Amt)                       AS total_transaction_amount,
    SUM(Interest_Earned)                       AS total_interest_earned,
    SUM(Total_Trans_Amt) + SUM(Interest_Earned) AS total_revenue,
    ROUND(100.0 * SUM(CASE WHEN Delinquent_Acc = '1' THEN 1 ELSE 0 END)
          / COUNT(*), 2)                       AS delinquency_rate_pct
FROM cc_detail;

-- Expected output:
-- total_records = 10,293 | unique_customers = 10,293
-- total_transaction_amount = $45,533,021
-- total_interest_earned = $7,982,479.81
-- total_revenue = $53,515,500.81
-- delinquency_rate_pct = 6.06%
```
> **Why this matters:** This single query is the **golden-source validation** — if these numbers match the Power BI KPI cards, the entire pipeline is trustworthy. Always reconcile SQL aggregates against the BI layer before publishing.

---

### Query 5: Revenue by Card Category (Drives a Dashboard Visual)

```sql
-- Purpose: Power the "Revenue by Card Category" bar chart on Page 1.

SELECT
    Card_Category,
    COUNT(*)                                   AS transaction_records,
    SUM(Total_Trans_Amt + Interest_Earned)     AS total_revenue,
    ROUND(100.0 * SUM(Total_Trans_Amt + Interest_Earned)
          / SUM(SUM(Total_Trans_Amt + Interest_Earned)) OVER (), 2) AS revenue_share_pct
FROM cc_detail
GROUP BY Card_Category
ORDER BY total_revenue DESC;

-- Result preview:
-- Blue      | 9,384 records | $37.8M | 70.6%
-- Silver    |   649 records |  $4.6M |  8.7%
-- Gold      |   193 records |  $2.1M |  3.9%
-- Platinum  |    67 records |  $0.9M |  1.8%
```
> **Why this matters:** Window functions (`OVER ()`) let you compute percentage-of-total in a single pass — no subqueries, no temp tables. This is the kind of SQL that distinguishes a junior from a senior analyst.

---

## 📐 Key DAX Measures

The Power BI model uses a combination of **calculated columns** (for grouping) and **measures** (for aggregation). Below are the most business-critical ones.

### Measure 1: Total Revenue

```dax
Total Revenue =
    SUMX(
        'cc_detail',
        'cc_detail'[Total_Trans_Amt] + 'cc_detail'[Interest_Earned]
    )
```
> **Purpose:** Computes the headline revenue metric by summing transaction amount and interest earned row-by-row. Using `SUMX` (an iterator) instead of `SUM` + `SUM` ensures the calculation respects any filter context — critical when users slice by Quarter, Card Category, or Expense Type.

---

### Measure 2: Total Transactions

```dax
Total Transactions =
    SUM('cc_detail'[Total_Trans_Ct])
```
> **Purpose:** Aggregates the per-week transaction count into a single KPI card (667,234 total). Used in the combo chart's secondary axis to show volume-vs-revenue trends.

---

### Measure 3: Delinquency Rate

```dax
Delinquency Rate % =
    DIVIDE(
        CALCULATE(
            COUNTROWS('cc_detail'),
            'cc_detail'[Delinquent_Acc] = "1"
        ),
        COUNTROWS('cc_detail'),
        0
    ) * 100
```
> **Purpose:** Returns the delinquency percentage (6.06%) and dynamically recalculates under any slicer selection. `DIVIDE` is used instead of `/` to avoid divide-by-zero errors when filters exclude all rows.

---

### Measure 4: Activation Rate (30 Days)

```dax
Activation 30D Rate % =
    DIVIDE(
        CALCULATE(
            COUNTROWS('cc_detail'),
            'cc_detail'[Activation_30_Days] = 1
        ),
        COUNTROWS('cc_detail'),
        0
    ) * 100
```
> **Purpose:** Measures the percentage of accounts activated within 30 days of issuance (57.46%). This is a critical onboarding-funnel KPI — low rates trigger workflow review.

---

### Calculated Column 1: Age Group

```dax
AgeGroup =
    SWITCH(
        TRUE(),
        'cust_detail'[Customer_Age] < 30, "21-30",
        'cust_detail'[Customer_Age] < 40, "31-40",
        'cust_detail'[Customer_Age] < 50, "41-50",
        'cust_detail'[Customer_Age] < 60, "51-60",
        "60+"
    )
```
> **Purpose:** Bins continuous age into 5 discrete bands for slicer-driven analysis. `SWITCH(TRUE(), ...)` is the idiomatic DAX pattern for ranged conditions — cleaner and faster than nested `IF`.

---

### Calculated Column 2: Income Group

```dax
IncomeGroup =
    SWITCH(
        TRUE(),
        'cust_detail'[Income] < 35000,  "Low",
        'cust_detail'[Income] < 70000,  "Middle",
        'cust_detail'[Income] < 105000, "Upper-Middle",
        "High"
    )
```
> **Purpose:** Segments customers into 4 income bands based on observed distribution. Used as a slicer on the Customer page to enable income-stratified marketing analysis.

---

### Measure 5: Average Satisfaction Score

```dax
Avg Satisfaction =
    AVERAGE('cust_detail'[Cust_Satisfaction_Score])
```
> **Purpose:** Returns the mean satisfaction score (3.19 / 5) and dynamically recalculates under any demographic filter — letting the customer-success team identify which segments are happiest and which need intervention.

---

## 📁 Repository Structure

```
credit-card-analytics/
│
├── 📊 PowerBI/
│   └── Credit Card Analsis.pbix          # Main Power BI report file
│
├── 🗄️ SQL/
│   └── credit_card_schema.sql            # Schema + COPY ingestion + validation queries
│
├── 📂 Data/
│   ├── raw/
│   │   ├── credit_card.csv               # 10,108 transactional rows (52 weeks)
│   │   ├── customer.csv                  # 10,108 customer demographic rows
│   │   ├── cc_add.csv                    # 185 Week-53 incremental transactions
│   │   └── cust_add.csv                  # 185 Week-53 incremental customers
│   └── processed/
│       └── (generated by SQL COPY — not versioned)
│
├── 🖼️ Screenshots/
│   ├── cc_transaction_dashboard.png      # Page 1 hero screenshot
│   ├── cc_customer_dashboard.png         # Page 2 hero screenshot
│   └── data_model.png                    # Star schema diagram
│
├── 📄 README.md                          # You are here
└── 📜 LICENSE                            # MIT License
```

---

## 🚀 How to Use This Project

### Prerequisites

- [ ] **Power BI Desktop** (Feb 2026 or later) — [Download free](https://powerbi.microsoft.com/desktop/)
- [ ] **PostgreSQL 14+** (optional, only if you want to rebuild the schema)
- [ ] **Git** for cloning the repository
- [ ] A code editor (VS Code recommended) for viewing SQL & DAX

### Step-by-Step Setup

**1. Clone the repository**
```bash
git clone https://github.com/[your-username]/credit-card-analytics.git
cd credit-card-analytics
```

**2. (Optional) Recreate the SQL schema**
```bash
# Connect to your PostgreSQL instance
psql -U postgres -d postgres -f SQL/credit_card_schema.sql

# Or run file-by-file in pgAdmin / DBeaver:
#   1. CREATE DATABASE ccdb;
#   2. CREATE TABLE cc_detail (...);
#   3. CREATE TABLE cust_detail (...);
#   4. COPY cc_detail   FROM 'D:\credit_card.csv'  DELIMITER ',' CSV HEADER;
#   5. COPY cust_detail FROM 'D:\customer.csv'     DELIMITER ',' CSV HEADER;
#   6. COPY cc_detail   FROM 'D:\cc_add.csv'       DELIMITER E'\t' CSV HEADER;
#   7. COPY cust_detail FROM 'D:\cust_add.csv'     DELIMITER ',' CSV HEADER;
```

**3. Open the Power BI report**
```
Open PowerBI/Credit Card Analsis.pbix
→ The report will load with embedded data automatically
→ No SQL connection required for viewing
```

**4. Refresh data (if connecting to your own PostgreSQL)**
```
Home → Transform Data → Data source settings
→ Change source to your PostgreSQL instance
→ Close & Apply
→ Home → Refresh
```

**5. Interact with the dashboard**
```
→ Use slicers on Page 1: Quarter, Card Category
→ Use slicers on Page 2: Gender, Income Group, Age Group
→ Hover any visual for tooltip details
→ Click a bar to cross-filter other visuals on the page
```

---

## 💡 Key Business Insights

> 🔍 **Finding 1:** Revenue grew **3.97%** from Q1 to Q4 ($11.25M → $11.70M) — a healthy, steady trajectory.
> → **Impact:** Leadership can confidently increase Q1 acquisition spend knowing the back-half trend supports payback.

> 🔍 **Finding 2:** The **Blue card generates 70.6% of total revenue ($37.8M)**, while Platinum contributes only 1.8%.
> → **Impact:** Massive premium-tier upgrade opportunity. Targeted migration campaigns to high-income Blue customers could meaningfully lift per-account revenue.

> 🔍 **Finding 3:** **Bills (24.5%)**, Entertainment, and Fuel together drive **58.5%** of all transaction spend.
> → **Impact:** The flat 1× rewards structure under-monetizes Bills. A category-tiered cashback model would directly reward the highest-yield behaviors.

> 🔍 **Finding 4:** **Swipe transactions still drive 70.3%** of revenue; Chip adoption is only 23.9%.
> → **Impact:** Significant fraud-risk reduction available through chip-migration campaigns. A one-time cashback incentive on Chip transactions could shift behavior quickly.

> 🔍 **Finding 5:** **6.06% delinquency rate** combined with **42.5% non-activation within 30 days** signals a leaky onboarding funnel.
> → **Impact:** Risk and customer-success teams should jointly review the delinquent cohort for common attributes and tighten underwriting for similar profiles. Activation workflow redesign is overdue.

> 🔍 **Finding 6:** Average satisfaction score is **3.19 / 5** — below the industry-healthy 3.5 threshold.
> → **Impact:** Sub-3.5 satisfaction is a churn risk signal. Prioritize outreach to the lowest-quartile cohort and review service touchpoints for the underperforming segments.

---

## 👨‍💻 About The Analyst

**[Your Name]**
Data Analyst | Financial Analytics & BI Specialist

I build end-to-end analytics solutions that turn raw operational data into executive-grade dashboards. My focus is the full stack — SQL data warehousing, Power BI semantic modeling, DAX measures, and the business storytelling that ties it all together.

- 📧 **Email:** [your.email@example.com](mailto:your.email@example.com)
- 💼 **LinkedIn:** [linkedin.com/in/your-profile](https://linkedin.com/in/your-profile)
- 🌐 **Portfolio:** [your-portfolio.com](https://your-portfolio.com)
- 🐙 **GitHub:** [github.com/your-username](https://github.com/your-username)
- 📊 **Fiverr / Upwork:** [your-freelance-link](https://fiverr.com/your-profile)

---

## 🤝 Let's Work Together

Are you a financial services business looking to:

→ **Democratize data access** across product, risk, and marketing teams?
→ **Identify revenue concentration risks** before they become problems?
→ **Segment customers** for targeted acquisition and retention campaigns?
→ **Make data-driven decisions** instead of gut-driven ones?

**I can help. Let's talk.**

[📩 Contact Me](mailto:your.email@example.com) · [📅 Book a Call](https://calendly.com/your-link)

---

## ⭐ Support This Project

If this project helped you, inspired you, or taught you something new — please consider giving it a star! ⭐

![GitHub Repo stars](https://img.shields.io/github/stars/[your-username]/credit-card-analytics?style=social)
![GitHub forks](https://img.shields.io/github/forks/[your-username]/credit-card-analytics?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/[your-username]/credit-card-analytics?style=social)

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details. You're free to use, modify, and distribute this work with attribution.

---

<div align="center">

*Built with ❤️, PostgreSQL, and Power BI by **[Your Name]** · 2025*

*If you found this useful, give it a ⭐ — it helps others discover it too.*

</div>
