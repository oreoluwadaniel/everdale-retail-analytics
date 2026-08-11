# Everdale Retail Group - Portfolio Dataset

Synthetic but production-realistic transactional data for a Nigerian omnichannel retailer.
Coverage: 1 Jan 2023 to 31 Dec 2025. Currency: NGN. VAT: 7.5%.

## Tables

| File | Rows | Grain | Primary Key |
|---|---|---|---|
| orders.csv | 61,175 | One row per order (header) | OrderID |
| order_details.csv | 194,480 | One row per product line on an order | OrderDetailID |
| customers.csv | 35,001 | One row per registered customer (+ walk-in C-00000) | CustomerID |
| products.csv | 1,200 | One row per SKU | ProductID |
| categories.csv | 6 | One row per category | CategoryID |
| suppliers.csv | 40 | One row per supplier | SupplierID |
| employees.csv | 300 | One row per employee | EmployeeID |
| stores.csv | 29 | 26 retail stores + 3 online fulfilment centres | StoreID |
| regions.csv | 6 | One row per sales region | RegionID |
| calendar.csv | 1,096 | One row per calendar day | Date |
| payment_methods.csv | 7 | One row per payment method | PaymentMethodID |
| promotions.csv | 53 | One row per promotion campaign | PromoID |
| returns.csv | ~3,100 | One row per returned order line | ReturnID |

## Key relationships

- order_details.OrderID -> orders.OrderID
- order_details.ProductID -> products.ProductID
- order_details.PromoID -> promotions.PromoID (blank when no promo)
- orders.CustomerID -> customers.CustomerID (C-00000 = anonymous walk-in)
- orders.StoreID -> stores.StoreID (WH-xxx rows fulfil online orders)
- orders.EmployeeID -> employees.EmployeeID (blank for online orders)
- orders.PaymentMethodID -> payment_methods.PaymentMethodID
- orders.RegionID / stores.RegionID / customers.RegionID -> regions.RegionID
- products.CategoryID -> categories.CategoryID
- products.SupplierID -> suppliers.SupplierID
- returns.OrderID + returns.ProductID -> the original order line
- orders.OrderDate -> calendar.Date

## Money logic (line level)

- GrossAmount = Quantity x UnitPrice
- DiscountAmount = GrossAmount x DiscountPct
- SalesAmount = GrossAmount - DiscountAmount (VAT exclusive)
- TaxAmount = SalesAmount x 7.5%
- Profit (you compute this) = SalesAmount - (Quantity x UnitCost)

## Warning

This data intentionally contains real-world imperfections: duplicates, missing
values, invalid dates, out-of-policy discounts, text stuck in numeric columns,
inconsistent city spellings and mixed date formats. Do not analyse it before
cleaning it. Finding and fixing these issues is part of the project.
