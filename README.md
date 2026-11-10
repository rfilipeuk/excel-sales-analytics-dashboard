[README.md](https://github.com/user-attachments/files/34637213/README.md)
# Sales & Business Analytics Dashboard (Excel)

An interactive, fully dynamic sales dashboard built by hand in Microsoft Excel: Power Query, PivotTables, slicers, conditional formatting and a small formula "engine" that drives the year-over-year indicators.

![Dashboard preview](images/dashboard.png)

> **Why Excel?** AI tools can now generate a dashboard in minutes. I built this one manually, on purpose, to rebuild muscle memory with the core tools every analyst should know cold, and to understand exactly how every number on the screen is produced.

---

## Business questions

1. How are revenue, expenses, profit and margin performing, and how do they compare with the previous year?
2. Which product categories, regions and customer segments drive revenue and profit?
3. Is there seasonality in sales?
4. Where is profitability being lost?

## Key findings (2023–2025, 2,000 orders)

| Metric | Value |
|---|---|
| Total revenue | **$484,559** |
| Total expenses | $325,687 |
| Net profit | **$158,872** |
| Profit margin | **32.8%** |

- **2025 vs 2024:** revenue grew **+6.3%**, but expenses grew faster (**+7.1%**), so margin slipped **0.5 percentage points** (33.3% → 32.8%). Over the full period revenue is roughly flat (2025 is +0.3% vs 2023).
- **Furniture is the main revenue driver** (≈53% of sales, ≈51% of profit); Technology is second (≈29% of sales).
- **Office Supplies is the weak spot:** only 4% of revenue and 2% of profit, with a 17% margin (vs 32–38% elsewhere). It accounts for **224 of the 272 loss-making orders (82%)**.
- **Clothing & Accessories has the highest margin (37.6%)** but only 14% of revenue, so it is a potential growth lever.
- **Q4 is the strongest quarter (28% of annual sales)**, with October the peak month ($51.1K).
- **Europe and North America** lead on revenue ($137.0K and $133.9K) with ~33–34% margins. **Corporate** is the lowest-margin segment (30.4%).

## Features

- **4 KPI cards** (Revenue, Expenses, Net Profit, Margin) with automatic YoY pills: green ▲ / red ▼, with colour logic reversed for expenses (a rise in cost is bad).
- **Monthly Sales Trend**: sales columns, profit line and 3-month moving average.
- **Sales vs Profit by Category** and **Sales by Customer Segment** (donut with a dynamic total in the centre).
- **Recent Transactions** table that always shows the latest 7 orders for the current filters.
- **Slicers** for Year, Region and Customer Segment that update every element at once.
- Custom dark theme, consistent colour palette and typography.

## How it works

```
global_ecommerce_sales (Power Query table)
        │
        ▼
   PivotCache ──► 7 PivotTables (Calc_Engine sheet) ──► Slicers (Year, Region, Segment)
                         │
                         ├─ KPI totals            → KPI cards
                         ├─ Month / Category / Segment pivots → PivotCharts
                         ├─ PT_ByYear + PT_SelYears → YoY "engine"
                         └─ PT_Recent (sorted newest first) → Recent Transactions
```

**YoY logic.** Slicers only filter PivotTables, so the YoY needs a trick:
- `PT_SelYears` is connected to the Year slicer and tells the model which years are selected (`K20 = MAX(selected years)`).
- `PT_ByYear` is connected to Region and Segment but **not** to Year, so it always holds every year.
- `INDEX/MATCH` reads the latest selected year and the year before it from `PT_ByYear`; YoY = current ÷ previous − 1. If there is no previous year in the data, the pill shows **N/A**.
- Margin YoY is shown in **percentage points**, not percent.

**Number formats and colours.** The ▲/▼ symbols come from custom number formats (`"▲ "+0.0%" YoY";"▼ "-0.0%" YoY";"● "0.0%" YoY"`). The green/red colouring comes from conditional formatting rules, so no formatting is manual.

## Data

`global_ecommerce_sales`: 2,000 orders, 20 columns, 2 Jan 2023 – 31 Dec 2025.

| Field group | Columns |
|---|---|
| Order | Order_ID, Order_Date, Year, Month_Name, Month_Num |
| Customer | Customer_Name, Customer_Segment, Country, Region |
| Product | Product_Category, Product_Name, Quantity, Unit_Price, Discount_Percent |
| Financials | Total_Sales, Shipping_Cost, Expense, Profit, Profit_Margin_% |

**Quality checks performed:** no missing values, no duplicate Order_IDs, `Profit = Total_Sales − Expense` for every row, `Total_Sales = Quantity × Unit_Price × (1 − Discount)` for every row, and date parts consistent with Order_Date.

## Tools & skills

Microsoft Excel (Mac) · Power Query · PivotTables & PivotCharts · Slicers and report connections · INDEX/MATCH · custom number formats · conditional formatting · dashboard layout & colour design.

## Repository structure

```
├── Sales_Analytics_Dashboard.xlsx   # the workbook
├── images/
│   └── dashboard.png                # screenshot
└── README.md
```

## How to use

1. Download `Sales_Analytics_Dashboard.xlsx` and open it in **Microsoft Excel (desktop)**. Slicers and PivotCharts are not supported in most other spreadsheet apps.
2. Go to the **Dashboard** sheet and click the slicers (Year, Region, Customer Segment). Use the funnel icon on a slicer to clear its filter.
3. Read **Key_Takeaways** for the findings and recommendations. The **Calc_Engine** sheet holds the PivotTables and the YoY logic.
4. If the data changes, use **Data → Refresh All**.

The design uses the Aptos Display font (Microsoft 365). Excel falls back to a default font if it is not installed.

## Possible next steps

- Add a forecast for the next quarter.
- Rebuild the same dashboard in Power BI for comparison.
- Add discount-level analysis (discounts vs margin).

## Author

**Filipe Rodrigues**: data analytics, computational mathematics. See my other projects in this GitHub profile.
