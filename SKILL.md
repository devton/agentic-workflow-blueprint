---
name: workflow-blueprint
description: Blueprint workflow to scaffold agent-oriented documentation with progressive disclosure and executable contracts.
---

## agentic-workflows.blueprint

### Goal

Provide a reusable blueprint workflow that scaffolds an "agentic workflows" documentation structure for any codebase, with progressive disclosure, executable contracts (workflows), and self-contained interactive visual HTML manuals (`README.html` / `<filename>.html`).

### Scope

- Applies to: any repository that wants an agent-oriented documentation system (orchestrator + workflow skills + references + runbooks + visual HTML manuals).
- Does not cover: implementing product features; this is scaffolding/documentation only.

### Triggers

- "Create agentic workflow structure"
- "Set up skills folder + workflows"
- "Make docs agent-friendly"
- "Refactor AGENTS.md into linked skills"
- "Generate HTML manuals for skills and runbooks"

### Inputs

- `projectSlug`: short identifier for the repo (e.g. `my-backend`)
- `baseBranch`: default integration branch (e.g. `develop`, `main`)
- `techStack`: short list (e.g. `NestJS + MikroORM + Graphile Worker`)
- `existingRootDoc`: root instruction file path (`AGENTS.md`, `CLAUDE.md`, etc.)
- `workflowsWanted`: list of workflow ids to scaffold (e.g. `modules`, `specs`, `document`, `network-engineering`, `infra-operations`, `iac`, `os-platform`, `html-manual`, `plan-to-blueprint`)
- `constraints`: project hard rules (e.g. "no emojis", "mock external boundaries only")
- `skillName` (optional): slash-invocation name for the project entry skill; defaults to `projectSlug` (lowercase, hyphens only)

### Outputs

Creates a minimal, navigable structure with matching HTML visual manuals:

```
skills/<projectSlug>/
  SKILL.md
  README.html                       ← Interactive visual HTML manual for project skill
  template.json                     ← Declarative manifest (commands, entry)
  reference/
    routing-matrix.md
    routing-matrix.html             ← Visual HTML version matching base filename
    role-contracts.md
    role-contracts.html             ← Visual HTML version matching base filename
    hook-blueprint.md (optional)
    hook-blueprint.html (optional)
  workflows/
    <workflowName>/
      SKILL.md
      README.html                   ← Interactive visual HTML manual for workflow
      template.json (optional)      ← Per-workflow manifest when useful
docs/runbooks/
  agent-role-system.md
  agent-role-system.html           ← Visual HTML version matching runbook filename
  agent-role-hooks.md (optional)
  agent-role-hooks.html (optional)
  plan-to-blueprint.md (when workflow included)
  plan-to-blueprint.html (when workflow included)
```

And updates the root doc (`AGENTS.md` or equivalent) to link to the new entrypoints.

This blueprint folder carries bundled workflows (`embed-aihero-radioactive`, `mattpocock/wayfinder`, `mattpocock/grilling`, `mattpocock/domain-modeling`, `mattpocock/research`, `mattpocock/prototype`, `mattpocock/to-spec`, `radioactive`, `brainstorming`, `plan-writing`, `ui-ux-pro-max`, `remotion-video-motion`, `thermo-nuclear-code-quality-review`, `thermo-fix`, `html-manual`, `changelog-generator`, `document`, `review`, `changelog`, `linear`, `mcp-linear-planner`, `mcp-linear-sync`, `network-engineering`, `infra-operations`, `iac`, `os-platform`, `implementing-devsecops-security-scanning`, `scanning-containers-with-trivy-in-cicd`, `scanning-docker-images-with-trivy`, `scanning-kubernetes-manifests-with-kubesec`, `implementing-network-policies-for-kubernetes`, `implementing-rbac-hardening-for-kubernetes`, `implementing-pod-security-admission-controller`, `securing-aws-iam-permissions`, `securing-container-registry-images`, `securing-kubernetes-on-cloud`, `triaging-vulnerabilities-with-ssvc-framework`, `performing-kubernetes-cis-benchmark-with-kube-bench`, `analyzing-kubernetes-audit-logs`, `securing-github-actions-workflows`, `performing-container-image-hardening`, `remediating-s3-bucket-misconfiguration`, `performing-container-security-scanning-with-trivy`, `performing-vulnerability-scanning-with-nessus`, `implementing-syslog-centralization-with-rsyslog`, `c4-architecture`, `plan-to-blueprint`) to demonstrate full-lifecycle chained execution, Matt Pocock / AI Hero methodology embedding, plan-to-skill transformation, visual documentation generation, MCP integration patterns, and infrastructure operations coverage.

It can also carry runbook examples under `runbooks/` to show operator-facing execution playbooks for those workflows.

### Invariants (guardrails)

- Progressive disclosure: root doc stays short; details live behind links.
- Executable contracts: every workflow is written as a contract the agent can follow:
  - `Goal`, `Scope`, `Triggers`, `Inputs`, `Invariants`, `Procedure`, `Outputs`, `Review gate`, `References`.
- Visual HTML Manuals (Default Behavior): Every skill, workflow contract, reference doc (`.md`), and runbook (`.md`) MUST have a corresponding self-contained interactive visual HTML document (`README.html` for skills/workflows, `<filename>.html` matching base name for references/runbooks) formatted with Tailwind CSS CDN, dark mode (`bg-zinc-950 text-zinc-100`), glassmorphic styling, and method/status badges. The source `.md` file MUST link to its `.html` companion under `## References`.
- No duplication: do not copy/paste long rules across files; link to the source of truth.
- Consistency: workflow ids and file paths must match exactly across all references.
- Minimal surface: only add the workflows actually requested.
- Source of truth: `SKILL.md` is authoritative; `template.json` is a complementary declarative layer for agents/tools that expose command interfaces.
- Command routing: router-only — users invoke `/<skillName> <cmd>`; the entry skill resolves `cmd` to `workflows/<cmd>/SKILL.md` and loads only that contract.
- Manifest sync: every command in `template.json` must map to an existing workflow folder and match `routing-matrix.md`.

### Procedure

#### 1) Create the project entry skill

Create `skills/<projectSlug>/SKILL.md` as the global entrypoint:

- A short description of what the skill is for.
- An "orchestrator" section that explains:
  - how to classify a task
  - how to select one workflow
  - how to close (validation gate if applicable)
- A **Command routing** section (router-only):
  - invocation pattern: `/<skillName> <cmd>` (e.g. `/my-backend document`)
  - map each `cmd` in `workflowsWanted` to `workflows/<cmd>/SKILL.md`
  - if the agent does not parse subcommands, read `cmd` from the user message and load the mapped workflow contract
  - do not create flat alias skills unless explicitly requested
- A list of internal workflow helpers:
  - `workflows/<name>/SKILL.md` links
- Project constraints and hard rules (short bullets).

Create `skills/<projectSlug>/template.json` as the declarative manifest:

```json
{
  "name": "<skillName>",
  "version": "1.0.0",
  "entry": "SKILL.md",
  "routing": "router-only",
  "commands": [
    {
      "name": "<workflowName>",
      "description": "Short trigger description",
      "skill": "workflows/<workflowName>/SKILL.md"
    }
  ]
}
```

- Include one `commands[]` entry per item in `workflowsWanted`.
- Keep `name` aligned with `skillName` (folder may remain `projectSlug`).

#### 2) Create references (progressive disclosure)

In `skills/<projectSlug>/reference/`, create:

- `routing-matrix.md`: task category -> workflow mapping; include slash form `/<skillName> <cmd>` alongside intent triggers.
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
- `Invariants`: the project's hard rules + workflow-specific rules
- `Procedure`: deterministic steps (evidence-driven; use git diff when documenting)
- `Outputs`: what files/notes/checkpoints must be produced
- `Review gate`: checklist with pass/fail criteria
- `References`: links back to `skills/<projectSlug>/SKILL.md` and any deep dives

Optionally, for each workflow, add `workflows/<workflowName>/template.json` when an external tool needs a standalone command descriptor; keep it minimal (`name`, `entry`, `parent`).

#### 4) Generate Visual HTML Manuals (Default Requirement)

For every generated or updated markdown file:

- **Skills & Workflows:** Generate `README.html` inside the skill/workflow folder (`skills/<projectSlug>/README.html` and `skills/<projectSlug>/workflows/<workflowName>/README.html`).
- **References:** Generate `<filename>.html` in `skills/<projectSlug>/reference/` matching the markdown filename (e.g. `routing-matrix.md` ➔ `routing-matrix.html`, `role-contracts.md` ➔ `role-contracts.html`).
- **Runbooks:** Generate `<filename>.html` in `docs/runbooks/` matching the markdown filename (e.g. `agent-role-system.md` ➔ `agent-role-system.html`).
- Format each HTML file as a single, self-contained visual manual using Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com"></script>`), dark mode (`bg-zinc-950 text-zinc-100`), glassmorphic cards (`background: rgba(24, 24, 27, 0.65); backdrop-filter: blur(12px)`), method/status badges, and code snippets.
- Update each source `.md` file to include a reference link to its `.html` companion under `## References` (e.g. `[Interactive HTML View](./README.html)` or `[Visual HTML Version](./routing-matrix.html)`).

#### 5) Wire everything into the root doc

Update `existingRootDoc` to include:

- "Start here": link to `skills/<projectSlug>/SKILL.md` and `skills/<projectSlug>/README.html`
- Under "Skills" (or similar), list:
  - the project skill & visual manual link
  - internal workflow skills & visual manual links
  - runbook links & visual manual links

Do not duplicate workflow contents in the root doc.

#### 6) Optional: deprecate legacy skill locations (wrapper)

If there are existing skills in other directories:

- Keep the file
- Add a top banner:
  - "Moved: canonical workflow is at `skills/<projectSlug>/workflows/...`"
- Leave the rest as a deep dive reference

#### 7) Consistency verification (required)

Before declaring the scaffold done:

- Verify every link path exists (`.md` and `.html`).
- Verify workflow ids are consistent:
  - `projectSlug.workflow.*` matches the file it lives in.
- Verify root doc points only to canonical locations.
- Verify `template.json` `commands[].name` values exist under `workflows/` and appear in Command routing + `routing-matrix.md`.
- Verify router-only behavior is documented in the project entry `SKILL.md` (no undocumented flat aliases).
- Verify every `.md` file has a matching `.html` visual manual companion.

### Review gate (must pass)

- Root doc remains minimal and only links out.
- Each workflow has the full contract sections (Goal..References).
- Constraints are explicit and testable (no vague "best practices").
- Self-contained interactive HTML visual manuals (`README.html` for skills/workflows, `<filename>.html` for references/runbooks) are generated for all files.
- Every source `.md` file links to its `.html` companion under `## References`.
- No duplication between root, project skill, and workflows.
- All links resolve (`.md` and `.html`).
- `template.json` is valid JSON and consistent with `workflowsWanted`.
- Command routing resolves every listed subcommand to exactly one workflow contract.

### Notes

- This blueprint is intentionally stack-agnostic. For stack-specific rules (logging, ORM patterns, testing rules, network policies, IaC standards, and OS baselines), keep them in the project skill and link them from workflows.
- Installable skill contract with frontmatter: see `SKILL.md` (`name: workflow-blueprint`).

### References

- [Interactive HTML View](./README.html)
