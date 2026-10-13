# Revlytics — Verified Business Findings

**Purpose:** A short, recruiter-readable evidence guide for the claims in the submitted business-management resume. This independent portfolio project uses the historical [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), **not Meesho transaction data**.

## Executive scorecard

| Metric | Reproduced result | Definition |
| --- | ---: | --- |
| Delivered orders | **96,478** | Orders whose final status is `delivered` |
| Distinct customers | **93,358** | Distinct `customer_unique_id`, not order-specific `customer_id` |
| Revenue analyzed | **R$15,419,773.75** | Order-level scope aggregated from payment/item data |
| Average order value | **R$159.83** | Revenue analyzed / delivered orders |
| High-value orders | **7.86%** | Order revenue greater than R$347.88 (Q3 + 1.5×IQR) |
| Share of revenue from high-value orders | **33.69%** | High-value orders' revenue / total analyzed revenue |
| Repeat-customer rate | **3.00%** | 2,801 customers with at least two orders / 93,358 customers |
| Later-day repeat-customer rate | **2.16%** | Customers purchasing on at least two separate calendar days |
| High-value at-risk customers | **12,607** | RFM snapshot segment defined in the project's analysis |
| Historical revenue from that segment | **R$4,886,831.08** | Already observed past revenue; **not forecast recoverable revenue** |
| Late-delivery rate | **6.77%** | Delivered calendar date later than the promised calendar date; missing dates excluded |
| Negative-review concentration | **32.45%** | Negative reviews (1–2★) on late orders / all negative reviews |

The resume rounds those figures to 7.9% of orders, 33.7% of revenue, 3.0% repeat rate, ~R$4.9M in *historical* revenue, 6.8% late deliveries and 32.4% of negative reviews.

## How the findings translate to decisions

1. **Revenue concentration:** 7.9% of orders generate 33.7% of revenue. *Proposed test:* proactive tracking and exception handling for high-value orders; measure incremental service cost as a guardrail.
2. **Customer retention:** 3.0% of customers placed 2+ orders. *Proposed test:* randomized Day-7/Day-30 retention journeys focused on valuable first-time buyers; measure incremental later-day repeat rate and margin versus control.
3. **Fulfillment and experience:** late orders are only 6.8% of delivered orders but contribute 32.4% of negative reviews. *Proposed test:* diagnose seller/geography/peak-period delay hotspots and test operational fixes alongside calibrated promises; monitor logistics cost and review outcomes.
4. **At-risk buyers:** the 12,607-person RFM segment generated R$4.89M of *past* revenue. *Proposed test:* controlled win-back experiment, **not** an assertion that the historical amount is recoverable.

## Verification performed

- The local Python pipeline was rerun from all nine source Olist tables, reconstructing **96,478 delivered orders, 93,358 customers and R$15,419,773.75 revenue**.
- Feature engineering reproduced **12,607 high-value at-risk customers**; a separate check of the star-schema CSVs reproduced the high-value revenue share, repeat rate, late rate and late share of negative reviews.
- The pandas KPI validation script reported **0 mismatches for 38 comparable saved DAX measures**. This checks **the exported DAX KPI values**, not a fresh execution of Power BI Desktop on another machine.
- SQL warehouse scripts are implemented but **PostgreSQL execution and three-way SQL–pandas–DAX reconciliation were not verified** in this audit.

The original [dashboard screenshots](../dashboard/screenshots/) and [KPI dictionary](../README.md) provide the current public context. Full pipeline source and Power BI artifacts should be checked in alongside the documentation to make the project independently reproducible.

## Interpretation and limitations

- Scope: **delivered orders in the Olist dataset, September 2016–August 2018**, not a complete funnel containing every cancellation or return.
- Observational evidence shows **association, not causation**. No proposed experiment has been run and no incremental revenue or review improvement is claimed.
- Revenue is **not contribution margin, profit, or platform commission revenue**.
- The 3.00% repeat measure counts multiple same-day orders; the 2.16% later-day metric specifically excludes that interpretation.
- Historical customer value is **not a forecast** of incremental revenue achievable via campaigns.
- Raw data is sourced from Olist and credited under its stated [CC BY-NC-SA 4.0 terms](https://creativecommons.org/licenses/by-nc-sa/4.0/).

**Audit date:** October 13, 2026. **Project:** Dev Pratap Singh / Revlytics.
