# Financial AI System — Master Plan

## Vision

A privacy-conscious, AI-augmented personal financial system combining a local LLM (for data handling and narration), Claude Code (for strategic reasoning), and eventually a local dashboard — built incrementally, starting with manual strategic sessions and automating the data layer over time.

---

## Architecture (Target State)

```
┌─────────────────────────────────────────────────────────────────┐
│                        LOCAL MACHINE                            │
│                                                                  │
│  ┌──────────────┐    ┌─────────────────┐    ┌────────────────┐  │
│  │  Data Layer  │    │  Local AI Layer │    │   Dashboard    │  │
│  │              │    │                 │    │                │  │
│  │ SimpleFIN /  │───▶│ Ollama + Hermes │───▶│  React/Next.js │  │
│  │ Teller.io    │    │ (categorization,│    │  (localhost)   │  │
│  │ (read-only)  │    │  narration,     │    │                │  │
│  │              │    │  summaries)     │    └────────────────┘  │
│  │ DuckDB       │    │                 │             │          │
│  │ (local store)│    │ MCP Servers     │             │          │
│  └──────────────┘    │ (tool calling)  │             │          │
│                      └─────────────────┘             │          │
│                               │                      │          │
└───────────────────────────────┼──────────────────────┼──────────┘
                                │ anonymized/           │
                                │ aggregated only       │
                                ▼                       ▼
                     ┌──────────────────┐    ┌──────────────────┐
                     │   Claude Code    │    │   Claude.ai      │
                     │   (strategic     │    │   (ad-hoc        │
                     │   sessions)      │    │   advisor chat)  │
                     └──────────────────┘    └──────────────────┘
```

### Privacy Model

- **Individual transactions** never leave the local machine
- **Local LLM** (Hermes via Ollama) handles all raw data processing
- **Claude Code** receives only anonymized aggregates and summaries
- **Cloud boundary** is enforced at the summary layer

---

## Implementation Phases

### Phase 1 — Manual Strategic Sessions (Current)

Build Claude Code skills for structured financial reasoning. Data is entered manually using a standardized snapshot template. No automation, no local infrastructure required — just skills and discipline.

**Deliverables:** 10 Claude Code skills (see Skill Suite below)

**Status:** In progress

---

### Phase 2 — Local Data Foundation

Set up local storage and account sync. All data stays on-device.

- **Account aggregator:** SimpleFIN Bridge (preferred, privacy-first) or Teller.io (cleaner API)
- **Database:** DuckDB — analytical, in-process, file-based
- **Schema:** transactions, account_balances, net_worth_snapshots, budget_envelopes, goals
- **Sync:** scheduled pull via SimpleFIN Bridge relay

---

### Phase 3 — Local AI Layer

Deploy a local LLM to automate data wrangling and narration.

- **Runtime:** Ollama
- **Model:** NousResearch Hermes-3 (strong tool-calling and instruction following)
  - 8B for CPU-only machines; 70B if GPU available
- **Responsibilities:**
  - Transaction categorization
  - Anomaly detection ("this merchant is unusual")
  - Budget variance narration
  - Weekly/monthly summary generation (structured JSON → dashboard)

---

### Phase 4 — Dashboard

Local Next.js web app running on localhost.

**Core panels:**
- Net worth timeline with goal overlay
- Monthly cash flow (income vs spending, surplus trend)
- Spending by category (heatmap or treemap)
- Budget envelope tracker (actual vs budgeted)
- Debt paydown progress
- Investment allocation + drift from target
- AI narration feed (Hermes-generated weekly commentary)
- "Start Strategic Session" — packages a summary and opens Claude Code

---

### Phase 5 — Claude Code MCP Integration

Give Claude Code read-only access to local DuckDB during strategic sessions, so data doesn't need to be copy-pasted.

**MCP servers to build:**
- `mcp-finance-db` — queries balances, net worth, spending aggregates
- `mcp-budget` — reads envelope actuals and variance
- `mcp-goals` — reads goal progress and projected completion dates

---

### Phase 6 — Advanced Workflows (Ongoing)

- Scenario modeling with Monte Carlo simulation
- Tax optimization with bracket awareness
- Investment rebalancing alerts with drift thresholds
- Multi-year projection engine
- Migration path for Brisbane/AU financial context if needed

---

## Skill Suite

All skills live in the Claude Code project. They define what data to collect, how to reason about it, and what output to produce.

### Tier 1 — Foundation (Build First)

| Skill | Purpose | Cadence |
|-------|---------|---------|
| `financial-snapshot` | Master input template — standardizes how financial state is summarized before any session | Every session |
| `quarterly-strategy` | Big-picture prioritization across all financial dimensions | Quarterly |
| `scenario-compare` | Model a specific decision (buy vs lease, refi, job change, etc.) | As needed |

### Tier 2 — Planning

| Skill | Purpose | Cadence |
|-------|---------|---------|
| `debt-strategy` | Paydown ordering, refinancing analysis, payoff timelines | Monthly / as needed |
| `investment-review` | Allocation drift, rebalancing logic, contribution optimization | Quarterly |
| `tax-optimization` | Year-round tax positioning, bracket management, deduction strategy | Quarterly + year-end |
| `goal-modeling` | College fund, home payoff, retirement — gap analysis and timeline | Quarterly |

### Tier 3 — Tactical

| Skill | Purpose | Cadence |
|-------|---------|---------|
| `budget-diagnosis` | Paste last month's actuals; identify leaks and get fixes | Monthly |
| `cash-flow-optimizer` | Bill timing, paycheck allocation, float management | Monthly / as needed |

### Skill Design Principles

Each skill must:
1. Define exactly what data to collect before running (no ambiguity)
2. Provide a structured input template (consistent, low-friction data entry)
3. Contain the analytical framework and reasoning steps
4. Produce prioritized, actionable output — not just analysis
5. Know which other skills to invoke if scope expands mid-session

---

## Data Collection Standards

The `financial-snapshot` skill establishes the canonical input format used across all other skills. Categories:

- **Income:** gross, net, irregular sources, next expected changes
- **Assets:** cash/emergency fund, taxable investments, retirement accounts, real estate equity, other
- **Liabilities:** mortgage balance/rate/payment, other debt (balance, rate, minimum)
- **Monthly cash flow:** fixed expenses, variable spending by category, discretionary
- **Goals:** each with target amount, target date, current progress, priority rank
- **Tax context:** filing status, bracket, key deductions, HSA/FSA status
- **Investment allocation:** current vs target, accounts with each allocation (401k (roth & traditional), brokerage, HSA, 529s, etc.)

---

## Technology Decisions (To Resolve)

| Decision | Options | Recommendation |
|----------|---------|----------------|
| Account aggregator | SimpleFIN Bridge vs Teller.io | SimpleFIN (more private); Teller if API simplicity matters more |
| Local model hardware | CPU-only vs GPU | Determines Hermes model size (8B vs 70B) |
| Dashboard delivery | Next.js local app vs Obsidian plugin vs TUI | Next.js for richest visualization |
| Cloud data boundary | Aggregates only vs fully air-gapped | Aggregates to Claude; raw data stays local |
| Local DB | DuckDB vs SQLite | DuckDB (better analytics) |

---

## Current Focus

> **Phase 1 — Skill development, manual data entry**
>
> Building the 10-skill suite starting with the 3 foundation skills:
> `financial-snapshot` → `quarterly-strategy` → `scenario-compare`
>
> Once skills are drafted, run a real session against actual financial data to validate and iterate.
