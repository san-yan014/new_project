# Online Retail Analytics

An end-to-end analytics project on a UK online retailer's transactions: raw data goes through a tested dbt pipeline into a star schema, then SQL analysis and a dashboard answer one business question.

> **Business question:** Which customers keep coming back, and what can the business do about it?

**Status:** in progress. Setup is done and raw data loading is underway. See the [roadmap](#roadmap).

## Stack

| Layer | Tool |
|---|---|
| Warehouse | [DuckDB](https://duckdb.org/) (local, no cloud account needed) |
| Transformation | [dbt](https://www.getdbt.com/) with `dbt-duckdb` |
| Analysis | SQL |
| Dashboard | Tableau Public or Power BI (not decided yet) |

## Data

- **[UCI Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii):** about 1M transactions from a UK gift and homeware store, December 2009 to December 2011. One row is one product line on one invoice. Many customers are wholesalers.
- **A/B test dataset (to be chosen, e.g. Cookie Cats):** Online Retail II has no experiment, so a second small dataset covers the significance-testing piece.

Raw data is not committed. Download it into `data/` (see [Reproduce](#reproduce)).

## Pipeline

```
Raw file  ->  DuckDB (raw)  ->  dbt staging  ->  dbt marts (star schema)  ->  SQL analysis  ->  Dashboard
```

1. **Load:** the raw Online Retail II file is loaded into DuckDB unchanged.
2. **Staging (dbt):** cast types, standardize column names, and handle known problems such as missing customer IDs, cancellations (invoices starting with `C`), duplicates and zero or odd prices.
3. **Marts (dbt):** a star schema with a sales fact table and customer, product and date dimensions.
4. **Tests (dbt):** automated data tests such as uniqueness, not-null keys and accepted values.
5. **Analysis (SQL):**
   - *Cohort retention:* of customers who first bought in a given month, how many came back later?
   - *RFM segmentation:* group customers by recency, frequency and monetary value.
   - *A/B test:* analyze an experiment with a significance test.
6. **Dashboard:** a public dashboard summarizing retention, segments and recommendations.

## Repo layout

```
data/          raw files and the DuckDB database (not committed)
dbt_project/   dbt models: staging, marts, tests
analysis/      cohort, retention, RFM and A/B analysis
```

## Roadmap

- [x] Choose dataset, warehouse and tools
- [x] Project setup (virtual environment, dbt-duckdb, folders)
- [ ] Load raw data into DuckDB
- [ ] dbt staging and mart models with tests
- [ ] SQL analysis: cohorts, retention, RFM, A/B test
- [ ] Dashboard
- [ ] Findings and recommendations written up here

## Findings

_To be added once the analysis is complete._

## Reproduce

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install dbt-duckdb
```

Then download `online_retail_II.zip` from the UCI link above into `data/`. Load and build steps will be added as they are implemented.
