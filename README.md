# Everdale Retail Analytics - Daniel Olatunji

An Excel retail analysis built from 194,480 order-line records for a Nigerian omnichannel retailer. It covers revenue, profit, customers, channels, and forecasting, with the data problems documented instead of hidden.

I'm a data analyst based in Lagos, Nigeria, working mostly in Excel, Power Query, Power Pivot, DAX, and statistical forecasting. I built this dataset to behave like a real retail export: duplicate order lines, missing sales amounts, discounts outside policy, and deliveries dated before the order. Finding those problems and deciding what to do with them was a big part of the work.

**Contact:** danolatunji25@gmail.com

---

## The business problem

Everdale is an omnichannel retailer with physical stores and online sales. Management needs to answer three questions:

1. **Growth vs. margin:** online sales are growing, but is the extra revenue worth the margin?
2. **Category mix:** which categories bring in revenue and which ones actually make money?
3. **Seasonality:** what happens around salary weeks, Black Friday, December, and Ramadan, and can the forecast account for it?

The project builds the transaction layer and reporting needed to answer those questions instead of stopping at a simple "revenue is up" result.

**Tools:** Excel, Power Query, Power Pivot-style modelling, formula-driven validation, statistical forecasting
**Coverage:** Jan 2023 - Dec 2025 | **Currency:** NGN | **Grain:** one row per order line

---

## What's in here

| Folder | Contents |
|---|---|
| [`dataset/`](dataset/) | 13 normalized source CSVs and the dataset README |
| [`docs/`](docs/) | Business context, data dictionary, architecture, analytical method, and data-quality issue log |
| [`workbook/`](workbook/) | The Excel build with Power Query, the Master table, executive dashboard, and forecast dashboard |

The workbook is 113MB after the data model and pivot caches are built and is tracked with Git LFS.

---

## Data model

The source is split across 13 tables: `orders`, `order_details`, `customers`, `products`, `categories`, `suppliers`, `employees`, `stores`, `regions`, `calendar`, `payment_methods`, `promotions`, and `returns`.

The main table is at **order-line grain**. `order_details` is the fact table and the other tables are dimensions. That means revenue can be summed by line, but orders and customers need `DISTINCTCOUNT` rather than a row count.

```text
Raw CSV tables
     |
Power Query ingestion
     |
Profiling
     |
Validation against business rules
     |
Cleaning / exception flags
     |
Analysis-ready Master table
     |
KPI + profitability analysis
     |
Forecasting
     |
Executive dashboards
```

---

## What the quality checks found

The source data contains deliberate problems. Profiling and validation found:

| Check | Failed rows |
|---|---:|
| Duplicate OrderDetailID | 250 |
| Non-numeric/null Quantity | 6 |
| Quantity <= 0 | 25 |
| Discount outside 0-50% policy | 40 |
| Missing SalesAmount | 60 |
| SalesAmount mismatch (> N0.01 from GrossAmount - Discount) | 25 |
| Delivery date before order date | 30 |
| Refund amount greater than the original line's sales | 1 |

These numbers are not supposed to be added together. One order line can fail more than one check, so each check is reported separately.

---

## Corrections I made to my own numbers

Two metrics in an earlier version of the workbook were wrong.

**Promotion metric.** An earlier version called 188,694 rows "orders on promotion." That was actually promotional order lines, not distinct orders, because the Master table is at order-line grain. I corrected the label and calculate distinct promotional orders separately where needed.

**Repeat-customer rate.** The workbook reports 49.0% repeat customers. That number is based on customers with more than one purchase in the active-purchaser population used by `Customer_Features`, not the full customer table. I documented the denominator instead of calling it overall retention.

**Revenue.** Revenue, profit, and AOV depend on how cancelled, returned, and refunded orders are treated. The methodology document states the filter used for each number.

---

## Questions this answers

- How fast is revenue growing, and where is the growth coming from?
- How much revenue comes from online versus stores, and what does that do to margin?
- Which regions and categories drive revenue?
- Which categories have the strongest margins?
- How much revenue depends on promotions?
- How does demand change around salary weeks, Black Friday, December, and Ramadan?
- How accurate is the forecast on a holdout period?
- Where are data-quality problems putting the reporting at risk?

## Forecasting

The workbook runs an ETS revenue forecast and compares it with a simple three-month moving-average baseline on a six-month holdout, using MAPE. The result is a model comparison, not a promise about future sales. See [`docs/04_ANALYTICAL_METHODOLOGY.md`](docs/04_ANALYTICAL_METHODOLOGY.md) for the calculation and limitations.

---

## About the data

Everything in `dataset/` is synthetic. It was built to look like a Nigerian retail export without using real customer, employee, or supplier information. The email addresses and phone numbers are generated. The duplicates, missing values, bad discounts, and date problems are intentional so the validation work can be tested.

---

## About me

I'm Daniel Olatunji, a data analyst working across Excel, Power Query, Power BI, SQL, and Python, with a focus on data quality, ETL, and reporting automation. If you're hiring for a data analyst, BI developer, or analytics engineer role and want to talk through this build, including the numbers I had to go back and correct, reach me at **danolatunji25@gmail.com**.

See also: [Data Analytics & ETL Portfolio](https://github.com/oreoluwadaniel/data-analytics-etl-portfolio) and [Kavora CRM Migration & Data Governance](https://github.com/oreoluwadaniel/kavora-crm-migration-data-governance).
