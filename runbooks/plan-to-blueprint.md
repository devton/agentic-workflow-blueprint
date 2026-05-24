# Runbook: Plan -> Blueprint

## Objective

Convert a technical plan into a durable, executable skill structure the agent can rerun with bounded context and explicit review gates.

## When to use

- After Plan mode (or equivalent) produced a multi-step implementation plan.
- When the team wants the plan to survive beyond a single chat session.
- Before starting implementation that spans multiple workflows or PRs.

## Inputs required

- `projectSlug`
- `skillName` (optional; defaults to `projectSlug`)
- `baseBranch`
- `techStack`
- `existingRootDoc`
- `technicalPlan` (milestones, tasks, acceptance criteria, constraints)
- `workflowsWanted` (initial list; may be refined during the run)
- `constraints`

## Steps

1. Invoke the project entry skill router with subcommand:
   - `/<skillName> plan-to-blueprint`
   - If subcommands are not parsed, invoke `/<skillName>` and pass `plan-to-blueprint` in the prompt.
2. Run workflow `plan-to-blueprint` using `technicalPlan` as primary evidence.
3. Confirm proposed `workflowsWanted` mapping (plan section -> workflow id).
4. Wait for scaffold outputs:
   - `skills/<projectSlug>/SKILL.md`
   - `skills/<projectSlug>/template.json`
   - `skills/<projectSlug>/workflows/*/SKILL.md`
   - updated `existingRootDoc`
5. Validate review gate from the workflow (manifest, routing, links, contract completeness).
6. Smoke-test routing with one subcommand, e.g. `/<skillName> document` (or first workflow in the manifest).

## Exit criteria

- Entry skill and `template.json` exist and list the same commands.
- Every command routes to exactly one workflow contract.
- Root doc points to canonical skill paths only.
- Handoff note documents plan-to-workflow mapping and open gaps.

## Failure handling

- If plan sections cannot map cleanly to workflows, stop and request plan refinement (do not invent workflows).
- If manifest and routing-matrix disagree, fix manifest first, then re-run consistency verification.
- If review gate fails after one correction pass, stop automation and request manual intervention.
