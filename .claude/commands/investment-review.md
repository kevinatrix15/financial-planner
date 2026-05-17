---
description: Portfolio allocation drift detection, rebalancing plan, and contribution optimization
allowed-tools: Read
---

## Cadence
Quarterly

## Purpose
Identify how far your current portfolio has drifted from your target allocation, generate specific rebalancing actions, and optimize where to direct new contributions across your tax-advantaged and taxable accounts.

## Required Input

Paste the Investment Allocation and Tax Context sections from your snapshot, or say "read latest" to load from `data/snapshots/`.

Also confirm:
- Target allocation (if not in snapshot, ask before proceeding)
- Whether you prefer to rebalance by trading existing holdings, redirecting contributions, or both

## Step 1 — Load Data

If "read latest", use Read tool to load from `data/snapshots/`. Extract the Investment Allocation and Tax Context sections, plus the YTD Contribution Headroom table.

## Step 2 — Compute Blended Allocation

Aggregate all accounts (401k, Roth IRA, HSA, brokerage, etc.) into a single blended portfolio view:

For each account, map its holdings to the standard asset classes in the target allocation. If an account uses a target-date fund, note that and estimate its approximate underlying allocation (e.g., 2055 fund ≈ 90% stocks / 10% bonds).

Compute:
- **Total portfolio value** = sum of all investment account balances
- **Current % per asset class** = asset class total ÷ total portfolio × 100
- **Drift** = current % − target % (positive = overweight, negative = underweight)
- Flag any asset class with |drift| > 5%

## Step 3 — Generate Rebalancing Actions

Prioritize in this order to minimize tax drag:
1. **Redirect new contributions** to underweight asset classes (no tax event)
2. **Rebalance within tax-advantaged accounts** (401k, IRA, HSA) — no tax consequences
3. **Rebalance taxable accounts last** — only if drift can't be corrected via steps 1–2

For each action, specify:
- Which account to act in
- Which fund/asset class to buy or sell
- Dollar amount or % of account

If drift is entirely correctable through contribution redirection, say so — no selling required.

## Step 4 — Contribution Optimization

Using the Tax Context and YTD Headroom table, determine the optimal order to direct new monthly contributions:

Priority framework:
1. **401k up to employer match** — guaranteed 50–100% return, always first
2. **HSA (if eligible)** — triple tax advantage; invest if EF is funded
3. **Pay off high-rate debt** (>6%) — guaranteed return, tax-equivalent yield often beats investing
4. **Max 401k remaining headroom** — if in 22%+ bracket, prioritize traditional; if in lower bracket or expect higher future rates, prioritize Roth
5. **Roth IRA** — after 401k, especially for younger earners or those expecting higher future bracket
6. **529** — if college goals are funded, fund other goals first
7. **Taxable brokerage** — after all tax-advantaged options exhausted

Show the remaining headroom per account and the recommended next-dollar priority.

## Output Structure

### Key Numbers
Bold at top: total portfolio value, number of asset classes drifted >5%, largest drift, recommended next-dollar account.

### Allocation Drift Table

| Asset Class | Target % | Current % | Drift | Status |
|-------------|----------|-----------|-------|--------|
| US Large Cap | X% | X% | +/-X% | ✓ / ⚠ >5% / 🔴 >10% |
| ... | | | | |

### Rebalancing Actions

List each action as a specific instruction:
- "In [account]: Buy $X of [fund] ([asset class])"
- "In [account]: Sell $X of [fund], Buy $X of [fund]"
- "Redirect [amount] of monthly [account] contribution from [fund] to [fund]"

If no rebalancing needed: "All asset classes within 5% of target. No action required."

### Contribution Optimization Plan

| Priority | Account | Recommended Action | Monthly Amount | Remaining Headroom |
|----------|---------|-------------------|----------------|-------------------|
| 1 | 401k | Contribute to match | $X | $X |
| 2 | HSA | Max out | $X | $X |
| ... | | | | |

### Mermaid Charts

Two pie charts — current allocation and target allocation:

```mermaid
pie title Current Allocation
    "US Large Cap" : value
    "US Small/Mid" : value
    "International" : value
    "Bonds" : value
    "REITs" : value
    "Cash" : value
```

```mermaid
pie title Target Allocation
    "US Large Cap" : value
    "US Small/Mid" : value
    "International" : value
    "Bonds" : value
    "REITs" : value
    "Cash" : value
```

## Handoffs
- `/tax-optimization` — if contribution headroom analysis reveals significant unused space
- `/scenario-compare` — if there's a decision between Roth vs traditional contributions worth modeling
- `/goal-modeling` — if rebalancing affects the investment return assumptions behind a goal projection
