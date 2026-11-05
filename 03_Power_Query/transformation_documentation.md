# Power Query Transformation & ETL Documentation

## Overview
This document specifies the Data Extraction, Transformation, and Loading (ETL) pipeline implemented in **Power Query** to ingest the raw Microsoft Excel workbook (`electronics_inventory_raw.xlsx`) into **Microsoft Power BI**.

The ETL process enforces strict type validation, handles nullable delivery dates, trims whitespace, and ensures referential integrity across all dimension and fact tables.

---

## Source Connection
- **Source File**: `02_Dataset/raw/electronics_inventory_raw.xlsx`
- **Connector**: `Excel.Workbook(File.Contents(".../electronics_inventory_raw.xlsx"), null, true)`
- **Loading Strategy**: Each worksheet is loaded as an independent query representing a normalized table.

---

## Table-by-Table ETL Specifications

### 1. `Categories`
- **Source Sheet**: `Categories`
- **Transformation Steps**:
  1. Promote first row to headers.
  2. Trim leading/trailing whitespace on `CategoryName` and `CategoryDescription`.
  3. Enforce text types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"CategoryID", type text},
         {"CategoryName", type text},
         {"CategoryDescription", type text}
     })
     ```
  4. Verify uniqueness of `CategoryID` and `CategoryName`.

### 2. `Components`
- **Source Sheet**: `Components`
- **Transformation Steps**:
  1. Promote headers.
  2. Trim whitespace across text attributes.
  3. Enforce explicit column types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"ComponentID", type text},
         {"ComponentName", type text},
         {"CategoryID", type text},
         {"UnitOfMeasure", type text},
         {"StandardUnitCost", type number},
         {"ReorderLevel", Int64.Type},
         {"ConditionStatus", type text},
         {"RentEligible", type logical}
     })
     ```
  4. Validate `StandardUnitCost >= 0` and `ReorderLevel > 0`.
  5. Check referential integrity: every `CategoryID` exists in `Categories`.

### 3. `Suppliers`
- **Source Sheet**: `Suppliers`
- **Transformation Steps**:
  1. Promote headers.
  2. Standardize city names and supplier classifications.
  3. Enforce explicit column types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"SupplierID", type text},
         {"SupplierName", type text},
         {"City", type text},
         {"SupplierType", type text}
     })
     ```
  4. Verify `SupplierID` uniqueness (`SUP001` to `SUP050`).

### 4. `InventorySnapshots`
- **Source Sheet**: `InventorySnapshots`
- **Transformation Steps**:
  1. Promote headers.
  2. Convert `SnapshotDate` to `type date` using invariant ISO format (`yyyy-MM-dd`).
  3. Enforce explicit column types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"SnapshotID", type text},
         {"ComponentID", type text},
         {"SnapshotDate", type date},
         {"QuantityOnHand", Int64.Type},
         {"StorageLocation", type text}
     })
     ```
  4. Validate that `QuantityOnHand >= 0`.
  5. Confirm composite uniqueness on `(ComponentID, SnapshotDate)`.

### 5. `PurchaseOrders`
- **Source Sheet**: `PurchaseOrders`
- **Transformation Steps**:
  1. Promote headers.
  2. Convert date fields (`OrderDate`, `ExpectedDeliveryDate`, `ActualDeliveryDate`) to `type date`.
  3. Handle nullable values on `ActualDeliveryDate`:
     - Retain `null` for orders where `OrderStatus` is `Pending` or `Cancelled`.
     - Confirm that `Delivered` orders have a non-null `ActualDeliveryDate`.
  4. Enforce explicit column types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"PurchaseOrderID", type text},
         {"SupplierID", type text},
         {"OrderDate", type date},
         {"ExpectedDeliveryDate", type date},
         {"ActualDeliveryDate", type date},
         {"OrderStatus", type text}
     })
     ```

### 6. `PurchaseOrderDetails`
- **Source Sheet**: `PurchaseOrderDetails`
- **Transformation Steps**:
  1. Promote headers.
  2. Enforce explicit column types:
     ```powerquery
     Table.TransformColumnTypes(Source, {
         {"POLineID", type text},
         {"PurchaseOrderID", type text},
         {"ComponentID", type text},
         {"QuantityOrdered", Int64.Type},
         {"UnitPurchaseCost", type number}
     })
     ```
  3. Validate that `QuantityOrdered > 0` and `UnitPurchaseCost > 0`.
  4. Confirm referential integrity against `PurchaseOrders[PurchaseOrderID]` and `Components[ComponentID]`.

---

## Power Query Best Practices Applied
- **Query Renaming**: Clean, PascalCase table names matching data modeling standards.
- **Explicit Type Casting**: Avoid automatic 'Any' type assignments to preserve VertiPaq compression.
- **Step Documentation**: Clear, readable M-code step names (`Promoted Headers`, `Changed Type`, `Filtered Rows`).
- **Load Optimization**: Load only validated analytical tables into the data model.
