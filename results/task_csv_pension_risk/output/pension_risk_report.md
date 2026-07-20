# US Federal Pension Fund — Obligation Risk Assessment

**Data source:** `us_pension_by_state.csv` (federal pension payments by state and congressional district)
**Report date:** July 16, 2026
**Scope:** Current retirees (payees) and deferred (vested, not-yet-drawing) beneficiaries

---

## 1. Executive Summary

The federal pension system covered in this dataset currently pays benefits to **877,305 retirees** for a total annual outlay of **$5.71 billion**, with an additional **483,720 deferred (vested) payees** who will draw benefits in the future. The system-wide **deferred-to-payee ratio is 0.55**, meaning there is roughly one future beneficiary "in the pipeline" for every two current retirees — a manageable aggregate picture that masks significant geographic variation.

Key findings:

- **Future-obligation pressure is concentrated.** Ten states/jurisdictions carry deferred-to-payee ratios above 0.75 (classified as High Risk), including Washington DC at an extreme 5.69 and New Jersey above 1.0 (more deferred than current payees).
- **Payment concentration is moderate but meaningful.** The top 5 states account for ~38% of total payments; the top 10 account for ~61%.
- **District-level hotspots exist.** Two Ohio districts, one Indiana, one Michigan, and one Pennsylvania district each exceed $70M in annual payments, with OH-13 alone at $111M.
- **Majority of states are Medium Risk.** 25 of 52 assessed states fall in the 0.50–0.75 band; 17 are Low Risk.

---

## 2. Deferred-to-Payee Ratio — Top 10 States

The deferred-to-payee ratio (`DEFERRED_COUNT / PAYEE_COUNT`) is a forward-looking indicator: it measures how many vested future beneficiaries exist per current retiree. States with ratios ≥ 0.75 and at least 100 payees are ranked below.

| Rank | State | Current Payees | Deferred Payees | Annual Payments | Deferred/Payee Ratio |
|----:|-------|---------------:|----------------:|----------------:|---------------------:|
| 1 | DC — Washington DC | 544 | 3,093 | $3,675,062 | **5.69** |
| 2 | NJ — New Jersey | 23,635 | 25,187 | $148,148,931 | **1.07** |
| 3 | AK — Alaska | 671 | 663 | $4,255,260 | **0.99** |
| 4 | ND — North Dakota | 160 | 158 | $595,977 | **0.99** |
| 5 | UT — Utah | 2,846 | 2,508 | $22,130,089 | **0.88** |
| 6 | CA — California | 44,586 | 35,564 | $357,704,351 | **0.80** |
| 7 | NY — New York | 51,688 | 40,592 | $338,720,440 | **0.79** |
| 8 | CO — Colorado | 11,959 | 9,104 | $116,137,831 | **0.76** |
| 9 | CT — Connecticut | 8,922 | 6,733 | $47,692,611 | **0.76** |
| 10 | HI — Hawaii | 4,605 | 3,475 | $44,406,428 | **0.76** |

**Observations:**

- **Washington DC** is a structural outlier. With only 544 current payees but 3,093 deferred, the ratio is inflated because a large federal workforce (still employed and accruing credits) is domiciled in DC relative to the current retiree base.
- **New Jersey** is the largest state where deferred beneficiaries *already exceed* current payees (ratio > 1), pointing to a steep ramp in obligations ahead.
- **California and New York** — two of the largest absolute payment states — both sit above 0.78, meaning their already-large payment volumes will grow materially as deferred workers retire.

---

## 3. Concentration Risk

States ranked by total annual payee amount:

| Rank | State | Annual Payments | Share of National Total |
|----:|-------|----------------:|------------------------:|
| 1 | OH — Ohio | $536,200,568 | 9.39% |
| 2 | PA — Pennsylvania | $456,366,987 | 7.99% |
| 3 | FL — Florida | $429,006,153 | 7.51% |
| 4 | MI — Michigan | $389,726,463 | 6.82% |
| 5 | CA — California | $357,704,351 | 6.26% |
| 6–10 | TX, NY, GA, IL, VA | ~$1.32 B combined | ~23.3% |

- **Top 5 states** account for **$2.17 B — 37.98%** of all federal pension payments in the dataset.
- **Top 10 states** account for **$3.50 B — 61.32%** of all payments.
- The remaining 40+ states/territories share the other ~39% of payments.

This is a **moderate concentration**. No single state exceeds 10% of total payments, but the Rust Belt/Great Lakes cluster (OH, PA, MI) together represent nearly a quarter of national outlays (24.2%), suggesting exposure to regional economic and demographic shifts.

---

## 4. District-Level Hotspots

The five individual congressional districts (excluding state-total rows) with the highest annual pension payments represent **geographic concentrations** that could drive localized fiscal and political sensitivity:

| Rank | District | Annual Payments | Current Payees | Avg Annual Benefit |
|----:|----------|----------------:|---------------:|-------------------:|
| 1 | OH-13 (Ohio) | **$111,171,352** | 14,270 | ~$7,790 |
| 2 | IN-1 (Indiana) | **$99,231,495** | 10,268 | ~$9,664 |
| 3 | MI-5 (Michigan) | **$97,822,484** | 6,351 | ~$15,403 |
| 4 | PA-7 (Pennsylvania) | **$72,016,117** | 8,919 | ~$8,075 |
| 5 | OH-6 (Ohio) | **$71,732,509** | 10,647 | ~$6,737 |

**Notes:**
- OH-13 alone pays out more than the entire state totals of Alaska, North Dakota, South Dakota, Vermont, Wyoming and several territories combined.
- MI-5 has the highest average benefit (~$15,400) among the hotspots, suggesting a concentration of higher-grade/longer-career retirees.
- Three of the five hot districts are in the Great Lakes/Rust Belt (OH, MI), reinforcing the state-level concentration finding.

---

## 5. Risk Tier Classification

States (with ≥100 payees) are classified into three tiers based on the deferred-to-payee ratio:

| Risk Tier | Threshold | # of States |
|-----------|-----------|------------:|
| **High Risk** | ratio > 0.75 (significant future obligations relative to current payees) | **10** |
| **Medium Risk** | ratio 0.50 – 0.75 (moderate future obligations) | **25** |
| **Low Risk** | ratio < 0.50 (manageable future obligations) | **17** |

*9 smaller jurisdictions (e.g., territories, foreign armed-forces designators with fewer than 100 payees) were excluded from tier classification due to statistical unreliability.*

### High-Risk States (ratio > 0.75)

| State | Deferred/Payee Ratio | Current Payees | Annual Payments |
|-------|---------------------:|---------------:|----------------:|
| Washington DC | 5.69 | 544 | $3.68 M |
| New Jersey | 1.07 | 23,635 | $148.15 M |
| Alaska | 0.99 | 671 | $4.26 M |
| North Dakota | 0.99 | 160 | $0.60 M |
| Utah | 0.88 | 2,846 | $22.13 M |
| California | 0.80 | 44,586 | $357.70 M |
| New York | 0.79 | 51,688 | $338.72 M |
| Colorado | 0.76 | 11,959 | $116.14 M |
| Connecticut | 0.76 | 8,922 | $47.69 M |
| Hawaii | 0.76 | 4,605 | $44.41 M |

Of particular concern are **New Jersey, California, and New York**, which combine high ratios with large absolute payment bases — these states will drive the largest *absolute* increase in obligations as deferred populations retire.

---

## 6. Overall Risk Profile & Recommendations

### Risk Profile

- **Aggregate solvency indicator is healthy (national ratio 0.55).** The system has roughly one future beneficiary in the pipeline for every two current payees, indicating a mature but not exploding obligation curve.
- **Bimodal risk exists beneath the average.** While most states (42 of 52) are at Medium or Low risk, a small group carries outsized future obligations.
- **Payment concentration is moderate.** The top 5/10 states hold ~38%/~61% of current payments, which is diversified enough to avoid single-point-of-failure risk but concentrated enough that adverse trends in a handful of states (e.g., Ohio, Pennsylvania, California) move the system total materially.
- **District hotspots are pronounced in the industrial Midwest.** OH-13, IN-1, MI-5, PA-7, and OH-6 are where current cash outflows peak per district.

### Recommendations

1. **Stress-test New Jersey, California, and New York first.** These large states combine high deferred ratios with high absolute outlays. Project benefit flows under multiple retirement-timing scenarios to size future cash needs.
2. **Investigate DC's extreme ratio (5.69).** This likely reflects active federal employees not yet separated — confirm data semantics (are all deferred DC-located employees actually future DC payees, or is this a residency/headquarters artifact?).
3. **Monitor the Great Lakes payment cluster.** Ohio, Pennsylvania, and Michigan together represent ~24% of current payments. Demographic trends (retirement of the remaining federal workforce, net migration) in these states will dominate near-term cash flow.
4. **District-level outreach/planning.** The five hotspot districts should be treated as key constituencies for benefit administration, customer-service resourcing, and communications as deferred populations transition to payee status.
5. **Reassess annually.** Deferred-to-payee ratios are a leading indicator; trending the ratio year-over-year per state is more informative than a single snapshot. Tier-classification thresholds should remain stable for comparability.
6. **Apply minimum-sample exclusion consistently.** The <100-payee cutoff used here should be a permanent feature of the dashboard to avoid over-interpreting ratios from tiny populations (e.g., ND at 160 payees is close to the threshold and warrants cautious interpretation).

---

*End of report.*
