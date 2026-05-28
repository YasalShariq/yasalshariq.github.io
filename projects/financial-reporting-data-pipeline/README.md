# 📊 Financial Statement → Data Table
### Transforming Cross-Tab Reports into Analyzable Data Using Excel Power Query

---

## Overview

Financial statements are designed for human readability — but that cross-tab layout with merged headers and grouped rows makes them nearly impossible to work with in pivot tables, charts, or any kind of dynamic analysis.

This project demonstrates how to use **Excel Power Query** to unpivot a multi-month financial statement (tracking Revenues and Expenses across Actual, Budget, and Variance figures for April and May) into a clean, flat data table — turning a report *format* into a data *structure*.

---

## Workbook Structure

| Sheet | Description |
|---|---|
| **Before** | The original financial statement with cross-tabulated months and merged column headers |
| **After** | The Power Query output: a normalized table with columns for `Class`, `Account`, `Month`, `Measure`, and `Amount` |
| **Recreated using Pivot Tables** | Proof of concept showing how the clean data table can instantly recreate the original report layout — and any other slice you need |

---

## Before & After

**Before** — a cross-tab report designed for reading, not analysis:

```
                  April                       May
            Actual   Budget   Variance   Actual   Budget   Variance
Revenues
  Alcoholic Beverages  15,747   25,012    -9,264   22,133   41,881   -19,747
  Food & Non-Alc Bev   34,523   44,740   -10,216   51,007   71,125   -20,117
  ...
```

**After** — a flat, normalized table ready for any analysis tool:

```
Class       Account                  Month   Measure   Amount
Revenues    Alcoholic Beverages      April   Actual    15747.28
Revenues    Alcoholic Beverages      April   Budget    25012.00
Revenues    Alcoholic Beverages      April   Variance  -9264.72
Revenues    Alcoholic Beverages      May     Actual    22133.53
...
```

The result is a tidy **36-row table** that's fully pivot-ready.

---

## How to Explore the Query

1. Open the workbook in Excel
2. Go to **Data → Queries & Connections**
3. Click **Edit** on the `Data Cleaning` query to open the Power Query Editor

---

## Power Query Steps

The query named **Data Cleaning** applies 14 steps to transform the source data:

| # | Step | What It Does |
|---|---|---|
| 1 | **Source** | Connects to the *Before* worksheet as the raw input table |
| 2 | **Filled Down** | Fills blank cells downward to propagate class labels (`Revenues` / `Expenses`) across merged rows |
| 3 | **Transposed Table** | Flips rows and columns to make the multi-level month headers easier to manipulate |
| 4 | **Filled Down1** | Repeats the fill-down on the transposed layout to propagate month names (`April`, `May`) across their Actual / Budget / Variance columns |
| 5 | **Merged Columns** | Combines the month and measure header rows into a single label (e.g. `April_Actual`) |
| 6 | **Transposed Table1** | Transposes back to restore the original row/column orientation |
| 7 | **Promoted Headers** | Promotes the first row (now containing proper column names) into the table header |
| 8 | **Changed Type** | Sets initial data types for the columns |
| 9 | **Unpivoted Other Columns** | ⭐ The core transformation — pivots all month/measure columns into rows, creating Attribute and Value pairs |
| 10 | **Split Column by Delimiter** | Splits combined labels (e.g. `April_Actual`) back into separate `Month` and `Measure` values |
| 11 | **Changed Type1** | Updates data types after the split |
| 12 | **Replaced Value** | Cleans up formatting artefacts or label inconsistencies |
| 13 | **Renamed Columns** | Assigns final clean column names: `Class`, `Account`, `Month`, `Measure`, `Amount` |
| 14 | **Filtered Rows** | Removes subtotal/total rows (e.g. *Total Revenues*, *Total Expenses*) that belong in the report view but not in the flat data table |

---

## Why This Matters

Once the data is in a flat table format, you can:

- **Slice any way you want** with a Pivot Table — by month, measure, account, or class
- **Chart trends** across months without manual data wrangling
- **Feed the data into Power BI**, Tableau, or any other BI tool
- **Add new months** to the source and refresh the query in one click

The *Recreated using Pivot Tables* sheet demonstrates this by rebuilding the original report layout from the flat table in seconds.

---

## Tools Used

- Microsoft Excel
- Power Query (M language)
