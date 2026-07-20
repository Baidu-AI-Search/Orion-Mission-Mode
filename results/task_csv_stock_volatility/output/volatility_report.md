# Apple (AAPL) 2014 Daily Volatility Report

## Data Overview
- **Ticker:** AAPL (Apple Inc.)
- **Period:** 2014-01-02 to 2014-12-12 (calendar year 2014)
- **Trading days:** 240 (239 daily returns)
- **Start adjusted close:** $77.45
- **End adjusted close:** $110.03
- **Full-year return:** +42.07%
- **52-week high:** $118.80 on 2014-11-28
- **52-week low:** $69.00 on 2014-01-31

---

## 1. Daily Volatility Metrics
Daily close-to-close percentage returns were computed as

> r_t = (P_t / P_(t-1) − 1) × 100

| Metric | Value |
|---|---|
| Daily return standard deviation (σ) | **1.4707%** |
| Mean daily return | 0.1578% |
| Median daily return | 0.1393% |
| Average daily absolute % change | **1.0642%** |
| Up days / Down days | 129 / 110 |

---

## 2. Annualized Volatility
Using the standard industry convention of √252 trading days:

> **Annualized volatility = σ_daily × √252 = 1.4707% × √252 = 23.35%**

This places AAPL's 2014 realized volatility in the mid-20s (percent annualized), a level
consistent with a large-cap U.S. technology stock experiencing event-driven moves
(e.g., Q1 earnings reaction, iPhone 6 launch in September, split-adjusted price action).

---

## 3. Top 5 Most Volatile Trading Days
Ranked by the largest absolute daily percentage change:

| Rank | Date | Daily Return | Adj. Close |
|---|---|---|---|
| 1 | 2014-01-28 | -7.51% | $70.90 |
| 2 | 2014-04-24 | +7.40% | $79.66 |
| 3 | 2014-10-21 | +4.78% | $102.18 |
| 4 | 2014-12-02 | -4.47% | $113.05 |
| 5 | 2014-09-04 | -4.13% | $98.03 |

---

## 4. Average Daily Absolute Percentage Change
Across all 239 daily returns in 2014, the **mean absolute daily move was
**1.06%**, meaning on a typical trading day AAPL closed roughly ±1.06% away from the prior session's close. This is noticeably larger than the S&P 500's typical ~0.5–0.7% daily absolute move in the same year, reflecting AAPL's higher single-stock risk.

---

## 5. Quarterly Volatility Comparison
Daily-return standard deviation and mean absolute change by calendar quarter:

| Quarter | Months | Daily σ (%) | Avg Abs Daily Move (%) | Trading Days |
|---|---|---|---|---|
| Q1 | Jan–Mar | 1.582 | 1.131 | 60 |
| Q2 | Apr–Jun | 1.386 | 0.907 | 63 |
| Q3 | Jul–Sep | 1.304 | 1.045 | 64 |
| Q4 | Oct–Dec | 1.641 | 1.201 | 52 |

**Highest-volatility quarter:** Q4 (Oct–Dec) at σ = 1.64% daily.
**Lowest-volatility quarter:** Q3 (Jul–Sep) at σ = 1.30% daily.

---

## 6. Summary & Interpretation

* **Overall realized volatility for AAPL in 2014 was approximately 23.3% annualized**,
  with daily returns averaging ±1.06% in absolute terms. The stock finished the period
  covered by the dataset on 2014-12-12 at $110.03, up +42.1%
  from the January open — a year of strong gains accompanied by meaningful single-stock risk.

* **Volatility was back-half loaded.** Q4 (Oct–Dec) was the most volatile quarter
  (σ = 1.64% daily), edging out Q1 (σ = 1.58%). Q3 (Jul–Sep) was the calmest
  (σ = 1.30%), with Q2 (Apr–Jun) close behind. This pattern lines up with AAPL's 2014
  catalysts:
  - **Q1** opened the year with the single largest down day of the dataset — Jan 28 (−7.51%) —
    after disappointing Q1 FY14 earnings/guidance, followed by a late-January low of $69.00.
  - **Q2–Q3** trended higher into the product-launch build-up; Q2's standout move was the
    Apr 24 post-earnings gap-up of +7.40%, after which daily swings settled down.
  - **Q4** combined the Sept 9 iPhone 6 / Apple Watch announcement (and a −4.13% shakeout on
    Sep 4 just ahead of it), the mid-October market-wide pullback (including a +4.78% snap-back
    on Oct 21), and a −4.47% drop on Dec 2 — pushing quarterly σ to its highest level of the
    year even though the stock was grinding toward its November high of $118.80.

* The **top-5 single-day moves** are split almost evenly between earnings reactions and
  product-launch / macro windows, confirming that idiosyncratic corporate events — not broad
  index moves alone — drove AAPL's extreme days in 2014.

* For positioning: even in a year when AAPL rallied ~40%, holders had to stomach routine daily
  swings of 1–2% and occasional 4–8% shock days around news. Vol-targeted position sizing,
  stop placement, and option-pricing assumptions should reflect the ~23%
  annualized realized vol and the Q4 > Q1 > Q2 > Q3 seasonal pattern observed here.

---
*Report generated from `apple_stock_2014.csv` (adjusted closing prices). Returns are simple
close-to-close percentage changes; standard deviation uses sample estimator (N−1 denominator);
annualization uses √252.*
