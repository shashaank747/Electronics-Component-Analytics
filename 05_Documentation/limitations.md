# Project Limitations & Future Enhancements

## Project Assumptions & Operational Boundaries

1. **Synthetic Nature of Dataset**:
   - All records (components, vendors, purchase orders, monthly snapshots) are generated specifically for demonstration and educational purposes. While modeled after industry benchmarks, they do not reflect actual proprietary operational data of a commercial enterprise.

2. **Standard Costing vs FIFO/LIFO**:
   - Inventory valuation utilizes **Standard Unit Cost** as a constant benchmark rather than dynamically calculating rolling weighted average, FIFO (First-In, First-Out), or LIFO cost layers.

3. **Monthly Snapshot Granularity**:
   - Inventory observation occurs at **monthly end-of-period snapshots** (12 observation intervals per year) rather than real-time daily transaction-by-transaction perpetual ledger balances.

---

## Technical & Tooling Scope Constraints

1. **Excel as Source Data Store**:
   - The primary data source is a structured Microsoft Excel workbook (`electronics_inventory_raw.xlsx`). While ideal for prototyping, small business reporting, and Power Query ingestion, enterprise deployments handling millions of records would typically ingest directly from an ERP database or data warehouse.

2. **Descriptive & Diagnostic Focus**:
   - The solution focuses on **Descriptive Analytics** (what occurred) and **Diagnostic Analytics** (why variances or delays happened). It does not include automated Machine Learning predictive demand forecasting or prescriptive replenishment optimization algorithms.

---

## Future Enhancement Roadmap

1. **Power BI Service Deployment**:
   - Publish `.pbix` to Power BI Service with scheduled automated cloud dataset refreshes and enterprise workspace sharing.
2. **Row-Level Security (RLS)**:
   - Implement role-based security filters ensuring regional procurement officers or category managers view only relevant data subsets.
3. **Automated Power Automate Alerts**:
   - Integrate Power BI data-driven alerts with Microsoft Power Automate to trigger automated email or Microsoft Teams notifications when a component hits a zero-stock threshold.
