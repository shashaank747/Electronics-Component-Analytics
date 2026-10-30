# Business Requirements

## Analytical Modules

### Module A: Inventory Analytics
Understand what components the business has in stock and evaluate inventory health:
- **Component Stock Quantities**: Overall stock quantities by component and category.
- **Stock Levels against Reorder Thresholds**: Comparison of on-hand inventory vs designated reorder points.
- **Zero-Stock and Low-Stock Components**: Identification of immediate replenishment needs.
- **Inventory Value**: Valuation of stock on hand using standard unit costs.
- **Category-Wise Stock Distribution**: Quantity and valuation breakdown across 8 component categories.
- **Component Condition**: Tracking new, refurbished, or used inventory status.
- **Rental Availability**: Identification of components available for commercial rental.
- **Monthly Stock-Level Snapshots**: Trend analysis of historical monthly inventory levels.

### Module B: Procurement Analytics
Understand what the business is ordering and how suppliers perform:
- **Purchase Order Quantities**: Component volume ordered per purchase order.
- **Purchase Order Value**: Total committed spend across purchase orders.
- **Purchasing Trends**: Monthly spend and procurement trajectory over time.
- **Supplier-Wise Order Value**: Supplier spend concentration and dependency analysis.
- **Component-Wise Purchase Costs**: Price variance analysis across suppliers for identical parts.
- **Pending and Cancelled Orders**: Order fulfillment status tracking.
- **Delivery Lead Time**: Turnaround time from order placement to actual delivery.
- **On-Time Delivery Percentage (OTD %)**: Proportion of delivered orders arriving on or before the committed delivery date.

---

## Business Questions

Our SQL queries and Power BI reports will answer these specific business questions:

### Inventory Questions
1. How many distinct electronic components are available?
2. What is the total stock quantity?
3. Which categories contain the most stock?
4. Which components are below their reorder levels?
5. Which components have zero stock?
6. What is the estimated inventory value?
7. Which categories account for the highest inventory value?
8. How does stock availability change across monthly snapshots?
9. How many components are marked as available for rental?

### Procurement Questions
1. How many purchase orders were created?
2. What is the total value of non-cancelled purchase orders?
3. Which suppliers have the highest purchase order value?
4. Which components are purchased most frequently?
5. What are the monthly purchasing trends?
6. How many orders are delivered, pending or cancelled?
7. What is the average delivery lead time?
8. What percentage of delivered orders arrived on time?
9. How do purchase prices for the same component vary across suppliers?

> **Metric Calculation Policy**: Metrics will only be calculated when the necessary source fields are verified and available in the dataset.
