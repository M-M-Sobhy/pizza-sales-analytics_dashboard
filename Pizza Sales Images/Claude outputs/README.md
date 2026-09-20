<div align="center">

# 🍕 Pizza Sales Analytics Dashboard

### 📊 SQL-Powered Business Insights, visualized in Power BI

![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-be185d?style=for-the-badge)

</div>

---

## 📌 About This Project

A full year (2015) of pizza order-line data, turned into a decision-ready sales report. Raw transactions are answered as **12 business questions** through **16 SQL Server views**, then visualized in a **2-page interactive Power BI dashboard** covering revenue, order trends, category/size mix, and best vs. worst sellers.

| 🧩 | What it covers |
|---|---|
| 💰 | Revenue, average order value, and pizzas sold — the core KPIs |
| 📅 | Daily & monthly ordering trends across the year |
| 🍕 | Sales split by pizza category and size |
| 🏆 | Top & bottom 5 pizzas by revenue, quantity, and order count |

---

## 🖥️ Dashboard Pages

### 1️⃣ Home
Five headline KPI cards (Avg Order Value, Total Orders, Total Pizza Sold, Total Revenue, Avg Pizzas per Order), daily & monthly order trends, and the sales mix by category and size — filterable by pizza category and date range.

![Home](assets/home.png)

### 2️⃣ Best / Worst Seller
Top 5 and Bottom 5 pizzas side by side across three lenses — Total Orders, Quantity, and Revenue — plus a plain-language callout of which pizza and which size drive (or drag down) each metric.

![Best / Worst Seller](assets/best-worst-seller.png)

---

## 🗃️ Data Source & Modeling Approach

Unlike a dimensional (star/snowflake) model, this project keeps a **single flat order-line table** — `dbo.pizza_sales` — and builds a **SQL view layer** on top of it as a semantic layer. Power BI (and Excel) connect straight to these views instead of raw aggregation logic living in DAX or pivot formulas.

**`dbo.pizza_sales`** (48,620 rows, one row per pizza line within an order):

| Column | Description |
|---|---|
| `order_id` | Order identifier (one order can have several pizza lines) |
| `pizza_id` / `pizza_name_id` | Line and SKU identifiers (e.g. `hawaiian_m`) |
| `order_date`, `order_time` | When the order was placed |
| `pizza_name`, `pizza_category`, `pizza_size` | What was ordered — name, category (Classic/Chicken/Supreme/Veggie), size (S/M/L/XL/XXL) |
| `pizza_ingredients` | Ingredient list for the pizza |
| `quantity`, `unit_price`, `total_price` | Line-level quantity and pricing |

---

## 🧮 SQL Views (the semantic layer)

Each view answers one lettered question from the project's business-questions brief.

| # | Business Question | View |
|---|---|---|
| A.1 | Total Revenue | `vw_KPI_1_Total_Revenue` |
| A.2 | Average Order Value | `vw_KPI_2_Avg_Order_Value` |
| A.3 | Total Pizzas Sold | `vw_KPI_3_Total_Pizza_Sold` |
| A.4 | Total Orders | `vw_KPI_4_Total_Orders` |
| A.5 | Average Pizzas per Order | `vw_KPI_5_Avg_Pizzas_Per_Order` |
| B | Daily Trend for Total Orders | `vw_Chart_1_Daily_Trend_Total_Orders` |
| C | Monthly Trend for Total Orders | `vw_Chart_2_Monthly_Trend_Total_Orders` |
| D | % of Sales by Pizza Category | `vw_Chart_3_Pct_Sales_By_Category` |
| E | % of Sales by Pizza Size | `vw_Chart_4_Pct_Sales_By_Size` |
| F | Total Pizzas Sold by Pizza Category | `vw_Chart_5_Total_Pizza_Sold_By_Category` |
| G | Top 5 Pizzas by Revenue | `vw_Top5_Pizzas_By_Revenue` |
| H | Bottom 5 Pizzas by Revenue | `vw_Bottom5_Pizzas_By_Revenue` |
| I | Top 5 Pizzas by Quantity | `vw_Top5_Pizzas_By_Quantity` |
| J | Bottom 5 Pizzas by Quantity | `vw_Bottom5_Pizzas_By_Quantity` |
| K | Top 5 Pizzas by Total Orders | `vw_Top5_Pizzas_By_Orders` |
| L | Bottom 5 Pizzas by Total Orders | `vw_Bottom5_Pizzas_By_Orders` |

> 💡 `All_Pizza_Sales` is a plain pass-through view (`SELECT * FROM pizza_sales`) kept as an optional, ready-to-share copy of the source table.

### ⚠️ Notes on the script as provided

- `Final_Query.sql` runs `USE [Pizza];` at the top, then switches to `USE [Pizza DB];` before creating the Top 5 / Bottom 5 views — so those 6 views get created in a **different database** than the KPI/chart views. Worth pointing both `USE` statements at the same database name before running this end to end.
- The `vw_Chart_4_Pct_Sales_By_Size` view aliases its percentage column as `PST` (likely meant to read `PCT`, matching the category view) — harmless, but worth renaming for consistency if you're citing the column elsewhere.

---

## 🛠️ Tech Stack

![SQL Server](https://img.shields.io/badge/-SQL%20Server%20Views-CC2927?style=flat-square&logo=microsoftsqlserver&logoColor=white)
![Power BI Desktop](https://img.shields.io/badge/-Power%20BI%20Desktop-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/-Excel-217346?style=flat-square&logo=microsoftexcel&logoColor=white)

---

## 📁 Repository Structure

```
📦 pizza-sales-analytics-dashboard
 ┣ 🍕 pizza_sales.csv                        → Raw source data (48,620 rows)
 ┣ 📊 pizza_sales_excel_file.xlsx            → Excel version of the dataset
 ┣ 🗄️ Final_Query.sql                        → All 17 SQL views (KPIs, charts, top/bottom sellers)
 ┣ 📄 PIZZA SALES SQL QUERIES - Lab.docx     → Business questions (unsolved)
 ┣ 📄 PIZZA SALES SQL QUERIES - Solution.docx → Business questions with SQL answers
 ┣ 🖼️ assets/                                 → Screenshots used in this README
 ┗ 📘 README.md
```

## 🚀 How to Explore

1. Restore `pizza_sales.csv` (or the Excel file) into SQL Server as `dbo.pizza_sales`.
2. Run **`Final_Query.sql`** to create all 17 views (fix the `USE` statement noted above first).
3. Connect Power BI (or Excel) to the views and open the report — **Home** and **Best/Worst Seller** pages, filterable by pizza category and date range.

---

## 📜 License

MIT — feel free to fork, star, and use in your own portfolio.

## 👨‍💻 About Me

Hi, I'm **Mohamed Sobhy** — a graduate student and ML/Data Science researcher, working on data analytics and machine learning projects across research and applied domains.

🔗 GitHub: [M-M-Sobhy](https://github.com/M-M-Sobhy)
💼 LinkedIn: [Mohamed Mahmoud Sobhy](https://www.linkedin.com/in/mohamed-mahmoud-sobhy-668ba937a)
🎥 YouTube: [@M_Sob7y](https://www.youtube.com/@M_Sob7y)

---

<div align="center">💡 If this helped you, consider giving the repo a ⭐</div>
