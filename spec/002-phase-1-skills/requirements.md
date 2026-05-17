# Requirements: Phase 1 Skill Suite

## Feature Overview

A suite of 12 Claude Code custom skills (11 analytical + 1 visualization utility) that enable structured financial reasoning without requiring any local infrastructure. Skills are invoked during manual sessions where the user provides aggregated financial data via a standardized input template (`financial-snapshot`). Claude Code performs the analysis and returns prioritized, actionable recommendations.

This is Phase 1 of a multi-phase Financial AI System. The entire value chain is: manual data entry → skill invocation → structured analysis → actionable output. No account syncing, database, or local LLM is required, but may be used if available.

---

## User Stories

### US-01: Session Initialization
As a user starting a financial planning session, I want a standardized snapshot template to fill out so that Claude has consistent, complete context without me needing to remember what to include.

**Acceptance criteria:**
- Skill prompts for all required data categories in a copy-paste-friendly template (markdown document or other stored on disk for ease of future reference)
- Template covers income, assets, liabilities, monthly cash flow, goals, tax context, and investment allocation
- Output confirms what data was received and flags any gaps
- Session can proceed to any other skill after snapshot is submitted

### US-02: Quarterly Strategy Review
As a user doing a quarterly review, I want a comprehensive prioritization session so that I know exactly where to focus my financial energy for the next 90 days.

**Acceptance criteria:**
- Skill ingests the financial-snapshot and any QTD updates
- Output ranks top 3–5 focus areas with rationale
- Each recommendation includes a specific action and expected impact
- Skill flags tradeoffs when priorities conflict (e.g., emergency fund vs. investing)

### US-03: Decision Modeling
As a user facing a major financial decision (buy vs. rent, pay off debt vs. invest, refi, job change), I want to model the alternatives so I can choose confidently.

**Acceptance criteria:**
- Skill accepts a decision description and relevant parameters
- Output compares at least 2 scenarios across financial impact, risk, and timeline
- Recommendation includes sensitivity to key assumptions
- Skill identifies what additional data would change the recommendation
- Create model visuals for ease of digestion, with easy to use modeling tools for exploring parameter variations

### US-04: Debt Paydown Planning
As a user with multiple debts, I want a paydown strategy so I minimize interest paid while maintaining cash flow.

**Acceptance criteria:**
- Skill accepts all debts (balance, rate, minimum payment)
- Output recommends avalanche or snowball ordering with rationale
- Includes payoff timeline and total interest cost for recommended vs. alternative strategies
- Flags refinancing opportunities if applicable

### US-05: Investment Portfolio Review
As a user wanting to review my investments, I want to understand allocation drift and contribution optimization so my portfolio stays aligned with my goals.

**Acceptance criteria:**
- Skill accepts current allocation and target allocation per account
- Output identifies drift beyond acceptable thresholds (e.g., >5%)
- Recommends rebalancing actions with specific buy/sell guidance
- Includes contribution optimization across tax-advantaged accounts (401k, Roth IRA, HSA, 529)

### US-06: Tax Optimization
As a user wanting to reduce my tax burden, I want proactive positioning guidance so I'm not leaving money on the table throughout the year.

**Acceptance criteria:**
- Skill accepts filing status, bracket, key deductions, and account types
- Output identifies at least 3 actionable tax-saving opportunities
- Covers contribution headroom (401k, IRA, HSA), tax-loss harvesting opportunities, and withholding accuracy, including personal and employer contribution limits
- Year-end version flags time-sensitive actions

### US-07: Goal Progress Tracking
As a user with multiple financial goals (emergency fund, home purchase, retirement, college for multiple children, vacation, car purcahse), I want gap analysis and timeline projections so I know if I'm on track.

**Acceptance criteria:**
- Skill accepts each goal with target amount, target date, current balance, and monthly contribution
- Output shows projected completion date vs. target date for each goal
- Flags goals at risk and recommends contribution adjustments
- Models impact of redirecting funds between goals
- Effective use of visualizations to illustrate goal progress at a glance

### US-08: Budget Diagnosis
As a user reviewing last month's or current month's spending to date, I want to identify leaks and get concrete fixes so I can improve my savings rate.

**Acceptance criteria:**
- Skill accepts monthly actuals by category
- Output identifies top 3 overspend categories with specific reduction suggestions
- Compares actuals to prior periods or targets if provided
- Recommends a revised budget allocation
- Track current month's spending against budget envelopes, tracking at-risk categories and identifying other envelopes to borrow from where needed
- Allow for month-to-month rollover for irregular categories and savings goals

### US-10: Financial Health Scoring
As a user starting a planning session or doing a periodic check-in, I want a holistic financial health scorecard so I can immediately see which dimensions need the most attention.

**Acceptance criteria:**
- Skill scores each financial dimension: liquidity, debt health, savings rate, investment diversification, goal progress, and tax efficiency
- Each dimension shows its score, the key metric driving it, and its status
- Output flags the weakest dimension as the top priority for the session
- Compares to prior scores if a previous snapshot is available
- Provides a visual scorecard at a glance

### US-09: Cash Flow Optimization
As a user managing monthly cash flow, I want bill timing and paycheck allocation guidance so I avoid shortfalls and maximize float.

**Acceptance criteria:**
- Skill accepts income dates, bill due dates, and fixed vs. variable expense split
- Output recommends an optimal pay cadence for bills relative to paycheck timing
- Identifies periods of cash flow risk and mitigation strategies
- Suggests automation opportunities (auto-pay, scheduled transfers)

---

## Functional Requirements

### P0 — Must Have (Foundation Tier)

| ID | Requirement |
|----|-------------|
| F-01 | `financial-snapshot` skill: master input template capturing income, assets, liabilities, monthly cash flow, goals, tax context, and investment allocation; template is saved as a markdown file on disk for reuse and future reference |
| F-02 | `financial-snapshot` skill: validates completeness of submitted data and flags missing or ambiguous fields |
| F-03 | `quarterly-strategy` skill: ingests snapshot + QTD context and produces ranked priority list with actions |
| F-04 | `scenario-compare` skill: accepts a decision description and 2+ scenarios and produces a structured comparison with recommendation; output includes visual model (ASCII table or markdown) and supports interactive parameter variation within the session |
| F-05 | All Tier 1 skills: reference the standardized snapshot data format established in F-01 |
| F-06 | All skills: produce output with prioritized, actionable recommendations — not just analysis |
| F-07 | All skills: define exactly what data to collect at the top of the prompt (no ambiguity) |

### P1 — Should Have (Planning Tier)

| ID | Requirement |
|----|-------------|
| F-08 | `debt-strategy` skill: accepts multi-debt inputs and outputs paydown ordering, timeline, and interest cost comparison; flags refinancing opportunities when applicable |
| F-09 | `investment-review` skill: accepts current and target allocation per account, outputs drift analysis, rebalancing actions, and contribution optimization across tax-advantaged accounts (401k, Roth IRA, HSA, 529) |
| F-10 | `tax-optimization` skill: outputs at least 3 actionable tax-saving opportunities from snapshot context, including contribution headroom analysis against personal and employer limits (401k, IRA, HSA) |
| F-11 | `goal-modeling` skill: projects completion dates and flags at-risk goals with contribution adjustments; output includes a visual progress summary (e.g., progress bars or timeline) across all goals at a glance |
| F-12 | All Tier 2 skills: indicate which other skills to invoke when analysis reveals out-of-scope issues |

### P2 — Nice to Have (Tactical Tier)

| ID | Requirement |
|----|-------------|
| F-13 | `budget-diagnosis` skill: accepts last month's or current month's actuals-to-date and outputs top overspend categories with specific cuts; tracks spending against defined budget envelopes, flags at-risk envelopes, and identifies other envelopes to borrow from when needed; supports month-to-month rollover for irregular categories and savings goals |
| F-14 | `cash-flow-optimizer` skill: outputs bill timing recommendations relative to paycheck cadence; identifies periods of cash flow risk with mitigation strategies; suggests automation opportunities (auto-pay, scheduled transfers) |
| F-15 | Skills suggest relevant follow-up skills at the end of their output |
| F-16 | `net-worth-tracker` skill: computes net worth across snapshots, breaks down by category (liquid / invested / real estate / debt), surfaces MoM/QoQ trend, and benchmarks against FI milestones |
| F-17 | `financial-health-score` skill: scores 6 financial dimensions (liquidity, debt health, savings rate, investment diversification, goal progress, tax efficiency) and flags the weakest as the session priority |

---

## Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NF-01 | **Privacy**: Skills must not encourage or require the user to include individual transaction data — only category-level aggregates |
| NF-02 | **Consistency**: All skills must accept the `financial-snapshot` format as their primary context input |
| NF-03 | **Low friction**: The snapshot template must be completable in under 10 minutes for a prepared user |
| NF-04 | **Actionability**: Every skill output must end with a numbered action list, not just observations |
| NF-05 | **Portability**: Skills are plain markdown files; no external dependencies, APIs, or local services required |
| NF-06 | **Cadence awareness**: Each skill must document its intended cadence (every session / monthly / quarterly / as needed) |
| NF-07 | **Scope handoff**: Each skill must identify when a topic exceeds its scope and name the skill to invoke instead |

---

## Constraints and Assumptions

- **Manual data entry only**: No account sync or automation in Phase 1. User copy-pastes or types financial data.
- **No local infrastructure**: Skills are Claude Code custom slash commands — markdown files only. No databases, servers, or local models.
- **Aggregated data only**: User provides category totals, balances, and rates — not individual transactions. This enforces the privacy boundary.
- **Claude Code context**: Skills are designed for Claude Code sessions, not claude.ai chat. Users are expected to have Claude Code installed.
- **US financial context**: Tax rules, account types (401k, IRA, HSA, 529), and terminology are US-centric for Phase 1. AU migration is out of scope until Phase 6.
- **Single-user**: Skills are designed for a single household/individual, not multi-account advisory scenarios.

---

## Out of Scope

- Automated account synchronization (SimpleFIN, Teller.io) — Phase 2
- Local database storage (DuckDB) — Phase 2
- Local LLM for transaction categorization (Ollama/Hermes) — Phase 3
- Dashboard visualization (Next.js) — Phase 4
- MCP server integration for direct DB access during sessions — Phase 5
- Monte Carlo simulation and multi-year projections — Phase 6
- Investment transaction execution or brokerage integration — never
- Tax return preparation or legal/CPA advice — never

---

## Success Metrics

| Metric | Target |
|--------|--------|
| All 12 skills implemented | 12/12 before Phase 2 begins |
| Skills validated against real data | At least 1 full session run with actual financial data per skill |
| Snapshot completion time | Under 10 minutes for a prepared user |
| Action list adoption | User acts on at least 1 recommendation per session |
| Skill handoff accuracy | Every session that needs a follow-up skill gets correctly directed |
| Zero infrastructure dependency | Skills run in a fresh Claude Code project with no setup beyond file placement |
