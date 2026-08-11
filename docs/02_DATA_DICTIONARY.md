# Data Dictionary

## Fact

`order_details` â€” one row per product line on an order.

Core measures:
- `Quantity`
- `UnitPrice`
- `DiscountPct`
- `DiscountAmount`
- `SalesAmount`
- `UnitCost`
- `TaxAmount`
- `PromoID`

## Order header

`orders` â€” one row per order. Provides order date/time, customer, store, employee, channel, payment, status, shipping and delivery attributes.

## Dimensions

- `customers` â€” customer identity and segment attributes
- `products` â€” SKU, category, supplier, cost and retail price
- `categories` â€” category targets
- `suppliers` â€” supplier and lead-time attributes
- `employees` â€” employee/store assignment
- `stores` â€” store/fulfilment-centre attributes
- `regions` â€” management region hierarchy
- `calendar` â€” date dimension
- `payment_methods` â€” payment reference data
- `promotions` â€” campaign reference data
- `returns` â€” returned order-line events

## KPI grain rules

- Revenue: sum valid SalesAmount at line grain using the documented order-status treatment.
- Orders: DISTINCTCOUNT(OrderID).
- Customers: DISTINCTCOUNT(CustomerID).
- AOV: revenue / distinct orders.
- Promotion usage: explicitly distinguish promotional lines from distinct promotional orders.
- Repeat customer rate: customers with more than one purchase / active purchaser population.
