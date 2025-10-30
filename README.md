# Mini-case 1 — E-commerce ROI Dashboard (Power BI, DAX, Power Query)

A Power BI project analyzing e-commerce marketing performance across channels.  
This mini-case demonstrates **data cleaning in Power Query**, **DAX measures** for ROI & margin,  
and an **interactive dashboard** with weekly trends and channel performance.

## 🧭 Goal
Which channels/campaigns and product categories drive **Revenue** & **Gross Margin**, and what is our **ROAS** per week?

## 🧩 Data & Grain
- **DimProduct (≈60)** — product info (a few missing `cost`, some spacing/casing noise)  
- **DimCustomer (≈220)** — segment/country/city  
- **DimCampaign (8)** — channel with inconsistent casing  
- **FactMarketingSpend (daily)** — 2025-02-01 .. 2025-04-30  
- **FactOrders (≈2.3–2.7k)** — order lines; `discount` sometimes `"0,1"`; a few duplicates  

**Grain**
- **FactOrders:** 1 row = 1 order line  
- **FactMarketingSpend:** 1 row = 1 day per campaign

## 📐 Data Model (star schema)
- FactOrders ➜ DimCustomer (`CustomerID`)  
- FactOrders ➜ DimProduct (`ProductID`)  
- FactOrders ➜ DimCampaign (`CampaignID`)  
- FactMarketingSpend ➜ DimCampaign (`CampaignID`)  
- Date table related to `FactOrders[order_date]` and `FactMarketingSpend[date]`.

## ⚙️ Power Query Highlights (FactOrders example)
1) **Source → Promote Headers**  
2) **Change Type (Using Locale)** for `discount` (Decimal, Dutch)  
3) **Trim & Clean** text (`OrderLineID`, `channel`, names)  
4) **Rename** to snake_case  
5) **Filter** `(null)` on key columns (if any)  
6) **Remove duplicates** on `orderline_id`  
7) **Custom columns**  
   - `revenue = qty * unit_price * (1 - discount)`  
   - `margin = revenue - qty * cost`

*(Other tables got similar light cleanup: types + Trim/Clean; channels normalized to `channel_norm`.)*

## 🧮 Key DAX Measures
- `Revenue`  
- `Cost`  
- `Gross Margin = [Revenue] - [Cost]`  
- `Spend`  
- `ROAS = DIVIDE([Revenue], [Spend])`  
- `Revenue WoW %` (time-intelligence)

*(Date table includes `WeekNum`, `YearWeek`, and `Week` label; `Week` sorted by `YearWeek`.)*

## 📈 Dashboard Overview
- **Cards:** Revenue, Gross Margin, ROAS  
- **Line chart:** Revenue by **Week** (avg line for reference)  
- **Matrix:** Performance by **channel_norm** (Revenue, Gross Margin, ROAS) with conditional formatting on ROAS  
- **Slicer:** Month

## 💡 Insights (examples)
- Email tends to achieve the **highest ROAS** due to low spend and stable revenue.  
- Paid Search contributes the **largest revenue** but shows **lower ROAS** → optimization opportunity.  
- Weekly margin peaks mid-period, suggesting a seasonal/promo effect.

## 🖼️ Screenshots
| ✅ Power Query Applied Steps | ![Power Query](screenshots/01_powerquery_applied_steps.png) |
| ✅ DAX ROAS Formula | ![ROAS Formula](screenshots/02a_dax_roas_formula.png) |
| ✅ DAX Revenue WoW Formula | ![Revenue WoW](screenshots/02b_dax_revenue_wow_formula.png) |
| ✅ Final Dashboard | ![Dashboard Overview](screenshots/03_dashboard_overview.png) |

## ▶️ How to open
- Open `minicase1_ecom_roi.pbix` in **Power BI Desktop (2024+)**.  
- Data is embedded via CSV import; no extra setup required.

---

**Tools:** Power BI Desktop, Power Query, DAX  
**Author:** Thijs ter Haar (2025) • Practice for **Microsoft PL-300**

