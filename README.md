# Late Delivery Root-Cause Analysis: Olist Brazilian E-commerce

Which orders arrive late, where, and why? An SQL + Python + Tableau analysis of 96,469 delivered orders.

**Live dashboard:** [Olist Late Delivery Dashboard on Tableau Public](https://public.tableau.com/app/profile/anna.m.paul6609/viz/OLIST_17918029783250/Dashboard1)
![Dashboard](Dashboard%201.png)
## Key findings

| Metric | Result |
|---|---|
| Orders analyzed (delivered, with dates) | 96,469 |
| Overall late rate (delivered after the estimated date) | **8.11%** |
| Average delay when late | **9.6 days** |
| Late rate when the seller handed off **late** vs on time | **24.41%** vs **6.49%** (about 3.8x) |
| Highest late-rate states (500+ orders) | MA 19.67%, CE 15.32%, BA 14.04% |
| Late rate, cross-state vs same-state seller | 9.26% vs 6.06% |
| Average review score, late vs on-time | **2.57** vs **4.29** stars |

**Takeaways**
- Late seller handoffs are strongly associated with late delivery, which makes them a high-value lever for improvement.
- Delays are concentrated in the North and Northeast (MA, CE, BA), where the late rate runs roughly 2x the national average.
- Late delivery is associated with a drop of about 1.7 review stars.

> These are associations in observational data, not proof of causation.

## Tools
- **SQL (SQLite):** joins, CTEs, window functions (`NTILE`, `RANK`, `LAG`)
- **Python:** pandas, matplotlib, in Google Colab
- **Tableau Public:** the interactive dashboard

## Data
[Olist Brazilian E-commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) on Kaggle. Covers about 100K orders from 2016 to 2018. The raw CSVs aren't included here; download them from Kaggle.

## Method
1. Loaded 7 CSV tables into SQLite and built one analysis table with one row per delivered order.
2. Included only `order_status = 'delivered'` orders with delivery and carrier dates.
3. **Late** = delivered after the estimated delivery date.
4. For orders with several items, seller, product and category are taken from item #1.
5. Split each order's journey into a seller stage (purchase to carrier handoff) and a carrier stage (handoff to customer).
6. **Late handoff** = the carrier received the order after the seller's `shipping_limit_date`.
7. Applied minimum order thresholds (500 for states, 300 for categories, 30 for sellers, 100 for months) so small samples don't distort rates.

## Limitations
- Seller attribution uses the first item in multi-item orders.
- Findings show association, not causation.
- The data covers 2016 to 2018 and one marketplace; results may not generalize.
- Very early months (Sep to Dec 2016) are excluded from the trend because of tiny order counts.

## Repository
```
olist_late_delivery_analysis.ipynb   # full analysis
images/dashboard.png                 # dashboard screenshot
README.md
```

## How to run
1. Open the notebook in Google Colab.
2. Run the `kagglehub` download cell, or upload the Kaggle zip.
3. Run all cells top to bottom. The "Resume metrics" section prints the headline numbers.
