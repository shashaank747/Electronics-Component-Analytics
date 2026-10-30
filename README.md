# Electronics Component Inventory and Procurement Analysis Dashboard

An end-to-end Data Analytics and Business Intelligence project simulating an electronics component distributor or small electronics manufacturing business.

## Project Summary
- **Domain**: Electronics Component Inventory & Procurement Management
- **Primary Users**: Inventory Managers, Procurement Managers
- **Currency**: Indian Rupee (INR - ₹)
- **Unit of Measure**: Piece (standardized across all components)
- **Data Source**: 100% synthetic relational dataset across 6 logical tables
- **Tech Stack**: Excel, Power Query, MySQL, Python (Pandas/Jupyter), Power BI, DAX

---

## Repository Structure

```text
Electronics-Component-Analytics/
│
├── .gitignore
├── README.md
├── PROJECT_CONTEXT.md
│
├── 01_Project_Planning/
│   ├── project_overview.md              # Project title, users, purpose & disclaimer
│   ├── problem_statement.md             # Background, operational challenges & solution
│   ├── business_requirements.md         # Analytical modules & 18 business questions
│   ├── project_scope.md                 # Scope matrix (in-scope vs excluded)
│   └── project_phases.md                # 7-phase implementation roadmap
│
├── 02_Dataset/
│   ├── raw/                             # Raw source data (electronics_inventory_raw.xlsx)
│   ├── processed/                       # Cleaned, transformed datasets
│   └── data_dictionary.md               # Complete schema, ER diagram & table specifications
│
├── 03_Power_Query/
│   └── transformation_documentation.md  # ETL steps, type conversions & cleaning rules
│
├── 04_MySQL/
│   ├── 01_create_database.sql           # Database initialization
│   ├── 02_create_tables.sql             # Relational DDL with PK & FK constraints
│   ├── 03_import_data.sql               # Data ingestion scripts
│   ├── 04_data_validation.sql           # Integrity, uniqueness & null checks
│   ├── 05_inventory_analysis.sql        # Stockout, valuation & ABC analysis queries
│   ├── 06_procurement_analysis.sql      # Lead time, spend & vendor queries
│   └── 07_views.sql                     # Reporting views for BI ingestion
│
├── 05_Python/
│   ├── 01_data_profiling.ipynb          # Missing value & distribution profiling
│   ├── 02_exploratory_analysis.ipynb    # Visual EDA and supplier variance analysis
│   └── 03_metric_validation.ipynb       # Validation of SQL & DAX calculations
│
├── 06_Power_BI/
│   ├── Electronics_Inventory_Procurement.pbix # Interactive dashboard file
│   ├── dax_measures.md                  # DAX measure definitions & formulas
│   └── data_model_documentation.md      # Star schema & cardinality documentation
│
├── 07_Documentation/
│   ├── data_quality_report.md           # Source data quality assessment
│   ├── sql_documentation.md             # Query catalog & execution notes
│   ├── analysis_findings.md             # Key findings across inventory & procurement
│   ├── business_recommendations.md      # Actionable recommendations for management
│   └── limitations.md                   # Assumptions & future enhancement roadmap
│
├── 08_Presentation/
│   └── project_presentation.pptx        # Executive slide deck
│
└── 09_Screenshots/
    ├── data_model/                      # Power BI / MySQL ER model diagrams
    ├── sql/                             # Query execution screenshots
    └── dashboard/                       # Power BI report page visuals
```

---

## Core Analytical Modules

### 1. Inventory Analytics
- **Component Stock Quantities & Category Distribution** across 8 component categories.
- **Stock Status Alerts**: Identification of components below reorder levels or with zero stock.
- **Inventory Valuation**: Standard unit cost valuation in INR (₹).
- **Rental Availability & Component Condition**.
- **Monthly Stock Snapshots** (1,200 observation history).

### 2. Procurement Analytics
- **Purchase Order Tracking**: Spend, order volumes, and status (`Delivered`, `Pending`, `Cancelled`).
- **Supplier Performance**: Average delivery lead times and On-Time Delivery % (OTD).
- **Cost Variance Analysis**: Tracking unit purchase price differences for identical parts across vendors.

---

## Documentation Quick Links
- [Project Overview](01_Project_Planning/project_overview.md)
- [Problem Statement](01_Project_Planning/problem_statement.md)
- [Business Requirements & Questions](01_Project_Planning/business_requirements.md)
- [Project Scope](01_Project_Planning/project_scope.md)
- [Project Phases](01_Project_Planning/project_phases.md)
- [Data Dictionary & Database Schema](02_Dataset/data_dictionary.md)
- [Data Model Documentation](06_Power_BI/data_model_documentation.md)
