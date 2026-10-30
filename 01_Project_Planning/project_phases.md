# Project Phases

```mermaid
flowchart LR
    P1["Phase 1: Planning & Scope"] --> P2["Phase 2: Synthetic Data Design"]
    P2 --> P3["Phase 3: Power Query ETL"]
    P3 --> P4["Phase 4: MySQL Analytics"]
    P4 --> P5["Phase 5: Python Validation"]
    P5 --> P6["Phase 6: Power BI Dashboards"]
    P6 --> P7["Phase 7: Recommendations"]
```

## Implementation Phases

### Phase 1: Planning and Requirements
- Document project overview, industry scenario, problem statement, and scope boundaries.
- Formulate 18 specific business questions across Inventory and Procurement modules.

### Phase 2: Synthetic Dataset Design & Creation
- Design relational schema across 6 normalized tables.
- Generate fictional synthetic records in Excel (`electronics_inventory_raw.xlsx`):
  - 8 Categories
  - 100 Components
  - 10 Suppliers
  - 1,200 Inventory Snapshots (monthly intervals)
  - 120 Purchase Orders
  - 300 Purchase Order Details

### Phase 3: Data Transformation (Power Query)
- Connect to raw Excel sheets and inspect data quality.
- Cleanse records, enforce types, format date fields, and handle missing values.
- Export clean tables for relational database ingestion.

### Phase 4: Relational Modeling & SQL Analytics (MySQL)
- Execute DDL scripts creating tables with Primary Keys, Foreign Keys, and Unique constraints.
- Ingest cleaned data into MySQL.
- Author SQL queries answering all 18 business questions and build analytical views.

### Phase 5: Exploratory Data Analysis & Validation (Python)
- Perform distribution analysis, null-checking, and data profiling using Jupyter Notebooks.
- Cross-validate SQL metrics against Python calculations.

### Phase 6: Business Intelligence & Reporting (Power BI)
- Implement Star/Snowflake schema data modeling with appropriate cardinality.
- Build robust DAX measures for valuation, stockout alerts, lead times, and OTD %.
- Design multi-page interactive dashboards for Inventory and Procurement managers.

### Phase 7: Business Insights & Presentation
- Compile data quality reports, SQL query documentation, and analytical findings.
- Deliver strategic recommendations for inventory holding and supplier management.
- Finalize project presentation slides and repository documentation.
