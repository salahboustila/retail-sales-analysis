# Retail Sales Analysis — Excel / Google Sheets

End-to-end analysis of a messy retail sales dataset (915 transactions, Jan 2024 – Dec 2025): data cleaning, pivot tables, and business insights answering 7 questions a manager would actually ask.

**Final basis after cleaning: 872 valid orders, $842,566 revenue.**

## Business Questions & Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Total revenue and regional split | $842,566 — West 32.6%, North 27.5%, East 25.6%, South 12.7%, Unassigned 1.6% |
| 2 | Best / worst product category | Electronics $522,934 (62.1%) / Office Supplies $20,972 (2.5%) |
| 3 | Top 10 products by revenue | Wireless Mouse leads at $129,424 (15.4%); revenue falls ~90% after rank 9 |
| 4 | Top sales representative | **Sara Khan, $204,607** — invisible in raw data (split across "Sara Khan" / "S. Khan") |
| 5 | Highest sales month & trend | November 2024, $113,278 (52 orders); strong 2024 growth, H2-2025 "decline" is incomplete data, not a real drop |
| 6 | Average order value & top payment method | AOV $966; Cash, 315 orders (36.4%) — only clear after fixing inconsistent casing |
| 7 | Region with highest AOV | West, $1,102/order — 26% above East, which earns through volume, not ticket size |

## The Real Work: Data Cleaning

The raw data contained deliberate traps. Each was flagged with an `INCLUDE` column (flag, don't delete) so every exclusion stays auditable:

- **Duplicates** — 12 extra rows across 11 OrderIDs double-counting ~$1,500. Removed via a running-count formula that keeps only the first occurrence: `COUNTIF($A$2:A2,A2)>1`
- **Outliers & errors** — quantity 9999 entries, zero/negative totals, missing unit prices (negatives assumed data errors, not refunds — documented assumption)
- **Split identities** — "Sara Khan" vs "S. Khan": consolidating them moved her from 4th place to #1. Assumption flagged for confirmation with the sales manager
- **Inconsistent casing** — `north`/`EAST`, `CASH`/`Cash`/`credit card`: standardized with `PROPER(TRIM())`. Raw casing made the payment ranking look like a three-way near-tie; the cleaned data shows Cash leads by 50+ orders
- **Mixed date formats** — DD/MM and MM/DD strings parsed per-row; ambiguous dates assumed DD/MM (documented)
- **Missing values** — labelled `Unassigned` / `Unknown` instead of dropped, so data gaps stay visible in every pivot

## Method Notes

- Narrative insights are built with `TEXT()` formulas referencing pivot cells, so they update automatically when the data changes — hardcoded numbers drift
- Q4 includes a deliberate "naive vs corrected" comparison showing how uncleaned data credits the wrong top performer
- Q5's peak-month answer is paired with a data-completeness check (orders per month) before trusting the trend

## Tools

Google Sheets / Excel: pivot tables, `COUNTIF`/`COUNTIFS`, `PROPER`/`TRIM`, `TEXT`, date parsing, filtered aggregation.

## What I'd Do Next

Rebuild the same analysis in SQL and Python (pandas) to compare workflows — case-sensitive grouping there makes the cleaning steps mandatory rather than optional.

---

*Salah Eddine Boustila — [github.com/salahboustila](https://github.com/salahboustila)*
