# Customer Segmentation & Retention Analysis

**Tools:** Python (Pandas)
**Dataset:** [Online Retail II (UCI / Kaggle)](https://archive.ics.uci.edu/dataset/502/online+retail+ii): real transactions from a UK-based online gift retailer, December 2009 to December 2011

## Business question

Which customers generate the most revenue, how many of them keep coming back, and where should a retention budget go?

## Key findings

| # | Finding | Evidence |
|---|---|---|
| 1 | **Revenue is highly concentrated.** The top 20% of customers drive **77%** of revenue | 5,878 customers |
| 2 | **"Champions" generate 81% of revenue** from about 30% of customers | 1,738 customers, 80.8% of revenue |
| 3 | The **Loyal** segment adds another 14% of revenue | 1,781 customers |
| 4 | **At-risk and Lost customers are 40% of the base but only about 5% of revenue** | 2,359 customers, 5.3% of revenue |
| 5 | **About 21% of new customers return in their first month** (simple average across cohorts) | Month-1 retention ranged from 9% to 35% |
| 6 | **The December 2009 cohort retained best** (35% in month 1), **December 2010 worst** (9%) | Cohort retention table |

## Recommendations

1. **Protect the Champions first.** Losing even a few of them costs more than losing a whole other segment. Use early-warning checks, for example a Champion who has not ordered in 60+ days.
2. **Run a win-back campaign on the "At risk" segment** (1,212 customers). They still carry some revenue history and are cheaper to recover than new customers are to acquire.
3. **Investigate the weak December 2010 cohort.** Customers acquired in the holiday season retained noticeably worse, so seasonal buyers may need a different follow-up than regular buyers.

## Approach

1. **Cleaned the data** (1,067,371 raw rows reduced to 805,549):
   - Removed rows with no customer ID, since they can't be tied to a customer
   - Removed cancelled invoices (invoice numbers starting with "C")
   - Removed rows with zero or negative quantity or price
   - Created `revenue` = quantity x price
2. **RFM scoring.** For each customer I calculated:
   - **Recency:** days since last purchase
   - **Frequency:** number of distinct invoices
   - **Monetary:** total spend

   Each was scored 1 to 4 using quartiles. The three scores were added together and grouped into four segments: Champions (10+), Loyal (7-9), At risk (5-6), and Lost (below 5).
3. **Cohort retention.** Customers were grouped by the month of their first purchase, and I measured the percentage who purchased again in each following month.
4. **Concentration check.** I calculated the share of revenue from the top 20% of customers.

## Results at a glance

| Segment | Customers | Revenue share |
|---|---|---|
| Champions | 1,738 | 80.8% |
| Loyal | 1,781 | 14.0% |
| At risk | 1,212 | 3.8% |
| Lost | 1,147 | 1.5% |

## Repository structure

```
├── notebooks/     # retention_analysis.ipynb
├── data/          
└── README.md
```

## How to reproduce

1. Download the Online Retail II dataset and save it as `online_retail.csv` next to the notebook.
2. `pip install pandas jupyter`
3. Run the notebook from top to bottom.

## Limitations and next steps

- **Segment names are my own cut-offs,** not an industry standard. Changing the score thresholds would change how many customers fall into each segment, so I treat the segments as a way to prioritize, not as fixed truths.
- **Retention is measured by "purchased in month N,"** so a customer who buys every other month looks like they churned in the off months. A longer-gap definition of "active" would give higher numbers.
- **Recent cohorts have little history.** Later months have fewer observations, so their retention figures are less reliable.
- **Many customers are wholesalers,** which likely explains the strong revenue concentration. Segmenting retail and wholesale customers separately would be a useful next step.
- **Next steps:** estimate customer lifetime value, test a churn-prediction model, and build a Power BI dashboard for the segments.

## About

Built as a data analytics portfolio project. Questions or feedback are welcome.
