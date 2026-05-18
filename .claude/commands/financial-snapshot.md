---
description: Fill out or update your financial snapshot — run this at the start of every session
allowed-tools: Read, Write
---

## Cadence
Every session

## Purpose
Establish complete financial context before any analytical work. The filled snapshot is saved to `data/snapshots/` and read by all other skills. Running this first ensures every skill has consistent, up-to-date input.

## Step 1 — Load or Start Snapshot

Check whether an existing snapshot is available:

```
data/snapshots/
```

If a recent snapshot exists (within 90 days), read it with the Read tool and ask the user:
> "I found your snapshot from [date]. Do you want to (1) use it as-is, (2) update specific sections, or (3) start fresh?"

If no snapshot exists, present the blank template below and ask the user to fill it out.

## Step 2 — Snapshot Template

Ask the user to fill in all sections. They can paste the filled template directly into the chat.

```
# Financial Snapshot
**Date:** YYYY-MM-DD
**Period:** [e.g., Q2 2026 / May 2026]
**Last updated:** YYYY-MM-DD

---

## Income

| Source            | Gross/Month | Net/Month | Notes                  |
|-------------------|-------------|-----------|------------------------|
| Primary salary    | $           | $         |                        |
| Spouse salary     | $           | $         |                        |
| Rental / side     | $           | $         |                        |

**Next expected changes:** 

---

## Assets

### Liquid & Near-Liquid
| Account              | Balance  | Notes  |
|----------------------|----------|--------|
| Checking 1           | $        |        |
| High-yield savings   | $        | emergency fund |

### Tax-Advantaged Investments
| Account            | Balance  | YTD Contributed | Employer YTD |
|--------------------|----------|-----------------|--------------|
| 401k — Traditional | $        | $               | $            |
| 401k — Roth        | $        | $               | (shared)     |
| Roth IRA           | $        | $               | N/A          |
| HSA                | $        | $               | $            |
| 529 — [Child name] | $        | $               | N/A          |

### Taxable Investments
| Account    | Balance  | Notes |
|------------|----------|-------|
| Brokerage  | $        |       |

### Real Assets
| Asset        | Est. Value | Mortgage Balance | Equity |
|--------------|------------|-----------------|--------|
| Primary home | $          | $               | $      |

---

## Liabilities

| Debt          | Balance | Rate | Min Pmt | Actual Pmt | Type        |
|---------------|---------|------|---------|------------|-------------|
| Mortgage      | $       | %    | $       | $          | mortgage    |
| Auto loan     | $       | %    | $       | $          | installment |
| Credit card 1 | $       | %    | $       | $          | revolving   |

---

## Monthly Cash Flow

### Fixed Expenses
| Category                   | Budget | Last Month Actual |
|----------------------------|--------|-------------------|
| Mortgage/Rent              | $      | $                 |
| Insurance (auto/home/life) | $      | $                 |
| Utilities (avg)            | $      | $                 |
| Subscriptions              | $      | $                 |
| Childcare / tuition        | $      | $                 |

### Variable Expenses
| Category             | Budget | Last Month Actual |
|----------------------|--------|-------------------|
| Groceries            | $      | $                 |
| Dining / takeout     | $      | $                 |
| Gas / transportation | $      | $                 |
| Entertainment        | $      | $                 |
| Kids activities      | $      | $                 |
| Healthcare / copays  | $      | $                 |
| Home maintenance     | $      | $                 |
| Miscellaneous        | $      | $                 |

### Savings & Investment Contributions
| Category                | Monthly | Notes               |
|-------------------------|---------|---------------------|
| 401k (employee)         | $       | employer match: $   |
| Roth IRA                | $       |                     |
| HSA                     | $       | employer contrib: $ |
| 529 contributions       | $       |                     |
| Brokerage               | $       |                     |
| Extra mortgage principal| $       |                     |
| Sinking funds           | $       |                     |

**Monthly surplus / deficit:** $

---

## Goals

| Goal                   | Target $ | Target Date | Current $ | Mo. Contribution | Priority |
|------------------------|----------|-------------|-----------|-----------------|----------|
| Emergency fund (X mo.) | $        | YYYY-MM     | $         | $               | 1        |
| [Child] college        | $        | YYYY        | $         | $               | 2        |
| Retirement             | $        | YYYY        | $         | $               | 3        |
| Home payoff            | $        | YYYY        | $         | $               | 4        |

---

## Tax Context

| Field                      | Value                    |
|----------------------------|--------------------------|
| Filing status              | MFJ / Single / HOH       |
| Estimated marginal bracket | %                        |
| Estimated effective rate   | %                        |
| Deduction method           | Standard / Itemized      |
| W-4 withholding on track?  | Yes / No / Unknown       |
| Tax-loss harvesting opps?  | Yes / No / Unknown       |

### YTD Contribution Headroom

| Account          | YTD Contributed | Personal Limit | Employer Limit | Remaining |
|------------------|-----------------|----------------|----------------|-----------|
| 401k (all types) | $               | $24,500        | $72,000 total  | $         |
| Roth IRA         | $               | $7,500         | N/A            | $         |
| HSA (family)     | $               | $8,750         | $              | $         |

---

## Investment Allocation

### Target Allocation
| Asset Class             | Target % |
|-------------------------|----------|
| US Stocks — Large Cap   | %        |
| US Stocks — Small/Mid   | %        |
| International           | %        |
| Bonds / Fixed Income    | %        |
| REITs                   | %        |
| Cash / Money Market     | %        |

### Current Allocation by Account
| Account          | Holdings / Fund | Allocation | Balance |
|------------------|-----------------|------------|---------|
| 401k Traditional | [fund name]     |            | $       |
| Roth IRA         | [fund name]     |            | $       |
| HSA              | [fund name]     |            | $       |
| Brokerage        | [fund name]     |            | $       |
```

## Step 3 — Validate Completeness

Once the user submits data, check each section:

| Section | Check |
|---------|-------|
| Income | At least one income source with gross and net filled |
| Assets — Liquid | At least one liquid account with balance |
| Assets — Investments | At least one investment account |
| Liabilities | All debts listed (even if $0 balance) |
| Monthly Cash Flow | Both budget and actual columns filled for at least fixed expenses |
| Goals | At least one goal with target amount and target date |
| Tax Context | Filing status and bracket filled |
| Investment Allocation | Target allocation adds to 100% |

Flag any section that is blank or incomplete with ⚠. Flag critical gaps (no income, no goals) with ✗. If a section is fully complete, mark ✓.

## Step 4 — Compute Key Metrics

Calculate and display prominently:

- **Net worth** = total assets − total liabilities
- **Savings rate** = (monthly contributions + surplus) ÷ gross monthly income × 100
- **Debt-to-income ratio** = total monthly debt payments ÷ gross monthly income × 100
- **Emergency fund coverage** = liquid savings ÷ total monthly fixed + variable expenses (in months)
- **Monthly surplus/deficit** = net income − fixed expenses − variable expenses − contributions

## Step 5 — Save Snapshot

Use the Write tool to save the filled snapshot to:
`data/snapshots/YYYY-[period].md`

For example: `data/snapshots/2026-Q2.md` or `data/snapshots/2026-05.md`

Confirm the save path to the user.

## Step 6 — Suggest Next Skills

Based on the snapshot data, recommend the most relevant next skill:

- Emergency fund < 3 months → `/budget-diagnosis` or `/cash-flow-optimizer`
- High-rate revolving debt (>15%) → `/debt-strategy`
- Investment allocation drift >5% on any asset class → `/investment-review`
- Any goal projected to miss target date → `/goal-modeling`
- Tax contribution headroom >$3,000 remaining and Q3/Q4 → `/tax-optimization`
- Unsure where to start → `/financial-health-score`
- Quarterly review → `/quarterly-strategy`

## Handoffs
- `/financial-health-score` — for a holistic view of which area needs the most attention
- `/quarterly-strategy` — for big-picture prioritization
- Any specific skill listed above based on the data
