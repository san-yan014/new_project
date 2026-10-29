# Project Notes: Retail Analytics Pipeline

Working notes for revisiting the project. Update the progress checklist as we go.

## Goal

Build a portfolio project for **data analyst roles**. It should show SQL, dbt, a BI tool and statistics, which are the gaps on the current resume. The business question: **which customers keep coming back, and what can the business do about it?**

## Tools and decisions

| Choice | Why |
|---|---|
| Dataset: UCI Online Retail II | Repeat customers, so cohort retention is meaningful (Olist was rejected: almost no repeat buyers) |
| Warehouse: DuckDB | Free, local, no cloud account, easy for anyone to reproduce |
| Transformations: dbt (`dbt-duckdb`) | Organizes SQL into staging and mart layers with automated tests |
| Dashboard: Tableau Public or Power BI | Not decided yet |
| A/B test data | A second small dataset (e.g. Cookie Cats), because Online Retail II has no experiment |

Environment note: system Python is 3.9.6, so dbt-core installed as 1.10.23 (not the newest). It works; it prints a deprecation warning.

## Folder layout

```
new_project/
  .venv/          Python environment (not committed)
  data/           raw files and the DuckDB database (not committed)
  dbt_project/    dbt models (SQL transformations)
  analysis/       cohort, retention, RFM and A/B analysis
  PROJECT_NOTES.md
  resume_new.pdf
```

## The dataset: Online Retail II

*(From the dataset's public documentation. Not yet verified against the downloaded file.)*

- Real transactions from a UK online store selling **gift items and homeware**.
- Covers **December 2009 to December 2011**, about 1M rows.
- Many customers are **wholesalers** buying in bulk.
- **One row = one product line on one invoice.** An order with three products is three rows with the same invoice number.

| Column | Meaning |
|---|---|
| Invoice | Invoice number. A leading "C" means a cancellation or return |
| StockCode | Product ID |
| Description | Product name |
| Quantity | Units bought (negative for returns) |
| InvoiceDate | Date and time of purchase |
| Price | Unit price in GBP |
| Customer ID | Customer identifier |
| Country | Customer's country (mostly UK) |

**Good for:** cohort retention, RFM segmentation, revenue trends, a star schema.
**Not available:** experiment data (hence the second dataset) and customer demographics (we describe customers by behavior only).
**Expected data problems to check:** missing Customer IDs, returns/cancellations, duplicates, zero or odd prices.

## Pipeline plan

1. **Choose tools and data.**
2. **Set up the project:** folders, virtual environment, dbt-duckdb.
3. **Load raw data:** download, unzip, first look (columns, row counts, missing values, oddities), load into DuckDB.
4. **Build dbt models:**
   - *Staging:* clean types, remove obvious junk.
   - *Marts:* star schema with a sales fact table plus customer, product and date dimensions.
   - *Tests:* automated checks (e.g. no null customer IDs in the customer dimension).
5. **Answer business questions in SQL:**
   - *Cohort retention:* of customers who first bought in a month, how many returned later?
   - *RFM segmentation:* group customers by recency, frequency and monetary value.
   - *A/B test:* analyze an experiment with a significance test.
6. **Present it:** dashboard with a public link, a README (question, method, findings, recommendation), and a resume update.

**Later:** finish the Medicare Spending Variation project the same way (SQL analysis plus dashboard).

## Progress

- [x] 1. Choose tools and data
- [x] 2. Project setup (venv, dbt-duckdb, folders)
- [ ] 3. Load raw data
  - [x] Downloaded `data/online_retail_II.zip` (about 45.6 MB, from UCI). Not yet opened or checked
  - [ ] List zip contents (confirm file is intact and see the format)
  - [ ] Unzip
  - [ ] First look at the data
  - [ ] Load into DuckDB
- [ ] 4. dbt models
- [ ] 5. SQL analysis
- [ ] 6. Dashboard, README, resume

## Working agreement

One step at a time. Explain each action before running it, run only that action, then wait for a go-ahead.
