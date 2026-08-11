# Analytical Methodology

## Revenue

Revenue is calculated from validated order-line SalesAmount. Cancelled orders should be excluded from revenue measures.

## Profit

`Profit = SalesAmount - (Quantity × UnitCost)`

## AOV

`AOV = Revenue / Distinct OrderID`

AOV must use distinct orders, not order lines.

## Repeat customer rate

`Repeat Customer Rate = Customers with >1 purchase / Active purchaser population`

The denominator must be stated because including non-purchasers or the anonymous walk-in record changes the result.

## Forecast evaluation

The workbook compares ETS against a naive three-month moving-average benchmark on a six-month holdout.

Use MAPE as an evaluation statistic, not as a guarantee of future performance.
