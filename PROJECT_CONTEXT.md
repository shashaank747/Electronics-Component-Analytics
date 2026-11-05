# Project Context

## Project Title
**Electronics Component Inventory and Procurement Analysis Dashboard**

## Business Context & Industry Scenario
A fictional electronics component distributor or small electronics manufacturing business handling a diverse catalog of components (passive components, discrete semiconductors, integrated circuits, sensors, microcontrollers, connectors, and electromechanical devices).

### Target Users & Personas
- **Inventory Managers**: Require visibility into real-time stock levels, low-stock warnings, inventory valuation, category distribution, and monthly stock trajectories.
- **Procurement Managers**: Require visibility into purchase order volumes, spend trends, vendor price variance, order fulfillment status, delivery lead times, and supplier on-time delivery percentages.

---

## Purpose & Scope
Analyze electronic component inventory and procurement records to deliver actionable operational intelligence:
- Track stock availability against reorder thresholds to prevent production line stoppages and stockouts.
- Provide dynamic inventory valuation in Indian Rupee (INR - ₹).
- Monitor procurement spend and evaluate supplier delivery reliability.
- Support data-driven purchasing and stock optimization decisions through interactive dashboards.

> **Data Authenticity Disclaimer**: All records in this project are 100% synthetic. The project demonstrates an analytical data architecture using a simulated business scenario and does not claim to solve an actual company's operational problem.

---

## Technical Stack & Architecture

| Purpose | Technology |
| :--- | :--- |
| **Source Data** | Microsoft Excel (`electronics_inventory_raw.xlsx`) |
| **Data Cleaning and ETL** | Power Query |
| **Data Modelling & Relationships** | Microsoft Power BI |
| **Calculations & Business Logic** | DAX (Data Analysis Expressions) |
| **Visualization & Reporting** | Microsoft Power BI Interactive Dashboards |

### Operational Workflow

```text
Excel Source Workbook (6 Worksheets)
       ↓
Power Query – Data Cleaning, Shaping & Type Casting
       ↓
Power BI – Star/Snowflake Data Modelling & Relationships
       ↓
DAX – Calculated Measures and Operational KPI Formulas
       ↓
Interactive Inventory & Procurement Dashboards
       ↓
Business Insights and Actionable Recommendations
```
