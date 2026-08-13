# Data Dictionary

## Fact

`order_details` one row per product line on an order.

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

`orders` one row per order. Provides order date/time, customer, store, employee, channel, payment, status, shipping and delivery attributes.

## Dimensions

- `customers` customer identity and segment attributes
- `products` SKU, category, supplier, cost and retail price
- `categories` category targets
- `suppliers` supplier and lead-time attributes
- `employees` employee/store assignment
- `stores` store/fulfilment-centre attributes
- `regions` management region hierarchy
- `calendar` date dimension
- `payment_methods` payment reference data
- `promotions` campaign reference data
- `returns` returned order-line events

## KPI grain rules

- Revenue: sum valid SalesAmount at line grain using the documented order-status treatment.
- Orders: DISTINCTCOUNT(OrderID).
- Customers: DISTINCTCOUNT(CustomerID).
- AOV: revenue / distinct orders.
- Promotion usage: explicitly distinguish promotional lines from distinct promotional orders.
- Repeat customer rate: customers with more than one purchase / active purchaser population.
