# ☕ MR. BROS COFFEE — Power BI Sales Dashboard

> A polished Power BI report that visualizes coffee shop sales (hourly, daily, product mix, payments) and surfaces actionable insights for operations, marketing, and finance.

---

# 🔎 Project overview
This report is designed to give a clear, operational view of coffee sales:
- Total revenue, transaction count and average ticket.
- Top products by revenue and transaction volume.
- Payment channel breakdown (card, cash, UPI, etc.).
- Hourly patterns and a weekday × hour heatmap to identify staffing peaks.
- Interactive filters (date range, product, payment method, hour-of-day) and bookmarks (Reset / presets).

**Target audience:** store managers, shift supervisors, marketing owners, and analysts.

---

# 📈 Quick insights (sample)
- **Total Sales** — quick KPI at top.  
- **Peak hours** — visible from hourly column chart and heatmap.  
- **Best-selling items** — bar chart ranking products.  
- **Payment mix** — donut/pie showing channel shares.  

These are interactive — slicers and bookmarks refine the view.

---

# 🧾 Data dictionary

- `Date` — Date of transaction (Date)  
- `Time` — Raw time string (cleaned in ETL)  
- `DateTime` — Combined Date + Time (DateTime)  
- `hour_of_day` — 0–23 integer  
- `Weekday` — text name (Mon, Tue, …)  
- `Weekdaysort` — 1–7 integer for sorting weekdays  
- `Month_name` — text (Jan … Dec)  
- `Monthsort` — 1–12 integer for sorting months  
- `cash_type` — payment method (Card/Cash/UPI)  
- `coffee_name` — product name (Latte, Espresso, …)  
- `money` — transaction amount (decimal / currency)

---

# 🧭 What the dashboard shows (visual-by-visual)

**Left control column (filters & controls)**  
- Logo (top)  
- `coffee_name` slicer (search-enabled)  
- `cash_type` slicer  
- Bookmarks & Buttons (Reset, Last 30 days, All time)
- My LinkedIn Profile

**Main canvas**  
- **Top KPIs** — single-value cards with optional sparklines.  
- **Line chart** — Sales over time (Date or DateTime).  
- **Bar chart** — Sales by `coffee_name` (Top N).  
- **Donut chart** — payment channel share.  
- **Column chart** — Sales by `hour_of_day`.  
- **Matrix (Weekday × hour_of_day)** with background conditional formatting — heatmap showing concentration of sales.  

---

# 🛠 How it was built (ETL & modeling — summary)

## Power Query (ETL)
1. `Get Data` → CSV.  
2. Set column data types:
   - `Date` → Date  
   - `DateTime` → Date/Time  
   - `money` → Decimal Number  
   - `hour_of_day`, `Weekdaysort`, `Monthsort` → Whole Number  
3. Clean text columns (Trim / Clean).  
4. Clean `Time` values (remove decimals).  
5. Create `DateTime` by combining `Date` and cleaned `Time`.  
6. Remove unused columns.  
7. Close & Apply.

## Modeling
- Measures (not calculated columns) for KPIs.  
- `Sort by Column`:  
  - `Month_name` → `Monthsort`  
  - `Weekday` → `Weekdaysort`  
- Optional: add a dedicated `Date` table for advanced time intelligence.

---

# 🧮 Key DAX measures
```dax
Total Sales = SUM(CoffeeSales[money])

Transactions = COUNTROWS(CoffeeSales)

Avg Ticket = DIVIDE([Total Sales], [Transactions], 0)

Sales Running Total =
CALCULATE(
  [Total Sales],
  FILTER(ALLSELECTED(CoffeeSales), CoffeeSales[Date] <= MAX(CoffeeSales[Date]))
)

Sales % of Total = DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(CoffeeSales)), 0)

Last Data Date =
VAR d = MAX(CoffeeSales[Date])
RETURN "Data up to " & FORMAT(d, "dd-MMM-yyyy")
```
# ⚙️ How to run, reproduce & publish
# Local (Power BI Desktop)

git clone https://github.com/Mohd-Atir/Mr.-Bros-Coffee-Dashboard.git
cd Mr.-Bros-Coffee-Dashboard

1- Open report/MR_BROS_Coffee.pbix in Power BI Desktop OR create new PBIX and Get Data -> CSV → data/coffee_sales_sample.csv.
2- In Power Query, apply the transforms described above.
3- Create measures in Modeling pane, design visuals in Report view.
4- Test bookmarks (Ctrl+Click in Desktop).
5- Save .pbix.

# 📝 License & credits

Author: Mohd Atir (dashboard creator)
Tools: Power BI Desktop, Power Query, DAX

# 📄 License: MIT (or add a license of your choice).

# For support, contact LinkedIn
