# Financial Planner

A personal financial planning system using Claude Code custom skills for structured financial reasoning.

## How to Use

1. **Start every session** with `/financial-snapshot` — fill out the template or read from an existing snapshot in `data/snapshots/`
2. **Run analytical skills** by name — see the full list below
3. **Save session output** to `data/sessions/YYYY-MM-DD-[topic].md` if you want to keep a record

## Skills

| Skill | Purpose | Cadence |
|-------|---------|---------|
| `/financial-snapshot` | Fill out or update your financial snapshot | Every session |
| `/financial-health-score` | Holistic scorecard — use this if unsure where to start | Quarterly |
| `/quarterly-strategy` | Big-picture 90-day prioritization | Quarterly |
| `/scenario-compare` | Model a specific decision (buy vs rent, refi, job change) | As needed |
| `/debt-strategy` | Paydown ordering, refinancing, payoff timelines | Monthly |
| `/investment-review` | Allocation drift and contribution optimization | Quarterly |
| `/tax-optimization` | Tax-saving opportunities and contribution headroom | Quarterly + year-end |
| `/goal-modeling` | Gap analysis and timeline projection for all goals | Quarterly |
| `/budget-diagnosis` | Spending leaks, envelope tracking, month-to-month rollover | Monthly |
| `/cash-flow-optimizer` | Bill timing and cash flow gap prevention | Monthly |
| `/net-worth-tracker` | Net worth trend and FI benchmark tracking | Monthly |
| `/mermaid` | Generate a Mermaid diagram for any financial data | As needed |

## Privacy Rules

- **Never include individual transactions** — category-level aggregates only
- **Snapshot files live in `data/`** which is gitignored — they never leave this machine
- **Do not suggest external services** or ask for account credentials

## Output Style

- Lead with numbers, not narration
- Bold the 3–5 most important metrics at the top
- Every session ends with a numbered action list
- Flag which skill to run next when handing off
