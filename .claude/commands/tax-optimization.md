---
description: Year-round tax positioning — contribution headroom, bracket management, and time-sensitive actions
allowed-tools: Read
---

## Cadence
Quarterly + year-end (run in Oct/Nov for deadline-sensitive actions)

## Purpose
Identify actionable tax-saving opportunities from your current financial snapshot. Focuses on contribution headroom, bracket management, tax-loss harvesting, and withholding accuracy. Always produces a ranked action list with estimated dollar impact.

## Required Input

Paste the Tax Context section (including YTD Contribution Headroom table) and Investment Allocation section from your snapshot, or say "read latest" to load from `data/snapshots/`.

Also provide the current month/quarter — needed to flag time-sensitive year-end actions.

## Step 1 — Load Data

If "read latest", use Read tool to load from `data/snapshots/`. Extract Tax Context, YTD Contribution Headroom, and Investment Allocation sections.

Identify the current quarter (Q1–Q4) to determine whether year-end deadline items apply.

## Step 2 — Contribution Headroom Analysis

For each tax-advantaged account, compute:
- **Personal remaining headroom** = personal limit − YTD contributed
- **Combined remaining headroom** (401k) = $72,000 combined limit − (employee YTD + employer YTD)
- **Months remaining in tax year** = 12 − current month
- **Monthly contribution needed to max out** = remaining headroom ÷ months remaining

Flag accounts where:
- Headroom > $1,000 and months remaining ≥ 3 → actionable opportunity
- On pace to miss max by >$500 → highlight as priority

Note: contribution limits in this analysis are sourced from the snapshot's YTD Headroom table (user-maintained), not hardcoded. If the table is blank, ask the user to fill it before proceeding.

## Step 3 — Bracket Management

Using the filing status and marginal bracket from the snapshot:

**Traditional vs Roth guidance:**
- 22% bracket or below → Roth contributions generally favored (pay tax now at lower rate)
- 24% bracket → evaluate based on expected retirement bracket; traditional if expecting lower income in retirement
- 32%+ bracket → traditional 401k/IRA strongly favored (large current-year deduction)

**Roth conversion opportunity:**
- Check if current year income leaves room below the next bracket threshold
- If in 12% or 22% bracket with capacity: "You have ~$X of room before hitting the next bracket — consider a Roth conversion of up to $X from your Traditional 401k/IRA"

**Income deferral:**
- If bonus or irregular income is expected: note impact on bracket and whether deferring makes sense

## Step 4 — Tax-Loss Harvesting

From the Investment Allocation section, check for positions that may be underwater:
- If the snapshot doesn't include cost basis or unrealized gain/loss data, note that TLH assessment requires this data and suggest running `/investment-review` first
- If TLH candidates are present: estimate tax savings as unrealized loss × marginal rate
- Remind: 30-day wash sale rule applies; replacement fund must be substantially different

## Step 5 — Withholding Check

Based on W-4 status from snapshot:
- "Unknown" → recommend user check YTD federal withholding on their most recent pay stub against projected tax liability; suggest using the IRS Tax Withholding Estimator
- "No" → flag as priority: underpayment penalty risk if gap > $1,000

## Step 6 — Year-End Deadlines (Q3/Q4 Only)

If current month is July or later, add a deadline checklist:

| Action | Deadline | Est. Savings |
|--------|----------|-------------|
| Max 401k contributions | Dec 31 (payroll cutoff often Nov) | varies |
| Max HSA contributions | Apr 15 following year | varies |
| Roth IRA contributions | Apr 15 following year | |
| Tax-loss harvest | Dec 31 | varies |
| Charitable giving (cash) | Dec 31 | marginal rate × donation |
| Required Minimum Distributions | Dec 31 | penalty avoidance |
| 529 state deduction contributions | Dec 31 (most states) | varies |

Only show rows that are relevant given the user's situation.

## Output Structure

### Key Numbers
Bold at top: total remaining tax-advantaged headroom (all accounts combined), estimated tax savings from fully utilizing headroom, current bracket, months remaining in tax year.

### Contribution Headroom Table

| Account | YTD Contributed | Personal Limit | Remaining | Mo. to Max | Status |
|---------|-----------------|----------------|-----------|------------|--------|
| 401k | $X | $24,500 | $X | $X/mo | ✓/⚠/🔴 |
| Roth IRA | $X | $7,500 | $X | $X/mo | |
| HSA | $X | $8,750 | $X | $X/mo | |
| 529 | $X | $19,000 gift | $X | | |

### Tax-Saving Opportunities (Ranked by Dollar Impact)

List at least 3, ordered by estimated dollar impact:
1. **[Opportunity]** — Est. savings: $X — Action: [specific step]
2. ...

### Bracket Management Recommendation
One paragraph with specific Roth vs traditional guidance and any conversion or deferral opportunity.

### Withholding Status
One sentence on whether withholding is on track, with action if not.

### Year-End Deadline Checklist
(Q3/Q4 only — omit in Q1/Q2)

### Mermaid Chart

`xychart-beta` bar chart — YTD contributed vs remaining headroom per account:

```mermaid
xychart-beta
    title "Tax-Advantaged Contribution Progress"
    x-axis ["401k", "Roth IRA", "HSA", "529"]
    y-axis "Amount ($)" 0 --> [max_limit]
    bar [ytd_contributed_values]
    bar [remaining_headroom_values]
```

Caption: first bar = contributed YTD, second bar = remaining headroom.

## Handoffs
- `/investment-review` — if TLH candidates need full analysis or contribution routing needs portfolio context
- `/scenario-compare` — if a Roth conversion amount is large enough to model the long-term tax impact
- `/goal-modeling` — if 529 contribution decision affects a college goal timeline
