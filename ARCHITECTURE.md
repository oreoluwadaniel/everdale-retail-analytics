# Architecture

```text
Raw retail files
      |
      v
Power Query / ETL
      |
      +--> data quality checks
      |       +--> grain checks
      |       +--> duplicate checks
      |       +--> status checks
      |
      v
13-table analytical model
      |
      +--> sales and customer metrics
      +--> promotion analysis
      +--> profitability
      +--> repeat-customer analysis
      |
      v
Forecasting holdout
      |
      +--> baseline
      +--> candidate model
      +--> error comparison
      |
      v
Decision outputs
```

## Key design decisions

- Order-line grain is kept explicit so order counts are not inflated by one-to-many joins.
- Revenue uses documented order-status rules.
- Promotion analysis uses distinct orders where the business question is order-level.
- Forecasts are compared with a baseline before being used for planning.
- Data-quality exceptions are documented rather than silently removed.

## Reproduction

The repository README is the entry point. The model, transformation notes, validation work, and final workbook should be read in that order.
