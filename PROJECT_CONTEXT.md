# Project Context

## Project Title
**Electronics Component Inventory and Procurement Analysis Dashboard**

## Business Context & Industry Scenario
A fictional electronics component distributor or small electronics manufacturing business handling a diverse catalog of components (passives, semiconductors, ICs, sensors, microcontrollers, electromechanical parts).

### Target Users & Personas
- **Inventory Managers**: Require visibility into real-time stock levels, low-stock warnings, inventory valuation, category distribution, and monthly stock trajectories.
- **Procurement Managers**: Require visibility into purchase order volumes, spend trends, vendor price variance, order fulfillment status, delivery lead times, and supplier on-time delivery rates.

---

## Purpose & Scope
Analyze electronic component inventory and procurement records to deliver actionable operational intelligence:
- Track stock availability against reorder levels to prevent shortages.
- Provide dynamic inventory valuation in Indian Rupee (INR - ₹).
- Monitor procurement spend and evaluate supplier delivery reliability.
- Support data-driven purchasing and stock optimization decisions.

> **Data Authenticity Disclaimer**: All records in this project are 100% synthetic. The project demonstrates an analytical data architecture using a simulated business scenario and does not claim to solve an actual company's operational problem.

---

## Technical Stack & Architecture
1. **Raw Data**: Relational synthetic Excel workbook (`electronics_inventory_raw.xlsx`) across 6 worksheets.
2. **ETL Pipeline**: Power Query for data cleansing, type conversions, and validation.
3. **Relational Database**: MySQL for normalized storage, integrity enforcement, analytical queries, and views.
4. **Data Science / EDA**: Python (Jupyter, Pandas) for exploratory analysis, distribution checks, and metric validation.
5. **Business Intelligence**: Power BI for data modeling (Star/Snowflake), DAX measures, and interactive dashboards.
