# Operational Analysis Findings

## 1. Overview
This document compiles the analytical findings and business insights derived from the **Power BI Data Model** and **DAX Measures** for the **Electronics Component Inventory & Procurement Analytics** dashboard.

All metrics are evaluated against the verified operational dataset spanning 2025.

---

## 2. Module A: Inventory Analytics Findings

### Finding 1: Component Catalog Scale & Category Breakdown
- The catalog comprises **105 distinct electronic components** across 8 standardized functional categories.
- Component distribution by category:
  - `Passive Components` (`CAT001`): 15 components
  - `Sensors` (`CAT005`): 15 components
  - `Microcontrollers and Development Boards` (`CAT006`): 15 components
  - `Integrated Circuits` (`CAT004`): 14 components
  - `Diodes and LEDs` (`CAT002`): 13 components
  - `Transistors and MOSFETs` (`CAT003`): 12 components
  - `Electromechanical Components` (`CAT008`): 11 components
  - `Connectors and Cables` (`CAT007`): 10 components

### Finding 2: Inventory Holding Valuation & Capital Allocation
- Total inventory holding valuation across typical monthly snapshots ranges from **₹4.8M to ₹5.6M**.
- **Capital Concentration (The 80/20 Rule)**:
  - `Microcontrollers & Dev Boards` (`CAT006`) accounts for approximately **42% of total inventory valuation**, despite representing only 14.3% of catalog part numbers.
  - `Sensors` (`CAT005`) accounts for approximately **24% of holding valuation**.
  - Combined, microcontrollers and sensors tie up **two-thirds (~66%) of all inventory working capital**.
  - Conversely, `Passive Components` (`CAT001`) and `Diodes and LEDs` (`CAT002`) account for over 60% of physical piece count but represent less than 8% of total holding valuation.

### Finding 3: Stockout Incidents & Reorder Deficits
- Across the 1,260 snapshot observations recorded throughout 2025:
  - **24 Zero-Stock Events** (`QuantityOnHand = 0`) occurred, averaging ~2 components per month experiencing temporary stockouts.
  - **114 Low-Stock Events** (`0 < QuantityOnHand < ReorderLevel`) occurred, averaging 9 to 11 components per month requiring urgent purchase requisitions.
  - Semiconductor ICs and microcontrollers experienced the longest stockout durations, driven by longer vendor lead times.

### Finding 4: Monthly Stock Trajectory
- Physical stock levels remained relatively stable between January and May 2025, experienced high consumption drawdown in June/July (mid-year assembly peak), and recovered following Q3/Q4 replenishment shipments.

### Finding 5: Rental Fleet Monetization Potential
- **16 distinct components** (principally development platforms such as Arduino Mega, STM32 Blue Pill boards, ESP32 evaluation kits, and RFID sensor modules) are designated as **`RentEligible = TRUE`**.
- This indicates an untapped opportunity to commercialize idle prototyping equipment through rental programs to academic and research institutions.

---

## 3. Module B: Procurement Analytics Findings

### Finding 1: Purchase Order Fulfillment Distribution
- Across 2025, **100 total purchase orders** were issued:
  - **Delivered**: 80 orders (**80.0%**) — fulfilled and physically received.
  - **Pending**: 12 orders (**12.0%**) — open commitments placed in late Q4 2025.
  - **Cancelled**: 8 orders (**8.0%**) — voided due to vendor stockouts or obsolete revisions.

### Finding 2: Net Procurement Spend & Budget Distribution
- Total net procurement spend across all non-cancelled purchase orders (115 line items across 92 active orders) is approximately **₹2.4M**.
- Monthly purchasing spend peaked in **March** (Q1 end stocking) and **September** (Q3 pre-build stocking), aligning with manufacturing run cycles.

### Finding 3: Supplier Spend Concentration
- Purchasing is distributed across 50 registered suppliers, but heavily concentrated:
  - The **Top 10 Suppliers** capture approximately **45% of total procurement expenditure**.
  - Leading suppliers by spend include:
    1. `ElectroVibe Components Pvt Ltd` (`SUP001`) — Primary semiconductor distributor.
    2. `Apex Micro Devices India` (`SUP002`) — Direct OEM supplier for microcontrollers.
    3. `Silicon Bridge Technologies` (`SUP003`) — Wholesale IC importer.

### Finding 4: Delivery Lead Time & Vendor Reliability
- **Average Delivery Lead Time**: **11.4 calendar days** for delivered orders (ranging between 5 days for local passives and 22 days for imported dev boards).
- **On-Time Delivery Rate (OTD %)**: **76.25%** (61 out of 80 delivered orders arrived on or before expected delivery date).
- **Delay Patterns**: 19 orders arrived late, experiencing an average delay of 2.8 days past the committed expected date. Delays peaked during Q2/Q3 logistics crunches.

### Finding 5: Supplier Price Variance
- Actual unit purchase prices in `PurchaseOrderDetails` varied by **$\pm 3\%$ to $\pm 12\%$** against benchmark standard costs in `Components`.
- Volume purchases for passive components achieved discounts of up to 10%, while expedited spot buys for specialized microcontroller modules carried premiums of up to 12%.
