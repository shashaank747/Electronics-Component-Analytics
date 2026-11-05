# Project Scope & Boundaries

## 1. Project Purpose & Scope Definition
The **Electronics Component Inventory & Procurement Analytics** project is an analytical reporting and Business Intelligence solution designed to provide decision support for inventory and procurement leadership. 

The project consolidates historical operational records from Microsoft Excel, transforms them in Power Query, models relationships in Power BI, and delivers interactive visual dashboards.

> [!IMPORTANT]
> **Operational Boundary**: This is an analytical reporting solution, NOT a transactional Enterprise Resource Planning (ERP) or Warehouse Management System (WMS). It supports strategic and operational decision-making but does not execute live commercial transactions, generate supplier purchase orders, or process financial settlements.

---

## 2. In-Scope Functionality
The following capabilities and components are fully included within the project scope:

1. **Source Data Architecture**:
   - Maintenance of 6 structured, relational worksheets inside `02_Dataset/raw/electronics_inventory_raw.xlsx`.
   - Comprehensive component catalog with standardized standard unit costs in INR (₹).
   - Supplier master directory covering 50 vendors across Indian industrial hubs.
   - Monthly periodic inventory snapshots covering 12 observation dates in 2025.
   - Purchase order headers tracking fulfillment dates and statuses (Delivered, Pending, Cancelled).
   - Purchase order line-item details tracking quantities ordered and actual purchase costs.

2. **Power Query ETL Pipeline**:
   - Automated ingestion of all 6 Excel worksheets.
   - Explicit data type casting (Text, Whole Number, Decimal Number, Date, Boolean).
   - Validation of primary key uniqueness and foreign key integrity.
   - Controlled handling of nullable values (preserving `null` for unfulfilled delivery dates).
   - String trimming and text case standardization.

3. **Power BI Data Modelling**:
   - Single-direction 1-to-many star/snowflake schema relationships.
   - Proper cardinality enforcement without ambiguous many-to-many relationships.
   - Conformed component dimension connecting both inventory and procurement facts.
   - Dedicated analytical DAX Date table for time intelligence.

4. **DAX KPI & Calculation Engine**:
   - Inventory holding valuation based on `QuantityOnHand × StandardUnitCost`.
   - Dynamic classification of stock health: Healthy, Low Stock, and Zero Stock.
   - Net committed procurement spend excluding cancelled orders.
   - Average delivery lead time in calendar days for delivered orders.
   - Supplier On-Time Delivery percentage (OTD %).
   - Purchase price variance analysis across vendors for identical components.

5. **Interactive Power BI Dashboards**:
   - Page 1: **Inventory Analytics Dashboard** (stock quantities, valuation, low/zero stock alerts, category distribution, monthly stock trajectories, rental eligibility).
   - Page 2: **Procurement Analytics Dashboard** (spend by vendor, monthly expenditure trends, lead-time distribution, on-time delivery rates, order status pipeline, component purchase frequency vs volume).
   - Interactive cross-filtering, slicers (Date, Category, Supplier, Status), and tooltips.

6. **Documentation & Deliverables**:
   - Comprehensive project overview, problem statement, business requirements, and phases.
   - Full data dictionary with column specifications and ER diagrams.
   - Technical Power Query transformation documentation and DAX measures catalog.
   - Data quality audit, analytical findings report, business recommendations, and limitations.

---

## 3. Out-of-Scope Capabilities
The following functional areas are explicitly excluded from the project:

| Excluded Functional Area | Rationale |
| :--- | :--- |
| **Transactional ERP System** | The project analyzes historical and periodic data; it does not process live purchase requisitions or execute vendor orders. |
| **Live Warehouse Scanning (RFID / Barcode)** | Physical inventory counting is represented via simulated monthly snapshot records, not live hardware telemetry. |
| **Real-time Inventory Ledger Updates** | Data reflects monthly end-of-period observations, not minute-by-minute perpetual inventory movements. |
| **Payment & Financial Settlement Processing** | The dataset contains purchase order commitment costs, not cash disbursements, bank reconciliations, or invoice payments. |
| **Automated Supplier Communication** | Does not generate EDI transmissions, automated vendor purchase orders, or email RFQs. |
| **Manufacturing Production Scheduling** | Does not schedule shop-floor machines, shift allocations, or work-in-progress (WIP) routings. |
| **Manufacturing Defect Analysis** | Component quality is categorized by recorded status (`New`, `Refurbished`); scrap rates and defect root-cause analyses are excluded. |
| **Machine-Learning Predictive Forecasting** | Focuses on descriptive and diagnostic analytics; predictive demand modeling is reserved for future enhancements. |
| **Live Database Servers & Query Engines** | Technology architecture is strictly restricted to Microsoft Excel, Power Query, and Power BI Desktop. |

---

## 4. Key Stakeholders & Target Users
- **Inventory Managers**: Needs automated oversight of stock availability, reorder thresholds, inventory valuation, and warehouse space distribution.
- **Procurement Managers**: Needs tracking of supplier spend commitments, vendor delivery lead times, on-time delivery compliance, and purchase price variances.
- **Operations & General Management**: Evaluates aggregate working capital allocation and operational supply chain risks.

---

## 5. Data & Technical Boundaries

| Boundary Dimension | Standard / Specification |
| :--- | :--- |
| **Data Nature** | 100% Synthetic operational records designed for simulation and demonstration |
| **Time Horizon** | Full operational calendar year **2025** (January 1, 2025 – December 31, 2025) |
| **Snapshot Frequency** | 12 Monthly end-of-month snapshots (`2025-01-31` through `2025-12-31`) |
| **Reporting Currency** | Indian Rupee (**INR - ₹**) |
| **Unit of Measurement** | **Piece** (standardized across all 105 components) |
| **Software Platform** | Microsoft Excel, Power Query, Microsoft Power BI Desktop (Local Tabular Engine) |
