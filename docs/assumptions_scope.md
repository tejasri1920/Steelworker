# Project Requirements: Quality Defect Dashboard

## Context
As a quality engineer, I need to distinguish recurring defect issues from one-off incidents by viewing summary-level information across multiple production lines and lots.

## Assumptions
* **Assumption 1: Common Keys.** All Excel source files contain a common key (e.g., `Lot_ID`) allowing for relational mapping, even if column names differ.
* **Assumption 2: Source of Truth.** Excel files represent the "Source of Truth" (simulating exports from an older ERP) rather than a live database connection.

## Scope

### In Scope
* **Recurrence Detection:** Identifying if the same defect type appears across multiple lots over time.
* **Data Aggregation:** Consolidating inspection data from daily and weekly logs.
* **Defect Filtering:** Automatically excluding non-defect records (Qty = 0) from counts.
* **Data Sufficiency:** Explicitly indicating when available data is insufficient to determine a trend.
* **Summary Reporting:** Providing high-level views for management reports/meetings.

### Out of Scope
* Root cause analysis (RCA) or automated troubleshooting.
* Predictive or AI-based quality forecasting.
* Real-time monitoring (Dashboard is based on periodic file updates).
* Source data validation (The tool will not prevent "bad entries" in the original Excel files).
* Complex RBAC (The tool is for small team/department-wide trust-based use).

## Usage & Constraints
* **Users:** Operators, Engineers, Managers.
* **Context:** Factory floor kiosks, office desktops, and mobile viewing.
* **Volume:** Hundreds to thousands of rows across multiple files.
