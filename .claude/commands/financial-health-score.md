---
description: Six-dimension financial health scorecard — session opener that tells you exactly which skill to run next
allowed-tools: Read
---

## Cadence
Quarterly, or as a session opener before any other skill

## Purpose
Score your financial health across six dimensions on a 0–100 scale, identify which areas need the most attention, and produce a prioritized session plan — telling you exactly which skills to run and in what order. If a prior snapshot exists, show how each dimension has changed.

## Required Input

Say "read latest" to load from `data/snapshots/`, or paste the full snapshot directly.

For trend comparison, also say "read prior" — the skill will load the previous snapshot from `data/snapshots/` to show score deltas per dimension.

## Step 1 — Load Data

If "read latest", use Read tool to load the most recent snapshot from `data/snapshots/`. If trend requested, also read the prior snapshot (second-most-recent file in the directory).

Extract from snapshot:
- **Liquid assets** (checking, savings, HYSA, money market)
- **Monthly fixed + variable expenses** (total monthly spend)
- **Monthly net income** (after tax)
- **Monthly gross income**
- **Monthly savings and contributions** (total flowing to savings/investments)
- **All debt balances and interest rates** (to compute DTI and flag high-rate debt)
- **Monthly minimum debt payments** (for DTI)
- **Investment allocation** (current % vs target % per asset class)
- **Goals table** (current balance, target, projected date, status)
- **Tax Context YTD headroom table** (YTD contributed vs personal limit per account)

## Step 2 — Score All Six Dimensions

Score each dimension 0–100. Partial credit is linear unless stated otherwise.

---

### Dimension 1: Liquidity (Emergency Fund Coverage)
**Metric**: Emergency fund months = liquid assets ÷ monthly expenses

| Coverage | Score |
|----------|-------|
| ≥ 6 months | 100 |
| 4–5.9 months | 70–99 (linear) |
| 2–3.9 months | 30–69 (linear) |
| 1–1.9 months | 10–29 (linear) |
| < 1 month | 0–9 (linear) |

Score = min(100, (EF months ÷ 6) × 100)

**Status label**:
- 90–100: Fully funded
- 70–89: Nearly there
- 40–69: Building
- 0–39: Priority gap

---

### Dimension 2: Debt Health
**Metric**: Two sub-scores averaged.

**Sub-score A — DTI** (monthly debt payments ÷ gross monthly income):
- DTI < 15%: 100
- 15–20%: 75
- 20–28%: 50
- 28–36%: 25
- > 36%: 0

**Sub-score B — High-rate revolving debt penalty**:
- No revolving/personal debt above 10% APR: 100
- Has revolving debt 10–18% APR: 60
- Has revolving debt above 18% APR: 20
- Has revolving debt above 25% APR: 0

**Debt health score** = (Sub-score A + Sub-score B) ÷ 2

**Status label**:
- 90–100: Healthy
- 70–89: Manageable
- 40–69: Needs attention
- 0–39: High priority

---

### Dimension 3: Savings Rate
**Metric**: Net savings rate = (total monthly savings + contributions) ÷ gross monthly income

| Rate | Score |
|------|-------|
| ≥ 20% | 100 |
| 15–19.9% | 75 |
| 10–14.9% | 50 |
| 5–9.9% | 25 |
| < 5% | 0 |

Score = min(100, (savings rate ÷ 0.20) × 100)

**Status label**:
- 90–100: High saver
- 70–89: On track
- 40–69: Room to grow
- 0–39: Priority gap

---

### Dimension 4: Investment Diversification
**Metric**: Max drift from target across all tracked asset classes

Compare current allocation % to target % for each asset class. Find the largest single-asset-class drift (absolute value).

| Max drift | Score |
|-----------|-------|
| ≤ 5% | 100 |
| 5–10% | 70 |
| 10–15% | 40 |
| > 15% | 10 |

If no investment allocation data in snapshot, score = 50 and flag as "data needed."

**Status label**:
- 90–100: Well-balanced
- 70–89: Minor drift
- 40–69: Rebalancing needed
- 0–39: Significant drift

---

### Dimension 5: Goal Progress
**Metric**: % of goals currently on track (projected date ≤ target date)

Count goals with a target date and contribution > $0. For each, assess on-track status using the same projection logic as `/goal-modeling` (FV formula). 

Score = (goals on track ÷ total goals with target dates) × 100

If no goals defined: score = 50, flag as "no goals tracked."

**Status label**:
- 90–100: All on track
- 70–89: Mostly on track
- 40–69: Some at risk
- 0–39: Most off track

---

### Dimension 6: Tax Efficiency
**Metric**: % of annual contribution headroom being utilized

For each tax-advantaged account in the YTD headroom table, compute utilization rate at the current pace:
- Annualized contribution = YTD contributed ÷ months elapsed × 12
- Utilization % = min(100%, annualized contribution ÷ personal limit)

**Overall tax efficiency** = average utilization % across all tracked accounts (401k, IRA, HSA, 529 if applicable)

Score = min(100, average utilization × 100)

If no YTD data available: score = 50, flag as "data needed."

**Status label**:
- 90–100: Maximizing tax shelter
- 70–89: Good utilization
- 40–69: Leaving headroom unused
- 0–39: Underutilizing tax accounts

---

## Step 3 — Compute Overall Score

**Overall health score** = average of all six dimension scores (equal weight)

Classify overall:
- 85–100: Strong — maintain momentum
- 70–84: Good — a few areas to sharpen
- 50–69: Fair — 2–3 dimensions need focused attention
- 0–49: Needs work — prioritize the bottom dimensions immediately

## Step 4 — Trend Analysis (if prior snapshot available)

For each dimension, compare score to the prior snapshot's scores (compute prior scores using same formulas). Show delta per dimension: +X or −X.

If prior snapshot is not available, note that trend comparison will be available after the next quarterly review.

## Step 5 — Session Plan

**Check for data gaps first.** If any dimension was flagged "data needed" or "no goals tracked" (Goal Progress, Tax Efficiency, or Investment Diversification scored as 50 due to missing data), prepend `/financial-snapshot` as step 0 of the session plan with the note: "Update your snapshot to fill in missing sections before analytical skills can score accurately."

Identify the bottom 2–3 dimensions by score (excluding any "data needed" placeholders). Map each to the skill most likely to improve it:

| Dimension | Recommended Skill |
|-----------|------------------|
| Liquidity | `/budget-diagnosis` (find savings to redirect) or `/quarterly-strategy` |
| Debt Health | `/debt-strategy` |
| Savings Rate | `/budget-diagnosis` or `/cash-flow-optimizer` |
| Investment Diversification | `/investment-review` |
| Goal Progress | `/goal-modeling` |
| Tax Efficiency | `/tax-optimization` |

Order the recommended skills by dimension score (lowest first). If two dimensions map to the same skill, list it once with both dimensions noted.

State explicitly: "Run these skills in this order today: 1. X, 2. Y, 3. Z"

## Output Structure

### Overall Score

**Financial Health Score: XX/100** — [classification label]
Bold: overall score, classification, number of dimensions scored, date of snapshot.

### Dimension Scorecard

| Dimension | Score | Status | Key Metric | Trend |
|-----------|-------|--------|-----------|-------|
| Liquidity | XX | Fully funded | X.X months EF | +X |
| Debt Health | XX | Manageable | DTI X%, no high-rate debt | +X |
| Savings Rate | XX | On track | X% of gross income | −X |
| Investment Diversification | XX | Minor drift | Max X% drift | +X |
| Goal Progress | XX | Mostly on track | X of X goals on track | — |
| Tax Efficiency | XX | Good utilization | X% avg headroom used | +X |
| **Overall** | **XX** | **[Label]** | | **+X** |

Omit Trend column if no prior snapshot.

### Bottom Dimensions — Detail

For each of the bottom 2–3 dimensions (lowest scores), one paragraph:
- What the score means in plain language
- What specific number is driving the low score
- What it would take to move from current score to the next tier
- Which skill addresses it

### Your Session Plan

A clearly numbered action list:

**Run these skills today:**
1. `/skill-name` — addresses [dimension] (score: XX) — [one-line reason]
2. `/skill-name` — addresses [dimension] (score: XX) — [one-line reason]
3. `/skill-name` — addresses [dimension] (score: XX) — [one-line reason]

If overall score is ≥ 85: "Your finances are in strong shape. Consider running `/quarterly-strategy` to set 90-day focus areas, or `/net-worth-tracker` to review progress against FI benchmarks."

### Mermaid Chart

Bar chart of all six dimension scores on a 0–100 scale:

```mermaid
xychart-beta
    title "Financial Health Score by Dimension"
    x-axis ["Liquidity", "Debt Health", "Savings Rate", "Diversification", "Goal Progress", "Tax Efficiency"]
    y-axis "Score (0-100)" 0 --> 100
    bar [score1, score2, score3, score4, score5, score6]
```

Caption: identify which bars are below 70 (attention needed) and which are above 85 (strong).

If trend data available, add a second bar series for prior scores:

```mermaid
xychart-beta
    title "Financial Health Score — Current vs Prior"
    x-axis ["Liquidity", "Debt Health", "Savings Rate", "Diversification", "Goal Progress", "Tax Efficiency"]
    y-axis "Score (0-100)" 0 --> 100
    bar [prior_scores]
    bar [current_scores]
```

## Handoffs
- `/quarterly-strategy` — if you want a 90-day action plan after seeing the scorecard
- `/net-worth-tracker` — if your scores are strong and you want to assess FI progress
- Run the skills listed in "Your Session Plan" in order
