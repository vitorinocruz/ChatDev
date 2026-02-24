# Zero-Code Multi-Agent App Plan (Local) — Business Ideas Portfolio

## Goal
Build a local DevAll app that can **ingest, organize, prioritize, execute, and monitor** your 17 business ideas portfolio with minimal/no coding, using the existing ChatDev 2.0 workflow runtime.

## What you already have
- A complete portfolio with 17 ideas, 105 artifacts, and a clear phase-based rollout.
- Rich `.docx` assets per idea that should become the primary context source for planning and execution.

## Target outcome
A local app with 4 operational modes:
1. **Portfolio Control Tower** (ranking, roadmap, KPI tracking)
2. **Idea Sprint Executor** (weekly action plan per selected idea)
3. **Execution Ops Manager** (task queue, owners, deadlines, blockers)
4. **Review & Re-Plan** (weekly/monthly retrospectives and reprioritization)

---

## Recommended local architecture (zero-code first)

### 1) Runtime
- Use DevAll backend + frontend locally (`make dev`).
- Store workflow YAMLs in `yaml_instance/` and manage via the Launch/Workflow UI.

### 2) Data model (through attachments + YAML vars)
- Inputs:
  - `MEMORY.MD`
  - Per-idea docs (`01-Business-Design.docx`, `02-Implementation-Plan.docx`, etc.)
- Variables (YAML placeholders):
  - `${PORTFOLIO_GOAL}` (e.g., "Reach €10k MRR by Month 18")
  - `${WEEKLY_HOURS}` (e.g., 20)
  - `${CAPITAL_BUDGET}`
  - `${RISK_PROFILE}` (conservative/balanced/aggressive)
  - `${FOCUS_PHASE}` (1..4)

### 3) Agent roles (configured in workflow nodes)
- **Portfolio Strategist**: decides priority stack and phase alignment.
- **Execution Planner**: converts strategy into concrete weekly plans.
- **Ops Coordinator**: assigns tasks, due dates, dependencies, and blockers.
- **Risk & Compliance Reviewer**: flags platform/legal/policy risks.
- **Finance Analyst**: revenue/cashflow projections and variance tracking.
- **Growth Analyst**: channel experiments and KPI suggestions.
- **Reporter**: compiles decision logs and summaries.

### 4) Core tools/features you should use
- Attachment upload endpoints for `.docx` and memory files.
- Workflow execution endpoint for repeatable runs.
- Batch execution endpoint for comparing multiple ideas in parallel.
- Artifact event polling for progressive outputs in Launch mode.

---


## Ready-to-run workflow template in this repo
- File: `yaml_instance/business_ideas_portfolio_manager.yaml`
- Purpose: A concrete multi-agent chain you can run today in Launch mode.
- Nodes: Portfolio Strategist → Execution Planner → Finance/Risk Reviewer → Operator Report.
- Attachments to upload per run: `MEMORY.MD` plus the specific idea `.docx` files for current focus.

### Quick start (local)
1. Configure environment:
   - `cp .env.example .env`
   - set `API_KEY` and `BASE_URL`
2. Start app:
   - `make dev`
3. In Web Console Launch tab:
   - select `business_ideas_portfolio_manager.yaml`
   - upload `MEMORY.MD` and chosen idea docs
   - run with prompt: `Generate this week's plan under my constraints.`

---

## Workflow set to create (suggested)

## WF-01: Portfolio Intake & Structuring
**Purpose:** Build a normalized portfolio index from your 17 folders.

**Input:** `MEMORY.MD` + selected per-idea docs.

**Output artifacts:**
- `portfolio_registry.json`
- `idea_scorecard_table.md`
- `dependency_synergy_map.md`

**Frequency:** Run once, then refresh monthly.

---

## WF-02: Prioritization Engine
**Purpose:** Rank ideas for the next 90 days under your constraints.

**Logic (agent discussion):**
- time-to-first-revenue
- setup complexity
- risk/regulatory/platform concentration
- synergy multiplier with active ideas
- your available hours and budget

**Output artifacts:**
- `next_90_day_priority.md`
- `do_now_do_later_drop.md`

**Frequency:** Weekly or biweekly.

---

## WF-03: Weekly Sprint Planner (per chosen idea)
**Purpose:** Create a realistic 7-day execution plan.

**Input:** one target idea folder + constraints vars.

**Output artifacts:**
- `weekly_sprint_plan.md`
- `task_board.csv` (task, owner, ETA, dependency, status)
- `risk_log.md`

**Frequency:** Weekly.

---

## WF-04: Execution Monitor
**Purpose:** Convert progress notes into status dashboards and next actions.

**Input:** prior sprint outputs + execution notes.

**Output artifacts:**
- `weekly_exec_report.md`
- `kpi_snapshot.json`
- `blockers_and_decisions.md`

**Frequency:** 2–3x per week.

---

## WF-05: Financial Reforecast
**Purpose:** Track expected vs actual and update 3/6/12-month projections.

**Input:** financial projection docs + latest KPI snapshot.

**Output artifacts:**
- `reforecast_3_6_12m.md`
- `cashflow_alerts.md`
- `break_even_tracker.csv`

**Frequency:** Monthly.

---

## WF-06: Batch Scenario Runner
**Purpose:** Compare multiple idea scenarios in one run.

**Example scenarios:**
- Scenario A: (07 + 13) quick wins only
- Scenario B: (08 + 06 + 11) audience flywheel
- Scenario C: (16 + 17) agency + micro-SaaS scale

**Output artifacts:**
- `scenario_comparison_matrix.md`
- `recommended_path_with_tradeoffs.md`

**Frequency:** Monthly or before major pivots.

---

## 14-day local rollout plan

### Days 1–2: Environment
1. Copy `.env.example` to `.env` and set model credentials.
2. Start with `make dev`.
3. Validate workflows (`make validate-yamls`).

### Days 3–5: Baseline workflows
1. Clone a simple demo YAML as your starter template.
2. Build WF-01 and WF-02 in the Workflow UI.
3. Run with 3 ideas first (07, 08, 13) before full 17-idea scope.

### Days 6–9: Execution operations
1. Build WF-03 and WF-04.
2. Add standardized output artifact names.
3. Establish a weekly cadence (Monday plan, Wednesday review, Friday report).

### Days 10–12: Finance + scenarios
1. Build WF-05 and WF-06.
2. Add scenario assumptions as YAML variables.
3. Compare at least 3 strategy bundles.

### Days 13–14: Stabilization
1. Freeze v1 workflows.
2. Document your operating playbook (`runbook.md`).
3. Move to recurring weekly cycles.

---

## Suggested KPI dashboard (minimal)
- **Portfolio level:** active ideas, monthly revenue, MRR, runway, risk concentration.
- **Idea level:** leads, conversion rate, delivery SLA, gross margin, weekly growth.
- **Execution level:** planned vs completed tasks, blocker age, cycle time.

---

## Practical operating cadence
- **Monday (60–90 min):** run WF-02 + WF-03
- **Wednesday (30 min):** run WF-04
- **Friday (45 min):** run WF-04 + short re-plan
- **Month-end (90 min):** run WF-05 + WF-06

---

## Risks to manage from day one
- Over-parallelization (too many ideas active at once)
- Platform concentration risk (single-channel dependence)
- Unrealistic weekly task loads
- Inconsistent artifact naming/history
- Financial optimism bias without monthly reforecasting

---

## "Start tomorrow" simple configuration
If you want the fastest start:
1. Activate only **2 ideas**: 07 (Notion Store) + 13 (Pinterest Affiliate)
2. Use just **3 workflows** first: WF-02, WF-03, WF-04
3. Set constraints:
   - `${WEEKLY_HOURS}=15`
   - `${CAPITAL_BUDGET}=300`
   - `${RISK_PROFILE}=balanced`
4. Produce exactly 3 outputs each week:
   - `next_7_days.md`
   - `task_board.csv`
   - `weekly_exec_report.md`

This keeps the system lean and immediately executable.
