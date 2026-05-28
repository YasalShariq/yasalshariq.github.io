# Sales Performance Dashboard

A single-page Power BI report that tracks year-to-date sales against the prior year across products, accounts, and geographies — built on a clean star schema with a dedicated measures table.

## What It Shows

- **KPI Cards** — S_YTD, S_PYTD, YTD vs PYTD variance, and Gross Profit % at a glance
- **Bottom 10 Treemap** — spots underperforming countries by YTD vs PYTD delta
- **Waterfall Chart** — breaks down the YTD vs PYTD swing by country → product type → product name
- **Combo Chart** — overlays current vs prior-year sales trend, segmented by product type
- **Scatter Plot** — maps GP% against S_YTD per account to surface high-volume / low-margin outliers
- **Dynamic Slicer** — lets viewers toggle between sales metrics without duplicating pages

## Data Model

Star schema with four tables and a dedicated measures layer:

| Table | Role |
|---|---|
| `Fact_Sales` | Transactional sales data |
| `Dim_Date` | Calendar dimension for time intelligence |
| `Dim_Product` | Product hierarchy (type → name) |
| `Dim_Accounts` | Account/customer dimension |
| `_Measures` | Isolated DAX measure table |
| `Slc_Values` | Field parameter for slicer switching |

## Key DAX Measures

`S_YTD`, `S_PYTD` — year-to-date and prior-year-to-date sales using `TOTALYTD` / `SAMEPERIODLASTYEAR`  
`YTD vs PYTD` — absolute variance driving the waterfall and treemap  
`GP%` — gross profit margin feeding the scatter analysis

## Tools

Power BI Desktop · DAX · Star Schema Modeling

## How to View

Download `analysis.pbix` and open it in [Power BI Desktop](https://www.microsoft.com/en-us/download/details.aspx?id=58494) (free).
