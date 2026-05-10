# Agentic Workflows Blueprint

A meta-skill that teaches your AI agent how to build operational skills
for each module and system in your project — so it loads only the context
it needs, when it needs it.

Instead of one massive `AGENTS.md`, your agent gets a routing system:
it classifies the task, loads the right skill, executes deterministically,
and hands off to the next step. Less tokens, more precision.

---

## Quick run

```bash
npx skills add devton/agentic-workflow-blueprint
```

Then call the agent in your target repository with a direct prompt like:

```text
blueprint this project.
```

After that, wait for the agent to finish the scaffold. The expected first
deliverable is the main project skill file at `skills/<projectSlug>/SKILL.md`.

---

## The problem it solves

You have a project with multiple modules. Your AI agent loads everything
every time — all rules, all context, all history. That burns tokens and
dilutes focus.

This blueprint lets the agent build **scoped skills per module**, each
with its own contract, inputs, outputs, and review gates. The agent
only loads what's relevant to the current task.

---

## How it works in practice

**Without this blueprint:**
You tell your agent "add a payment module". It loads your entire
AGENTS.md (500+ lines), guesses conventions, and wings it.

**With this blueprint:**

1. You trigger this skill once: *"set up my project"* — it asks a few
   questions and scaffolds `skills/<project>/` with routing, contracts,
   and workflows.
2. Next time you say *"add a payment module"*, the agent classifies the
   task → loads only `skills/<project>/workflows/modules/SKILL.md`
   → follows the deterministic procedure → passes the review gate
   → hands off.
3. You adjust any workflow in plain conversation — it's just markdown
   the agent reads and follows.

---

## Why this saves tokens

- **Scoped loading** — agent loads only the skill for the current step,
  not the whole project context.
- **Bounded contracts** — clear inputs, outputs, and pass/fail gates
  keep the agent deterministic.
- **Focused retries** — when something fails, the agent retries only
  the failed gate, not the entire conversation.
- **Structured handoffs** — steps communicate via defined inputs/outputs,
  not free-form context dumps.

---

## Blueprint vision

```mermaid
flowchart TD
    A[Entry Skill: Orchestrator] --> B["Execution chain<br/>Workflow 1 → Workflow 2 → Workflow 3 → Delivery"]

    B -. "loads only current contract" .-> C[Context Slice 1]
    B -. "loads only current contract" .-> D[Context Slice 2]
    B -. "loads only current contract" .-> E[Context Slice 3]

    C --> F[Compact handoff to next step]
    D --> F
    E --> F
```

---

## Getting started

When you trigger this blueprint, the agent will ask:

- **projectSlug** — short name for the repo (e.g. `my-backend`)
- **baseBranch** — main integration branch (e.g. `main`, `develop`)
- **techStack** — what's the stack? (e.g. `NestJS + PostgreSQL + Redis`)
- **workflowsWanted** — which workflows to scaffold
  (e.g. `modules`, `specs`, `document`, `review`)
- **constraints** — hard rules
  (e.g. "no ORM, raw SQL only", "all API calls go through service layer")

Then it generates the full structure and wires everything into your
`AGENTS.md` or `CLAUDE.md` — no manual wiring needed.

### What gets generated

```
skills/<projectSlug>/
  SKILL.md                          ← Orchestrator (entry point)
  reference/
    routing-matrix.md               ← Task → workflow mapping
    role-contracts.md               ← Roles, boundaries, handoffs
    hook-blueprint.md               ← Optional automation hooks
  workflows/
    <workflowName>/SKILL.md         ← One per workflow

docs/runbooks/
  agent-role-system.md              ← Operator-facing playbooks
  agent-role-hooks.md               ← Optional hook runbook
```

---

## Included workflow examples

These are **templates** — copy, rename, and adapt to your project's
actual workflows (deploy, test, migrate, etc).

| Workflow | What it does | When to use |
|----------|-------------|-------------|
| `document` | Builds docs from git diff evidence | After implementation is done |
| `review` | Validates docs with deterministic pass/fail | Auto-step after `document` |
| `changelog` | Generates changelog from approved docs | After `review` passes |
| `linear` | Creates Linear projects/issues via MCP | Multi-PR initiatives |
| `mcp-linear-planner` | Preflight check + execution plan for Linear | When you need stricter control |
| `mcp-linear-sync` | Executes planned Linear operations | After `planner` validates |

### Chained flow example (document → review → changelog)

```
attempt = 1
while attempt <= 3:
  doc = run(document)
  review = run(review, input=doc)
  if review.pass:
    run(changelog, input=doc)
    break
  attempt += 1
```

1. `document` produces docs from implementation evidence.
2. `review` validates against the evidence.
3. If fail → feed findings back to `document`, retry (max 3).
4. If pass → `changelog` generates the entry.

---

## The contract format

Every workflow is an executable contract the agent follows:

```
Goal        → 1 sentence: what this workflow does
Scope       → applies to / does not cover
Triggers    → file patterns + intent phrases
Inputs      → what the workflow needs
Invariants  → hard rules that cannot be violated
Procedure   → deterministic steps (evidence-driven)
Outputs     → what must be produced
Review gate → pass/fail checklist
References  → links to parent skill and related workflows
```

This format keeps each workflow self-contained, linkable, and bounded.
Agents don't need to reason over the entire project — they follow the
contract for the current step.

---

## Included runbooks

Runbooks are operator-facing execution playbooks (for humans managing
agent runs). Workflow SKILL files are agent-facing contract definitions.

- **`document-review-changelog.md`** — operational guide for the 3-step
  doc loop with retry logic.
- **`linear-mcp.md`** — operational guide for direct Linear workflow.
- **`mcp-linear-sync.md`** — operational guide for the planner → sync
  decomposition.

---

## How to use this repo

1. Read `SKILL.md` for the full blueprint procedure.
2. Trigger the skill in your agent with your project details.
3. The agent scaffolds the structure and wires it into your root doc.
4. Adjust workflows as needed — it's markdown, edit in conversation.
5. Add new workflows over time by following the contract format.

Stack-agnostic by design. Stack-specific rules (ORM patterns, testing
conventions, deployment quirks) go in the project skill and get linked
from individual workflows.
