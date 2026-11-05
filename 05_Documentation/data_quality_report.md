# Data Quality & Verification Report

## 1. Executive Summary
This report provides a formal data quality audit of the **Electronics Component Inventory & Procurement Analytics** source dataset and Power Query transformation pipeline.

The dataset is maintained in Microsoft Excel (`02_Dataset/raw/electronics_inventory_raw.xlsx`) across 6 worksheets and imported into Microsoft Power BI Desktop. The audit validates completeness, identifier uniqueness, referential integrity across dimensional relationships, and domain-specific business rules.

---

## 2. Table-by-Table Verification Audit

| Worksheet / Dataset | Verified Records | Primary Key Integrity | Foreign Key Validity | Missing / Null Value Audit | Overall Quality Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`Categories`** | 8 | 100% Unique (`CAT001`–`CAT008`) | N/A (Root Dimension) | 0 Nulls across all fields | **PASSED (100%)** |
| **`Components`** | 105 | 100% Unique (`CMP001`–`CMP105`) | 100% Match to `Categories` | 0 Nulls across all fields | **PASSED (100%)** |
| **`Suppliers`** | 50 | 100% Unique (`SUP001`–`SUP050`) | N/A (Root Dimension) | 0 Nulls across all fields | **PASSED (100%)** |
| **`InventorySnapshots`** | 1,260 | 100% Unique (`INV0001`–`INV1260`) | 100% Match to `Components` | 0 Nulls across all fields | **PASSED (100%)** |
| **`PurchaseOrders`** | 100 | 100% Unique (`PO001`–`PO100`) | 100% Match to `Suppliers` | Nulls only on unfulfilled actual delivery dates | **PASSED (100%)** |
| **`PurchaseOrderDetails`**| 125 | 100% Unique (`POL0001`–`POL0125`) | 100% Match to `PO` & `CMP` | 0 Nulls across all fields | **PASSED (100%)** |

---

## 3. Detailed Data Integrity Audits

### A. Primary Key & Identifier Uniqueness
- **Validation Rule**: All primary key identifiers must be strictly unique and follow standardized alphanumeric formats.
- **Results**:
  - `Categories[CategoryID]`: 8 distinct values, 0 duplicates.
  - `Components[ComponentID]`: 105 distinct values, 0 duplicates.
  - `Suppliers[SupplierID]`: 50 distinct values, 0 duplicates.
  - `InventorySnapshots[SnapshotID]`: 1,260 distinct values, 0 duplicates.
  - `PurchaseOrders[PurchaseOrderID]`: 100 distinct values, 0 duplicates.
  - `PurchaseOrderDetails[POLineID]`: 125 distinct values, 0 duplicates.
- **Outcome**: 100% Passed.

### B. Referential Integrity & Foreign Key Matching
- **Component $\rightarrow$ Category**: Every component in `Components` maps to an active `CategoryID` in `Categories`. No orphan component items exist.
- **Snapshot $\rightarrow$ Component**: Every snapshot record in `InventorySnapshots` references an active `ComponentID` in `Components`.
- **Purchase Order $\rightarrow$ Supplier**: All 100 purchase orders map to a valid `SupplierID` in `Suppliers`.
- **Order Details $\rightarrow$ Purchase Order & Component**: Every line in `PurchaseOrderDetails` references an existing `PurchaseOrderID` in `PurchaseOrders` and an active `ComponentID` in `Components`.
- **Outcome**: 100% Referential Integrity confirmed across all five relationships.

### C. Controlled Null Handling & Status Business Rules
- **Validation Rule**: In `PurchaseOrders`, actual delivery date must exist if and only if the order status is `Delivered`. For `Pending` and `Cancelled` orders, actual delivery date must remain blank (`null`).
- **Results**:
  - `Delivered` Orders (80 records): 80 records have non-null `ActualDeliveryDate` (100% compliant).
  - `Pending` Orders (12 records): 12 records have `ActualDeliveryDate = null` (100% compliant).
  - `Cancelled` Orders (8 records): 8 records have `ActualDeliveryDate = null` (100% compliant).
- **Outcome**: Perfectly adhering to operational business logic. Power Query preserves these legitimate blanks without inserting artificial zeros.

### D. Value Range & Boundary Checks
- `Components[StandardUnitCost]`: Values range from ₹0.50 to ₹720.00; zero negative or zero-cost anomalies exist.
- `Components[ReorderLevel]`: Values range from 10 to 600 pieces; zero negative or zero-threshold anomalies exist.
- `InventorySnapshots[QuantityOnHand]`: Values range from 0 to 2,500 pieces. Stockout occurrences (`QuantityOnHand = 0`) represent deliberate operational scenarios (24 total events across 1,260 observations).
- `PurchaseOrderDetails[QuantityOrdered]`: Values range from 10 to 2,000 pieces; strictly positive integers.
- `PurchaseOrderDetails[UnitPurchaseCost]`: Values range from ₹0.40 to ₹680.00; strictly positive currency values.

---

## 4. Power Query Column Profiling Summary
During ingestion into Power Query:
- Column Quality profiling confirmed **100% Valid values** across all essential key and measure fields.
- Zero data type conversion errors occurred during automated M-language casting.
- Text fields (`CategoryName`, `ComponentName`, `SupplierName`, `City`, `StorageLocation`) were verified free of extraneous leading/trailing whitespace.
