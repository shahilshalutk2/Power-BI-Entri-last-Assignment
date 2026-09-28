# Logistics & Transportation: Fleet Performance & Delivery Efficiency

A Power BI operations dashboard designed to monitor and optimize fleet performance, on-time delivery rates, vehicle fuel economy, and operational cost metrics across transport routes[cite: 1].

---

## 📌 Project Overview
This project analyzes delivery operations for a logistics organization[cite: 1]. By integrating trip-level operations with vehicle master data, the dashboard delivers actionable insights into delivery timeliness, fuel economy trends, vehicle maintenance expenditures, and route bottlenecks to support data-driven dispatch decisions[cite: 1].

---

## 🛠️ Tech Stack & Tools
* **Business Intelligence:** Microsoft Power BI Desktop[cite: 1]
* **Data Processing & ETL:** Power Query (M Formula Language)[cite: 1]
* **Analytical Modeling:** DAX (Data Analysis Expressions)[cite: 1]
* **Source Data Format:** Microsoft Excel (`.xlsx`)[cite: 1, 2]

---

## 📂 Data Model & Schema
The project uses a relational star-like schema connecting two primary tables[cite: 1]:
* **`Vehicle_Master`** (Dimension Table): `Vehicle_ID` (PK), `Vehicle_Type`, `Capacity_kg`, `Maintenance_Cost`[cite: 1].
* **`Trip_Data`** (Fact Table): `Trip_ID`, `Vehicle_ID` (FK), `Driver_ID`, `Origin`, `Destination`, `Distance_km`, `Fuel_Consumed_L`, `Delivery_Status`, `Delivery_Date`[cite: 1].
* **Relationship:** One-to-Many (`1:*`) from `Vehicle_Master[Vehicle_ID]` to `Trip_Data[Vehicle_ID]` with single cross-filter direction[cite: 1].

---

## 🧹 Key ETL & Data Cleaning Steps
1. **Ghost Column Removal:** Filtered out unformatted trailing blank columns (`Column10` through `Column26`) loaded from raw Excel sheets[cite: 2].
2. **Text Anomaly & Identifier Fixes:** Replaced irrelevant text in the trip identifier column with a sequential `Trip_ID` index (`T001`–`T050`)[cite: 1, 4].
3. **Data Type Casting & Value Replacements:** Replaced irregular string calculation errors (`116+666` replaced with numeric equivalent in `Distance_km`), casting distance to Whole Number, fuel to Decimal, and dates to standard Date[cite: 1].
4. **Calculated Route Attribute:** Created standard route descriptors combining origin and destination points:
   ```powerquery
   [Origin] & " -> " & [Destination]
   
## Dax Measures
-- 1. Helper Measures
Total Trips = COUNTROWS('Trip_Data')

Total Distance = SUM('Trip_Data'[Distance_km])

Total Fuel Consumed = SUM('Trip_Data'[Fuel_Consumed_L])

On-Time Trips = 
CALCULATE(
    COUNTROWS('Trip_Data'),
    'Trip_Data'[Delivery_Status] = "On-Time"
)

-- 2. Core Assignment Measures
Fuel Efficiency = 
DIVIDE([Total Distance], [Total Fuel Consumed], 0)

On-Time Delivery % = 
DIVIDE([On-Time Trips], [Total Trips], 0)

Cost per km = 
VAR FuelRate = 1.50
VAR TotalFuelCost = [Total Fuel Consumed] * FuelRate
VAR TotalMaintenanceCost = SUM('Vehicle_Master'[Maintenance_Cost])
RETURN
DIVIDE(TotalFuelCost + TotalMaintenanceCost, [Total Distance], 0)

-- 3. Delivery Time Metric Proxy
Avg Delivery Time (hrs) = 
DIVIDE(AVERAGE('Trip_Data'[Distance_km]), 50, 0)
 # 📊 Dashboard Visualizations
* KPI Summary Cards: High-level metrics tracking Cost per km ($1.55/km), On-Time Delivery % (60.0%), Average Delivery Duration (21.4 hrs), and Fleet Fuel Efficiency (11.58 km/L).
* Bar Chart (On-Time Delivery % by Route): Horizontal ranked comparison of route performance to pinpoint high-delay lanes versus 100% on-time routes.
* Line Chart (Fuel Efficiency Trend by Month): Time-series analysis monitoring monthly fleet fuel economy trends.
* Geographic Map Visual: Geospatial mapping of origin-destination delivery points scaled by trip volume with route efficiency tooltips.
* Geographic Map Visual: Geospatial mapping of origin-destination delivery points scaled by trip volume with route efficiency tooltips.
# 👤 Author
Shahil Shalu TK
