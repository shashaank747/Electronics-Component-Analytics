# Project Scope, Assumptions & Limitations

## 1. Executive Summary
This document defines the operational assumptions, analytical limitations, and scope boundaries of the **Electronics Component Inventory & Procurement Analytics** project.

Documenting these boundaries ensures that stakeholders, developers, and report consumers understand the analytical context and interpret dashboard metrics appropriately.

---

## 2. Core Project Assumptions & Dataset Limitations

### 1. Synthetic Data Nature
- All records—including component specifications, supplier profiles, purchase order lines, and monthly stock levels—are **100% synthetic**.
- The dataset was created specifically to simulate a realistic electronics manufacturing and distribution business scenario for analytical demonstration. It does not represent proprietary operational data from any real-world commercial enterprise.

### 2. Periodic Snapshot Nature (Not Transactional Movements)
- Inventory data is recorded as **periodic monthly snapshots** (12 discrete observation dates across 2025).
- The dataset does **not** contain transaction-level perpetual inventory logs (individual material receipts, work-in-progress scrap issuances, bin transfers, or daily assembly consumption).
- Macro trend visuals illustrate stock level shifts across snapshot intervals, not continuous material flow.

### 3. Purchase Order Value vs Cash Expenditure
- Procurement value calculations represent the monetary value of **purchase order line items** (`QuantityOrdered × UnitPurchaseCost`).
- The dataset contains **no cash disbursement records, banking transactions, or invoice settlement logs**. Consequently, procurement spend metrics reflect purchasing commitments, not verified cash flow outlays.

### 4. Supplier Performance Evaluation Scope
- Supplier delivery performance is evaluated strictly using available purchase order milestones (`OrderDate`, `ExpectedDeliveryDate`, `ActualDeliveryDate`, `OrderStatus`).
- Quality inspection defect rates, supplier return-to-vendor (RTV) credits, and incoming shipment compliance scores are outside the current dataset scope.

### 5. Standard Benchmark Costing vs Perpetual Costing
- Inventory valuation is calculated using constant standard benchmark unit costs (`Components[StandardUnitCost]`) rather than rolling weighted average or FIFO/LIFO cost layering.

---

## 3. Technology & Architecture Boundaries

1. **Analytical Reporting vs Operational Execution**:
   - The Power BI dashboard is designed for **decision support and performance analysis**. It does not execute operational transactions, trigger purchase orders, or interface directly with supplier web portals.
2. **Local Tabular Model Scope**:
   - The solution is architected as an Excel + Power Query + Power BI Desktop project. It does not rely on an external database server or custom data engineering scripts.
3. **Descriptive & Diagnostic Focus**:
   - The project delivers **descriptive** (what happened) and **diagnostic** (why it happened) Business Intelligence. It does not claim predictive demand forecasting via machine learning or automated algorithmic stock replenishment.

---

## 4. Recommended Future Enhancements
The following capabilities represent valuable avenues for future expansion:
- **Power BI Service Deployment**: Publishing the `.pbix` solution to a cloud workspace with scheduled automated dataset refresh and mobile app distribution.
- **Row-Level Security (RLS)**: Enforcing security roles so category managers or regional purchasing agents view only their assigned component families or vendor territories.
- **Automated Reorder Alerts via Power Automate**: Linking Power BI KPI threshold alerts to automated Microsoft Teams or email notifications when components drop into the low-stock zone.
