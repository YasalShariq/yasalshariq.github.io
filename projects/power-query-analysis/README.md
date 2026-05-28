# 📋 Employee Timesheet Analysis — Power Query Project

### Automating Raw Timesheet Cleanup and Structuring Using Excel Power Query

---

## Overview

Raw timesheet exports from workforce management systems are designed for printing, not analysis. They arrive as tab-delimited `.txt` files loaded with noise: metadata header rows describing the export period, employee names embedded as mid-table label rows rather than a proper column, decimal-encoded time values, Excel serial number dates, and subtotal rows scattered throughout the data.

This project demonstrates how to use **Excel Power Query** to fully automate the cleaning and restructuring of this raw export — fetching the source file directly from a GitHub URL, stripping all noise, tagging each record with its correct employee name, converting numeric date and time values into human-readable formats, and outputting a clean, analysis-ready flat table — all within a single refreshable query.

---

## Workbook Structure

| Sheet / Object | Description |
|---|---|
| **Timesheet Data** | The Power Query output: a clean flat table with one row per employee per work day |
| **Query: 2015-03-14** | The Power Query that fetches, cleans, and transforms the raw `.txt` source file |

---

## Before & After

**Before** — a tab-delimited export designed for printing, not data analysis:

```
Date:    Work Date				
From:    3/1/2015				
To:      3/14/2015				
					
Work Date    Out     Reg Hrs    OT Hrs    Misc Hrs    Expenses
Contingent Worker:    John Thompson				
42065        0.75    8.50       0.00      0.00        0.00
42066        0.75    8.00       0.00      0.00        0.00
42067        0.75    8.50       0.00      0.00        0.00
...
Number of Record(s):    10    80.00    0.00    0.00    0.00
Contingent Worker:    Bob Johnson				
42065        0.75    9.50       0.00      0.00        0.00
...
```

**After** — a clean, flat table ready for pivot tables, charts, or any analysis tool:

```
Date        EmployeeName      Reg Hrs    OT Hrs    Misc Hrs    Out        Expenses
3/3/2015    John Thompson     8.50       0.00      0.00        6:00 PM    0.00
3/4/2015    John Thompson     8.00       0.00      0.00        6:00 PM    0.00
3/5/2015    John Thompson     8.50       0.00      0.00        6:00 PM    0.00
3/6/2015    John Thompson     8.50       0.00      0.00        6:00 PM    0.00
3/7/2015    John Thompson     6.50       0.00      0.00        3:30 PM    0.00
...
3/3/2015    Bob Johnson       9.50       0.00      0.00        6:00 PM    0.00
3/4/2015    Bob Johnson       8.00       0.00      0.00        6:00 PM    0.00
...
```

The result is a clean **20-row table** (10 records per employee) with no metadata, no subtotals, human-readable dates and times, and a proper `EmployeeName` column — fully pivot-ready.

---

## How to Explore the Query

1. Open `power_query_project.xlsx` in Excel
2. Go to **Data → Queries & Connections**
3. Right-click the query named **2015-03-14** → **Edit**
4. In the Power Query Editor, each step is listed under **Applied Steps** on the right panel — click any step to preview the data at that exact stage of the transformation

---

## Power Query Steps

The query named **2015-03-14** applies the following steps to transform the raw timesheet export:

| # | Step | What It Does |
|---|---|---|
| 1 | **Source** | Connects to the raw `.txt` file hosted on GitHub via its direct URL, importing it as a tab-delimited table |
| 2 | **Removed Top Rows** | Strips the 4-row metadata block at the top of the file — the `Date:`, `From:`, `To:`, and blank separator rows — which describe the export period but are not data |
| 3 | **Promoted Headers** | Promotes the first remaining row (`Work Date`, `Out`, `Reg Hrs`, `OT Hrs`, `Misc Hrs`, `Expenses`) into the table's column headers |
| 4 | **Added Employee Column** | Creates a new helper column that detects each "Contingent Worker:" row and extracts the employee name that follows the label |
| 5 | **Filled Down** | Propagates each captured employee name downward through all the rows beneath it, so every individual work record is tagged with the correct employee |
| 6 | **Filtered Rows** | Removes three categories of noise: the "Contingent Worker:" label rows (now redundant), the "Number of Record(s):" per-employee subtotal rows, and any remaining blank rows |
| 7 | **Converted Date** | Converts the `Work Date` column from Excel serial number format (e.g. `42065`) to a human-readable date (e.g. `3/3/2015`) using Excel's base date of December 30, 1899 |
| 8 | **Converted Time** | Converts the `Out` column from its decimal fraction representation (e.g. `0.75` = 18/24 hours) to a readable time value (e.g. `6:00 PM`) |
| 9 | **Changed Type** | Sets final data types across all columns: `Date` for Work Date, `Time` for Out, `Decimal Number` for Reg Hrs, OT Hrs, Misc Hrs, and Expenses |
| 10 | **Renamed Columns** | Renames `Work Date` → `Date` and the helper column → `EmployeeName` for clarity and consistency |
| 11 | **Reordered Columns** | Moves `EmployeeName` to appear directly after `Date`, placing the most identifying fields first for a logical reading order |

---

## Why This Matters

Once the data is in a flat table format, you can:

- **Filter or sort by employee** without manually parsing name rows embedded in the raw data
- **Build Pivot Tables** to summarize total, regular, and overtime hours by employee or date range in seconds
- **Chart attendance patterns** across the pay period without any manual data prep
- **Scale across multiple pay periods** — add additional `.txt` exports as new queries, then append them into a single master table spanning months or years
- **Refresh in one click** — go to **Data → Refresh All** to pull the latest version of the source file from GitHub at any time

---

## Data Source

Timesheet files are imported directly from GitHub via their raw URLs:

```
https://raw.githubusercontent.com/YasalShariq/yasalshariq.github.io/main/projects/power-query-analysis/2015-03-14.txt
```

---

## Requirements

- Microsoft Excel 2016 or later (Power Query is built in)
- Internet access (to refresh data from the GitHub raw URL)

---

## Tools Used

- Microsoft Excel
- Power Query (M language)
