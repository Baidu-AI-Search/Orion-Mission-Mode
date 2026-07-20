# Sales & Expenses Data Summary Report

**Report Date:** July 16, 2026  
**Data Sources:** `quarterly_sales.csv`, `company_expenses.xlsx`

---

## 1. Quarterly Sales Analysis (CSV)

The sales dataset contains 24 transaction records spanning January–December 2024 across four regions and three product lines.

### Key Metrics

| Metric | Value |
|---|---|
| **Total Revenue** | $119,900.00 |
| **Total Cost** | $71,940.00 |
| **Total Profit (Revenue − Cost)** | $47,960.00 |
| **Overall Profit Margin** | 40.0% |
| **Total Units Sold** | 3,775 units |

### Revenue by Region

| Region | Revenue | % of Total |
|---|---|---|
| **East** 🏆 | $33,075 | 27.6% |
| South | $30,925 | 25.8% |
| North | $28,675 | 23.9% |
| West | $27,225 | 22.7% |

**Top-performing region:** **East** ($33,075), narrowly edging out the South.

### Revenue by Product

| Product | Revenue | Units Sold | Avg Unit Price |
|---|---|---|---|
| **Widget B** 🏆 | $47,400 | 1,580 | $30 |
| Widget A | $37,250 | 1,490 | $25 |
| Widget C | $35,250 | 705 | $50 |

**Top-selling product by revenue:** **Widget B** ($47,400), representing ~39.5% of total revenue. Although Widget C has the highest unit price ($50), its lower volume makes Widget B the overall revenue leader.

---

## 2. Q1 2024 Expenses Analysis (Excel)

The Excel workbook includes two sheets: **Q1_Expenses** (12 employee-submitted expense reports) and **Budgets** (departmental quarterly allocations).

### Key Metrics

| Metric | Value |
|---|---|
| **Total Q1 Expenses** | $15,430.00 |
| Number of Expense Reports | 12 |
| Number of Departments | 4 |
| Number of Employees Submitting | 5 |

### Expenses by Department

| Department | Q1 Actual | Q1 Budget | Variance ($) | Variance (%) |
|---|---|---|---|---|
| **Engineering** 🏆 | $7,680 | $25,000 | −$17,320 | −69.3% (under) |
| Marketing | $4,350 | $15,000 | −$10,650 | −71.0% (under) |
| Sales | $2,780 | $12,000 | −$9,220 | −76.8% (under) |
| HR | $620 | $8,000 | −$7,380 | −92.3% (under) |
| **Total** | **$15,430** | **$60,000** | **−$44,570** | **−74.3% (under)** |

- **Highest-spend department:** **Engineering** ($7,680), driven largely by equipment purchases and travel.
- **All departments came in well under budget** in Q1, with HR the furthest below budget at only 7.7% utilization.

### Expenses by Employee

| Employee | Department | Total Expenses |
|---|---|---|
| **Alice Chen** 🏆 | Engineering | $5,400 |
| Bob Martinez | Marketing | $4,350 |
| Carol Davis | Sales | $2,780 |
| Diana Park | Engineering | $2,280 |
| Eve Johnson | HR | $620 |

- **Highest-spending employee:** **Alice Chen** (Engineering) at $5,400, primarily from a $3,200 equipment purchase plus $2,200 in travel.
- Notably, Bob Martinez accounts for 100% of the Marketing department's Q1 expenses.

---

## 3. Overall Insights & Recommendations

1. **Healthy unit economics.** The company achieved a 40% profit margin across all products and regions, suggesting cost-of-goods is well-controlled at ~60% of revenue across the widget portfolio.

2. **Widget B is the volume-and-revenue champion.** Despite Widget C's higher price point ($50 vs. $30), Widget B's strong unit volume (1,580 units) makes it the revenue anchor. Marketing and production should continue prioritizing Widget B while exploring whether targeted promotions can lift Widget C's unit volume.

3. **East region leads — but the race is tight.** Revenue is fairly balanced across regions (range of $27K–$33K). The East's lead is only ~21% over the lowest region (West), suggesting national demand is healthy with no region severely underperforming.

4. **Significant Q1 budget under-utilization across the board.** Total Q1 expenses ($15,430) reached only **25.7%** of the $60,000 quarterly budget. This could indicate:
   - Delayed spending that will hit in later quarters (watch Q2–Q4 for catch-up),
   - Conservative/over-budgeting, especially in HR (only $620 spent against an $8,000 budget), or
   - Missing expense reports not yet filed.
   Finance should reconcile to confirm whether the under-spend is genuine savings or a timing/reporting gap.

5. **Concentration risk in Engineering and Marketing spending.** Alice Chen alone accounts for 35% of all Q1 expenses, and Bob Martinez accounts for 100% of Marketing spend. Approvers should verify these expenses align with planned initiatives and that no single point-of-failure exists in expense reporting workflows.

6. **Combine sales strength with expense discipline.** If current gross margins (~40%) hold and departments continue to operate well below budget, full-year profitability could outperform plan — provided later-quarter expenses don't spike to consume the unused Q1 budget.
