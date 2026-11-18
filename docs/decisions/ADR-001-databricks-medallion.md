# ADR-001: Use Databricks and the medallion architecture

**Status:** Accepted
**Date:** 2026-11-18

## Context

CryptoTrace needs a pipeline that loads the Elliptic dataset, cleans it, and produces results for a dashboard. The largest file is about 658 MB. The work must be repeatable and visible on GitHub, and each step must be easy to check.

## Decision

- Use Databricks Free Edition as the platform.
- Use the medallion architecture:
  - **Bronze:** raw copies of the files, with only tracking columns added.
  - **Silver:** cleaned, typed and checked data.
  - **Gold:** final tables for analysis and the Power BI dashboard.
- Store every layer as Delta tables in the `bronze`, `silver` and `gold` schemas.
- Write the pipeline in Python (PySpark) and SQL, and link the Databricks workspace to GitHub with a Git folder.

## Alternatives considered

- **Local DuckDB with dbt.** This is free and runs on a laptop. I did not choose it because I wanted a cloud platform that matches how data engineering teams work, so the process is easy to repeat and show.
- **Pandas scripts only.** This is simple, but it does not handle a file this size well and does not show clear layers. I did not choose it.

## Consequences

- The pipeline depends on Databricks Free Edition. Compute can stop and sessions can disconnect. To reduce the risk, each step saves a table and I commit to GitHub often.
- Data files must never go into GitHub. The `.gitignore` file blocks CSV files.
- The ethics reviewer should be told about this platform choice.
- Every step must be recorded in the project journal.
