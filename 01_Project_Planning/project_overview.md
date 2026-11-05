# Project Overview

## 1. Project Title
**Electronics Component Inventory & Procurement Analytics**

## 2. Project Description
This is an end-to-end Data Analytics and Business Intelligence project focused on inventory management and procurement analysis within the electronics industry. 

The project simulates the day-to-day operations of a fictional electronics component distributor and small-scale electronics manufacturing business. The organization manages a diverse catalog of electronic components—such as resistors, capacitors, diodes, transistors, power MOSFETs, integrated circuits, sensors, microcontrollers, and interconnect hardware—used in assembly operations and finished goods.

The business requires operational visibility into component stock levels, inventory valuation, reorder thresholds, supplier purchasing commitments, and vendor delivery fulfillment. The project organizes synthetic operational records into structured datasets, prepares them via Power Query, models relationships in Power BI, and delivers interactive dashboards for decision support.

---

## 3. Industry Scenario & Context
In the electronics manufacturing and distribution domain, companies manage thousands of individual part numbers across multiple functional categories. A typical printed circuit board (PCB) or control unit assembly requires coordinating dozens of distinct components from multiple suppliers. 

Failure to maintain balanced stock levels results in either costly production line halts (due to stockouts) or tied-up working capital and component obsolescence (due to overstocking). Furthermore, purchasing managers must monitor supplier reliability, purchase price variance across vendors, and delivery lead times to protect gross margins and meet production schedules.

---

## 4. Primary Users & Stakeholders
- **Inventory Managers**: Responsible for maintaining stock availability, monitoring zero-stock and low-stock components, tracking inventory valuation in Indian Rupee (INR - ₹), understanding category distributions, and evaluating rental asset availability.
- **Procurement Managers**: Responsible for tracking purchase order execution, managing vendor relationships, evaluating delivery lead times, monitoring On-Time Delivery (OTD %) rates, and analyzing purchase price variances.

---

## 5. Core Objectives
1. **Consolidate Disconnected Records**: Organize multi-sheet operational data into a unified, relational star/snowflake data model inside Microsoft Power BI.
2. **Automate Data Preparation**: Implement an automated ETL pipeline using Power Query to cleanse, normalize, and validate data types.
3. **Enable Inventory Health Monitoring**: Provide real-time visibility into components below reorder thresholds, stockouts, and capital valuation.
4. **Evaluate Supplier Performance**: Measure delivery turnaround times, on-time arrival percentages, and vendor spend concentration.
5. **Support Executive Decision-Making**: Deliver interactive, role-tailored dashboard pages with dynamic slicers, cross-filtering, and DAX KPI calculations.

---

## 6. Strict Technology Stack

This project is built exclusively using the Microsoft Business Intelligence stack:

| Purpose | Technology | Role in Architecture |
| :--- | :--- | :--- |
| **Source Data Storage** | Microsoft Excel | Six structured worksheets in `02_Dataset/raw/electronics_inventory_raw.xlsx` |
| **Data Cleaning & ETL** | Power Query | Ingestion, data typing, missing value handling, text normalization, date parsing |
| **Data Modelling** | Microsoft Power BI Desktop | Star/snowflake schema design, 1-to-many single-direction relationships, conformed dimensions |
| **Calculations & Metrics** | DAX (Data Analysis Expressions) | Measure formulas for inventory valuation, stock conditions, spend, lead time, and OTD % |
| **Visual Reporting** | Microsoft Power BI Desktop | Interactive multi-page dashboards, KPI cards, trend visuals, and detailed matrix tables |

> [!IMPORTANT]
> **Technology Boundary**: External database engines, query servers, and custom scripting languages are strictly excluded from this project. All data storage, transformation, modeling, and visualization remain entirely within Excel, Power Query, and Power BI Desktop.

---

## 7. Data Authenticity & Scope Standards
- **Data Authenticity**: **100% Synthetic Data**. All records, part numbers, vendor names, dates, costs, and quantities are fictional and generated specifically for simulation and educational analytics.
- **Reporting Currency**: Indian Rupee (**INR - ₹**).
- **Unit of Measurement**: Standardized to **Piece** across all component categories.
- **Operational Horizon**: Annual operational data spanning the calendar year **2025**.

---

## 8. Primary Analytical Modules

### Module A: Inventory Analytics (For Inventory Managers)
- Component catalog breadth (105 distinct parts).
- Total physical stock volume on hand for selected snapshot dates.
- Stock distribution across 8 component categories.
- Immediate identification of low-stock parts (`QuantityOnHand < ReorderLevel`).
- Immediate identification of zero-stock parts (`QuantityOnHand = 0`).
- Inventory holding valuation in INR (₹) using standard unit costs.
- Capital concentration by component category.
- Monthly stock quantity trends across 12 snapshot periods.
- Tracking of rental-eligible components.

### Module B: Procurement Analytics (For Procurement Managers)
- Total purchase orders issued (100 purchase orders).
- Net procurement commitment value across non-cancelled orders.
- Supplier spend concentration across 50 registered vendors.
- Frequently procured components and total pieces ordered.
- Monthly procurement expenditure trajectory across 2025.
- Purchase order status breakdown (Delivered: 80, Pending: 12, Cancelled: 8).
- Average delivery lead time in days for fulfilled orders.
- Supplier On-Time Delivery Rate (OTD %).
- Purchase price variance analysis across vendors for identical parts.

---

## 9. Key Project Deliverables
1. **Source Excel Workbook**: `02_Dataset/raw/electronics_inventory_raw.xlsx` with 6 relational worksheets.
2. **Power Query ETL Documentation**: Step-by-step transformation rules and M-code documentation.
3. **Power BI Dashboard File**: `04_Power_BI/Electronics_Inventory_Procurement.pbix` featuring multi-page interactive reporting.
4. **DAX Measures Catalog**: Formula definitions, business logic, and calculation contexts.
5. **Data Model Documentation**: Star/snowflake schema diagram, table classifications, and relationship cardinalities.
6. **Analytical & Strategic Reports**: Data quality report, analytical findings, business recommendations, and project limitations.
7. **Executive Presentation**: Stakeholder slide decks in `06_Presentation/`.
