---
description: Model and compare 2+ alternatives for a major financial decision
argument-hint: <decision description>
---

## Cadence
As needed — before any major financial decision

## Purpose
Structure a financial decision by modeling 2+ scenarios side by side across cost, cash flow, opportunity cost, risk, and timeline. Produces a recommendation with explicit sensitivity analysis so you know which assumptions drive the outcome. Supports interactive parameter variation within the session.

## Required Input

Describe the decision you're facing. Include:
- What you're deciding between (e.g., "buy a house vs. continue renting")
- Key parameters for each option (price, rate, term, timing, etc.)
- Relevant snapshot context (income, assets, current housing cost, goals affected)

If key parameters are missing, ask for them before modeling. Don't model with unknowns — state the assumption clearly if one must be made.

## Step 1 — Define Scenarios

Confirm the scenarios with the user before calculating. Label them clearly (Scenario A, B, C or descriptive names). Identify:
- The **time horizon** for comparison (1yr, 5yr, 10yr, 30yr — use the natural horizon for the decision type)
- The **primary financial metric** to compare (net cost, net worth impact, monthly cash flow, total interest paid, etc.)
- Any **shared baseline** (e.g., current financial state from snapshot)

## Step 2 — Model Each Scenario

For each scenario, calculate across all relevant dimensions:

**Cost dimensions:**
- Upfront cost (down payment, closing costs, fees)
- Ongoing monthly cost (payment, insurance, taxes, maintenance)
- Total cost over the time horizon

**Opportunity cost:**
- What would the upfront capital earn if invested instead? (use 7% real return as default; state the assumption)
- What does the monthly cost difference buy if invested?

**Cash flow impact:**
- Change in monthly surplus/deficit vs. current state
- Break-even point (if applicable)

**Risk factors:**
- What goes wrong with this scenario? How bad?
- What assumptions are most fragile?

**Tax implications:**
- Deductions gained or lost
- Capital gains considerations
- Any bracket effects

## Step 3 — Sensitivity Analysis

Identify the **single assumption that most changes the recommendation**. Model it at 3 values (pessimistic / base / optimistic):

Example: "If home appreciation is 2% instead of 4%, Scenario A breaks even at year 12 instead of year 8."

State clearly: "The recommendation flips from A to B if [assumption] changes from X to Y."

## Step 4 — Recommendation

State a clear recommendation with:
- **Recommended scenario** and why
- **Confidence level** (High / Medium / Low) based on how sensitive the outcome is to key assumptions
- **Conditions that would change the recommendation** (e.g., "if rates drop below X%", "if you plan to move within 5 years")
- **What additional data would most improve confidence**

## Output Structure

### Decision Summary
One paragraph: what's being decided, the scenarios, and the time horizon.

### Scenario Comparison Table (ASCII)

```
                        │ Scenario A      │ Scenario B      │ Scenario C      │
────────────────────────┼─────────────────┼─────────────────┼─────────────────┤
Upfront cost            │ $X              │ $X              │ $X              │
Monthly cost            │ $X              │ $X              │ $X              │
Opp. cost (capital)     │ $X              │ $X              │ $X              │
Total cost (Xyr)        │ $X              │ $X              │ $X              │
Monthly cash flow Δ     │ +/-$X           │ +/-$X           │ +/-$X           │
Break-even              │ Xmo/yr          │ N/A             │ Xmo/yr          │
Key risk                │ [describe]      │ [describe]      │ [describe]      │
```

### Sensitivity Analysis
State the key swing assumption and show the three outcomes.

### Recommendation
Bold the recommended scenario. State confidence level and the conditions that would flip the recommendation.

### Mermaid Chart
Emit an `xychart-beta` line chart showing cumulative net cost (or net worth impact) of each scenario over the time horizon:

```mermaid
xychart-beta
    title "[Decision] — Cumulative Cost Over Time"
    x-axis [year_labels]
    y-axis "Cumulative Cost ($)" min --> max
    line [scenario_a_values]
    line [scenario_b_values]
```

Caption must identify which line is which scenario and where they cross (if applicable).

### Interactive Follow-ups
After the recommendation, offer specific parameter variations the user can ask about:
- "Ask me: what if the interest rate is X% instead?"
- "Ask me: what if I stay for only 5 years?"
- "Ask me: what if home appreciation is 0%?"

When the user asks a variation, re-run the relevant calculation and show only what changed — don't repeat the full output.

## Handoffs
- `/debt-strategy` — if the decision involves refinancing or new debt
- `/goal-modeling` — if the decision significantly affects a goal timeline
- `/tax-optimization` — if tax implications are material to the recommendation
- `/investment-review` — if a large lump sum is involved and asset allocation needs updating
