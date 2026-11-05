# Project Scope

## Scope Matrix

| Included in Scope | Excluded from Initial Scope |
| :--- | :--- |
| **Excel Source Dataset Management** (6 relational tables) | Complete ERP enterprise software development |
| **Power Query ETL** (Cleansing, shaping, type casting) | Live database servers and external relational stores |
| **Power BI Data Modelling** (Star/Snowflake relationships) | Custom code data pipelines and machine learning notebooks |
| **DAX Measures & KPIs** (Valuation, stockout alerts, OTD %) | Live real-time purchase order execution |
| **Inventory Analytics Dashboard** (Stock, valuation, reorder) | Live warehouse management / RFID hardware integration |
| **Procurement Analytics Dashboard** (Spend, lead time, OTD) | Production scheduling & shop-floor execution |
| **Monthly Stock Snapshot Analysis** (12-month trends) | Machine-learning demand forecasting |

## Scope Boundaries & Conventions
- **Source Data**: Structured Microsoft Excel workbook (`electronics_inventory_raw.xlsx`) containing 6 worksheets.
- **Data Authenticity**: 100% synthetic dataset created from scratch. No real company or college data is used.
- **Reporting Currency**: Indian Rupee (INR - ₹).
- **Unit of Measurement**: Standardized to **Piece** across all components so aggregated quantities remain consistent.
