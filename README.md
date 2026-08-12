# Everdale Retail Analytics

**An Excel retail analytics solution built on 194,480 order-line records to help management understand revenue growth, profitability, customer behaviour, channel performance, and seasonal demand.**

**Stack:** Excel, Power Query, Power Pivot, DAX, statistical forecasting  
**Coverage:** January 2023 to December 2025  
**Market:** Nigerian omnichannel retail  
**Currency:** NGN  
**Grain:** One row per order line

---

## The business problem

Revenue growth does not automatically mean better business performance.

Everdale sells through physical stores and online channels, so management needs to understand:

- Is online growth improving profit or just adding lower-margin sales?
- Which categories generate revenue versus actual profit?
- How dependent are sales on promotions?
- Which customers and regions contribute most to performance?
- How does demand change around salary periods, Black Friday, December, and Ramadan?
- Can historical sales patterns support a useful short-term forecast?

This project turns raw retail transactions into a controlled reporting and forecasting model built to answer those questions.

---

## The solution

```text
13 Source Tables
      ↓
Power Query
      ↓
Profile & Validate
      ↓
Clean + Flag Exceptions
      ↓
Order-Line Reporting Model
      ↓
Revenue | Profit | Customers | Channels
      ↓
Forecasting
      ↓
Executive Dashboards
