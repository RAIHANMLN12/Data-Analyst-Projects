# Retail Sales Performance Dashboard

## Overview

This project analyzes multi-year retail transaction data for a fictional retail chain to uncover sales and profitability trends across regions, product categories, and customers. It is built entirely in Microsoft Excel using pivot tables, charting, and dashboard design, and is designed to mirror the kind of monthly sales review an in-house retail analyst would produce.

## Business Context

Retail leadership needs a recurring view of where the business is winning and where it is losing money, broken down by region and product line, so that decisions on inventory, promotions, and account management can be made quickly. This project simulates that request: turning raw order-level transactions into a single-page dashboard that answers the questions leadership actually asks.

## Objectives

The analysis was structured to answer four core business questions:

1. What are total sales and profit by region and by product category?
2. Which product sub-categories are consistently unprofitable?
3. How do monthly sales trend over the full period covered by the data?
4. Who are the top 10 customers by total sales?

## Dataset

- **Source:** Superstore Dataset, Kaggle ([link](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final))
- **Grain:** One row per order line item
- **Fields used:** Order Date, Region, Category, Sub-Category, Customer Name, Sales, Profit

## Tools Used

- Microsoft Excel
- Pivot Tables (Rows, Columns, Values, Filters)
- Excel Charting (bar, line)
- Date grouping (Month, Year)

## Methodology

1. **Data preparation:** Verified the Order Date field was recognized as a true date type (not text) so it could be grouped by month and year in a pivot table.
2. **Region x Category breakdown:** Built a pivot table with Region in Rows and Category in Columns to compare total Sales and Profit across the two dimensions at once.
3. **Sub-category profitability:** Built a pivot table ranking all sub-categories by total Profit to identify which ones are structurally unprofitable.
4. **Monthly sales trend:** Grouped Order Date by Month and Year in a pivot table, then visualized the result as a line chart to show the sales trend across the full period covered by the data.
5. **Top customers:** Built a pivot table ranking customers by total Sales and filtered to the top 10.
6. **Dashboard assembly:** Consolidated all four analyses and their corresponding charts onto a single dashboard sheet for at-a-glance review.

## Key Insights

*Fill in with the specific findings from your pivot tables once your analysis is finalized, for example:*

- Highest and lowest performing regions by profit
- Sub-categories that lose money consistently, and by how much
- Months or seasons with the strongest and weakest sales
- Concentration of revenue among the top 10 customers

## Repository Structure

```
Retail-Sales-Performance-Dashboard/
│
├── data/
│   └── Superstore.csv
│
├── analysis.xlsx
│   ├── data              (raw transaction data)
│   ├── pivot table        (all four pivot analyses)
│   └── dashboard          (consolidated summary view)
│
└── README.MD
```

## Skills Demonstrated

- Pivot table design (Rows, Columns, Values, Filters)
- Date-based grouping and time series aggregation in Excel
- Chart selection matched to the underlying business question
- Dashboard layout and data storytelling for a non-technical audience

## Author

Raihan Maulana — Data Analyst Portfolio Project