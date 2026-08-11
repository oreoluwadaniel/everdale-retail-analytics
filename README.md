# Everdale Retail Analytics - Daniel Olatunji

An Excel-based retail intelligence build that takes 194,480 order-line records for a Nigerian omnichannel retailer and turns them into governed revenue, profitability, customer, channel, and forecast reporting, with the data-quality problems documented rather than hidden.

I'm a data analyst based in Lagos, Nigeria, working mostly in Excel, Power Query, Power Pivot, DAX, and statistical forecasting. This project uses synthetic data built to behave like a real retail export: duplicate order lines, missing sales amounts, discounts outside policy, deliveries dated before the order was placed. Finding those problems and deciding what to do with each one was most of the work.

**Contact:** oluwafikayore@gmail.com

---

## The business problem

Everdale is modeled as an omnichannel retailer, physical stores plus online fulfilment, and management is pulled in three directions at once:

1. **Growth vs. margin** - online sales are climbing, but channel economics can quietly erode profit.
2. **Category mix vs. profitability** - the category leading revenue is not always the category leading margin.
3. **Seasonality vs. planning** - demand spikes around salary weeks, Black Friday, December, and Ramadan, and a flat monthly average hides all of it.

The project builds an analysis-ready transaction layer and an executive reporting system to quantify all three, rather than reporting a single "revenue is up" headline and calling it done.

**Tools:** Excel, Power Query, Power Pivot-style modelling, formula-driven validation, statistical forecasting
**Coverage:** Jan 2023 - Dec 2025 | **Currency:** NGN | **Grain:** one row per order line

---

## What's in here

| Folder | Contents |
|---|---|
| [`dataset/`](dataset/) | The 13 normalized source CSVs (orders, order_details, customers, products, etc.) plus their own data README |
| [`docs/`](docs/) | Business context, data dictionary, architecture, analytical methodology, and the data-quality issue log |
| [`workbook/`](workbook/) | The full Excel build: Power Query, Master table, executive and forecast dashboards (113MB, via Git LFS) |

The full interactive workbook, [`workbook/Everdale_Analysis.xlsx`](workbook/Everdale_Analysis.xlsx), Power Query steps, the Master table, pivot-driven executive and forecast dashboards, comes out to 113MB once the data model and pivot caches build in. It's tracked with Git LFS, so cloning the repo pulls it automatically; if you're just browsing on GitHub, use the "Download" button on that file's page. Happy to walk through the Power Query steps or the DAX measures live too, email above.

---

## Data model

The source is normalized across 13 tables: `orders`, `order_details`, `customers`, `products`, `categories`, `suppliers`, `employees`, `stores`, `regions`, `calendar`, `payment_methods`, `promotions`, and `returns`.

The analytical grain is **order line**. `order_details` is the fact table; everything else is a dimension. That choice matters downstream: revenue is additive at line level, but orders and customers have to be counted with `DISTINCTCOUNT`, not by summing rows.

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

## What the quality checks actually caught

The source data has intentional, realistic defects built in. Here's what profiling and validation found:

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

These numbers aren't additive. A single order line can fail more than one rule at once, so they're reported separately instead of summed into one misleading total.

---

## Corrections I had to make to my own numbers

Two metrics in an earlier pass of this workbook were technically wrong, and leaving them uncorrected would have been the easier path. I'm documenting the fix instead:

**Promotion metric.** An earlier version labeled 188,694 rows as "orders on promotion." That's actually a count of promotional *order lines*, not distinct orders, because the Master table sits at order-line grain. Reporting it as a distinct-order KPI would overstate promotional reach. It's now labeled correctly and the distinct-order version is calculated separately where needed.

**Repeat-customer rate.** The workbook reports 49.0% repeat customers. That number only holds up under a specific definition: customers with more than one purchase, measured against the active-purchaser population feeding `Customer_Features`, not the full customer table. Calling it "overall retention" would be a stretch the data doesn't support, so the docs spell out the denominator instead of leaving it implied.

**Revenue.** Revenue, profit, and AOV all depend on how cancelled, returned, and refunded orders get treated. Rather than publish one clean "revenue" number, the documentation states the status filter explicitly, see [`docs/04_ANALYTICAL_METHODOLOGY.md`](docs/04_ANALYTICAL_METHODOLOGY.md).

---

## Business questions this answers

- How fast is revenue growing, and where is it coming from?
- How much revenue is online vs. in-store, and what does that split do to margin?
- Which regions and categories actually drive revenue?
- Which categories produce the strongest margins, revenue leader or not?
- How much of total revenue is promotion-dependent?
- How does demand move across salary weeks, Black Friday, December, and Ramadan?
- How accurate is the forecast when checked against a holdout period?
- Where do data-quality failures put reporting reliability at risk?

## Forecasting

The workbook runs an ETS-based revenue forecast against a naive three-month moving-average benchmark on a six-month holdout, evaluated with MAPE. It's a model-evaluation exercise, not a promise about future sales, see [`docs/04_ANALYTICAL_METHODOLOGY.md`](docs/04_ANALYTICAL_METHODOLOGY.md) for how the forecast should and shouldn't be read.

---

## About the data

Every file in `dataset/` is synthetic, built to resemble a real Nigerian retail export without containing any real customer, employee, or supplier information. Customer emails and phone numbers in the CSVs are generated, not scraped or anonymized from a real source. Where the data includes defects, duplicates, missing fields, invalid discounts, mismatched dates, that's intentional. It's what makes the validation layer worth building.

---

## About me

I'm Daniel Olatunji, a data analyst working across Excel, Power Query, Power BI, SQL, and Python, with a focus on data quality, ETL, and reporting automation. If you're hiring for a data analyst, BI developer, or analytics engineer role and want to talk through this build, including the two metrics I had to go back and correct, reach me at **oluwafikayore@gmail.com**.

See also: [Data Analytics & ETL Portfolio](https://github.com/oreoluwadaniel/data-analytics-etl-portfolio), four Excel/Power Query case studies across CRM, HR, inventory, and sales.
