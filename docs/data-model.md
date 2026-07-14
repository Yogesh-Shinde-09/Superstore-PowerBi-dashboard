# Data Model

## Dataset

Based on the classic **SuperStore** retail dataset — order-level transaction records including sales, profit, quantity, discount, shipping, and customer/geographic details.

## Suggested Table Structure (Star Schema)

| Table | Type | Description |
|---|---|---|
| `Orders` (Fact) | Fact table | One row per order line: Order ID, Order Date, Ship Date, Sales, Profit, Quantity, Discount, Segment, Ship Mode, Payment Mode |
| `Customers` (Dim) | Dimension | Customer ID, Customer Name, Segment |
| `Products` (Dim) | Dimension | Product ID, Category, Sub-Category, Product Name |
| `Geography` (Dim) | Dimension | State, City, Region, Postal Code (used for the map visual) |
| `Date` (Dim) | Dimension | Marked as the official Date table; drives all month/year time intelligence |

## Relationships

- `Orders[Product ID]` → `Products[Product ID]` (many-to-one)
- `Orders[Customer ID]` → `Customers[Customer ID]` (many-to-one)
- `Orders[State]` → `Geography[State]` (many-to-one)
- `Orders[Order Date]` → `Date[Date]` (many-to-one)

All relationships are single-direction (Fact → Dim), following standard star schema best practice for performance.

## Fields Used in This Report

| Visual | Fields |
|---|---|
| Sales by Category | `Products[Category]`, `Sum of Sales` |
| Sales by Sub-Category | `Products[Sub-Category]`, `Sum of Sales` |
| Sales by Ship Mode | `Orders[Ship Mode]`, `Sum of Sales` |
| Monthly Sales / Profit trend | `Date[Month]`, `Date[Year]`, `Sum of Sales`, `Sum of Profit` |
| Sales by Segment | `Customers[Segment]`, `Sum of Sales` |
| Sales by Payment Mode | `Orders[Payment Mode]`, `Sum of Sales` |
| Sales & Profit by State (map) | `Geography[State]`, `Sum of Sales`, `Sum of Profit` |
| Sales by Day + Forecast | `Date[Date]`, `Sum of Sales` |
| Sales by State (ranked bars) | `Geography[State]`, `Sum of Sales` |
| Region filter buttons | `Geography[Region]` |

## Notes

- Update this file with your actual column names/data types if they differ — this is written from the visuals shown in the report, not an inspected schema.
- If you're using the flat single-table version of the SuperStore CSV (no separate dimension tables), the DAX measures still apply — just swap the table references in `dax-measures.md` accordingly.
