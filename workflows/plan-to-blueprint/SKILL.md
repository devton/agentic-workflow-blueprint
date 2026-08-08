## agentic-workflows-blueprint.workflow.plan-to-blueprint

### Goal

Transform a technical plan into an executable project skill blueprint: entry skill, command routing, workflow contracts, manifests, and runbook links.

### Scope

- Applies to: converting structured implementation plans into `skills/<projectSlug>/` scaffold outputs.
- Does not cover: implementing product features described in the plan.

### Triggers

- "Turn this plan into a skill blueprint"
- "Convert plan to executable workflows"
- "Scaffold skills from this technical plan"
- After a Plan-mode session when the user wants persistent, rerunnable agent contracts

### Inputs

- `projectSlug`: short identifier for the target repo
- `skillName` (optional): slash name for router invocation; defaults to `projectSlug`
- `baseBranch`: default integration branch
- `techStack`: short stack description
- `existingRootDoc`: root instruction file path (`AGENTS.md`, `CLAUDE.md`, etc.)
- `technicalPlan`: structured plan (milestones, tasks, acceptance criteria, constraints)
- `workflowsWanted`: workflow ids inferred from the plan (must include at least one)
- `constraints`: project hard rules extracted from the plan
- `operationalContext` (optional): environment tiers, maintenance window, rollback strategy, compliance boundaries

### Invariants

- `SKILL.md` remains the source of truth; `template.json` mirrors routing, never replaces contracts.
- Router-only command model: `/<skillName> <cmd>` maps to `workflows/<cmd>/SKILL.md`.
- Every workflow in `workflowsWanted` gets a full contract (Goal through References).
- Do not invent scope not present in `technicalPlan`; mark gaps explicitly in handoff notes.
- Workflow ids and folder names must match exactly (`kebab-case`).
- For infra/network/IaC/OS plans, include explicit operational verification and rollback criteria in generated contracts.

### Procedure

1. Parse `technicalPlan` and extract: objectives, milestones, deliverables, constraints, and candidate workflow boundaries.
2. Propose `workflowsWanted` (confirm or refine with user if ambiguous).
   - Include `network-engineering`, `infra-operations`, `iac`, and `os-platform` when the technical plan includes those domains.
3. Scaffold `skills/<projectSlug>/SKILL.md` with orchestrator + **Command routing** (router-only).
4. Generate `skills/<projectSlug>/template.json` with `commands[]` for each workflow in `workflowsWanted`.
5. Create `reference/routing-matrix.md` and `reference/role-contracts.md` from plan evidence.
6. For each workflow id, create `workflows/<workflowName>/SKILL.md` with executable contract sections grounded in the plan.
7. Optionally add `workflows/<workflowName>/template.json` when a tool needs per-workflow manifests.
8. Wire `existingRootDoc` with "Start here" and skill/runbook links (no duplication of workflow bodies).
9. Add or update runbook `docs/runbooks/plan-to-blueprint.md` when operator guidance is needed.
10. Run consistency verification: links, ids, manifest commands, routing-matrix alignment.

### Outputs

- `skills/<projectSlug>/SKILL.md` (entry orchestrator with command routing).
- `skills/<projectSlug>/template.json` (declarative command manifest).
- `skills/<projectSlug>/reference/routing-matrix.md` and `role-contracts.md`.
- `skills/<projectSlug>/workflows/<workflowName>/SKILL.md` for each item in `workflowsWanted`.
- Updated `existingRootDoc` with canonical links.
- Handoff note listing mapped plan sections -> workflows and any unresolved gaps.

### Review gate

- [ ] Every command in `template.json` resolves to an existing `workflows/<cmd>/SKILL.md`.
- [ ] Entry skill documents router-only invocation (`/<skillName> <cmd>`).
- [ ] Each workflow contract is complete and traceable to plan evidence.
- [ ] Root doc links only to canonical paths; no duplicated workflow content.
- [ ] All referenced paths exist and ids follow `<projectSlug>.workflow.<name>`.

### References

- `../../SKILL.md`
- `../document/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
