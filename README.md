# 📊 Executive Retail Sales & Profitability Analysis

[![Live Interactive Dashboard](https://img.shields.io/badge/Power_BI-Interactive_Report-yellow?style=for-the-badge&logo=powerbi)](https://app.powerbi.com/view?r=eyJrIjoiOGFiY2E3MWUtY2NjNi00Y2ZkLWIxMWItYTE5MmI0NjVlZjIxIiwidCI6IjMyZWIxYjBhLWMwMmItNDBjNC04MzNiLTU4MjMxNWFiOTMzNiJ9)

## 📌 Project Overview
This project provides an interactive presentation for upper-level executives to analyze the company’s sales process, identify the most profitable regions, and assess the sales of different categories. This presentation was created in Power BI Desktop.

👉 **[Click Here to Launch Full-Screen Interactive Power BI Report](https://app.powerbi.com/view?r=eyJrIjoiOGFiY2E3MWUtY2NjNi00Y2ZkLWIxMWItYTE5MmI0NjVlZjIxIiwidCI6IjMyZWIxYjBhLWMwMmItNDBjNC04MzNiLTU4MjMxNWFiOTMzNiJ9)**

---

## 📸 Dashboard Preview

![Executive Sales Dashboard Preview](./dashboard_preview.png)

---

## 🛠️ Data Architecture & Modeling

### 1. Data Cleaning & Transformation (Power Query)
- Raw datasets (sales, customer, product, store) were transformed in Power Query Editor.
- Correct data types within date/time, string, or numeric fields were applied.
- A continuous and up-to-date M-code calendar dimension was created (`Dim_Date`) spanning 2020 through 2026.

### 2. Star Schema Data Model
Designed a high-performance Star Schema data model utilizing a $1:*$ relationship strategy connecting four dimension tables to the central fact table:
- `Transactions` (Fact Table)
- `Customers` (Dimension Table via `CustomerID`)
- `Products` (Dimension Table via `ProductID`)
- `Stores` (Dimension Table via `StoreID`)
- `Dim_Date` (Dimension Table via `Date`)

---

## 🧮 Key DAX Measures Created

To analyze the business process without affecting data storage performance, all key business metrics were calculated using explicit DAX measures stored in a dedicated `_Measures` table:

```dax
-- 1. Total Revenue
Total Revenue = 
SUMX(
    Transactions,
    Transactions[Quantity] * RELATED(Products[UnitPrice]) * (1 - Transactions[Discount])
)
```

```dax
-- 2. Total Cost
Total Cost = 
SUMX(
    Transactions,
    Transactions[Quantity] * RELATED(Products[CostPrice])
)
```

```dax
-- 3. Total Profit
Total Profit = [Total Revenue] - [Total Cost]
```

```dax
-- 4. Profit Margin %
Profit Margin % = 
DIVIDE([Total Profit], [Total Revenue], 0)
```

```dax
-- 5. Total Orders
Total Orders = DISTINCTCOUNT(Transactions[TransactionID])
```
