# Electronics Component Inventory & Procurement Analytics

[![Source: Excel](https://img.shields.io/badge/Source-Microsoft%20Excel-green.svg)](02_Dataset/raw/electronics_inventory_raw.xlsx)
[![ETL: Power Query](https://img.shields.io/badge/ETL-Power%20Query-2CA5E0.svg)](03_Power_Query/transformation_documentation.md)
[![Modelling: Power BI](https://img.shields.io/badge/Modelling-Power%20BI-F2C811.svg)](04_Power_BI/)
[![Calculations: DAX](https://img.shields.io/badge/Calculations-DAX-orange.svg)](04_Power_BI/dax_measures.md)
[![Currency: INR (₹)](https://img.shields.io/badge/Currency-INR%20(%E2%82%B9)-success.svg)](02_Dataset/)
[![License: MIT](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

An end-to-end Data Analytics and Business Intelligence project simulating an electronics component distributor and small-scale electronics manufacturing enterprise. The project consolidates disconnected inventory tracking records and procurement logs from **Microsoft Excel**, cleanses and transforms the data using **Power Query**, models star-schema relationships in **Microsoft Power BI**, and delivers executive decision-support dashboards powered by **DAX**.

---

## Table of Contents
- [Executive Summary](#executive-summary)
- [Technology Stack](#technology-stack)
- [Problem Statement & Proposed Solution](#problem-statement--proposed-solution)
- [End-to-End Workflow & Architecture](#end-to-end-workflow--architecture)
- [Relational Schema & Dataset Overview](#relational-schema--dataset-overview)
- [Analytical Modules & 18 Business Questions](#analytical-modules--18-business-questions)
- [Core KPI Definitions & DAX Formulas](#core-kpi-definitions--dax-formulas)
- [Power BI Dashboard Pages](#power-bi-dashboard-pages)
- [Key Business Insights & Findings](#key-business-insights--findings)
- [Strategic Management Recommendations](#strategic-management-recommendations)
- [Repository Structure](#repository-structure)
- [Quickstart & Replication Guide](#quickstart--replication-guide)
- [Project Scope & Deliverables](#project-scope--deliverables)

---

## Executive Summary

| Attribute | Specification |
| :--- | :--- |
| **Project Type** | End-to-end Data Analytics & Business Intelligence Solution |
| **Industry Scenario** | Fictional Electronics Component Distributor & Small Manufacturing Enterprise |
| **Primary Users** | **Inventory Managers** & **Procurement Managers** |
| **Reporting Currency**| Indian Rupee (**INR - ₹**) |
| **Unit of Measurement**| **Piece** (standardized across all component categories) |
| **Data Authenticity** | **100% Synthetic** (Relational dataset created from scratch for simulation) |
| **Core Architecture** | Microsoft Excel $\rightarrow$ Power Query $\rightarrow$ Power BI Data Model $\rightarrow$ DAX Measures $\rightarrow$ Dashboards |

---

## Technology Stack

This project is built strictly using the modern Microsoft Business Intelligence ecosystem:

| Purpose | Technology | Implementation Details |
| :--- | :--- | :--- |
| **Source Data** | Microsoft Excel | Structured relational workbook (`electronics_inventory_raw.xlsx`) across 6 worksheets |
| **Data Cleaning and ETL** | Power Query | Automated M-language transformations, type casting, date normalization, null handling |
| **Data Modelling** | Microsoft Power BI | Star/snowflake schema design, 1-to-many single-direction relationships, conformed dimensions |
| **Calculations** | DAX | Calculated measures for valuation, safety stock alerts, lead time, and delivery reliability |
| **Visualization** | Microsoft Power BI | Executive dashboards, card KPIs, trend lines, matrix breakdowns, interactive slicers |

---

## Problem Statement & Proposed Solution

### The Challenge
Electronics businesses manage thousands of discrete and integrated components across categories like resistors, capacitors, power semiconductors, sensors, microcontrollers, and electromechanical parts. 

When operational records are maintained in isolated spreadsheets:
- **Stockout & Shortage Risks**: Identifying components below safety thresholds or experiencing zero stock requires reactive manual audits, leading to production line halts.
- **Inaccurate Inventory Valuation**: Calculating true inventory carrying value across volatile parts categories is slow and error-prone.
- **Uncontrolled Procurement Spend**: Comparing purchase price variance across multiple vendors for identical parts is difficult without centralized purchasing analytics.
- **Supplier Delivery Bottlenecks**: Tracking supplier fulfillment delays, pending orders, and delivery lead times is time-consuming.

### Proposed Solution
This project implements an integrated analytical pipeline:
1. **Relational Data Management**: Six normalized tables maintained in a structured Microsoft Excel workbook.
2. **Power Query ETL**: Automated data ingestion, cleansing, type casting, date normalization, and referential validation.
3. **Power BI Data Modelling**: Star/snowflake schema design with single-direction filter propagation and conformed dimensions.
4. **DAX KPI Engine**: Robust measures for valuation, safety stock alerts, supplier lead time, and on-time fulfillment rates.
5. **Interactive Executive Dashboards**: Role-tailored dashboards for Inventory and Procurement leadership with dynamic cross-filtering and drill-through capabilities.

---

## End-to-End Workflow & Architecture

```mermaid
flowchart TD
    A["Microsoft Excel Source Workbook<br/>electronics_inventory_raw.xlsx (6 Worksheets)"]
    --> B["Power Query ETL Pipeline<br/>Data Cleaning, Type Casting & Referential Checks"]
    --> C["Power BI Data Model<br/>Star / Snowflake Schema Relationships"]
    --> D["DAX Calculations & KPI Engine<br/>Valuation, Stock Alerts, Lead Time & OTD %"]
    --> E["Interactive Power BI Dashboards<br/>Inventory & Procurement Reporting"]
    --> F["Business Insights & Strategic Recommendations<br/>Working Capital & Supplier Optimization"]
```

---

## Relational Schema & Dataset Overview

The dataset is organized across **6 normalized tables** stored within [`02_Dataset/raw/electronics_inventory_raw.xlsx`](02_Dataset/raw/electronics_inventory_raw.xlsx):

```mermaid
erDiagram
    Categories ||--o{ Components : "classifies (1:N)"
    Components ||--o{ InventorySnapshots : "tracked in (1:N)"
    Components ||--o{ PurchaseOrderDetails : "ordered in (1:N)"
    Suppliers ||--o{ PurchaseOrders : "fulfills (1:N)"
    PurchaseOrders ||--o{ PurchaseOrderDetails : "contains (1:N)"

    Categories {
        Text CategoryID PK
        Text CategoryName UK
        Text CategoryDescription
    }

    Components {
        Text ComponentID PK
        Text ComponentName
        Text CategoryID FK
        Text UnitOfMeasure
        Decimal StandardUnitCost
        Integer ReorderLevel
        Text ConditionStatus
        Boolean RentEligible
    }

    Suppliers {
        Text SupplierID PK
        Text SupplierName
        Text City
        Text SupplierType
    }

    InventorySnapshots {
        Text SnapshotID PK
        Text ComponentID FK
        Date SnapshotDate
        Integer QuantityOnHand
        Text StorageLocation
    }

    PurchaseOrders {
        Text PurchaseOrderID PK
        Text SupplierID FK
        Date OrderDate
        Date ExpectedDeliveryDate
        Date ActualDeliveryDate
        Text OrderStatus
    }

    PurchaseOrderDetails {
        Text POLineID PK
        Text PurchaseOrderID FK
        Text ComponentID FK
        Integer QuantityOrdered
        Decimal UnitPurchaseCost
    }
```

### Table Specifications & Live Record Counts

| # | Table Name | Record Count | Primary Key | Foreign Keys | Role / Contents |
| :-: | :--- | :---: | :---: | :---: | :--- |
| 1 | [**`Categories`**](02_Dataset/data_dictionary.md#table-1-categories) | **8** | `CategoryID` | — | Component category master catalog |
| 2 | [**`Components`**](02_Dataset/data_dictionary.md#table-2-components) | **105** | `ComponentID` | `CategoryID` | Electronic parts catalog with standard unit costs in Rupee (₹) |
| 3 | [**`Suppliers`**](02_Dataset/data_dictionary.md#table-3-suppliers) | **50** | `SupplierID` | — | Vendor profiles across Indian industrial hubs |
| 4 | [**`InventorySnapshots`**](02_Dataset/data_dictionary.md#table-4-inventorysnapshots) | **1,260** | `SnapshotID` | `ComponentID` | Monthly stock levels (105 parts × 12 months in 2025) |
| 5 | [**`PurchaseOrders`**](02_Dataset/data_dictionary.md#table-5-purchaseorders) | **100** | `PurchaseOrderID` | `SupplierID` | Order headers (Delivered: 80, Pending: 12, Cancelled: 8) |
| 6 | [**`PurchaseOrderDetails`**](02_Dataset/data_dictionary.md#table-6-purchaseorderdetails) | **125** | `POLineID` | `PurchaseOrderID`, `ComponentID` | Order line items with negotiated purchase prices (₹) |

---

## Analytical Modules & 18 Business Questions

### Module A: Inventory Analytics (For Inventory Managers)
Focuses on warehouse stock availability, stockout risks, holding valuation, and category distribution:

1. **Catalog Breadth**: How many distinct electronic components are actively managed? *(105 components)*
2. **Total Stock Volume**: What is the total aggregate inventory quantity on hand?
3. **Category Concentration**: Which component categories hold the highest stock volumes?
4. **Reorder Threshold Alerts**: Which specific components are currently below their reorder points?
5. **Stockout Incident Tracking**: Which components have zero stock and represent immediate shortage risks?
6. **Total Inventory Valuation**: What is the total monetary holding value of inventory in INR (₹)?
7. **High-Value Categories**: Which component categories account for the largest capital allocation?
8. **Monthly Stock Trajectory**: How do stock levels and consumption trends evolve across monthly snapshot cycles?
9. **Rental Asset Fleet**: How many components are eligible for commercial rental equipment programs?

### Module B: Procurement Analytics (For Procurement Managers)
Evaluates supplier spend, purchase volume, delivery turnaround times, and fulfillment reliability:

1. **PO Volume**: How many total purchase orders were issued across the operational year? *(100 POs)*
2. **Net Procurement Spend**: What is the total spend on non-cancelled purchase orders?
3. **Top Vendor Expenditure**: Which suppliers capture the largest share of procurement budget?
4. **Frequently Procured Parts**: Which electronic components are reordered most frequently?
5. **Monthly Purchasing Trend**: How does procurement expenditure vary month-over-month?
6. **Fulfillment Status Pipeline**: What proportion of orders are Delivered, Pending, or Cancelled?
7. **Delivery Lead Time**: What is the average lead time (in days) between order placement and receipt?
8. **On-Time Delivery Rate (OTD %)**: What percentage of delivered purchase orders arrived on or before schedule?
9. **Supplier Price Variance**: How do negotiated purchase prices for identical parts vary across different suppliers?

---

## Core KPI Definitions & DAX Formulas

All measures are documented in [`04_Power_BI/dax_measures.md`](04_Power_BI/dax_measures.md):

```dax
// 1. Total Inventory Valuation (₹)
Inventory Valuation = 
SUMX(
    InventorySnapshots,
    InventorySnapshots[QuantityOnHand] * RELATED( Components[StandardUnitCost] )
)

// 2. Zero-Stock Components Count
Zero Stock Components = 
CALCULATE(
    DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
    InventorySnapshots[QuantityOnHand] = 0
)

// 3. Low-Stock Components Alert (< Reorder Level)
Low Stock Components = 
CALCULATE(
    DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
    FILTER(
        InventorySnapshots,
        InventorySnapshots[QuantityOnHand] > 0 &&
        InventorySnapshots[QuantityOnHand] < RELATED( Components[ReorderLevel] )
    )
)

// 4. Net Committed Procurement Spend (₹)
Total Procurement Spend = 
CALCULATE(
    SUMX(
        PurchaseOrderDetails,
        PurchaseOrderDetails[QuantityOrdered] * PurchaseOrderDetails[UnitPurchaseCost]
    ),
    PurchaseOrders[OrderStatus] <> "Cancelled"
)

// 5. Average Delivery Lead Time (Days)
Average Delivery Lead Time (Days) = 
CALCULATE(
    AVERAGEX(
        PurchaseOrders,
        DATEDIFF( PurchaseOrders[OrderDate], PurchaseOrders[ActualDeliveryDate], DAY )
    ),
    PurchaseOrders[OrderStatus] = "Delivered" &&
    NOT ISBLANK( PurchaseOrders[ActualDeliveryDate] )
)

// 6. On-Time Delivery Rate (OTD %)
On-Time Delivery Rate (OTD %) = 
DIVIDE(
    CALCULATE( COUNTROWS( PurchaseOrders ), PurchaseOrders[OrderStatus] = "Delivered" && PurchaseOrders[ActualDeliveryDate] <= PurchaseOrders[ExpectedDeliveryDate] ),
    CALCULATE( COUNTROWS( PurchaseOrders ), PurchaseOrders[OrderStatus] = "Delivered" ),
    0
)
```

---

## Power BI Dashboard Pages

The solution includes a multi-page interactive dashboard within [`04_Power_BI/Electronics_Inventory_Procurement.pbix`](04_Power_BI/):

### Page 1: Inventory Analytics Dashboard
- **Executive KPI Cards**: Total Inventory Valuation (₹), Total Units on Hand, Zero Stock Alerts, Low Stock Alerts, Rental Fleet Size.
- **Stock Distribution by Category**: Clustered bar chart comparing piece quantity vs holding valuation across all 8 component categories.
- **Monthly Stock Trajectory**: Area line chart tracking physical inventory volume and capital holding fluctuations across 12 monthly snapshot dates in 2025.
- **Critical Stock Reorder Matrix**: Interactive table filtering parts below safety reorder levels, showing warehouse bin locations, condition status, and reorder deficits.

### Page 2: Procurement & Supplier Analytics Dashboard
- **Executive KPI Cards**: Total PO Spend (₹), Total PO Count, Delivered Orders, Average Lead Time (Days), On-Time Delivery Rate (OTD %).
- **Monthly Procurement Spend Trend**: Column chart displaying monthly purchasing expenditure over time.
- **Vendor Performance Matrix**: Scatter plot correlating Supplier Spend vs Average Lead Time and On-Time Delivery %.
- **Order Pipeline Breakdown**: Donut visual displaying proportions of Delivered (80%), Pending (12%), and Cancelled (8%) orders.
- **Supplier Price Variance Table**: Detailed drill-down analyzing unit purchase cost variance against standard reference costs.

---

## Key Business Insights & Findings

1. **Capital Concentration**: `Microcontrollers & Dev Boards` and `Sensors` account for **~66% of total inventory valuation**, despite comprising only 28% of catalog part numbers.
2. **Shortage Vulnerability**: An average of **9 to 11 components per month** dipped below safety thresholds, primarily driven by lead-time delays in semiconductor shipments.
3. **Supplier Fulfillment Reliability**: Current On-Time Delivery (OTD) rate is **76.25%**, with an average lead time of **11.4 days**.
4. **Spend Concentration**: The top 10 suppliers account for **~45% of total procurement spend**, indicating an opportunity for strategic volume discount renegotiation.

Full analytical findings are detailed in [`05_Documentation/analysis_findings.md`](05_Documentation/analysis_findings.md).

---

## Strategic Management Recommendations

1. **Implement ABC-XYZ Stock Categorization**: Apply tight weekly audits on Class A high-value microcontrollers to free working capital, while buffering Class C low-cost passives to prevent line halts.
2. **Automate Reorder Alerts**: Use Power BI visual threshold indicators to trigger purchase requisitions before parts reach zero stock.
3. **Consolidate Vendor Volume**: Consolidate purchasing from 50 fragmented suppliers into the top 12 preferred vendors to capture 5–8% volume discounts.
4. **Enforce Supplier SLAs**: Introduce contractual on-time fulfillment credits or penalty clauses for vendors with lead times exceeding 15 days.

Full strategic guidance is detailed in [`05_Documentation/business_recommendations.md`](05_Documentation/business_recommendations.md).

---

## Repository Structure

```text
Electronics-Component-Analytics/
│
├── README.md                            # Master project documentation
├── PROJECT_CONTEXT.md                   # Business domain, personas & pure BI workflow
├── .gitignore                           # Cleaned rules (Power BI, Excel, OS, logs)
│
├── 01_Project_Planning/
│   ├── project_overview.md              # Scope, users, purpose & synthetic data disclaimer
│   ├── problem_statement.md             # Operational challenges & Power BI BI solution
│   ├── business_requirements.md         # Modules A & B + 18 business questions
│   ├── project_scope.md                 # Scope matrix (in-scope vs out-of-scope)
│   └── project_phases.md                # 7-phase delivery roadmap
│
├── 02_Dataset/
│   ├── raw/
│   │   ├── .gitkeep
│   │   └── electronics_inventory_raw.xlsx # Intact source workbook (6 relational sheets)
│   ├── processed/
│   │   ├── .gitkeep
│   │   └── electronics_inventory_cleaned.xlsx
│   └── data_dictionary.md               # Schema specifications & ER diagram
│
├── 03_Power_Query/
│   └── transformation_documentation.md  # Detailed M-language ETL documentation
│
├── 04_Power_BI/
│   ├── Electronics_Inventory_Procurement.pbix # Interactive dashboard file
│   ├── dax_measures.md                  # Comprehensive catalog of DAX formulas
│   └── data_model_documentation.md      # Star schema design & cardinalities
│
├── 05_Documentation/
│   ├── data_quality_report.md           # Dataset quality, referential checks & audit
│   ├── analysis_findings.md             # Key inventory & procurement findings
│   ├── business_recommendations.md      # Actionable recommendations for leadership
│   └── limitations.md                   # Operational boundaries & roadmap
│
├── 06_Presentation/
│   ├── .gitkeep
│   ├── project_presentation.pptx
│   └── Electronics Component Inventory and Procurement Analysis Dashboard.pptx
│
└── 07_Screenshots/
    ├── dashboard/                       # Power BI report page captures
    └── data_model/                      # Power BI data model diagram captures
```

---

## Quickstart & Replication Guide

### 1. Clone the Repository
```bash
git clone https://github.com/shashaank747/Electronics-Component-Analytics.git
cd Electronics-Component-Analytics
```

### 2. Inspect Raw Datasets in Microsoft Excel
Open [`02_Dataset/raw/electronics_inventory_raw.xlsx`](02_Dataset/raw/electronics_inventory_raw.xlsx) to review the 6 relational worksheets:
- `Categories`: 8 category definitions
- `Components`: 105 component items with standard costs in Rupee (₹)
- `Suppliers`: 50 supplier records across industrial cities
- `InventorySnapshots`: 1,260 monthly observations across 2025
- `PurchaseOrders`: 100 purchase orders (Delivered, Pending, Cancelled)
- `PurchaseOrderDetails`: 125 line items with negotiated purchase prices (₹)

### 3. Review Power Query ETL Transformations
Refer to [`03_Power_Query/transformation_documentation.md`](03_Power_Query/transformation_documentation.md) for detailed M-language steps, type coercion, and null handling rules.

### 4. Open the Power BI Dashboard
1. Launch **Microsoft Power BI Desktop**.
2. Open [`04_Power_BI/Electronics_Inventory_Procurement.pbix`](04_Power_BI/Electronics_Inventory_Procurement.pbix).
3. If prompted to refresh data, confirm source points to `02_Dataset/raw/electronics_inventory_raw.xlsx`.
4. Interact with the multi-page dashboards:
   - **Page 1: Inventory Analytics Dashboard** (Valuation, stockout risk, category allocation, monthly trajectory)
   - **Page 2: Procurement Analytics Dashboard** (Spend by supplier, lead time analysis, on-time delivery rate, order status breakdown)

---

## Project Scope & Deliverables

> [!NOTE]
> **Synthetic Data Disclaimer**: All records in this repository are 100% synthetic and generated specifically for data modeling and educational demonstration. No proprietary, sensitive, or real company data is used.

### In-Scope vs Excluded
- **In-Scope**: Descriptive & diagnostic inventory analytics, supplier performance comparison, lead time tracking, inventory valuation in INR (₹), Power BI interactive dashboards.
- **Excluded**: Real-time ERP transaction processing, live database servers, live WMS barcode/RFID scanning, manufacturing line defect analysis, and machine-learning predictive forecasting.
