# Data Quality & Integrity Report

## Executive Summary
This report audits the data quality, referential integrity, and completeness of the **Electronics Component Inventory and Procurement Analysis** dataset.

The source dataset is maintained in the structured Microsoft Excel workbook `02_Dataset/raw/electronics_inventory_raw.xlsx` across 6 worksheets and processed via **Power Query**.

---

## Audit Summary by Table

| Table | Evaluated Records | Primary Key Integrity | Foreign Key Integrity | Missing / Null Violations | Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`Categories`** | 8 | 100% Unique (`CAT001`–`CAT008`) | N/A (Root Dimension) | 0 Nulls | **PASSED** |
| **`Components`** | 105 | 100% Unique (`CMP001`–`CMP105`) | 100% Match to `Categories` | 0 Nulls | **PASSED** |
| **`Suppliers`** | 50 | 100% Unique (`SUP001`–`SUP050`) | N/A (Root Dimension) | 0 Nulls | **PASSED** |
| **`InventorySnapshots`** | 1,260 | 100% Unique (`INV0001`–`INV1260`) | 100% Match to `Components` | 0 Nulls | **PASSED** |
| **`PurchaseOrders`** | 100 | 100% Unique (`PO001`–`PO100`) | 100% Match to `Suppliers` | Nulls only on unfulfilled dates | **PASSED** |
| **`PurchaseOrderDetails`**| 125 | 100% Unique (`POL0001`–`POL0125`) | 100% Match to `PO` & `CMP` | 0 Nulls | **PASSED** |

---

## Detailed Data Quality Checks

### 1. Primary Key Uniqueness & Formats
- All entity identifiers follow standardized alphanumeric prefixes (`CAT###`, `CMP###`, `SUP###`, `INV####`, `PO###`, `POL####`).
- Zero duplicate primary key values were identified.

### 2. Referential Key Relationships
- **Component to Category**: Every component references a valid `CategoryID` in `Categories`.
- **Snapshot to Component**: Every snapshot record references an active `ComponentID` in `Components`.
- **Purchase Order to Supplier**: Every purchase order references a registered `SupplierID` in `Suppliers`.
- **Line Item to Order & Component**: Every line in `PurchaseOrderDetails` links to an existing `PurchaseOrderID` and `ComponentID`.
- No orphan foreign keys exist.

### 3. Null Handling & Business Logic Validation
- In `PurchaseOrders`:
  - Exactly 80 orders are `Delivered`; 100% of these contain a valid `ActualDeliveryDate`.
  - Exactly 12 orders are `Pending` and 8 orders are `Cancelled`; 100% of these have blank (`null`) `ActualDeliveryDate`, adhering strictly to business rules.
- In `Components`:
  - `StandardUnitCost` is non-negative and greater than zero across all rows.
  - `ReorderLevel` is strictly positive.
  - `RentEligible` is strictly boolean (`TRUE` / `FALSE`).
- In `InventorySnapshots`:
  - `QuantityOnHand` is non-negative ($Q \ge 0$).
  - Exactly 12 monthly observations exist for every component.

### 4. Data Type Consistency
- Numeric and monetary columns (`StandardUnitCost`, `UnitPurchaseCost`) are standardized to 2 decimal places formatted in **INR (₹)**.
- Date fields are uniformly parsed in Power Query as ISO dates (`yyyy-MM-dd`).

---

## Power Query Quality Controls
- **Column Profiling**: Enabled column quality, distribution, and value profiles in Power Query Editor during ingestion.
- **Type Casting**: Explicit M-language transformations applied to eliminate automatic 'Any' type assignments and safeguard tabular model performance.
