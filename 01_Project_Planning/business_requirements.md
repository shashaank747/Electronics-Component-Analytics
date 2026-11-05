# Business Requirements Specification

## 1. Overview
This document specifies the functional business requirements, analytical logic, source data mapping, and calculation formulas for the **Electronics Component Inventory & Procurement Analytics** solution.

The requirements are divided into two dedicated modules:
- **Module A: Inventory Analytics** (designed for Inventory Managers)
- **Module B: Procurement Analytics** (designed for Procurement Managers)

All calculations are evaluated in **Power BI** using **DAX**, ingesting transformed datasets from **Microsoft Excel** via **Power Query**.

---

## 2. Module A: Inventory Analytics Requirements

### Requirement ID: `BR-INV-01`
- **Business Question**: How many distinct electronic components are maintained in the component catalogue?
- **Business Purpose**: Understand total catalog breadth and inventory SKU complexity under active management.
- **Source Worksheet**: `Components`
- **Required Columns**: `Components[ComponentID]`
- **Calculation / Logic**: Distinct count of component IDs: `DISTINCTCOUNT( Components[ComponentID] )`.
- **Expected Output**: Whole number (Expected dataset reference: 105 components).
- **Relevant Dashboard Module**: Inventory Analytics — Summary KPI Card.

---

### Requirement ID: `BR-INV-02`
- **Business Question**: What is the recorded stock quantity across the components for the selected inventory snapshot?
- **Business Purpose**: Track total physical pieces on hand at any specific snapshot date to monitor warehouse physical capacity.
- **Source Worksheet**: `InventorySnapshots`
- **Required Columns**: `InventorySnapshots[QuantityOnHand]`, `InventorySnapshots[SnapshotDate]`
- **Calculation / Logic**: Sum of quantity on hand under the active snapshot filter: `SUM( InventorySnapshots[QuantityOnHand] )`.
- **Expected Output**: Whole number formatted with commas (in pieces).
- **Relevant Dashboard Module**: Inventory Analytics — Summary KPI Card & Snapshot Filter.

---

### Requirement ID: `BR-INV-03`
- **Business Question**: Which electronic component categories contain the highest stock quantities?
- **Business Purpose**: Identify inventory volume concentration across the 8 component categories to guide bin allocation and warehouse floor planning.
- **Source Worksheets**: `Categories`, `Components`, `InventorySnapshots`
- **Required Columns**: `Categories[CategoryName]`, `Components[CategoryID]`, `InventorySnapshots[QuantityOnHand]`
- **Calculation / Logic**: Sum of `QuantityOnHand` grouped by `CategoryName` via active Power BI relationships.
- **Expected Output**: Ranked category breakdown / Clustered bar chart showing pieces per category.
- **Relevant Dashboard Module**: Inventory Analytics — Category Volume Distribution Chart.

---

### Requirement ID: `BR-INV-04`
- **Business Question**: Which components have a stock quantity below their respective reorder levels?
- **Business Purpose**: Provide early-warning replenishment alerts before components reach stockout, preventing assembly line interruptions.
- **Source Worksheets**: `Components`, `InventorySnapshots`
- **Required Columns**: `InventorySnapshots[QuantityOnHand]`, `Components[ReorderLevel]`, `Components[ComponentID]`
- **Calculation / Logic**: Count of components meeting: `QuantityOnHand > 0 && QuantityOnHand < RELATED( Components[ReorderLevel] )`.
- **Expected Output**: Count of critical components and detailed table showing component name, current stock, reorder level, and deficit.
- **Relevant Dashboard Module**: Inventory Analytics — Low Stock Warning KPI Card & Reorder Alert Table.

---

### Requirement ID: `BR-INV-05`
- **Business Question**: Which components have zero recorded stock?
- **Business Purpose**: Flag critical stockouts that represent immediate operational risks requiring emergency expediting.
- **Source Worksheet**: `InventorySnapshots`, `Components`
- **Required Columns**: `InventorySnapshots[QuantityOnHand]`, `InventorySnapshots[ComponentID]`, `Components[ComponentName]`
- **Calculation / Logic**: Count of components meeting: `QuantityOnHand = 0`.
- **Expected Output**: Whole number count and highlighted alert list of stockout parts.
- **Relevant Dashboard Module**: Inventory Analytics — Zero Stock Alert KPI Card & Out-of-Stock Table.

---

### Requirement ID: `BR-INV-06`
- **Business Question**: What is the estimated monetary value of the inventory?
- **Business Purpose**: Measure capital investment tied up in physical inventory for balance sheet reporting and insurance assessment.
- **Source Worksheets**: `InventorySnapshots`, `Components`
- **Required Columns**: `InventorySnapshots[QuantityOnHand]`, `Components[StandardUnitCost]`
- **Calculation / Logic**: Sum product of quantity on hand and standard benchmark unit cost:  
  $$\text{Inventory Value} = \sum (\text{QuantityOnHand} \times \text{StandardUnitCost})$$
- **Expected Output**: Monetary value formatted in Indian Rupees (**₹ #,##0.00**).
- **Relevant Dashboard Module**: Inventory Analytics — Total Valuation KPI Card.

---

### Requirement ID: `BR-INV-07`
- **Business Question**: Which component categories account for the highest inventory value?
- **Business Purpose**: Determine capital allocation across categories and pinpoint where working capital is heavily concentrated.
- **Source Worksheets**: `Categories`, `Components`, `InventorySnapshots`
- **Required Columns**: `Categories[CategoryName]`, `Components[StandardUnitCost]`, `InventorySnapshots[QuantityOnHand]`
- **Calculation / Logic**: Category-level aggregation of `QuantityOnHand × StandardUnitCost`.
- **Expected Output**: Value distribution chart and percentage share by category.
- **Relevant Dashboard Module**: Inventory Analytics — Category Valuation Breakdown.

---

### Requirement ID: `BR-INV-08`
- **Business Question**: How do recorded stock quantities change across the monthly inventory snapshots?
- **Business Purpose**: Observe macro inventory build-up, depletion, or seasonal stability across the operational year.
- **Source Worksheet**: `InventorySnapshots`
- **Required Columns**: `InventorySnapshots[SnapshotDate]`, `InventorySnapshots[QuantityOnHand]`
- **Calculation / Logic**: Trend line showing total `QuantityOnHand` plotted against `SnapshotDate` (12 monthly dates in 2025).
- **Expected Output**: Monthly area/line visual showing stock progression.
- **Important Note**: Represents monthly periodic snapshot observations, not transaction-level stock consumption or receipts.
- **Relevant Dashboard Module**: Inventory Analytics — Monthly Stock Trajectory Visual.

---

### Requirement ID: `BR-INV-09`
- **Business Question**: How many components are marked as eligible for rental programmes?
- **Business Purpose**: Identify assets that can be monetized through commercial equipment rental to labs and universities.
- **Source Worksheet**: `Components`
- **Required Columns**: `Components[RentEligible]`, `Components[ComponentID]`
- **Calculation / Logic**: Distinct count of components where `RentEligible = TRUE`.
- **Expected Output**: Whole number count of rental-eligible components.
- **Important Note**: `RentEligible` indicates whether the component type is eligible for rental, not whether it is currently rented.
- **Relevant Dashboard Module**: Inventory Analytics — Rental Fleet KPI Card & Catalog Filter.

---

## 3. Module B: Procurement Analytics Requirements

### Requirement ID: `BR-PROC-01`
- **Business Question**: How many purchase orders were issued during the operational year?
- **Business Purpose**: Measure total purchasing operational volume and workload handled by the procurement department.
- **Source Worksheet**: `PurchaseOrders`
- **Required Columns**: `PurchaseOrders[PurchaseOrderID]`
- **Calculation / Logic**: Distinct count of purchase order IDs: `DISTINCTCOUNT( PurchaseOrders[PurchaseOrderID] )`.
- **Expected Output**: Whole number (Expected dataset reference: 100 purchase orders).
- **Relevant Dashboard Module**: Procurement Analytics — Total POs KPI Card.

---

### Requirement ID: `BR-PROC-02`
- **Business Question**: What is the total value of non-cancelled purchase orders?
- **Business Purpose**: Quantify net committed procurement expenditure for financial planning and supplier budget tracking.
- **Source Worksheets**: `PurchaseOrders`, `PurchaseOrderDetails`
- **Required Columns**: `PurchaseOrderDetails[QuantityOrdered]`, `PurchaseOrderDetails[UnitPurchaseCost]`, `PurchaseOrders[OrderStatus]`
- **Calculation / Logic**: Sum product of ordered quantity and unit purchase cost, filtering out cancelled orders:  
  $$\text{Procurement Value} = \sum (\text{QuantityOrdered} \times \text{UnitPurchaseCost}) \quad [\text{for } \text{OrderStatus} \ne \text{'Cancelled'}]$$
- **Expected Output**: Currency value formatted in Indian Rupees (**₹ #,##0.00**).
- **Important Note**: Reflects purchase order commitment value, not verified cash disbursements or invoice settlement payments. Pending orders are included; cancelled orders are excluded.
- **Relevant Dashboard Module**: Procurement Analytics — Net Procurement Spend KPI Card.

---

### Requirement ID: `BR-PROC-03`
- **Business Question**: Which suppliers account for the highest purchasing value?
- **Business Purpose**: Identify key supplier partnerships, vendor dependency, and potential concentration risks.
- **Source Worksheets**: `Suppliers`, `PurchaseOrders`, `PurchaseOrderDetails`
- **Required Columns**: `Suppliers[SupplierName]`, `PurchaseOrders[SupplierID]`, `PurchaseOrderDetails[LineTotal]`
- **Calculation / Logic**: Sum of non-cancelled procurement value grouped by `SupplierName`, ranked descending.
- **Expected Output**: Horizontal bar chart / ranked matrix displaying top suppliers by spend.
- **Relevant Dashboard Module**: Procurement Analytics — Top Suppliers by Spend Visual.

---

### Requirement ID: `BR-PROC-04`
- **Business Question**: Which electronic components appear most frequently in purchase order line items or have the highest ordered quantities?
- **Business Purpose**: Distinguish components ordered frequently in small batches from those purchased in large bulk volumes.
- **Source Worksheets**: `Components`, `PurchaseOrderDetails`
- **Required Columns**: `Components[ComponentName]`, `PurchaseOrderDetails[POLineID]`, `PurchaseOrderDetails[QuantityOrdered]`
- **Calculation / Logic**:
  - *Order Frequency*: Count of line items per component (`COUNTROWS( PurchaseOrderDetails )`).
  - *Total Volume*: Sum of ordered pieces (`SUM( PurchaseOrderDetails[QuantityOrdered] )`).
- **Expected Output**: Two-column matrix or scatter visual distinguishing frequency vs volume.
- **Relevant Dashboard Module**: Procurement Analytics — Most Purchased Components Table.

---

### Requirement ID: `BR-PROC-05`
- **Business Question**: How does procurement value change across months?
- **Business Purpose**: Analyze monthly procurement spending patterns, seasonality, and quarterly purchasing cycles.
- **Source Worksheets**: `PurchaseOrders`, `PurchaseOrderDetails`
- **Required Columns**: `PurchaseOrders[OrderDate]`, `PurchaseOrderDetails[QuantityOrdered]`, `PurchaseOrderDetails[UnitPurchaseCost]`
- **Calculation / Logic**: Sum of non-cancelled line totals plotted chronologically by month of `OrderDate`.
- **Expected Output**: Monthly column chart displaying monthly procurement spend.
- **Relevant Dashboard Module**: Procurement Analytics — Monthly Spend Trend Visual.

---

### Requirement ID: `BR-PROC-06`
- **Business Question**: What proportion of purchase orders are Delivered, Pending, or Cancelled?
- **Business Purpose**: Monitor fulfillment pipeline health and identify operational disruption rates.
- **Source Worksheet**: `PurchaseOrders`
- **Required Columns**: `PurchaseOrders[OrderStatus]`, `PurchaseOrders[PurchaseOrderID]`
- **Calculation / Logic**: Order count and percentage share grouped by `OrderStatus`:
  - `Delivered`: 80 orders (80.0%)
  - `Pending`: 12 orders (12.0%)
  - `Cancelled`: 8 orders (8.0%)
- **Expected Output**: Donut chart and breakdown table of order statuses.
- **Relevant Dashboard Module**: Procurement Analytics — Order Pipeline Donut Visual.

---

### Requirement ID: `BR-PROC-07`
- **Business Question**: What is the average delivery lead time across suppliers?
- **Business Purpose**: Benchmark supplier turnaround times to set accurate production schedules and reorder lead-time buffers.
- **Source Worksheet**: `PurchaseOrders`
- **Required Columns**: `PurchaseOrders[OrderDate]`, `PurchaseOrders[ActualDeliveryDate]`, `PurchaseOrders[OrderStatus]`
- **Calculation / Logic**: Average calendar days between `OrderDate` and `ActualDeliveryDate`, evaluated exclusively for delivered orders:  
  $$\text{Delivery Lead Time} = \text{DATEDIFF}(\text{OrderDate}, \text{ActualDeliveryDate}, \text{DAY}) \quad [\text{for Delivered orders}]$$
- **Expected Output**: Decimal value in days (e.g., 11.4 Days) and distribution by supplier.
- **Relevant Dashboard Module**: Procurement Analytics — Average Lead Time KPI Card & Vendor Lead Time Visual.

---

### Requirement ID: `BR-PROC-08`
- **Business Question**: What percentage of delivered purchase orders arrived on or before schedule?
- **Business Purpose**: Evaluate supplier delivery reliability and compliance with committed delivery deadlines.
- **Source Worksheet**: `PurchaseOrders`
- **Required Columns**: `PurchaseOrders[ExpectedDeliveryDate]`, `PurchaseOrders[ActualDeliveryDate]`, `PurchaseOrders[OrderStatus]`
- **Calculation / Logic**:  
  $$\text{On-Time Delivery Rate (OTD \%)} = \frac{\text{Count of Delivered Orders where } \text{ActualDeliveryDate} \le \text{ExpectedDeliveryDate}}{\text{Total Delivered Orders}} \times 100$$
- **Expected Output**: Percentage KPI (e.g., 76.25% OTD). Only delivered orders are included in the denominator.
- **Relevant Dashboard Module**: Procurement Analytics — On-Time Delivery Rate KPI Gauge & Card.

---

### Requirement ID: `BR-PROC-09`
- **Business Question**: How do purchase prices for the same component vary across suppliers?
- **Business Purpose**: Identify price discrepancies, benchmark vendor competitiveness, and negotiate uniform pricing.
- **Source Worksheets**: `Suppliers`, `PurchaseOrders`, `PurchaseOrderDetails`, `Components`
- **Required Columns**: `Components[ComponentName]`, `Suppliers[SupplierName]`, `PurchaseOrderDetails[UnitPurchaseCost]`
- **Calculation / Logic**: Compare `UnitPurchaseCost` across suppliers for identical `ComponentID` values.  
  *Note*: Do not confuse `StandardUnitCost` (catalog valuation reference) with `UnitPurchaseCost` (actual recorded price paid).
- **Expected Output**: Comparison matrix showing minimum, maximum, and average unit purchase cost by component and vendor.
- **Relevant Dashboard Module**: Procurement Analytics — Supplier Price Variance Matrix.

---

## 4. Requirements Summary Matrix

| Req ID | Module | Key Measure | Source Table(s) | Target KPI Format |
| :--- | :--- | :--- | :--- | :--- |
| `BR-INV-01` | Inventory | Total Components | `Components` | 105 Parts |
| `BR-INV-02` | Inventory | Total Stock Quantity | `InventorySnapshots` | Whole Number (Pieces) |
| `BR-INV-03` | Inventory | Stock by Category | `Categories`, `Components`, `Snapshots` | Bar Visual (Pieces) |
| `BR-INV-04` | Inventory | Low Stock Alerts | `Components`, `InventorySnapshots` | Count & Deficit Table |
| `BR-INV-05` | Inventory | Zero Stock Alerts | `InventorySnapshots` | Count & Out-of-Stock Table |
| `BR-INV-06` | Inventory | Inventory Valuation | `Snapshots`, `Components` | Currency (₹) |
| `BR-INV-07` | Inventory | Category Valuation | `Categories`, `Components`, `Snapshots` | Value & % Share |
| `BR-INV-08` | Inventory | Monthly Stock Trend | `InventorySnapshots` | Area Line Chart |
| `BR-INV-09` | Inventory | Rental Eligibility | `Components` | Count (Eligible Parts) |
| `BR-PROC-01` | Procurement | Total Purchase Orders | `PurchaseOrders` | 100 Orders |
| `BR-PROC-02` | Procurement | Net Procurement Spend | `PurchaseOrders`, `PurchaseOrderDetails` | Currency (₹) |
| `BR-PROC-03` | Procurement | Supplier Spend | `Suppliers`, `PurchaseOrders`, `Details` | Ranked Bar Chart |
| `BR-PROC-04` | Procurement | Frequent Components | `Components`, `PurchaseOrderDetails` | Frequency vs Volume Table |
| `BR-PROC-05` | Procurement | Monthly Spend Trend | `PurchaseOrders`, `PurchaseOrderDetails` | Column Chart (Monthly ₹) |
| `BR-PROC-06` | Procurement | Order Status Pipeline | `PurchaseOrders` | Donut Chart (80/12/8) |
| `BR-PROC-07` | Procurement | Average Lead Time | `PurchaseOrders` | Days (Delivered Only) |
| `BR-PROC-08` | Procurement | On-Time Delivery Rate | `PurchaseOrders` | Percentage (OTD %) |
| `BR-PROC-09` | Procurement | Price Variance | `Suppliers`, `Details`, `Components` | Price Range Matrix (₹) |
