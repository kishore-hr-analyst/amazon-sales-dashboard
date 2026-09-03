# Data Dictionary

## Tables Overview

### Orders Table
| Column | Type | Description |
|--------|------|-------------|
| OrderID | Text | Unique order identifier |
| OrderDate | Date | Date order was placed |
| CustomerID | Text | Customer reference |
| TotalAmount | Currency | Order total amount |
| OrderStatus | Text | Completed/Pending/Cancelled |

### Customers Table
| Column | Type | Description |
|--------|------|-------------|
| CustomerID | Text | Unique customer identifier |
| CustomerName | Text | Full customer name |
| Country | Text | Customer location |
| Region | Text | Geographic region |
| CustomerTier | Text | Platinum/Gold/Silver/Bronze |
