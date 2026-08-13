---
name: embed-aihero-radioactive
description: |
  Scaffold option & workflow blueprint that adapts a project's /radioactive development lifecycle by embedding Matt Pocock / AI Hero skills (wayfinder, grilling, domain-modeling, research, prototype, to-spec), starting with project skill classification followed by wayfinder orientation.
---

## agentic-workflows-blueprint.workflow.embed-aihero-radioactive

### Goal

Adapt and scaffold a project's `radioactive` workflow (`skills/<projectSlug>/workflows/radioactive/SKILL.md`) into a 14-phase development lifecycle that embeds Matt Pocock's skills (`mattpocock/wayfinder`, `mattpocock/grilling`, `mattpocock/domain-modeling`, `mattpocock/research`, `mattpocock/prototype`, `mattpocock/to-spec`), ensuring Phase 1 ALWAYS executes the primary project skill first before running `/wayfinder`.

---

### Scope

- **Applies to**: Scaffolding or executing a project's `radioactive` workflow for complex features, domain-driven refactors, or cross-stack implementations requiring deep discovery, domain modeling, and quality-gated delivery.
- **Does not cover**: Single-file trivial bug fixes, pure documentation edits (use `document`), or isolated test additions.

---

### Triggers

- "Run embed-aihero-radioactive lifecycle"
- "/radioactive [feature description]" (when scaffolded with AI Hero embedding)
- "/embed-aihero-radioactive [feature description]"
- "Adapt radioactive workflow with Matt Pocock skills"

---

### Inputs

- `featureDescription`: Short text describing the feature or task (optional; if missing, triggers Phase 1 discovery).
- `projectSlug`: Short identifier for the repo (e.g., `my-backend`, `apix`).
- `baseBranch`: Target integration branch (default: `main` or `develop`).
- `techStack`: Core stack details (e.g., `NestJS + MikroORM + PostgreSQL`, `Rails + React`).
- `existingRootDoc`: Main instruction file (`AGENTS.md`, `CLAUDE.md`, etc.).
- `uiInvolved`: Boolean flag indicating if frontend/UI changes are required.
- `needsPrototype`: Boolean flag indicating if a proof-of-concept prototype spike is required.
- `maxFixAttempts`: Maximum iterations for the Phase 12 fix loop (default: 3).

---

### Invariants (Guardrails)

1. **Phase 1 Execution Order**: Phase 1 MUST ALWAYS execute the primary project entry skill (`skills/<projectSlug>/SKILL.md` or `AGENTS.md`) FIRST to classify the request and extract hard rules, and THEN execute `/wayfinder` for codebase mapping and intent framing.
2. **Sequential Phase Pipeline**: All 14 phases must execute in exact numerical order. No phase skipping.
3. **UUIDv7 Primary Key Mandatory Rule**: Every entity primary key or unique resource identifier MUST use UUIDv7 for time-ordered sorting and distributed uniqueness.
4. **Socratic Grilling Gate**: Phase 2 must present grilling questions and wait for explicit user response/confirmation before Phase 3 domain modeling.
5. **Spec Approval Gate**: Phase 7 must generate a structured task specification at `docs/tasks/task.<reference>.md` and obtain explicit user approval before Phase 8/9 execution.
6. **Executable Blueprint Contract**: Phase 8 must scaffold a reusable workflow contract at `skills/<projectSlug>/workflows/<feature-slug>/SKILL.md`.
7. **Pass/Fail Quality Gate**: Phase 10 specs must pass 100%. Phase 12 fix loop must resolve all 🔴 (Blockers) and 🟠 (High) findings before declaration of completion.
8. **Institutional Memory**: Phase 13 must capture newly discovered patterns into workflow contracts and update project documentation.
9. **Operational Safety for Infra Tasks**: When the feature affects network, infrastructure, IaC, or OS baselines, execution must include explicit rollback, change window alignment, and post-change health validation.

---

### Procedure

```
Phase 1  — CLASSIFY & WAYFIND  → Project Skill FIRST ➔ /mattpocock/wayfinder
Phase 2  — GRILLING            → /mattpocock/grilling (Deep Socratic interrogation & decision log)
Phase 3  — DOMAIN MODELING     → /mattpocock/domain-modeling (Entities, UUIDv7 keys, state machines)
Phase 4  — TECHNICAL RESEARCH  → /mattpocock/research (Technical spikes, dependency & API investigation)
Phase 5  — PROTOTYPE / SPIKE   → /mattpocock/prototype (Conditional: POC validation for high-risk logic)
Phase 6  — UX DESIGN           → /ui-ux-pro-max (Conditional: if UI involved)
Phase 7  — TO-SPEC             → /mattpocock/to-spec (Synthesize findings into docs/tasks/task.<reference>.md)
Phase 8  — BLUEPRINT CONTRACT  → /workflow-blueprint (Scaffold reusable feature contract)
Phase 9  — EXECUTE             → Implement tasks sequentially with evidence-based edits
Phase 10 — TESTS               → Run project specs/tests (100% pass required)
Phase 11 — REVIEW              → /thermo-nuclear-code-quality-review or /review
Phase 12 — FIX LOOP            → /thermo-fix × up to 3 (resolve 🔴→🟠, re-verify build)
Phase 13 — BLUEPRINTS UPDATE   → /workflow-blueprint (Update skills & institutional memory)
Phase 14 — CHANGELOG           → /changelog-generator or /changelog (Release notes)
```

#### Phase 1 — CLASSIFY & WAYFIND (Primary Project Skill ➔ /wayfinder)

1. **Step 1: Primary Project Skill Classification**:
   - Load the primary project entry skill (`skills/<projectSlug>/SKILL.md`, `.agents/skills/<projectSlug>/SKILL.md`, or `AGENTS.md`) **FIRST**.
   - Classify the user request against the project's routing matrix and extract project-wide hard constraints, tech stack details, and domain boundaries.
   - If NO primary project skill exists, trigger `/workflow-blueprint` init (or `plan-to-blueprint`) to scaffold baseline project context before proceeding.
2. **Step 2: Codebase Orientation with `/wayfinder`**:
   - Load the `wayfinder` skill (`skills/<projectSlug>/workflows/mattpocock/wayfinder/SKILL.md` or `workflows/mattpocock/wayfinder/SKILL.md`).
   - Map codebase entry points, data flows, route handlers, models, and subsystem boundaries relevant to the classified request.
   - Search repository for relevant controllers, schemas, database models, background jobs, and existing test coverage.
   - Identify existing architectural patterns, legacy constraints, and critical unknowns to feed Phase 2 grilling.
3. Output a 1-paragraph context summary combining the primary project skill rules and the `wayfinder.md` orientation brief.

#### Phase 2 — GRILLING with /mattpocock/grilling

1. Load `grilling` skill and parse the `wayfinder.md` brief and project rules.
2. Formulate 3 to 7 strategic questions covering Scope, Edge Cases, Data Contracts (UUIDv7 keys), and Performance/Security rules.
3. Present questions with trade-offs (Pros, Cons, Recommended Default).
4. **Wait for explicit user response.** Do NOT proceed until user answers are recorded into a binding **Decision Log** (`decision-log.md`).

#### Phase 3 — DOMAIN MODELING with /mattpocock/domain-modeling

1. Load `domain-modeling` skill and review `decision-log.md`.
2. Define Aggregate Roots, Entities, Value Objects, and field types.
3. **Enforce UUIDv7 primary keys** (`id: uuidv7`) across all domain models.
4. Model state transitions (using Mermaid `stateDiagram-v2`) and entity relationships (using Mermaid `erDiagram`).
5. Output `domain-model.md`.

#### Phase 4 — TECHNICAL RESEARCH with /mattpocock/research

1. Load `research` skill to address technical unknowns or API contract dependencies.
2. Execute temporary spikes in `scratch/` directory to measure performance or test library behavior.
3. Evaluate third-party dependencies, security surface, and API constraints.
4. Output `research-spike.md`.

#### Phase 5 — PROTOTYPE / SPIKE with /mattpocock/prototype (Conditional)

1. **Check condition**: Run if `needsPrototype` is true or if high-risk UI/algorithmic logic was identified.
2. Load `prototype` skill to build a lightweight proof-of-concept (POC) spike.
3. Perform interactive smoke tests or screenshot verification to test core hypotheses.
4. Record retrospective learnings in `prototype-report.md`.

#### Phase 6 — UX DESIGN with /ui-ux-pro-max (Conditional)

1. **Check condition**: Run if `uiInvolved` is true (or feature touches views, templates, or visual components).
2. Load `ui-ux-pro-max` skill.
3. Generate design tokens (color palette, CSS variables, typography) and responsive layout rules (using mobile-first `min-*` breakpoints).
4. Append UX design rules to `walkthrough.md`.

#### Phase 7 — TO-SPEC with /mattpocock/to-spec

1. Load `to-spec` skill and consolidate all upstream evidence (`wayfinder.md`, `decision-log.md`, `domain-model.md`, `research-spike.md`, `prototype-report.md`).
2. Produce a structured task specification at `docs/tasks/task.<reference>.md` featuring executive context, UUIDv7 domain models, numbered task steps with target file paths, verification commands, and rollback playbooks.
3. Present `docs/tasks/task.<reference>.md` to user and **gate on explicit user approval**.

#### Phase 8 — BLUEPRINT CONTRACT with /workflow-blueprint

1. Load `workflow-blueprint` (or `plan-to-blueprint`).
2. Transform approved task specification into an executable workflow contract saved at `skills/<projectSlug>/workflows/<feature-slug>/SKILL.md`.
3. Wire the new feature workflow into the project's routing matrix.

#### Phase 9 — EXECUTE

1. Execute tasks in `docs/tasks/task.<reference>.md` sequentially.
2. Read files before editing; apply minimal, targeted edits.
3. Track progress by marking `[/]` (in progress) and `[x]` (completed) in `docs/tasks/task.<reference>.md`.
4. For infra-impacting tasks, execute pre-change checks and capture rollback checkpoints before mutating state.

#### Phase 10 — TESTS

1. Run project test runner (e.g. `npm run test`, `rspec-rails`, `vitest`, `pytest`).
2. Write unit/integration/system tests for all newly created or modified logic.
3. Iterate until 100% of specs pass.
4. For operational tasks, run infrastructure checks (connectivity, policy checks, service health, drift validation) and archive outputs.

#### Phase 11 — REVIEW with Code Quality Engine

1. Run code review (`thermo-nuclear-code-quality-review` or `review` skill) against all files modified in the session.
2. Output a severity-ranked findings table:
   - 🔴 **Blocker**: Must fix before release.
   - 🟠 **High**: Fix in current cycle.
   - 🟡 **Medium**: Fix if trivial or record.
   - 🟢 **Low**: Informational.
3. Append findings table to `walkthrough.md` artifact.

#### Phase 12 — FIX LOOP (up to `maxFixAttempts` ×)

1. Iterate up to `maxFixAttempts` times:
   - Apply fixes for 🔴 Blocker and 🟠 High findings (`thermo-fix`).
   - Run build verification command (`bun run build`, `npm run test`, etc.).
   - Re-review modified files.
   - Stop loop early if zero 🔴 / 🟠 findings remain (verdict: APPROVED).
2. Update `docs/tasks/task.<reference>.md` and `walkthrough.md` with final verdict.
3. Create a clean git commit on the local working branch.

#### Phase 13 — BLUEPRINTS Update

1. Review discoveries made during execution.
2. Update existing project skills with new patterns, constraints, UUIDv7 schema details, or routing entries.
3. Scaffold new internal workflow blueprints if repeatable operations were introduced.
4. Update root doc (`AGENTS.md`) if core system rules changed.

#### Phase 14 — CHANGELOG Generation

1. Load `changelog-generator` (or `changelog` skill).
2. Analyze session git commits against base branch.
3. Generate customer-facing release notes categorized by Features, Fixes, and Improvements.
4. Append changelog entry to `walkthrough.md`.

---

### Scaffolding Target Projects

When `workflow-blueprint` scaffolds a project with `embed-aihero-radioactive`:
1. It creates/adapts `skills/<projectSlug>/workflows/radioactive/SKILL.md` using this 14-phase embedded contract.
2. It copies/scaffolds Matt Pocock's skills into `skills/<projectSlug>/workflows/mattpocock/` (`wayfinder`, `grilling`, `domain-modeling`, `research`, `prototype`, `to-spec`).
3. It registers `radioactive` and `mattpocock/*` subcommands in `skills/<projectSlug>/template.json` and `reference/routing-matrix.md`.

---

### Outputs

- Adapted `skills/<projectSlug>/workflows/radioactive/SKILL.md` contract.
- Scaffolded `skills/<projectSlug>/workflows/mattpocock/` skills.
- `wayfinder.md`: Orientation brief.
- `decision-log.md`: Grilling decision log.
- `domain-model.md`: Domain entity & state machine specification (with UUIDv7 keys).
- `research-spike.md` / `prototype-report.md` (when triggered).
- `docs/tasks/task.<reference>.md`: Approved execution plan.
- `walkthrough.md`: Decision log, UX specs, review tables, fix history, and user changelog.
- Clean git commit on local branch.

---

### Review gate

- [ ] Primary project skill loaded FIRST in Phase 1 before running `/wayfinder`.
- [ ] Phase 1 Wayfinder orientation brief (`wayfinder.md`) created.
- [ ] Phase 2 Socratic grilling questions answered by user and recorded in `decision-log.md`.
- [ ] Phase 3 Domain model defined with UUIDv7 primary keys and Mermaid state/ER diagrams.
- [ ] Phase 4 Technical research spike completed (if technical risks existed).
- [ ] Phase 5 Prototype POC validated (if prototype requested).
- [ ] Phase 6 UX design system generated (if UI feature).
- [ ] Phase 7 Technical specification (`docs/tasks/task.<reference>.md`) approved by user.
- [ ] Phase 8 Workflow contract created & registered in project routing matrix.
- [ ] Phase 9 Sequential execution complete.
- [ ] Phase 10 All test suites passing 100%.
- [ ] Phase 12 Zero Blocker (🔴) or High (🟠) findings remaining.
- [ ] Phase 13 Skills/blueprints updated with new knowledge.
- [ ] Phase 14 User-facing changelog generated.

---

### References

- `../../SKILL.md`
- `../mattpocock/wayfinder/SKILL.md`
- `../mattpocock/grilling/SKILL.md`
- `../mattpocock/domain-modeling/SKILL.md`
- `../mattpocock/research/SKILL.md`
- `../mattpocock/prototype/SKILL.md`
- `../mattpocock/to-spec/SKILL.md`
- `../radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
