---
description: Bill timing optimization, cash flow gap detection, and automation planning
allowed-tools: Read
---

## Cadence
Monthly / as needed (run when setting up a new budget, after a pay change, or if you've had a close call with overdraft)

## Purpose
Map income and expenses onto a monthly timeline, identify periods where the running cash balance dips dangerously low, and produce specific bill-timing and automation recommendations to smooth the flow.

## Required Input

Provide the following (or say "read latest" to load from `data/snapshots/` and then fill in timing details):

- **Paycheck dates**: which days of the month you get paid and the net amounts
  - Example: "1st and 15th, $4,200 each"
- **Bill due dates and amounts**: for all fixed and recurring expenses
  - Example: "Mortgage due 1st $2,100, car payment due 8th $380, utilities due 12th $240..."
- **Variable spending pattern**: roughly when variable expenses occur (e.g., groceries weekly, dining spread through month)
- **Starting cash balance**: checking account balance at the start of the month
- **Minimum buffer**: the lowest balance you're comfortable with (default: $500)

If bill due dates aren't known, ask before modeling — exact dates matter here.

## Step 1 — Load Snapshot (Optional)

If "read latest", use Read tool to load from `data/snapshots/`. Extract Monthly Cash Flow fixed and variable expense amounts. Then ask the user to provide the timing details (due dates, paycheck dates) not captured in the snapshot.

## Step 2 — Build the Monthly Cash Flow Calendar

Lay out all cash events across a 4-week view (or full month):

For each week:
- **Inflows**: paycheck deposits
- **Outflows**: bills due that week, estimated variable spending

Compute running balance after each event, starting from the opening balance:
- **Running balance** = previous balance + inflows − outflows

Identify all **gap periods**: weeks where the running balance drops below the minimum buffer ($500 default, or user-specified).

## Step 3 — Analyze Gap Risk

For each gap period, determine:
- **How low does the balance drop?** (trough amount)
- **What caused it?** (which bills clustered together, which paycheck is too far away)
- **How many days is the balance below buffer?** (severity)

Classify gaps:
- 🔴 **Critical** — balance goes negative or within $100 of $0
- ⚠ **At risk** — balance drops below buffer but stays positive
- ✓ **Healthy** — balance stays above buffer throughout

## Step 4 — Recommend Bill Timing Adjustments

For each gap period, identify which bill(s) could be moved to eliminate or reduce the gap:

Rules for suggesting changes:
- Prioritize bills with flexible due dates (utilities, subscriptions, credit cards) over fixed ones (mortgage, loan payments)
- Most billers allow due-date changes with one phone call or online request
- Aim to spread large fixed bills across the two paycheck windows
- Don't suggest moving a bill if it would create a gap elsewhere — verify the full-month impact

For each recommendation:
- Name the bill, current due date, and suggested new due date
- Show the before/after running balance at the trough point
- Note whether the change requires contacting the biller

## Step 5 — Automation Priority List

Recommend which bills to set on auto-pay, in priority order:
1. Bills where a missed payment has the most severe consequence (mortgage, utilities)
2. Bills where late fees are highest relative to bill size (credit cards)
3. Bills that are fixed and never vary (insurance, subscriptions)

Flag any bills that should NOT be auto-paid: variable amounts (credit card statement balance if you carry a balance — auto-pay minimum only), bills you want to review before paying.

## Output Structure

### Key Numbers
Bold at top: number of gap periods found, worst-case trough balance, largest single-week outflow, days below buffer.

### Cash Flow Gap Periods

For each gap: which week, trough balance, cause, severity classification.

### Bill Timing Recommendations

| Bill | Current Due Date | Suggested Due Date | Gap Eliminated | Action Required |
|------|-----------------|-------------------|----------------|-----------------|
| Utilities | 12th | 17th (after 2nd paycheck) | Week 2 trough +$240 | Call biller |
| ... | | | | |

### Automation Priority List

Numbered list: bill name, amount, recommended action (auto-pay full / auto-pay minimum / manual review).

### Mermaid Chart

`xychart-beta` line chart showing cumulative cash balance by week across the month, with an annotation for the buffer threshold:

```mermaid
xychart-beta
    title "Monthly Cash Flow — Running Balance"
    x-axis ["Week 1", "Week 2", "Week 3", "Week 4"]
    y-axis "Balance ($)" 0 --> [max_balance]
    line [balance_after_each_week]
```

Emit a second line flat at the buffer amount so the gap is visually obvious:

```mermaid
xychart-beta
    title "Cash Balance vs Buffer Threshold"
    x-axis ["Start", "Wk1", "Wk2", "Wk3", "Wk4"]
    y-axis "Balance ($)" 0 --> [max_balance]
    line [running_balance_values]
    line [buffer_line_values]
```

Caption identifies which line is balance vs buffer and calls out the gap weeks by name.

### ASCII Calendar Fallback

```
         Income      Bills                          Balance
─────────────────────────────────────────────────────────────
Start                                               $X,XXX
Week 1   +$4,200     Mortgage  -$2,100              $X,XXX
                     Car pmt   -$380                $X,XXX
Week 2               Utilities -$240     ⚠ Low      $XXX
Week 3   +$4,200     Insurance -$290                $X,XXX
Week 4               Variable  -$1,200              $X,XXX
─────────────────────────────────────────────────────────────
End of month                                        $X,XXX
```

## Handoffs
- `/budget-diagnosis` — if the cash flow issue is driven by overspending rather than timing
- `/debt-strategy` — if a debt minimum payment is causing the gap and paydown sequencing could help
- `/quarterly-strategy` — if the underlying issue is structural (income too low, fixed costs too high)
