---
description: Debt paydown ordering, payoff timelines, and refinancing assessment
allowed-tools: Read
---

## Cadence
Monthly / as needed

## Purpose
Determine the optimal order and pace to eliminate debt. Models both avalanche (highest rate first) and snowball (lowest balance first) strategies with full timelines and interest costs, flags refinancing opportunities, and shows the impact of paying extra.

## Required Input

Paste the Liabilities section from your snapshot, or say "read latest" to load from `data/snapshots/`.

Also provide:
- Monthly surplus available to put toward extra debt payments (beyond minimums)
- Any debts you're considering refinancing

## Step 1 — Load Data

If "read latest", use Read tool to load the most recent snapshot from `data/snapshots/`. Extract the Liabilities table and Monthly Cash Flow surplus.

If any debt is missing its rate or minimum payment, ask before proceeding.

## Step 2 — Build Debt Summary

For each debt, compute:
- **Monthly interest cost** = balance × (rate ÷ 12)
- **Payoff months at minimum** = use standard amortization for installment loans; for revolving, use minimum payment formula
- **Total interest remaining at minimum** = total paid − remaining balance

Sort table two ways: by rate descending (avalanche order) and by balance ascending (snowball order).

Flag any debt that exceeds these refi thresholds:
- Fixed-rate installment >6.5% — worth checking refi rates
- Variable/revolving >12% — immediate refinance candidate
- Mortgage >1% above current 30yr rate — refi analysis warranted

## Step 3 — Model Both Strategies

Apply the monthly surplus (beyond all minimums) as an extra payment directed to the target debt.

For **Avalanche** (highest rate first):
- Direct surplus to highest-rate debt until paid off
- Roll that payment to next highest rate
- Compute: total months to debt-free, total interest paid

For **Snowball** (lowest balance first):
- Direct surplus to lowest-balance debt until paid off
- Roll that payment to next lowest balance
- Compute: total months to debt-free, total interest paid, number of debts eliminated in first 12 months (motivation metric)

Show the difference: extra interest cost of snowball vs avalanche, and extra months.

## Step 4 — Acceleration Scenarios

Model one additional scenario: what if the user adds $200/mo more than the current surplus?
Show how many months that removes from the payoff timeline and how much interest it saves.

## Output Structure

### Key Numbers
Bold at top: total debt, weighted average rate, monthly minimum total, months to debt-free (recommended strategy), total interest remaining.

### Debt Summary Table

| Debt | Balance | Rate | Min Pmt | Monthly Interest | Avalanche Order | Snowball Order |
|------|---------|------|---------|-----------------|-----------------|----------------|
| ... | ... | ... | ... | ... | #N | #N |

### Strategy Comparison

| | Avalanche | Snowball | Difference |
|---|---|---|---|
| Total interest paid | $X | $X | +$X snowball |
| Months to debt-free | X | X | +X mo snowball |
| Debts gone in 12mo | X | X | |
| **Recommendation** | ✓ preferred if >$500 difference | ✓ preferred if motivation is the concern | |

### Refinancing Opportunities
List any flagged debts with current rate, estimated refi rate, and projected monthly/total savings. If none qualify, state that explicitly.

### Payoff Acceleration
Show the +$200/mo scenario: months saved and interest saved.

### Mermaid Charts

Emit an `xychart-beta` with two lines showing total remaining debt balance month by month for both strategies:

```mermaid
xychart-beta
    title "Total Debt Balance Over Time"
    x-axis [month_labels]
    y-axis "Balance ($)" 0 --> [starting_total]
    line [avalanche_balances]
    line [snowball_balances]
```

Follow with a `gantt` showing when each individual debt is paid off under the recommended strategy:

```mermaid
gantt
    title Debt Payoff Timeline — [Strategy]
    dateFormat YYYY-MM
    section Debts
    [Debt 1]   :active, [start], [payoff_date]
    [Debt 2]   :active, [start], [payoff_date]
    [Debt 3]   :active, [start], [payoff_date]
```

## Handoffs
- `/scenario-compare` — if a refinancing opportunity is significant enough to model in detail
- `/cash-flow-optimizer` — if freeing up minimum payments would meaningfully change cash flow timing
- `/quarterly-strategy` — if debt paydown ranking affects the broader 90-day plan
