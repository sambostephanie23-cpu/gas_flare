# 🔥 Global Gas Flaring Analysis (with Nigerian Deep-Dive)

## Overview
SQL analysis of global gas flaring activity across oil fields worldwide, with a focused sub-analysis on Nigeria — examining flaring volumes, carbon emission estimates, regulatory fine exposure, operator rankings, and year-over-year trend changes.

## Tools & Platform
- **SQL** (BigQuery)
- **Datasets:** Global Gas Flare dataset + Nigerian Gas Flare subset
- **Rows:** ~156,397 (global) | ~2,264 (Nigeria-specific) | 19 columns

## Key Questions Answered
- Which operators and locations contribute the most flaring volume globally?
- How has flaring trended year-over-year?
- For Nigeria specifically: what are the carbon emission and regulatory fine implications?
- Which Nigerian operators rank worst by flaring volume and YoY change?

## SQL Techniques Used
| Technique | Purpose |
|---|---|
| `RANK() OVER()` | Global operator and location ranking by flaring volume |
| `LAG() OVER(ORDER BY Year)` | Year-over-year comparison |
| `NULLIF()` | Safe division to avoid divide-by-zero errors |
| `ROUND()` | Clean numeric output |
| CTEs (`operator_level`, `location_level`, `annual_metrics`, `TIME_TREND_ANALYSIS`) | Modular multi-step analysis |
| `WHERE country = 'Nigeria'` | Country-level filter for focused sub-analysis |
| `LEFT JOIN` | Enriching base data with operator ranks, location ranks, and trend data |

## CTE Pipeline
```
operator_level → location_level → annual_metrics → TIME_TREND_ANALYSIS → final_select (Nigeria filter)
```

## Key Findings
- Nigerian flaring data includes estimated **carbon emissions (tons)** and **regulatory fine exposure (USD)** per operator
- YoY percentage change reveals which operators are worsening vs. improving
- Global operator rankings expose the top contributors to flaring volume across onshore and offshore fields

## Files
| File | Description |
|---|---|
| `global_gas_flare.csv` | Full global flaring dataset with operator & location ranks, YoY trends |
| `nigerian_gas_flare.csv` | Nigeria-filtered data with carbon emissions and regulatory fine estimates |
| `screenshots/` | BigQuery SQL query screenshots |

## Data Source
Global Gas Flare Monitor dataset — accessed via BigQuery

---
*Project by Stephanie Sambo | Data Analyst | Oil & Gas | SQL • BigQuery*
