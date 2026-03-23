# ⚡ EV Charging Infrastructure Analysis: End-to-End Data Pipeline

## 📖 Project Overview
This project provides a comprehensive, end-to-end data analysis of an Electric Vehicle (EV) charging network. The objective is to evaluate the financial viability, operational efficiency, and regional performance of various charging stations. By building a complete data pipeline—from raw data extraction and feature engineering to advanced SQL querying and interactive visualization—this project identifies key drivers of profitability, uncovers operational anomalies, and highlights areas for strategic network optimization.

## 🛠️ Tech Stack
* **Data Wrangling & Feature Engineering:** Python (Pandas, NumPy)
* **Exploratory Data Analysis (EDA):** SQL (CTEs, Window Functions, Aggregations)
* **Data Visualization & Business Intelligence:** Power BI (DAX Measures, Interactive Dashboards)

---

## ⚙️ Step 1: Data Cleaning & Feature Engineering (Python)
The raw dataset (`ev_charging_raw_data.csv`) contained foundational metrics such as installation costs, energy dispensed, and maintenance incidents. To enable deep financial analysis, Python was utilized to clean the data, handle formatting inconsistencies, and engineer critical business metrics.

**Calculated Features Engineered:**
* `Total_Revenue`: `Avg_Charging_Rate_INR` × `Total_Energy_Dispensed_kWh`
* `Total_Operational_Cost`: (`Total_Energy_Dispensed_kWh` × `Electricity_Cost_INR`) + `Total_Maintenance_Cost_INR`
* `Net_Profit`: `Total_Revenue` - `Total_Operational_Cost`
* `ROI` (Return on Investment): (`Net_Profit` / `Install_Cost_INR`) * 100

All financial figures were rounded to two decimal places for reporting accuracy, resulting in a robust, analysis-ready dataset (`ev_charging_cleaned_data.csv`).

---

## 🔍 Step 2: Advanced Data Analysis (SQL)
With the cleaned dataset, advanced SQL queries were executed to answer specific business questions and segment the charging stations based on their performance metrics. 

**Key Queries & Techniques Used:**
* **Aggregated Profitability:** Grouped data to determine which `Charger_Type` yields the highest overall net profit.
* **Regional Benchmarking:** Ranked cities based on their Average ROI to identify the most lucrative geographical markets.
* **Performance Categorization:** Utilized `CASE` statements to create a `Profitability_Tier`, segmenting stations into 'High' (> ₹500,000), 'Medium' (₹100,000 - ₹500,000), and 'Low' (< ₹100,000) tiers.
* **Underperformer Identification (Window Functions):** Employed `RANK() OVER (PARTITION BY City ORDER BY Net_Profit)` to isolate the bottom 3 performing stations within *each* city for targeted operational review.
* **Market Leaders (CTEs):** Used Common Table Expressions to identify the Top 10 cities whose average net profit exceeds the global network average.

---

## 📊 Step 3: Interactive Dashboard (Power BI)
To make these insights accessible and actionable for stakeholders, an interactive Power BI dashboard was developed. 

**Dashboard Components:**
* **KPI Cards:** High-level metrics displaying Total Network Revenue, Total Operational Cost, Overall Net Profit, Average Network ROI, and Total Energy Dispensed.
* **DAX Measures:** Custom DAX measures were written to dynamically calculate Profit Margins, Average Maintenance Costs per Incident, and contextual YTD Energy metrics.
* **Interactive Slicers:**
    * *City / Region:* For localized market analysis.
    * *Charger Type (DCFC vs. L2):* For hardware comparison.
    * *Operational Status:* To filter stations by Active, Offline, or Under Maintenance states.
* **Visual Charts:**
    * *Bar Charts & Column Charts:* Net Profit by City and Revenue distribution.
    * *Scatter Plots:* Mapping Installation Cost against Net Profit to visualize the break-even density.
    * *Line Charts:* Energy Dispensed trends over time.

---

## 💡 Key Insights

1. **The "Build Quality" Principle (Cost vs. Maintenance):** The analysis revealed a clear inverse relationship between `Install_Cost_INR` and `Total_Maintenance_Cost_INR`. Stations with higher initial installation costs (suggesting better build quality or premium hardware) suffer far fewer maintenance incidents, ultimately driving higher long-term `Net_Profit`.
2. **L2 Charger Vulnerability:** When analyzing hardware types, Level 2 (L2) chargers recorded a significantly higher rate of maintenance incidents compared to DC Fast Chargers (DCFC). This frequent downtime severely impacts their overall profitability.
3. **The "Offline" Anomaly (Data & Energy Leak):** A critical anomaly was discovered regarding stations marked with an "Offline" operational status. These stations are still accounting for approximately 5% of total energy and electricity consumption—a metric that should theoretically be null. Furthermore, these offline stations are disproportionately draining the maintenance budget. 
4. **Regional Wealth Concentration:** A concentrated subset of Top 10 cities significantly pulls up the network's average profit, boasting higher average ROIs due to optimal utilization rates.

---

## 🚀 Strategic Recommendations

* **Invest in Premium Hardware Upfront:** Because installation cost is inversely proportional to maintenance cost, future capital expenditure should prioritize higher-quality builds. Skimping on initial installation costs leads to margin-eroding maintenance cycles later.
* **Immediate Audit of Offline Stations:** An urgent technical audit must be conducted on all "Offline" stations. The 5% phantom energy consumption suggests either a severe hardware electrical leak, a software data-logging error, or unauthorized usage. Furthermore, the high maintenance spend on these inactive stations needs immediate financial review.
* **Shift Focus toward DCFC:** Given that L2 chargers suffer from higher maintenance incidents and lower overall revenue velocity, new network expansions—especially in the Top 10 high-performing cities—should heavily favor DC Fast Chargers.
* **Targeted Relocation:** Utilize the SQL "Bottom 3" analysis to identify chronic underperformers. If an L2 station in a low-tier city is constantly under maintenance and failing to generate profit, the asset should be decommissioned or relocated rather than continually repaired.
