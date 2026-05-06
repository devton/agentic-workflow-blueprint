## agentic-workflows.blueprint

### Goal

Provide a reusable blueprint workflow that scaffolds an “agentic workflows” documentation structure for any codebase, with progressive disclosure and executable contracts (workflows).

### Scope

- Applies to: any repository that wants an agent-oriented documentation system (orchestrator + workflow skills + references + runbooks).
- Does not cover: implementing product features; this is scaffolding/documentation only.

### Triggers

- “Create agentic workflow structure”
- “Set up skills folder + workflows”
- “Make docs agent-friendly”
- “Refactor AGENTS.md into linked skills”

### Inputs

- `projectSlug`: short identifier for the repo (e.g. `my-backend`)
- `baseBranch`: default integration branch (e.g. `develop`, `main`)
- `techStack`: short list (e.g. `NestJS + MikroORM + Graphile Worker`)
- `existingRootDoc`: root instruction file path (`AGENTS.md`, `CLAUDE.md`, etc.)
- `workflowsWanted`: list of workflow ids to scaffold (e.g. `modules`, `specs`, `document`)
- `constraints`: project hard rules (e.g. “no emojis”, “mock external boundaries only”)

### Outputs

Creates a minimal, navigable structure:

```
skills/<projectSlug>/
  SKILL.md
  reference/
    routing-matrix.md
    role-contracts.md
    hook-blueprint.md (optional)
  workflows/
    <workflowName>/
      SKILL.md
docs/runbooks/
  agent-role-system.md
  agent-role-hooks.md (optional)
```

And updates the root doc (`AGENTS.md` or equivalent) to link to the new entrypoints.

This blueprint folder can also carry example workflows (`document`, `review`,
`changelog`, `linear`) to demonstrate chained execution and MCP integration
patterns. Optional decomposition workflows (such as `mcp-linear-planner` and
`mcp-linear-sync`) can be added when needed.

It can also carry runbook examples under `runbooks/` to show operator-facing
execution playbooks for those workflows.

### Invariants (guardrails)

- Progressive disclosure: root doc stays short; details live behind links.
- Executable contracts: every workflow is written as a contract the agent can follow:
  - `Goal`, `Scope`, `Triggers`, `Inputs`, `Invariants`, `Procedure`, `Outputs`, `Review gate`, `References`.
- No duplication: do not copy/paste long rules across files; link to the source of truth.
- Consistency: workflow ids and file paths must match exactly across all references.
- Minimal surface: only add the workflows actually requested.

### Procedure

#### 1) Create the project entry skill

Create `skills/<projectSlug>/SKILL.md` as the global entrypoint:

- A short description of what the skill is for.
- An “orchestrator” section that explains:
  - how to classify a task
  - how to select one workflow
  - how to close (validation gate if applicable)
- A list of internal workflow helpers:
  - `workflows/<name>/SKILL.md` links
- Project constraints and hard rules (short bullets).

#### 2) Create references (progressive disclosure)

In `skills/<projectSlug>/reference/`, create:

- `routing-matrix.md`: task category -> workflow mapping.
- `role-contracts.md`: roles, boundaries, handoffs.
- Optional `hook-blueprint.md`: opt-in automation checklist.

Keep each file self-contained and linkable.

#### 3) Scaffold each workflow as an executable contract

For each workflow in `workflowsWanted`, create:

`skills/<projectSlug>/workflows/<workflowName>/SKILL.md` with:

- `id`: `"<projectSlug>.workflow.<workflowName>"` (as the first header line)
- `Goal`: 1 sentence
- `Scope`: applies/does not cover
- `Triggers`: file triggers + intent triggers
- `Inputs`: baseBranch, diff scope, required config
- `Invariants`: the project’s hard rules + workflow-specific rules
- `Procedure`: deterministic steps (evidence-driven; use git diff when documenting)
- `Outputs`: what files/notes/checkpoints must be produced
- `Review gate`: checklist with pass/fail criteria
- `References`: links back to `skills/<projectSlug>/SKILL.md` and any deep dives

#### 4) Wire everything into the root doc

Update `existingRootDoc` to include:

- “Start here”: link to `skills/<projectSlug>/SKILL.md`
- Under “Skills” (or similar), list:
  - the project skill
  - internal workflow skills (links to the new workflow SKILL.md files)
  - runbook links

Do not duplicate workflow contents in the root doc.

#### 5) Optional: deprecate legacy skill locations (wrapper)

If there are existing skills in other directories:

- Keep the file
- Add a top banner:
  - “Moved: canonical workflow is at `skills/<projectSlug>/workflows/...`”
- Leave the rest as a deep dive reference

#### 6) Consistency verification (required)

Before declaring the scaffold done:

- Verify every link path exists.
- Verify workflow ids are consistent:
  - `projectSlug.workflow.*` matches the file it lives in.
- Verify root doc points only to canonical locations.

### Review gate (must pass)

- Root doc remains minimal and only links out.
- Each workflow has the full contract sections (Goal..References).
- Constraints are explicit and testable (no vague “best practices”).
- No duplication between root, project skill, and workflows.
- All links resolve.

### Notes

- This blueprint is intentionally stack-agnostic. For stack-specific rules (logging, ORM patterns, testing rules), keep them in the project skill and link them from workflows.

