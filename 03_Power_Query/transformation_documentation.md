# Power Query Transformation & ETL Documentation

## 1. Overview & Architecture
This document details the Data Extraction, Transformation, and Loading (ETL) pipeline implemented in **Power Query** (the M-engine within Microsoft Power BI) to ingest, cleanse, validate, and shape the source data from the master Microsoft Excel workbook.

```text
Microsoft Excel Source Workbook (electronics_inventory_raw.xlsx)
       ↓
Power Query Ingestion (Excel.Workbook M-Function)
       ↓
Data Cleaning: Header Promotion, Trimming & Casing
       ↓
Explicit Type Casting & Date Format Coercion
       ↓
Null Handling & Referential Key Validation
       ↓
Power BI VertiPaq Tabular Ingestion (Star/Snowflake Model)
```

---

## 2. Source Workbook & Extraction Method
- **Source File**: `02_Dataset/raw/electronics_inventory_raw.xlsx`
- **Connector Function**:
  ```powerquery
  Source = Excel.Workbook(File.Contents("02_Dataset\raw\electronics_inventory_raw.xlsx"), null, true)
  ```
- **Ingestion Mode**: Each worksheet is accessed via its discrete `[Item="SheetName", Kind="Sheet"]` record, isolating the six tables into independent queries.

---

## 3. Table-by-Table Transformations (Implemented)

### Query 1: `Categories`
- **Source Sheet**: `Categories` (8 rows, 3 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted top row to column headers.
  2. *Text Normalization*: Applied `Text.Trim` to remove any accidental leading or trailing whitespace on `CategoryName` and `CategoryDescription`.
  3. *Type Enforcement*: Explicitly cast all columns to `type text`:
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
         {"CategoryID", type text},
         {"CategoryName", type text},
         {"CategoryDescription", type text}
     })
     ```
  4. *Uniqueness Verification*: Confirmed zero duplicates on `CategoryID`.

---

### Query 2: `Components`
- **Source Sheet**: `Components` (105 rows, 8 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted row 1 to column headers.
  2. *Text Trimming*: Trimmed `ComponentName`, `UnitOfMeasure`, and `ConditionStatus`.
  3. *Type Enforcement*: Explicitly cast fields to optimize Power BI VertiPaq compression:
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
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
  4. *Value Validation*: Verified that `StandardUnitCost` values are strictly positive decimals, `ReorderLevel` values are integers $> 0$, and `RentEligible` values are strictly boolean.

---

### Query 3: `Suppliers`
- **Source Sheet**: `Suppliers` (50 rows, 4 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted row 1 to headers.
  2. *City & Name Normalization*: Applied `Text.Trim` and standard casing across `SupplierName`, `City`, and `SupplierType`.
  3. *Type Enforcement*:
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
         {"SupplierID", type text},
         {"SupplierName", type text},
         {"City", type text},
         {"SupplierType", type text}
     })
     ```
  4. *Uniqueness Verification*: Confirmed 50 unique identifiers (`SUP001` to `SUP050`).

---

### Query 4: `InventorySnapshots`
- **Source Sheet**: `InventorySnapshots` (1,260 rows, 5 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted row 1 to headers.
  2. *Date Coercion*: Explicitly parsed `SnapshotDate` as `type date` using invariant ISO formatting (`yyyy-MM-dd`):
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
         {"SnapshotID", type text},
         {"ComponentID", type text},
         {"SnapshotDate", type date},
         {"QuantityOnHand", Int64.Type},
         {"StorageLocation", type text}
     })
     ```
  3. *Non-Negative Validation*: Verified that `QuantityOnHand` $\ge 0$.
  4. *Composite Uniqueness*: Verified that `ComponentID` and `SnapshotDate` form a unique composite pair per component per month.

---

### Query 5: `PurchaseOrders`
- **Source Sheet**: `PurchaseOrders` (100 rows, 6 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted row 1 to headers.
  2. *Multi-Date Parsing*: Converted `OrderDate`, `ExpectedDeliveryDate`, and `ActualDeliveryDate` to `type date`.
  3. *Controlled Null Handling*:
     - In `ActualDeliveryDate`, preserved legitimate `null` values for `Pending` (12 orders) and `Cancelled` (8 orders).
     - Verified that all 80 `Delivered` orders possess a valid date value.
     - Did **not** replace blanks with zero or arbitrary dates, preventing distortion of lead-time calculations.
  4. *Type Enforcement*:
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
         {"PurchaseOrderID", type text},
         {"SupplierID", type text},
         {"OrderDate", type date},
         {"ExpectedDeliveryDate", type date},
         {"ActualDeliveryDate", type date},
         {"OrderStatus", type text}
     })
     ```

---

### Query 6: `PurchaseOrderDetails`
- **Source Sheet**: `PurchaseOrderDetails` (125 rows, 5 columns)
- **Transformations Implemented**:
  1. *Header Promotion*: Promoted row 1 to headers.
  2. *Type Enforcement*:
     ```powerquery
     #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
         {"POLineID", type text},
         {"PurchaseOrderID", type text},
         {"ComponentID", type text},
         {"QuantityOrdered", Int64.Type},
         {"UnitPurchaseCost", type number}
     })
     ```
  3. *Integrity Verification*: Confirmed that every `PurchaseOrderID` exists in `PurchaseOrders` and every `ComponentID` exists in `Components`. Verified that `QuantityOrdered > 0` and `UnitPurchaseCost > 0`.

---

## 4. Output Datasets in Power BI
Once transformed, the six cleaned datasets load directly into the Power BI Desktop in-memory tabular data engine:
- **`Categories`**: Converted to a Dimension Table (8 rows).
- **`Components`**: Converted to a Conformed Dimension Table (105 rows).
- **`Suppliers`**: Converted to a Dimension Table (50 rows).
- **`InventorySnapshots`**: Converted to a Fact Table (1,260 rows).
- **`PurchaseOrders`**: Converted to a Fact Header / Bridge Table (100 rows).
- **`PurchaseOrderDetails`**: Converted to a Transactional Fact Table (125 rows).

---

## 5. Recommended Future Transformation Checks
The following validation enhancements are documented as best-practice recommendations for production scaling:
1. **Dynamic Path Parameterization**: Implement an M parameter (`SourceWorkbookPath`) so the dataset file path can be re-targeted across different user machines without modifying individual query M-code.
2. **Automated Error Row Isolation**: Add an M step to branch invalid records into an isolated `#ErrorLog` query if future Excel updates introduce ill-formed date strings or negative costs.
3. **Automated Date Boundary Audits**: Add an assertion step ensuring all order dates fall strictly within the operational year window.
