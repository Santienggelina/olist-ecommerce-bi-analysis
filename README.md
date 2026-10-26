# olist-ecommerce-bi-analysis
Power BI dashboard and analysis of 99K Olist e-commerce orders
# Olist E-Commerce Analysis: Where the Marketplace Can Grow and What Is Holding It Back
An end-to-end Power BI analysis of ~99K Brazilian marketplace orders (Jan 2017 – Aug 2018), covering revenue demand, delivery experience, and order fulfillment, with three prioritized business recommendations.
## Background
Olist is a Brazilian marketplace that connects small sellers to large online sales channels. This project analyzes its public order data to answer where revenue comes from, how delivery performance affects customer satisfaction, and where orders are lost before delivery.
## Business Questions
1. **Demand:** Where does revenue come from? (sales by state and product category)
2. **Experience:** Does delivery shape satisfaction? (delivery time, on-time rate, review score)
3. **Fulfillment:** Where do orders leak? (order status outside "delivered")
## Dataset
- **Source:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Scope:** ~99K orders, Jan 2017 – Aug 2018 (2018 is a partial year)
- **Note:** Raw CSV files are not included in this repo. Download them from Kaggle using the link above.
## Tools
- **Power BI Desktop** (Power Query for cleaning and transformation, DAX for measures)
- **PowerPoint** for the findings deck
## Key Metrics
| Metric | Value |
|---|---|
| Total sales | 13.59M |
| Orders | ~99K |
| Average order value | 136.68 |
| On-time delivery | 93.2% |
| Average review score | 4.09 |
| Average delivery time | 12.5 days |
Top categories by revenue: Health & Beauty, Gift Watches, Bed & Bath.
## Key Findings
1. **Revenue is concentrated in one state.** São Paulo (SP) accounts for 38.3% of revenue. The next largest states are Rio de Janeiro, Minas Gerais, Rio Grande do Sul, Paraná, and Santa Catarina. A logistics or competitive shock in SP would affect a large share of the business.
2. **Late delivery is linked to much lower reviews.** Orders delivered on time average a 4.3 review score, versus 2.3 for late orders (a 2.0-point gap). Although 93.2% of orders arrive on time, the late minority has an outsized effect on satisfaction.
3. **Most orders are delivered, but some are lost.** About 97% of orders are delivered. Of the ~3K orders not delivered, most are still in progress, but about 1.2K (1.2% of all orders) are canceled or unavailable, which may represent preventable loss.
## Recommendations (in priority order)
1. **Fix late delivery.** Review carrier performance and the accuracy of estimated delivery dates in the slowest regions. *Evidence: late orders score 2.3 vs 4.3 on time.*
2. **Diversify demand beyond São Paulo.** Pilot targeted acquisition in RJ, MG, RS, PR, and SC, and judge each by order value before scaling. *Evidence: SP is 38.3% of revenue.*
3. **Reduce avoidable loss.** Improve stock accuracy and flag sellers with repeated canceled or unavailable orders. *Evidence: about 1.2K orders, 1.2% of total.*
## Limitations
- 2018 data ends in August, so year-on-year comparisons are partial.
- No cost or margin data, so revenue is not profit.
- Delivery time and review score are correlated, but causation is untested.
## Next Steps
- Seller concentration analysis (top 10 sellers and a Pareto view)
- Payment mix and repeat-customer rate
- Excel scenario: improving on-time rate from 93% to 97% and the estimated effect on reviews
## Repository Structure
olist-ecommerce-bi-analysis/
├── README.md
├── Olist_Project.pbix
├── docs/
│   ├── Olist E-Commerce_Deck.pdf
│   └── Olist Project.pdf
└── dax/
    └── measures.md
```
## Data Model
![Data model](Screenshot%20%284287%29.png)
## DAX Measures
# DAX Documentation

All measures, calculated columns, and calculated tables used in `Olist_Project.pbix`.

## Measures

### Total Sales
```dax
Total Sales = SUM(olist_order_items_dataset[price])
```
Sum of item prices across all orders. Excludes freight. This is the 13.59M headline figure.

### Total Freight
```dax
Total Freight = SUM(olist_order_items_dataset[freight_value])
```
Sum of shipping fees charged on items.

### Total Orders
```dax
Total Orders = DISTINCTCOUNT(olist_orders_dataset[order_id])
```
Number of unique orders. Uses `DISTINCTCOUNT` because one order can have several item rows.

### Average Order Value
```dax
Average Order Value = DIVIDE([Total Sales], [Total Orders])
```
Revenue per order. `DIVIDE` returns blank instead of an error when orders are zero (for example, when a slicer filters everything out).

### Avg Review Score
```dax
Avg Review Score = AVERAGE(olist_order_reviews_dataset[review_score])
```
Average customer rating on a 1 to 5 scale.

## Calculated Column

### Delivery Status
```dax
Delivery Status =
IF(
    ISBLANK(olist_orders_dataset[order_delivered_customer_date]),
    BLANK(),
    IF(
        olist_orders_dataset[order_delivered_customer_date] <= olist_orders_dataset[order_estimated_delivery_date],
        "On-time",
        "Late"
    )
)
```
Classifies each delivered order as "On-time" (delivered on or before the estimated date) or "Late". Orders without a delivery date (not yet delivered, canceled, and so on) stay blank so they do not distort the on-time rate. This column drives the 4.3 vs 2.3 review comparison.

## Calculated Table

### Calendar
```dax
Calendar =
ADDCOLUMNS(
    CALENDAR(DATE(2016, 1, 1), DATE(2018, 12, 31)),
    "Year", YEAR([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "Month No", MONTH([Date]),
    "Month", FORMAT([Date], "MMM")
)
```
Date table covering 2016 to 2018, used for time-based analysis. The dataset ends in August 2018, so late 2018 dates have no orders.

## Notes

- Metrics such as the on-time delivery rate (93.2%) and average delivery time (12.5 days) are built on top of the `Delivery Status` column and the order date fields.

## How to Open

1. Download `Olist_Project.pbix`.
2. Open it with [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows only).
3. If you want to refresh the data, download the CSVs from Kaggle and update the file path in Power Query (Transform data > Data source settings).

Don't have Power BI? See the PDFs

## Author ##

**Santi Enggelina Ramadani**
https://www.linkedin.com/in/santienggelina/· (mailto:santiaramadani05@gmail.com)
