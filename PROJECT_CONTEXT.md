# Master Project Context: Electronics Component Inventory & Procurement Analytics

## 1. Project Identity & Purpose
- **Project Name**: Electronics Component Inventory & Procurement Analytics
- **Project Type**: End-to-end Data Analytics & Business Intelligence Solution
- **Domain**: Electronics Component Distribution & Small-Scale Manufacturing
- **Primary Stakeholders**: Inventory Managers, Procurement Managers, Operations Leadership
- **Reporting Currency**: Indian Rupee (**INR - ₹**)
- **Unit of Measurement**: **Piece** (standardized across all 105 components)
- **Data Authenticity**: **100% Synthetic Operational Data** created for demonstration and educational analytics

---

## 2. Technology Stack & Operational Guardrails

> [!CRITICAL]
> **STRICT TECHNOLOGY RESTRICTION**:
> This project must use **ONLY** the following Microsoft Business Intelligence tools:
> - **Microsoft Excel**: Source data storage and management.
> - **Power Query**: Data cleaning, transformation, and ETL.
> - **Microsoft Power BI Desktop**: Data modeling, star/snowflake schema relationships, and visual reporting.
> - **DAX (Data Analysis Expressions)**: KPI calculations, analytical measures, and aggregations.
> 
> **External relational database servers, custom script pipelines, and third-party notebook environments are NOT part of this project.** The architecture is built exclusively on Microsoft Excel, Power Query, and Microsoft Power BI Desktop. Do not introduce external tools or describe Excel worksheets as database tables.

---

## 3. Business Scenario & Functional Modules

### Business Context
The simulated company distributes electronic parts and manufactures control units and IoT devices. The business manages thousands of individual part numbers across 8 functional categories:
1. `Passive Components` (Resistors, capacitors, power inductors)
2. `Diodes and LEDs` (Rectifier diodes, Zener diodes, signal diodes, indicator LEDs)
3. `Transistors and MOSFETs` (BJTs, N-channel/P-channel MOSFETs, Darlington pairs)
4. `Integrated Circuits` (Op-amps, timers, voltage regulators, logic ICs)
5. `Sensors` (Temperature, humidity, ultrasonic, PIR, gas, light sensors)
6. `Microcontrollers and Development Boards` (8-bit/32-bit MCUs, dev boards, evaluation kits)
7. `Connectors and Cables` (Terminal blocks, pin headers, USB receptacles, jumper wires)
8. `Electromechanical Components` (Relays, switches, buzzers, cooling fans)

### Operational Modules

#### Module A: Inventory Analytics (For Inventory Managers)
- Catalog breadth: 105 distinct parts.
- Physical stock on hand: Evaluated at specific monthly snapshot dates.
- Inventory holding valuation: Based on `QuantityOnHand × StandardUnitCost` in INR (₹).
- Early warning alerts: Low-stock parts (`0 < QuantityOnHand < ReorderLevel`) and Zero-stock stockouts (`QuantityOnHand = 0`).
- Macro trajectory: Monthly stock volume trends across 12 snapshot dates in 2025.
- Rental fleet: Identification of components marked as `RentEligible = TRUE`.

#### Module B: Procurement Analytics (For Procurement Managers)
- Purchasing volume: 100 total purchase orders issued across 2025.
- Net committed spend: Evaluated across non-cancelled purchase order lines (`QuantityOrdered × UnitPurchaseCost`).
- Order fulfillment status: Delivered (80), Pending (12), Cancelled (8).
- Supplier delivery performance: Average Delivery Lead Time (days) and On-Time Delivery Rate (OTD %) for delivered orders.
- Vendor price comparison: Analyzing actual unit purchase price variances against benchmark standard costs across suppliers.

---

## 4. Source Data Specifications (`02_Dataset/raw/electronics_inventory_raw.xlsx`)

The raw workbook contains six logical worksheets:
1. **`Categories`** (8 rows, 3 columns): `CategoryID` (PK), `CategoryName`, `CategoryDescription`.
2. **`Components`** (105 rows, 8 columns): `ComponentID` (PK), `ComponentName`, `CategoryID` (FK), `UnitOfMeasure`, `StandardUnitCost`, `ReorderLevel`, `ConditionStatus`, `RentEligible`.
3. **`Suppliers`** (50 rows, 4 columns): `SupplierID` (PK), `SupplierName`, `City`, `SupplierType`.
4. **`InventorySnapshots`** (1,260 rows, 5 columns): `SnapshotID` (PK), `ComponentID` (FK), `SnapshotDate`, `QuantityOnHand`, `StorageLocation`. Covers 105 components across 12 monthly dates in 2025 (`2025-01-31` to `2025-12-31`).
5. **`PurchaseOrders`** (100 rows, 6 columns): `PurchaseOrderID` (PK), `SupplierID` (FK), `OrderDate`, `ExpectedDeliveryDate`, `ActualDeliveryDate`, `OrderStatus`. (80 Delivered, 12 Pending, 8 Cancelled).
6. **`PurchaseOrderDetails`** (125 rows, 5 columns): `POLineID` (PK), `PurchaseOrderID` (FK), `ComponentID` (FK), `QuantityOrdered`, `UnitPurchaseCost`.

---

## 5. Power BI Data Model Architecture

The data model features five core physical relationships with single-direction filter propagation ($1 \rightarrow N$):
- `Categories[CategoryID]` (1) $\rightarrow$ `Components[CategoryID]` (N)
- `Components[ComponentID]` (1) $\rightarrow$ `InventorySnapshots[ComponentID]` (N)
- `Components[ComponentID]` (1) $\rightarrow$ `PurchaseOrderDetails[ComponentID]` (N)
- `Suppliers[SupplierID]` (1) $\rightarrow$ `PurchaseOrders[SupplierID]` (N)
- `PurchaseOrders[PurchaseOrderID]` (1) $\rightarrow$ `PurchaseOrderDetails[PurchaseOrderID]` (N)

An analytical DAX **`Date`** table provides time intelligence, calendar hierarchies, and monthly trend slicing.

---

## 6. Critical Business Rules & Modeling Guidelines

1. **Snapshot Aggregation Context**:
   - `InventorySnapshots` contains periodic observations. Do **not** sum all 12 snapshot periods together to calculate current stock. Current inventory measures must always evaluate a selected or latest snapshot date.
2. **Cancelled Order Exclusion**:
   - `PurchaseOrders[OrderStatus] = "Cancelled"` orders must be excluded from procurement spend measures. Pending orders remain included because they are active uncancelled commitments.
3. **Lead Time & OTD Evaluation**:
   - Average delivery lead time (`ActualDeliveryDate - OrderDate`) and On-Time Delivery Rate (`ActualDeliveryDate <= ExpectedDeliveryDate`) must strictly filter for `OrderStatus = "Delivered"` and non-blank actual delivery dates.
4. **Cost Terminology Distinction**:
   - `StandardUnitCost` (in `Components`): Constant baseline reference cost used for inventory valuation.
   - `UnitPurchaseCost` (in `PurchaseOrderDetails`): Actual price negotiated on that specific purchase order.
5. **Rental Eligibility vs Current Rental**:
   - `RentEligible` is a catalog flag indicating whether a part model can be rented; it does not indicate current rental status.

---

## 7. Repository Layout

```text
Electronics-Component-Analytics/
│
├── README.md                            # Public-facing master project overview
├── PROJECT_CONTEXT.md                   # Developer & AI operational context
├── .gitignore                           # Repository ignore rules
│
├── 01_Project_Planning/                 # Scope, problem statement, requirements & phases
├── 02_Dataset/                          # Raw Excel workbook & data dictionary
├── 03_Power_Query/                      # ETL transformation documentation
├── 04_Power_BI/                         # PBIX dashboard, DAX catalog & model documentation
├── 05_Documentation/                    # Quality audit, analytical findings & recommendations
├── 06_Presentation/                     # Stakeholder presentation slide decks
└── 07_Screenshots/                      # Dashboard and data model captures
```
