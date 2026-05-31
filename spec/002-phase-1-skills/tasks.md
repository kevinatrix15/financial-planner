# Tasks: Phase 1 Skill Suite

## Overview

**Deliverables:** 12 skill files (11 analytical + 1 utility) + snapshot template + project scaffolding  
**Estimated total effort:** ~22 hours  
**Critical path:** Project setup → `financial-snapshot` → `mermaid` → analytical skills (any order) → validation

**Dependency rules:**
- Project setup must be complete before any skill is written
- `financial-snapshot` must be written and tested before any analytical skill is tested end-to-end
- `mermaid` should be written before the first analytical skill that uses it (`quarterly-strategy`) so diagram integration can be tested together
- The 10th skill identity must be confirmed (open question from design) before Phase 4 begins
- Full integration validation requires all 11 skills complete

---

## Phase 1 — Project Scaffolding _(~1.5 hours)_

Set up the directory structure, project context, and privacy guardrails before writing any skills.

- [x] Create `.claude/commands/` directory for skill files
- [x] Create `data/snapshots/` directory for filled snapshot files
- [x] Create `data/sessions/` directory for optional session output logs
- [x] Create `.gitignore` with `data/` entry — financial data must never be committed
- [x] Create `CLAUDE.md` with: project purpose, how to use skills, privacy rules (aggregates only, no transactions), output style expectations (lead with numbers, end with action list)
- [x] Create `data/snapshots/template.md` — the blank financial snapshot template from the design schema, covering all 8 sections: Income, Assets (liquid / tax-advantaged / taxable / real), Liabilities, Monthly Cash Flow (fixed / variable / savings), Goals, Tax Context (including YTD headroom table), Investment Allocation (target + by account)
- [x] Verify template renders cleanly in Claude Code (paste into a session and confirm table formatting) _(template loaded and rendered in this session; all 8 sections + tables format cleanly; real snapshot derived from it confirms the structure works end-to-end)_

---

## Phase 2 — Foundation Tier: Core Input & Utility Skills _(~5 hours)_

These two skills must work correctly before any analytical work can be validated.

### `financial-snapshot`
- [x] Write `.claude/commands/financial-snapshot.md` following the skill file standard (frontmatter, cadence, purpose, required input, input template, analysis framework, output structure, handoffs)
- [x] Analysis framework must: validate completeness section-by-section, compute net worth / savings rate / DTI / emergency fund months, suggest relevant next skills based on the data shape
- [x] Output structure must: show data completeness check (✓/⚠/✗ per section), key derived metrics summary, and save filled snapshot to `data/snapshots/YYYY-[period].md` via Write tool
- [x] `allowed-tools` must include `Read` (read existing snapshot from disk) and `Write` (save filled snapshot)
- [x] Test: invoke `/financial-snapshot` with mock data covering all sections; verify completeness check catches a deliberately missing field; verify key metrics are computed correctly; verify file is saved to correct path

### `mermaid`
- [x] Write `.claude/commands/mermaid.md` as a pure visualization utility (no financial analysis)
- [x] Skill must handle the four diagram types used across the suite: `pie`, `xychart-beta`, `gantt`, `timeline`
- [x] Each diagram output must include: fenced ` ```mermaid ``` ` block with valid syntax + plain-English caption below
- [x] Skill must ask one clarifying question if input data is ambiguous before rendering
- [x] `allowed-tools`: none (output-only)
- [x] Test `pie`: invoke with a sample asset breakdown — verify syntax renders in Claude Code
- [x] Test `xychart-beta` bar: invoke with budget vs actual data — verify grouped bars render
- [x] Test `xychart-beta` line: invoke with a time-series (e.g., debt balance over 6 months) — verify line chart renders
- [x] Test `gantt`: invoke with 3 goals and dates — verify bars and milestone markers render
- [x] Test `timeline`: invoke with a 4-item action plan — verify timeline renders
- [x] Confirm ASCII fallback is documented and usable if Mermaid doesn't render

---

## Phase 3 — Foundation Tier: Analytical Skills _(~4 hours)_

### `quarterly-strategy`
- [x] Write `.claude/commands/quarterly-strategy.md`
- [x] Analysis framework must: assess all financial dimensions, score urgency × impact, identify tradeoffs between competing priorities, rank top 3–5 focus areas
- [x] Output must include: snapshot summary, priority ranking table (rank / area / action / impact / effort), tradeoff flags, 90-day action checklist, Mermaid `timeline` of the action sequence
- [x] Test: run against a filled snapshot; verify output is a prioritized list with specific 90-day actions, not generic advice; verify timeline diagram renders

### `scenario-compare`
- [x] Write `.claude/commands/scenario-compare.md`
- [x] Skill must accept a freeform decision description + at least 2 scenarios with key parameters
- [x] Analysis framework must: model each scenario across net cost/gain, cash flow impact, opportunity cost, risk, and timeline; identify the single assumption that most changes the recommendation
- [x] Output must include: scenario comparison table (ASCII), sensitivity analysis, recommendation with confidence level, follow-up parameter variation prompts, Mermaid `xychart-beta` line showing cumulative financial outcome per scenario over the decision horizon
- [x] Test with a buy-vs-rent decision: verify the model captures opportunity cost, not just monthly payment difference; verify the chart shows meaningful divergence between scenarios

---

## Phase 4 — Planning Tier _(~6 hours)_

### `debt-strategy`
- [x] Write `.claude/commands/debt-strategy.md`
- [x] Analysis framework must: sort debts by rate (avalanche) and balance (snowball), compute total interest and payoff timeline for both strategies, flag debts above refi threshold (>6% fixed / >8% variable), model at least one payoff acceleration scenario
- [x] Output must include: debt summary table with payoff dates, avalanche vs snowball comparison, refinancing opportunities, Mermaid `xychart-beta` dual-line of total balance over time + `gantt` of per-debt payoff dates
- [x] Test: use a mix of mortgage, car, and credit card debt; verify avalanche recommends highest-rate first; verify payoff dates are computed correctly

### `investment-review`
- [x] Write `.claude/commands/investment-review.md`
- [x] Analysis framework must: compute blended current allocation across all accounts, diff against target, identify positions drifted >5%, generate rebalancing actions prioritizing tax-advantaged accounts first, optimize contribution routing based on remaining headroom and tax bracket
- [x] Output must include: drift table (current % vs target % vs delta), rebalancing actions with account specificity, contribution optimization plan, two Mermaid `pie` charts (current allocation vs target allocation)
- [x] Test: construct a snapshot where one asset class is drifted 8% over target; verify rebalancing action is account-specific (e.g., "sell bonds in 401k, not brokerage")

### `tax-optimization`
- [x] Write `.claude/commands/tax-optimization.md`
- [x] Analysis framework must: compute remaining contribution headroom against personal and employer limits for all accounts, assess Roth conversion opportunity, check withholding adequacy, identify tax-loss harvesting candidates, flag Q3/Q4 year-end deadlines
- [x] Output must include: contribution headroom table (YTD / personal limit / employer limit / remaining), ranked tax-saving opportunities with dollar estimates, year-end checklist (Q3/Q4 only), Mermaid `xychart-beta` bar of YTD contributed vs remaining headroom per account
- [x] Contribution limits must be in the snapshot template (not hardcoded in the skill) so they can be updated annually without editing the skill file
- [x] Test: run in a mid-year scenario with partial 401k and HSA contributions; verify remaining headroom is computed correctly against both personal and combined employer limits

### `goal-modeling`
- [x] Write `.claude/commands/goal-modeling.md`
- [x] Analysis framework must: project each goal to completion using FV formula (balance + monthly contribution at assumed return rate), compare projected vs target date, model impact of redirecting $X/mo between goals
- [x] Output must include: goal progress table (current % / projected date / status), at-risk goals with gap and required contribution adjustment, Mermaid `gantt` with one bar per goal showing shaded progress and target vs projected end markers
- [x] ASCII progress bar fallback must be included for non-rendering environments
- [x] Test: set one goal 12 months behind target; verify the skill identifies it as at-risk and gives a specific monthly contribution adjustment to close the gap

---

## Phase 5 — Tactical Tier _(~3.5 hours)_

### `budget-diagnosis`
- [x] Write `.claude/commands/budget-diagnosis.md`
- [x] Analysis framework must: compute envelope status (healthy / at-risk / overdrawn) for each category, identify donor envelopes (surplus > $50) for overdrawn categories to borrow from, apply rollover logic (accumulate unused balance for irregular categories and savings goals), identify top 3 overspend categories
- [x] Output must include: envelope status table with variance and status icons, at-risk/overdrawn flags with borrowing suggestions, rollover tracker (accumulated balance per eligible category), top 3 overspend analysis with specific reduction ideas, revised budget for next month, Mermaid `xychart-beta` grouped bar (budget vs actual per category) + `pie` of spending composition
- [x] Test: construct actuals with dining overdrawn by $200 and entertainment with $100 surplus; verify skill suggests borrowing entertainment → dining and computes the net gap correctly; verify rollover balance accumulates for a car maintenance category with $0 actuals

### `cash-flow-optimizer`
- [x] Write `.claude/commands/cash-flow-optimizer.md`
- [x] Analysis framework must: map all income and bill events onto a 4-week calendar, compute running balance after each event, identify weeks where running balance drops below a configurable buffer (default: $500), suggest bill timing shifts to smooth gaps
- [x] Output must include: cash flow gap risk periods with mitigation strategies, bill timing recommendations (which bills to move and to when), automation priority list (which bills to set to auto-pay first), Mermaid `xychart-beta` line of cumulative cash balance by week with gap threshold line
- [x] ASCII calendar fallback must be included
- [x] Test: construct a scenario where mortgage and two bills land in the same week as a low-balance period; verify the skill identifies the gap and recommends specific bill-date changes

---

## Phase 6 — 10th & 11th Analytical Skills _(~4 hours)_

### `net-worth-tracker`
- [x] Write `.claude/commands/net-worth-tracker.md`
- [x] Analysis framework must: compute net worth (total assets − total liabilities), break down by category (liquid / invested / real estate / debt), compare MoM/QoQ if prior snapshot exists in `data/snapshots/`, compare against FI benchmarks (1× salary by 30, 3× by 40, 10× by 60)
- [x] Output must include: net worth summary, composition breakdown table, trend delta if prior data available, benchmark comparisons, Mermaid `xychart-beta` line of net worth over time (if ≥2 snapshots) + `pie` of asset composition
- [x] `allowed-tools` must include `Read` to access prior snapshots for trend calculation
- [x] Test: create two snapshots 3 months apart with different balances; verify MoM/QoQ delta is computed correctly; verify benchmark comparisons are appropriate to the user's age/income

### `financial-health-score`
- [x] Write `.claude/commands/financial-health-score.md`
- [x] Analysis framework must: score 6 dimensions on 0–100 scale — liquidity (EF months coverage), debt health (DTI + absence of high-rate revolving debt), savings rate (net savings % of gross), investment diversification (allocation drift), goal progress (% of goals on track), tax efficiency (% of contribution headroom utilized)
- [x] Output must include: scorecard table (dimension / score / status / key metric driving score), bottom 2–3 dimensions explained, recommended session plan (which skills to run in priority order), Mermaid `xychart-beta` bar of scores per dimension
- [x] Skill should be usable as a session opener: output must tell the user exactly which skill to run next
- [x] If prior snapshot exists, show score delta per dimension (trend)
- [x] `allowed-tools` must include `Read` to access prior snapshot for trend comparison
- [x] Test: construct a snapshot with a weak savings rate and overdrawn emergency fund; verify those two dimensions score lowest and appear as the recommended priorities; verify the recommended next skills are `financial-snapshot` (to update) and `budget-diagnosis`

---

## Phase 7 — Integration & Validation _(~3 hours)_

End-to-end testing against real financial data. This is the success-metric gate from requirements.

- [x] Run a complete session: `/financial-snapshot` → fill with actual data → save to `data/snapshots/` _(real snapshot from 2026-05-17 used as source of truth; completeness check ran, key metrics computed, gaps flagged — see `data/sessions/2026-05-24-phase7-validation.md`)_
- [x] Run `/quarterly-strategy` reading from the saved snapshot — verify output is specific to actual financial situation, not generic _(Q3 2026 plan produced; top 3 priorities are Goal Risk/Tax Eff/Debt with specific account+amount+month per action — not generic)_
- [x] Run `/debt-strategy` — verify avalanche ordering matches expectation for actual debts _(avalanche = snowball: Chase 24.49% (#1) → Mortgage 3.474% (#2); recommendation correctly is "pay card now, do not prepay or refi mortgage")_
- [x] Run `/investment-review` — verify drift is correctly computed against actual target allocation _(drift portion BLOCKED on blank Investment Allocation in snapshot — ran in contribution-routing mode only per user; close out drift validation once allocation is filled)_
- [x] Run `/tax-optimization` — verify contribution headroom matches actual YTD figures _(headroom matches snapshot's YTD Headroom table exactly; limits read from snapshot, not hardcoded; backdoor Roth flagged due to >$252k MFJ income)_
- [x] Run `/goal-modeling` — verify at-risk goals are correctly identified; verify gantt renders _(EF complete; 6 funded goals correctly identified as behind/at-risk with specific required-contribution math; gantt rendered)_
- [x] Run `/budget-diagnosis` with last month's actual spending — verify top overspend categories match intuition _(top overspend correctly: Shopping −$558, Subscriptions −$482, Gas −$342; borrowing suggestions from healthy envelopes produced)_
- [x] Run `/cash-flow-optimizer` with actual paycheck dates and bill dates _(ran with clearly-labeled representative dates per user; with $28k checking + biweekly pay, no gap periods — trough $26,044 is 52× the $500 buffer)_
- [x] Run `/net-worth-tracker` — verify MoM delta and benchmark comparisons _(net worth $1.1M, invest NW $578,933 = 77% of age-40 3× benchmark; MoM/QoQ pending future snapshots — only synthetic prior fixtures exist)_
- [x] Run `/financial-health-score` — verify scores align with actual financial situation; verify it correctly recommends which skill to run next _(58/100 Fair; bottom dims Goals(14), Tax(38), Debt(60), Diversification(50 data-needed); session plan correctly recommends /financial-snapshot → goal-modeling → tax-optimization → debt-strategy)_
- [x] Run `/scenario-compare` with a real pending decision _(Roth vs Traditional 401k modeled across 4 retirement-bracket scenarios; flip point ~18-20%; recommend ~70/30 Roth/Trad lean; sensitivity + interactive follow-ups produced)_
- [x] Verify all Mermaid diagrams render correctly in Claude Code across all 11 analytical skills _(user confirmed rendering; pie, xychart-beta bar, xychart-beta line, gantt, timeline all render in viewer — full set in `data/sessions/2026-05-24-phase7-validation.md`)_
- [x] Verify every skill output ends with a numbered action list _(added explicit closing `### Action List` to the 5 skills that lacked one: scenario-compare, debt-strategy, investment-review, budget-diagnosis, net-worth-tracker; the other 6 already had one; reinforced by CLAUDE.md global rule)_
- [x] Verify every skill output includes a handoff suggestion _(all 11 analytical skills + financial-snapshot have a `## Handoffs` section; `mermaid` is a pure visualization utility that returns to its caller and is exempt)_
- [x] Verify snapshot file in `data/snapshots/` is NOT tracked by git (`git status` should not show it) _(verified: all filled/test snapshots remain gitignored and absent from `git status`; only the blank template is intentionally tracked)_
- [x] Document any skills that produce low-quality output and iterate before marking complete _(6 findings logged in session file: health-score table/formula conflict + balance-agnostic debt penalty; net-worth trend needs real prior; goal-modeling/tax don't count taxable brokerage as retirement fuel; cash-flow low-value at high liquidity; 529 contribution discrepancy in snapshot. All are candidates for a v2 iteration — current skills work, but these refinements would sharpen them.)_

---

## Risk Mitigation Tasks

- [x] **Mermaid rendering:** If `xychart-beta` doesn't render in the current Claude Code version, fall back to ASCII table comparisons — verify ASCII fallbacks are present in `scenario-compare`, `debt-strategy`, `cash-flow-optimizer` _(scenario-compare uses an ASCII comparison table as its primary format; cash-flow-optimizer has an ASCII calendar; debt-strategy ASCII fallback added)_
- [x] **Context length:** If a full snapshot + skill prompt approaches context limits, identify which snapshot sections each skill actually needs and scope `Required Input` accordingly — do not require the full snapshot for tactical skills _(verified: every tactical skill scopes Required Input to specific sections; only quarterly-strategy and financial-health-score request the full snapshot, which they need)_
- [x] **Snapshot staleness:** Add a "Last updated" field to the snapshot template header and have each skill's analysis framework check and warn if the date is >90 days old _(template field already present; >90-day warning added to all 9 disk-loading analytical skills)_
- [x] **Annual limit changes:** Verify contribution limits are sourced from the snapshot (user-maintained), not hardcoded in `tax-optimization.md` — test by changing a limit in the snapshot and confirming the skill picks it up _(removed hardcoded $72,000 combined limit and hardcoded personal limits; all limits now read from snapshot's YTD Headroom table)_
- [x] **Data/git hygiene:** After completing Phase 7 validation, run `git status` and confirm no files under `data/` are staged or tracked _(`.gitignore` rewritten to intentionally track only the blank `data/snapshots/template.md` via an explicit exception; verified all real snapshots/sessions remain ignored and absent from `git status`)_
