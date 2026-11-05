# Problem Statement

## 1. Electronics Industry Business Scenario
Electronics manufacturing and distribution enterprises operate in an environment characterized by rapid technological cycles, volatile component lead times, and extensive catalogs of discrete and integrated devices. A typical product—such as an industrial controller, consumer IoT device, or power management system—requires coordinating dozens of distinct components:
- Microcontrollers and development boards
- Passive components (metal film resistors, electrolytic capacitors, power inductors)
- Discrete semiconductors (switching diodes, rectifier diodes, BJTs, power MOSFETs)
- Integrated circuits (linear op-amps, timers, voltage regulators, shift registers)
- Precision environmental and physical sensors
- Connectors, terminal blocks, and cabling hardware
- Electromechanical devices (relays, switches, cooling fans)

To maintain continuous manufacturing operations and fulfill customer orders without disruption, the business must maintain balanced stock levels while constantly purchasing replenishments from diverse commercial distributors and manufacturers.

---

## 2. Existing Operational Challenges

In many small-to-medium electronics businesses, operational data is maintained in isolated, disconnected spreadsheets across different functional teams. The inventory team maintains warehouse stock sheets, bin allocations, and unit costs, while the procurement team maintains vendor logs, purchase order records, and receipt notes.

This fragmented record-keeping creates severe operational bottlenecks and manual reporting overhead:

### Inventory Management Difficulties
1. **Difficulty in Identifying Low-Stock Components**: Stock levels must be manually compared against minimum safety reorder thresholds across more than 100 part numbers. Components often fall below critical levels unnoticed until assembly lines stall.
2. **Delayed Zero-Stock Visibility**: Identifying parts that have completely run out of stock (`QuantityOnHand = 0`) requires manual auditing, creating reactive expediting and emergency procurement costs.
3. **Complex Inventory Valuation**: Estimating total inventory capital holding value in Indian Rupee (INR - ₹) across changing stock counts and standard unit costs is slow, error-prone, and infrequently calculated.
4. **Lack of Category-Wise Distribution Insight**: Managers cannot easily see whether capital is trapped in slow-moving high-cost microcontrollers or distributed effectively across high-turnover passives.
5. **Difficulty in Tracking Monthly Stock Trajectories**: Stock data exists as periodic snapshot observations; understanding whether inventory is accumulating, depleting, or stabilizing over time requires tedious multi-sheet consolidation.
6. **Obscured Rental Fleet Availability**: Identifying which high-value development boards or diagnostic sensor kits are eligible for rental programs requires cross-referencing static catalog notes.

### Procurement & Supplier Management Difficulties
1. **Incomplete Purchase Order Oversight**: Tracking the total volume and timeline of purchase orders across the operational year is fragmented.
2. **Inaccurate Procurement Spend Estimation**: Calculating committed procurement spend across purchase order line items requires manual summation while manually excluding cancelled orders.
3. **Unclear Supplier Spend Concentration**: Management lacks immediate visibility into which suppliers receive the highest purchasing budget and whether single-source dependencies exist.
4. **Uncertainty Over Frequently Procured Components**: The business struggles to distinguish components that are ordered frequently in small batches from those ordered infrequently in bulk.
5. **Difficulty in Comparing Monthly Spend Trends**: Purchasing expenditure trends across quarters cannot be dynamically analyzed against cash-flow requirements.
6. **Fragmented Order Fulfillment Tracking**: Monitoring the proportion of orders that are `Delivered`, `Pending`, or `Cancelled` is cumbersome.
7. **Manual Delivery Lead-Time Calculation**: Calculating the turnaround time (in days) between order date and actual receipt date requires manual date subtractions across order logs.
8. **Lack of On-Time Delivery (OTD %) Metrics**: Evaluating supplier delivery reliability against committed expected delivery dates is rare, preventing objective vendor performance reviews.
9. **Unmonitored Purchase Price Variance**: Identical electronic parts are purchased from different distributors at varying prices, but without centralized analytics, the procurement team cannot systematically compare vendor pricing.

---

## 3. Why an Integrated Analytical Dashboard is Needed
Relying on static spreadsheets and manual calculations leads to:
- **Excessive Administrative Overhead**: Analytical teams spend days preparing reports rather than deriving strategic insights.
- **Production Vulnerabilities**: Assembly lines risk shut-downs due to unnoticed stockouts.
- **Suboptimal Capital Allocation**: Working capital is unnecessarily tied up in excess inventory.
- **Weak Supplier Negotiation Leverage**: Purchasing teams negotiate without objective vendor scorecard data on lead times, pricing history, and delivery compliance.

---

## 4. Proposed Solution: Excel + Power Query + Power BI

The project proposes a unified Business Intelligence solution built on the Microsoft Analytics stack:

```text
Microsoft Excel Source Workbook (6 Worksheets)
       ↓
Power Query – Automated Data Cleaning, Shaping & Type Casting
       ↓
Power BI – Star/Snowflake Relational Modelling
       ↓
DAX – Calculated Measures and Operational KPI Formulas
       ↓
Interactive Inventory & Procurement Dashboards
```

1. **Structured Source Storage (Microsoft Excel)**: Store operational records in a clean, multi-table Excel workbook (`electronics_inventory_raw.xlsx`) across 6 logical worksheets (`Categories`, `Components`, `Suppliers`, `InventorySnapshots`, `PurchaseOrders`, `PurchaseOrderDetails`).
2. **Automated ETL Pipeline (Power Query)**: Ingest, clean, normalize, and validate all 6 datasets without manual copy-pasting. Handle null delivery dates, cast explicit data types, and enforce referential key integrity.
3. **Unified Star/Snowflake Data Model (Power BI Desktop)**: Establish deterministic, single-direction one-to-many relationships connecting component master data, monthly inventory snapshots, supplier profiles, purchase order headers, and line-item details.
4. **Mathematical KPI Engine (DAX)**: Author calculated measures for current inventory valuation, low-stock/zero-stock alerts, net procurement spend, average lead time, and on-time delivery percentages.
5. **Interactive Executive Dashboards**: Deliver tailored visual reporting for Inventory Managers and Procurement Managers, equipped with dynamic date slicers, category filters, supplier cards, and drill-down tables.
