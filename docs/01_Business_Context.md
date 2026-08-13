# Everdale Retail Business Context

## Business model

Everdale is modeled as a Nigerian omnichannel retailer with physical stores and online fulfilment centres.

## Management tensions

### 1. Growth vs margin
Online revenue growth can increase volume while changing fulfilment and discount economics.

### 2. Category mix vs profitability
A category can lead revenue without leading profit. Management therefore needs revenue and margin viewed together.

### 3. Seasonality vs planning
Salary weeks, Black Friday, December and Ramadan create non-uniform demand. Forecasting must account for recurring seasonal structure.

## Analytical grain

The primary fact grain is one row per order line.

This matters because:

- revenue is additive at line level;
- orders must be counted distinctly by OrderID;
- customers must be counted distinctly by CustomerID;
- promotion usage can be measured either by lines or distinct orders, but the denominator must be explicit.

## Key decisions

The analysis distinguishes:

- revenue
- gross/profit contribution
- order count
- average order value
- repeat-customer rate
- channel mix
- promotional exposure
- forecast accuracy

These definitions are documented rather than inferred from dashboard labels.
