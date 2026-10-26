# Olist E-Commerce Analysis: Where the Marketplace Can Grow and What Is Holding It Back

An end-to-end Power BI analysis of ~99K Brazilian marketplace orders (Jan 2017 – Aug 2018), covering revenue demand, delivery experience, and order fulfillment, with three prioritized business recommendations.

![Dashboard overview](dashboard-overview.png)

**Quick links:** [Findings deck (PDF)](Olist%20E-Commerce%20Deck.pdf) · [Dashboard (PDF)](Olist%20Project.pdf) · [Power BI file](Olist%20Project.pbix) · [DAX documentation](measures.md)

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

## Data Model

![Data model](Screenshot%20%284287%29.png)

## DAX Measures

Measures, the calculated column, and the calendar table are documented with formulas and explanations in [measures.md](measures.md).

## Repository Contents

| File | Description |
|---|---|
| `README.md` | Project overview |
| `Olist Project.pbix` | Power BI report (data model, DAX, dashboard) |
| `Olist Project.pdf` | Dashboard exported as PDF |
| `Olist E-Commerce Deck.pdf` | Findings and recommendations deck |
| `measures.md` | DAX documentation |
| `Screenshot (4287).png` | Data model (Model view) |

## How to Open

1. Download `Olist Project.pbix`.
2. Open it with [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows only).
3. To refresh the data, download the CSVs from Kaggle and update the file paths in Power Query (Transform data > Data source settings).

Don't have Power BI? Open the [dashboard PDF](Olist%20Project.pdf) or the [findings deck](Olist%20E-Commerce%20Deck.pdf) instead.

## Author

**Santi Enggelina Ramadani**
[LinkedIn](https://www.linkedin.com/in/santienggelina/) · [Email](mailto:santiaramadani05@gmail.com)
