---
description: Net worth calculation, composition breakdown, trend tracking, and FI benchmark comparison
allowed-tools: Read
---

## Cadence
Monthly or quarterly

## Purpose
Compute your current net worth, break it down by category, and track how it's changing over time. Compares your position against financial independence benchmarks. Best run right after updating your snapshot.

## Required Input

Say "read latest" to load from `data/snapshots/`, or paste the Assets and Liabilities sections directly.

For trend analysis, also say "read prior" — the skill will look for the previous snapshot in `data/snapshots/` to compute MoM or QoQ changes.

## Step 1 — Load Data

Use Read tool to load the most recent snapshot from `data/snapshots/`. If trend is requested, also read the previous snapshot (second-most-recent file in the directory, or the one from ~3 months ago for QoQ).

Extract: all asset balances (liquid, tax-advantaged investments, taxable investments, real assets) and all liability balances.

## Step 2 — Compute Net Worth

Aggregate into five categories:

| Category | Includes |
|----------|---------|
| **Liquid** | Checking, savings, HYSA, money market |
| **Tax-advantaged investments** | 401k (trad + Roth), IRA, HSA, 529 |
| **Taxable investments** | Brokerage accounts |
| **Real estate equity** | Home value − mortgage balance |
| **Other assets** | Any real assets not covered above |

**Total assets** = sum of all categories  
**Total liabilities** = sum of all debt balances (mortgage + installment + revolving)  
**Net worth** = total assets − total liabilities

Also compute:
- **Liquid net worth** = total assets − total liabilities − illiquid assets (real estate, retirement accounts with penalties)
- **Investment net worth** = tax-advantaged + taxable investments (excludes home equity and liquid cash)

## Step 3 — Trend Analysis

If a prior snapshot is available:

- **MoM change** = current net worth − prior month net worth (and as %)
- **QoQ change** = current − 3-months-ago (if available)
- **Annualized growth rate** = ((current ÷ prior)^(12 ÷ months_elapsed) − 1) × 100

Break down what drove the change:
- Investment returns (market movement on investment accounts)
- New contributions (new money added to investments or savings)
- Debt paydown (liabilities reduced)
- Real estate appreciation (home value change)

If prior snapshot not available: note that trend will be available after the next update, and suggest saving snapshots consistently.

## Step 4 — FI Benchmark Comparisons

Compare net worth and investment net worth against common benchmarks. Ask for (or use from snapshot) age and gross annual income.

Standard benchmarks (Fidelity / rule-of-thumb):

| Age | Investment Net Worth Target |
|-----|-----------------------------|
| 30  | 1× annual salary |
| 35  | 2× annual salary |
| 40  | 3× annual salary |
| 45  | 4× annual salary |
| 50  | 6× annual salary |
| 55  | 7× annual salary |
| 60  | 8× annual salary |
| 67  | 10× annual salary |

Show: current multiple, target multiple for age, gap or surplus, and years to reach next benchmark at current growth rate.

Also show the **4% rule FI number**: 25 × annual expenses = the portfolio size needed to retire. Show % progress toward this target.

## Output Structure

### Key Numbers
Bold at top: total net worth, MoM/QoQ change (if available), investment net worth, FI progress %.

### Net Worth Breakdown

| Category | Balance | % of Assets |
|----------|---------|-------------|
| Liquid | $X | X% |
| Tax-advantaged investments | $X | X% |
| Taxable investments | $X | X% |
| Real estate equity | $X | X% |
| **Total assets** | **$X** | 100% |
| Total liabilities | −$X | |
| **Net worth** | **$X** | |

### Trend (if prior data available)

| Period | Net Worth | Change | Change % | Primary Driver |
|--------|-----------|--------|----------|----------------|
| [Prior] | $X | — | — | — |
| [Current] | $X | +$X | +X% | [Investments / Contributions / Debt paydown] |

### FI Benchmark Progress

| Benchmark | Target | Current | Status |
|-----------|--------|---------|--------|
| Age X target (X× salary) | $X | $X | X% of target |
| FI number (25× expenses) | $X | $X | X% of target |
| Years to FI number (at current growth) | — | ~X years | |

### Mermaid Charts

Asset composition pie (assets only, not net worth — avoids negative value issue):

```mermaid
pie title Asset Composition
    "Liquid" : value
    "Tax-Advantaged Investments" : value
    "Taxable Investments" : value
    "Real Estate Equity" : value
```

If 2+ snapshots available, net worth trend line:

```mermaid
xychart-beta
    title "Net Worth Over Time"
    x-axis [period_labels]
    y-axis "Net Worth ($)" [min] --> [max]
    line [net_worth_values]
```

## Handoffs
- `/financial-health-score` — for a full cross-dimensional assessment of where to focus next
- `/investment-review` — if investment net worth is the largest component and allocation hasn't been reviewed recently
- `/goal-modeling` — if net worth growth rate is off-track for retirement or other long-horizon goals
- `/debt-strategy` — if liabilities are a large % of total assets (>40%)
