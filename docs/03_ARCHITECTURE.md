# Architecture

```text
13 normalized CSV tables
        │
        ▼
Power Query
        │
        ├── profiling
        ├── type enforcement
        ├── duplicate handling
        ├── business-rule validation
        └── exception flags
        │
        ▼
Analysis-ready Master table
        │
        ├── Revenue
        ├── Profitability
        ├── Customer analysis
        ├── Channel analysis
        └── Promotion analysis
        │
        ├───────────────┐
        ▼               ▼
Executive BI       Forecasting
Dashboard          + benchmark
```

The normalized source model is retained for auditability while the Master table provides a practical reporting grain for Excel analysis.
