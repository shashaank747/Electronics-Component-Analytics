# DAX Measures Catalog & Technical Documentation

This document catalogs and explains the DAX (Data Analysis Expressions) measures implemented in the **Power BI Data Model** for the **Electronics Component Inventory & Procurement Analytics** dashboard.

For every measure, this catalog documents its business purpose, DAX formula, mathematical explanation, underlying tables and columns, active filter contexts, and visual dashboard usage.

---

## 1. Inventory Analytics Measures

### Measure 1: `Total Distinct Components`
- **Business Purpose**: Counts the total number of unique electronic component part numbers maintained in the master catalog.
- **DAX Formula**:
  ```dax
  Total Distinct Components = 
  DISTINCTCOUNT( Components[ComponentID] )
  ```
- **Explanation**: Evaluates the distinct count of unique identifier strings in the `Components` table.
- **Tables & Columns Used**: `Components[ComponentID]`
- **Filters / Conditions**: None.
- **Dashboard Usage**: Inventory Analytics page — Headline Summary KPI Card.

---

### Measure 2: `Total Stock Quantity`
- **Business Purpose**: Calculates the aggregate physical stock on hand across all active filters for the selected snapshot date.
- **DAX Formula**:
  ```dax
  Total Stock Quantity = 
  SUM( InventorySnapshots[QuantityOnHand] )
  ```
- **Explanation**: Aggregates the physical units counted in `InventorySnapshots`. Respects the active snapshot date filter.
- **Tables & Columns Used**: `InventorySnapshots[QuantityOnHand]`
- **Filters / Conditions**: Must be evaluated in the context of a selected `SnapshotDate`.
- **Dashboard Usage**: Inventory Analytics page — KPI Card and Category Bar Chart values.

---

### Measure 3: `Inventory Valuation`
- **Business Purpose**: Calculates the total monetary valuation of physical inventory in Indian Rupee (INR - ₹) based on benchmark standard unit costs.
- **DAX Formula**:
  ```dax
  Inventory Valuation = 
  SUMX(
      InventorySnapshots,
      InventorySnapshots[QuantityOnHand] * RELATED( Components[StandardUnitCost] )
  )
  ```
- **Explanation**: Iterates row-by-row through `InventorySnapshots`, multiplies the recorded quantity on hand by the corresponding part's `StandardUnitCost` pulled from the dimension table via the `1 : N` relationship, and aggregates the sum product.
- **Tables & Columns Used**: `InventorySnapshots[QuantityOnHand]`, `Components[StandardUnitCost]`
- **Filters / Conditions**: Respects active snapshot date and category slicers. Formatted as currency (`₹ #,##0.00`).
- **Dashboard Usage**: Inventory Analytics page — Primary Capital Valuation KPI Card and Category Value Breakdown.

---

### Measure 4: `Zero Stock Components`
- **Business Purpose**: Identifies and counts components that currently have zero physical stock on hand, flagging critical stockouts.
- **DAX Formula**:
  ```dax
  Zero Stock Components = 
  CALCULATE(
      DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
      InventorySnapshots[QuantityOnHand] = 0
  )
  ```
- **Explanation**: Modifies filter context using `CALCULATE` to count distinct parts where `QuantityOnHand` is strictly equal to zero.
- **Tables & Columns Used**: `InventorySnapshots[ComponentID]`, `InventorySnapshots[QuantityOnHand]`
- **Filters / Conditions**: `InventorySnapshots[QuantityOnHand] = 0`
- **Dashboard Usage**: Inventory Analytics page — Red Alert KPI Card and Out-of-Stock Table Filter.

---

### Measure 5: `Low Stock Components`
- **Business Purpose**: Flags components whose physical stock has dipped below their safety reorder threshold but is still greater than zero.
- **DAX Formula**:
  ```dax
  Low Stock Components = 
  CALCULATE(
      DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
      FILTER(
          InventorySnapshots,
          InventorySnapshots[QuantityOnHand] > 0 &&
          InventorySnapshots[QuantityOnHand] < RELATED( Components[ReorderLevel] )
      )
  )
  ```
- **Explanation**: Applies a table filter checking whether on-hand stock is strictly positive yet below the part's designated `ReorderLevel` from `Components`.
- **Tables & Columns Used**: `InventorySnapshots[ComponentID]`, `InventorySnapshots[QuantityOnHand]`, `Components[ReorderLevel]`
- **Filters / Conditions**: `QuantityOnHand > 0 && QuantityOnHand < ReorderLevel`
- **Dashboard Usage**: Inventory Analytics page — Early Warning KPI Card and Replenishment Alert Matrix.

---

### Measure 6: `Healthy Stock Components`
- **Business Purpose**: Counts components whose stock level meets or exceeds the designated safety reorder threshold.
- **DAX Formula**:
  ```dax
  Healthy Stock Components = 
  CALCULATE(
      DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
      FILTER(
          InventorySnapshots,
          InventorySnapshots[QuantityOnHand] >= RELATED( Components[ReorderLevel] )
      )
  )
  ```
- **Explanation**: Counts distinct parts where `QuantityOnHand` is greater than or equal to `ReorderLevel`.
- **Tables & Columns Used**: `InventorySnapshots[ComponentID]`, `InventorySnapshots[QuantityOnHand]`, `Components[ReorderLevel]`
- **Filters / Conditions**: `QuantityOnHand >= ReorderLevel`
- **Dashboard Usage**: Inventory Analytics page — Stock Health Gauge visual and status donut chart.

---

### Measure 7: `Rental Eligible Components`
- **Business Purpose**: Counts the total number of catalog parts that are eligible for commercial rental programs.
- **DAX Formula**:
  ```dax
  Rental Eligible Components = 
  CALCULATE(
      DISTINCTCOUNT( Components[ComponentID] ),
      Components[RentEligible] = TRUE()
  )
  ```
- **Explanation**: Counts distinct components in the master catalog where the boolean flag `RentEligible` is true.
- **Tables & Columns Used**: `Components[ComponentID]`, `Components[RentEligible]`
- **Filters / Conditions**: `Components[RentEligible] = TRUE()`
- **Dashboard Usage**: Inventory Analytics page — Rental Fleet Size KPI Card.

---

## 2. Procurement Analytics Measures

### Measure 8: `Total Purchase Orders`
- **Business Purpose**: Counts the total volume of purchase orders issued across the operational period.
- **DAX Formula**:
  ```dax
  Total Purchase Orders = 
  DISTINCTCOUNT( PurchaseOrders[PurchaseOrderID] )
  ```
- **Explanation**: Evaluates the distinct count of purchase order header IDs.
- **Tables & Columns Used**: `PurchaseOrders[PurchaseOrderID]`
- **Filters / Conditions**: None (Expected reference: 100 POs).
- **Dashboard Usage**: Procurement Analytics page — Total Orders Headline KPI Card.

---

### Measure 9: `Delivered Orders`
- **Business Purpose**: Counts purchase orders that have been successfully fulfilled and received.
- **DAX Formula**:
  ```dax
  Delivered Orders = 
  CALCULATE(
      COUNTROWS( PurchaseOrders ),
      PurchaseOrders[OrderStatus] = "Delivered"
  )
  ```
- **Explanation**: Filters the `PurchaseOrders` header table for rows where `OrderStatus` equals "Delivered".
- **Tables & Columns Used**: `PurchaseOrders[OrderStatus]`
- **Filters / Conditions**: `PurchaseOrders[OrderStatus] = "Delivered"`
- **Dashboard Usage**: Procurement Analytics page — Fulfillment Pipeline visual and OTD denominator.

---

### Measure 10: `Pending Orders`
- **Business Purpose**: Counts active purchase orders currently in transit or awaiting delivery.
- **DAX Formula**:
  ```dax
  Pending Orders = 
  CALCULATE(
      COUNTROWS( PurchaseOrders ),
      PurchaseOrders[OrderStatus] = "Pending"
  )
  ```
- **Explanation**: Filters `PurchaseOrders` for rows where `OrderStatus` equals "Pending".
- **Tables & Columns Used**: `PurchaseOrders[OrderStatus]`
- **Filters / Conditions**: `PurchaseOrders[OrderStatus] = "Pending"`
- **Dashboard Usage**: Procurement Analytics page — In-Transit Orders Card and Status Donut.

---

### Measure 11: `Cancelled Orders`
- **Business Purpose**: Counts voided or cancelled purchase orders.
- **DAX Formula**:
  ```dax
  Cancelled Orders = 
  CALCULATE(
      COUNTROWS( PurchaseOrders ),
      PurchaseOrders[OrderStatus] = "Cancelled"
  )
  ```
- **Explanation**: Filters `PurchaseOrders` for rows where `OrderStatus` equals "Cancelled".
- **Tables & Columns Used**: `PurchaseOrders[OrderStatus]`
- **Filters / Conditions**: `PurchaseOrders[OrderStatus] = "Cancelled"`
- **Dashboard Usage**: Procurement Analytics page — Order Pipeline visual.

---

### Measure 12: `Total Procurement Spend`
- **Business Purpose**: Calculates the net committed spend in INR (₹) across all non-cancelled purchase order lines.
- **DAX Formula**:
  ```dax
  Total Procurement Spend = 
  CALCULATE(
      SUMX(
          PurchaseOrderDetails,
          PurchaseOrderDetails[QuantityOrdered] * PurchaseOrderDetails[UnitPurchaseCost]
      ),
      PurchaseOrders[OrderStatus] <> "Cancelled"
  )
  ```
- **Explanation**: Iterates through line items in `PurchaseOrderDetails`, multiplies ordered quantity by unit purchase price, and excludes lines belonging to cancelled purchase orders via relationship filter traversal. Formatted as currency (`₹ #,##0.00`).
- **Tables & Columns Used**: `PurchaseOrderDetails[QuantityOrdered]`, `PurchaseOrderDetails[UnitPurchaseCost]`, `PurchaseOrders[OrderStatus]`
- **Filters / Conditions**: `PurchaseOrders[OrderStatus] <> "Cancelled"` (includes Delivered and Pending orders).
- **Dashboard Usage**: Procurement Analytics page — Primary Committed Spend KPI Card and Vendor Spend visual.

---

### Measure 13: `Average Delivery Lead Time (Days)`
- **Business Purpose**: Measures the average turnaround time in calendar days between order placement and actual physical receipt for delivered orders.
- **DAX Formula**:
  ```dax
  Average Delivery Lead Time (Days) = 
  CALCULATE(
      AVERAGEX(
          PurchaseOrders,
          DATEDIFF( PurchaseOrders[OrderDate], PurchaseOrders[ActualDeliveryDate], DAY )
      ),
      PurchaseOrders[OrderStatus] = "Delivered" &&
      NOT ISBLANK( PurchaseOrders[ActualDeliveryDate] )
  )
  ```
- **Explanation**: Computes day difference using `DATEDIFF` between order date and actual delivery date for delivered orders with non-blank actual delivery dates, and averages the results.
- **Tables & Columns Used**: `PurchaseOrders[OrderDate]`, `PurchaseOrders[ActualDeliveryDate]`, `PurchaseOrders[OrderStatus]`
- **Filters / Conditions**: `OrderStatus = "Delivered"` and `NOT ISBLANK(ActualDeliveryDate)`
- **Dashboard Usage**: Procurement Analytics page — Lead Time KPI Card and Vendor Scorecard Scatter Visual.

---

### Measure 14: `On-Time Delivered Orders`
- **Business Purpose**: Counts delivered orders that arrived on or before the committed expected delivery date.
- **DAX Formula**:
  ```dax
  On-Time Delivered Orders = 
  CALCULATE(
      COUNTROWS( PurchaseOrders ),
      PurchaseOrders[OrderStatus] = "Delivered" &&
      PurchaseOrders[ActualDeliveryDate] <= PurchaseOrders[ExpectedDeliveryDate] &&
      NOT ISBLANK( PurchaseOrders[ActualDeliveryDate] )
  )
  ```
- **Explanation**: Filters delivered orders where `ActualDeliveryDate` is less than or equal to `ExpectedDeliveryDate`.
- **Tables & Columns Used**: `PurchaseOrders[OrderStatus]`, `PurchaseOrders[ActualDeliveryDate]`, `PurchaseOrders[ExpectedDeliveryDate]`
- **Filters / Conditions**: Delivered status, actual date $\le$ expected date, non-blank actual date.
- **Dashboard Usage**: Used internally as the numerator for On-Time Delivery Rate (OTD %).

---

### Measure 15: `On-Time Delivery Rate (OTD %)`
- **Business Purpose**: Evaluates supplier delivery reliability as the percentage of fulfilled deliveries that arrived on or before schedule.
- **DAX Formula**:
  ```dax
  On-Time Delivery Rate (OTD %) = 
  DIVIDE(
      [On-Time Delivered Orders],
      [Delivered Orders],
      0
  )
  ```
- **Explanation**: Safely divides on-time delivered order count by total delivered orders using `DIVIDE` to avoid divide-by-zero errors. Formatted as percentage (`0.0%`).
- **Tables & Columns Used**: Measures `[On-Time Delivered Orders]`, `[Delivered Orders]`
- **Filters / Conditions**: Only delivered orders are included in the denominator.
- **Dashboard Usage**: Procurement Analytics page — Primary Supplier Reliability Gauge and Vendor Performance Table.

---

## 3. Recommended Future Measures (Documented for Enhancement)

The following measures are documented as recommended analytical extensions:
1. **`Stockout Rate %`**:
   `DIVIDE( [Zero Stock Components], [Total Distinct Components], 0 )`
2. **`Inventory Turnover Ratio`**:
   `DIVIDE( [Total Procurement Spend], [Inventory Valuation], 0 )`
3. **`Purchase Price Variance (₹)`**:
   `SUMX( PurchaseOrderDetails, (PurchaseOrderDetails[UnitPurchaseCost] - RELATED(Components[StandardUnitCost])) * PurchaseOrderDetails[QuantityOrdered] )`
