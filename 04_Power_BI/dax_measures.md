# DAX Measures & Calculation Specifications

This document catalogs the DAX (Data Analysis Expressions) measures implemented in the **Power BI Data Model** for the **Electronics Component Inventory and Procurement Analysis Dashboard**.

All measures are organized into dedicated measure display folders.

---

## 1. Inventory Analytics Measures

### `Total Distinct Components`
Counts the total number of distinct electronic components managed in the catalog.
```dax
Total Distinct Components = 
DISTINCTCOUNT( Components[ComponentID] )
```

### `Total Stock Quantity`
Calculates the aggregate physical inventory units on hand across active filters.
```dax
Total Stock Quantity = 
SUM( InventorySnapshots[QuantityOnHand] )
```

### `Inventory Valuation`
Calculates total inventory monetary holding value in Indian Rupee (₹) based on standard unit cost.
```dax
Inventory Valuation = 
SUMX(
    InventorySnapshots,
    InventorySnapshots[QuantityOnHand] * RELATED( Components[StandardUnitCost] )
)
```

### `Zero Stock Components`
Counts components that currently have zero physical stock on hand.
```dax
Zero Stock Components = 
CALCULATE(
    DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
    InventorySnapshots[QuantityOnHand] = 0
)
```

### `Low Stock Components`
Counts components where current quantity on hand is below the minimum reorder threshold but greater than zero.
```dax
Low Stock Components = 
CALCULATE(
    DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
    FILTER(
        InventorySnapshots,
        InventorySnapshots[QuantityOnHand] > 0 &&
        InventorySnapshots[QuantityOnHand] < RELATED( Components[ReorderLevel] )
    )
)
```

### `Healthy Stock Components`
Counts components with stock at or above the designated safety reorder level.
```dax
Healthy Stock Components = 
CALCULATE(
    DISTINCTCOUNT( InventorySnapshots[ComponentID] ),
    FILTER(
        InventorySnapshots,
        InventorySnapshots[QuantityOnHand] >= RELATED( Components[ReorderLevel] )
    )
)
```

### `Rental Eligible Components`
Counts components marked as eligible for commercial rental equipment programs.
```dax
Rental Eligible Components = 
CALCULATE(
    DISTINCTCOUNT( Components[ComponentID] ),
    Components[RentEligible] = TRUE()
)
```

---

## 2. Procurement Analytics Measures

### `Total Purchase Orders`
Counts the total number of purchase order headers.
```dax
Total Purchase Orders = 
DISTINCTCOUNT( PurchaseOrders[PurchaseOrderID] )
```

### `Delivered Orders`
Counts purchase orders that have been successfully received and fulfilled.
```dax
Delivered Orders = 
CALCULATE(
    COUNTROWS( PurchaseOrders ),
    PurchaseOrders[OrderStatus] = "Delivered"
)
```

### `Pending Orders`
Counts purchase orders currently in transit or awaiting supplier fulfillment.
```dax
Pending Orders = 
CALCULATE(
    COUNTROWS( PurchaseOrders ),
    PurchaseOrders[OrderStatus] = "Pending"
)
```

### `Cancelled Orders`
Counts voided or cancelled purchase orders.
```dax
Cancelled Orders = 
CALCULATE(
    COUNTROWS( PurchaseOrders ),
    PurchaseOrders[OrderStatus] = "Cancelled"
)
```

### `Total Procurement Spend`
Calculates the net committed spend in INR (₹) across all non-cancelled purchase orders.
```dax
Total Procurement Spend = 
CALCULATE(
    SUMX(
        PurchaseOrderDetails,
        PurchaseOrderDetails[QuantityOrdered] * PurchaseOrderDetails[UnitPurchaseCost]
    ),
    PurchaseOrders[OrderStatus] <> "Cancelled"
)
```

### `Total Units Ordered`
Calculates total physical pieces ordered across valid purchase orders.
```dax
Total Units Ordered = 
CALCULATE(
    SUM( PurchaseOrderDetails[QuantityOrdered] ),
    PurchaseOrders[OrderStatus] <> "Cancelled"
)
```

### `Average Delivery Lead Time (Days)`
Calculates the average turnaround time in days between order placement and receipt date for delivered orders.
```dax
Average Delivery Lead Time (Days) = 
CALCULATE(
    AVERAGEX(
        PurchaseOrders,
        DATEDIFF( PurchaseOrders[OrderDate], PurchaseOrders[ActualDeliveryDate], DAY )
    ),
    PurchaseOrders[OrderStatus] = "Delivered" &&
    NOT ISBLANK( PurchaseOrders[ActualDeliveryDate] )
)
```

### `On-Time Delivered Orders`
Counts delivered orders that arrived on or before the committed expected delivery date.
```dax
On-Time Delivered Orders = 
CALCULATE(
    COUNTROWS( PurchaseOrders ),
    PurchaseOrders[OrderStatus] = "Delivered" &&
    PurchaseOrders[ActualDeliveryDate] <= PurchaseOrders[ExpectedDeliveryDate] &&
    NOT ISBLANK( PurchaseOrders[ActualDeliveryDate] )
)
```

### `On-Time Delivery Rate (OTD %)`
Calculates the percentage of delivered purchase orders arriving on or before schedule.
```dax
On-Time Delivery Rate (OTD %) = 
DIVIDE(
    [On-Time Delivered Orders],
    [Delivered Orders],
    0
)
```

### `Price Variance %`
Measures the percentage difference between actual unit purchase cost and standard benchmark cost.
```dax
Price Variance % = 
DIVIDE(
    AVERAGE( PurchaseOrderDetails[UnitPurchaseCost] ) - AVERAGE( Components[StandardUnitCost] ),
    AVERAGE( Components[StandardUnitCost] ),
    0
)
```
