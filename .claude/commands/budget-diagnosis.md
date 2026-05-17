---
description: Monthly spending diagnosis — envelope health, overspend analysis, and rollover tracking
allowed-tools: Read
---

## Cadence
Monthly (run in the first week of the new month, or mid-month for a current-month check-in)

## Purpose
Compare actuals to budget envelopes, flag overspent categories, surface surplus envelopes that can cover gaps, and track rollover balances for irregular categories. Produces a concrete revised budget for next month.

## Required Input

Paste the Monthly Cash Flow section (budget and actuals columns) from your snapshot, or say "read latest" to load from `data/snapshots/`.

Also provide (optional but improves output):
- Prior month actuals (for trend comparison)
- Which categories you treat as rollover envelopes (e.g., car maintenance, medical, gifts, vacation)
- Any one-time items in this month's actuals that shouldn't repeat (e.g., annual insurance payment)

If actuals are missing for any category, ask whether to treat it as $0 or skip it.

## Step 1 — Load Data

If "read latest", use Read tool to load from `data/snapshots/`. Extract the Monthly Cash Flow section.

Identify whether this is a prior-month review (full actuals) or a current-month check-in (partial actuals). If partial, note what fraction of the month has elapsed and adjust expectations accordingly.

## Step 2 — Compute Envelope Status

For each budget category, compute:
- **Variance** = budget − actual (positive = surplus, negative = overspend)
- **Status:**
  - ✓ **Healthy** — actual ≤ budget
  - ⚠ **At risk** — actual is between 80–100% of budget (for current-month check-ins only)
  - 🔴 **Overdrawn** — actual > budget

Flag any category where actual is $0 and budget > $0 as **↩ Rollover candidate** — the unspent budget accumulates.

Compute total:
- **Total budgeted**: sum of all budget column values
- **Total actual**: sum of all actual column values
- **Overall variance**: total budgeted − total actual
- **Net cash position**: monthly net income − total actual spending

## Step 3 — Identify Borrowing Opportunities

For each 🔴 Overdrawn category:
1. Find ✓ Healthy categories with surplus > $50 (donor candidates)
2. Rank donors by surplus size
3. Suggest the minimum necessary transfer: "Borrow $X from [donor] → cover [overdrawn]"
4. Check that borrowing doesn't push the donor into at-risk territory

If total overspend exceeds total available surplus, flag the net gap: "After all borrowing, there is still a $X shortfall — this will draw from monthly surplus or require budget adjustment next month."

## Step 4 — Apply Rollover Logic

For categories designated as rollover envelopes:
- **Accumulated rollover balance** = prior month rollover balance + this month's variance
- Positive rollover = saved up for a future irregular expense
- Negative rollover = drew ahead on the envelope (needs replenishment)

Display each rollover category with its current accumulated balance.

Example rollover categories: car maintenance, medical/dental, home repair, gifts, vacation, annual subscriptions, clothing.

If no prior rollover balances are provided, start from $0 and note that the balance will build from this month forward.

## Step 5 — Top 3 Overspend Analysis

For the three categories with the largest negative variance:
- State the overspend amount and percentage over budget
- Give 1–2 specific, actionable reduction suggestions (not generic advice)
- Estimate the realistic budget for next month given the pattern

Examples of specific suggestions:
- Dining $487 vs $300 budget → "Consider a 2-week no-restaurant challenge; meal prep Sunday to reduce weeknight orders"
- Groceries $923 vs $800 → "Check for duplicate household purchases; try store-brand proteins for 2 weeks"
- Amazon/Shopping $312 vs $150 → "Move non-urgent cart items to a wishlist with a 48-hour review rule"

## Output Structure

### Key Numbers
Bold at top: total budgeted, total actual, overall variance, net cash position, number of overdrawn envelopes.

### Envelope Status Table

| Category | Budget | Actual | Variance | Status |
|----------|--------|--------|----------|--------|
| Mortgage | $2,100 | $2,100 | $0 | ✓ |
| Groceries | $800 | $923 | -$123 | 🔴 Overdrawn |
| Dining | $300 | $487 | -$187 | 🔴 Overdrawn |
| Entertainment | $150 | $62 | +$88 | ✓ Surplus |
| Car maintenance | $100 | $0 | +$100 | ↩ Rollover |
| ... | | | | |

### Borrowing Suggestions
For each overdrawn category, show the proposed donor and transfer amount in plain language.

### Rollover Tracker

| Category | Last Month Balance | This Month Variance | New Balance | Notes |
|----------|--------------------|--------------------|-----------| ------|
| Car maintenance | $240 | +$100 | $340 | Building toward annual service |
| Medical | $0 | -$65 | -$65 | Borrowed ahead; replenish next month |

### Top 3 Overspend Analysis
One paragraph per category with the specific reduction suggestion and revised next-month budget.

### Revised Budget for Next Month
A clean budget table incorporating: rollover adjustments, known one-time items removed, suggested cuts applied.

### Mermaid Charts

Grouped bar chart — budget vs actual for variable expense categories:

```mermaid
xychart-beta
    title "Budget vs Actual — Variable Expenses"
    x-axis [category_names]
    y-axis "Amount ($)" 0 --> [max_value]
    bar [budget_values]
    bar [actual_values]
```

Spending composition pie (actuals only):

```mermaid
pie title Monthly Spending Composition
    "Housing" : value
    "Food" : value
    "Transport" : value
    "Kids" : value
    "Other" : value
```

Group minor categories into "Other" if they are each <5% of total spending.

## Handoffs
- `/cash-flow-optimizer` — if the net cash position is tight and bill timing is a factor
- `/goal-modeling` — if overspending is consistently eating into goal contributions
- `/quarterly-strategy` — if the budget pattern suggests a structural problem worth addressing at the 90-day level
