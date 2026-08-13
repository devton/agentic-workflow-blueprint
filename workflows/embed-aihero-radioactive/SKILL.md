---
name: embed-aihero-radioactive
description: |
  Full-cycle 14-phase development workflow blueprint embedding AI Hero skills (Wayfinder, Grilling, Domain Modeling, Research, Prototype, To-Spec) into Radioactive's quality-gated engineering pipeline.
---

## agentic-workflows-blueprint.workflow.embed-aihero-radioactive

### Goal

Provide a portable, end-to-end 14-phase development lifecycle workflow that embeds AI Hero's strategic orientation, Socratic grilling, domain modeling, technical research spikes, prototyping, and spec synthesis directly into Radioactive's quality-gated execution, review, fix, and changelog pipeline.

---

### Scope

- **Applies to**: Complex features, domain-driven refactors, mission-critical services, or cross-stack implementations (e.g. database + backend + API + UI) requiring deep discovery and robust architecture.
- **Does not cover**: Single-file trivial bug fixes, pure documentation edits (use `document`), or isolated test additions.

---

### Triggers

- "Run embed-aihero-radioactive lifecycle"
- "/embed-aihero-radioactive [feature description]"
- "AI Hero radioactive feature cycle"
- "Full-cycle AI Hero development"

---

### Inputs

- `featureDescription`: Short text describing the feature or task (optional; if missing, triggers Phase 1 Wayfinder discovery).
- `projectSlug`: Short identifier for the repo (e.g., `my-backend`, `apix`).
- `baseBranch`: Target integration branch (default: `main` or `develop`).
- `techStack`: Core stack details (e.g., `NestJS + MikroORM + PostgreSQL`, `Rails + React`).
- `existingRootDoc`: Main instruction file (`AGENTS.md`, `CLAUDE.md`, etc.).
- `uiInvolved`: Boolean flag indicating if frontend/UI changes are required.
- `needsPrototype`: Boolean flag indicating if a proof-of-concept prototype spike is required.
- `maxFixAttempts`: Maximum iterations for the Phase 12 fix loop (default: 3).

---

### Invariants (Guardrails)

1. **Sequential Phase Pipeline**: All 14 phases must execute in exact numerical order. No phase skipping.
2. **UUIDv7 Primary Key Mandatory Rule**: Every entity primary key or unique resource identifier MUST use UUIDv7 for time-ordered sorting and distributed uniqueness.
3. **Abstract Entrypoint Fallback**:
   - **If a primary project skill exists** (e.g. `skills/<projectSlug>/SKILL.md` or `.agents/skills/<projectSlug>/SKILL.md` or `AGENTS.md`): Read it, classify request, and enforce project-specific hard rules and routing.
   - **If NO primary project skill exists**: Trigger `/workflow-blueprint` init (or `plan-to-blueprint`) to scaffold baseline project context and routing before proceeding.
4. **Socratic Grilling Gate**: Phase 2 must present grilling questions and wait for explicit user response/confirmation before Phase 3 domain modeling.
5. **Spec Approval Gate**: Phase 7 must generate a structured `task.md` specification and obtain explicit user approval before Phase 8/9 execution.
6. **Executable Blueprint Contract**: Phase 8 must scaffold a reusable workflow contract at `skills/<projectSlug>/workflows/<feature-slug>/SKILL.md`.
7. **Pass/Fail Quality Gate**: Phase 10 specs must pass 100%. Phase 12 fix loop must resolve all 🔴 (Blockers) and 🟠 (High) findings before declaration of completion.
8. **Institutional Memory**: Phase 13 must capture newly discovered patterns into workflow contracts and update project documentation.
9. **Operational Safety for Infra Tasks**: When the feature affects network, infrastructure, IaC, or OS baselines, execution must include explicit rollback, change window alignment, and post-change health validation.

---

### Procedure

```
Phase 1  — WAYFINDER & INIT    → /wayfinder (Orienting codebase, mapping context & entrypoints)
Phase 2  — GRILLING            → /grilling (Deep Socratic interrogation & decision log)
Phase 3  — DOMAIN MODELING     → /domain-modeling (Entities, UUIDv7 keys, state machines, invariants)
Phase 4  — TECHNICAL RESEARCH  → /research (Technical spikes, dependency & API investigation)
Phase 5  — PROTOTYPE / SPIKE   → /prototype (Conditional: POC validation for high-risk logic)
Phase 6  — UX DESIGN           → /ui-ux-pro-max (Conditional: if UI involved)
Phase 7  — TO-SPEC             → /to-spec (Synthesize findings into actionable task.md)
Phase 8  — BLUEPRINT CONTRACT  → /workflow-blueprint (Scaffold reusable feature contract)
Phase 9  — EXECUTE             → Implement tasks sequentially with evidence-based edits
Phase 10 — TESTS               → Run project specs/tests (100% pass required)
Phase 11 — REVIEW              → /thermo-nuclear-code-quality-review or /review
Phase 12 — FIX LOOP            → /thermo-fix × up to 3 (resolve 🔴→🟠, re-verify build)
Phase 13 — BLUEPRINTS UPDATE   → /workflow-blueprint (Update skills & institutional memory)
Phase 14 — CHANGELOG           → /changelog-generator or /changelog (Release notes)
```

#### Phase 1 — WAYFINDER & INIT with /wayfinder

1. Load `wayfinder` skill to map codebase entry points, data flows, and subsystem boundaries.
2. Search repository for relevant routes, controllers, schemas, database models, and existing test coverage.
3. Identify existing patterns, architectural constraints, and critical unknowns.
4. Generate `wayfinder.md` orientation brief.

#### Phase 2 — GRILLING with /grilling

1. Load `grilling` skill and parse the `wayfinder.md` brief.
2. Formulate 3 to 7 strategic questions covering Scope, Edge Cases, Data Contracts (UUIDv7 keys), and Performance/Security rules.
3. Present questions with trade-offs (Pros, Cons, Recommended Default).
4. **Wait for explicit user response.** Do NOT proceed until user answers are recorded into a binding **Decision Log** (`decision-log.md`).

#### Phase 3 — DOMAIN MODELING with /domain-modeling

1. Load `domain-modeling` skill and review `decision-log.md`.
2. Define Aggregate Roots, Entities, Value Objects, and field types.
3. **Enforce UUIDv7 primary keys** (`id: uuidv7`) across all domain models.
4. Model state transitions (using Mermaid `stateDiagram-v2`) and entity relationships (using Mermaid `erDiagram`).
5. Output `domain-model.md`.

#### Phase 4 — TECHNICAL RESEARCH with /research

1. Load `research` skill to address technical unknowns or API contract dependencies.
2. Execute temporary spikes in `scratch/` directory to measure performance or test library behavior.
3. Evaluate third-party dependencies, security surface, and API constraints.
4. Output `research-spike.md`.

#### Phase 5 — PROTOTYPE / SPIKE with /prototype (Conditional)

1. **Check condition**: Run if `needsPrototype` is true or if high-risk UI/algorithmic logic was identified.
2. Load `prototype` skill to build a lightweight proof-of-concept (POC) spike.
3. Perform interactive smoke tests or screenshot verification to test core hypotheses.
4. Record retrospective learnings in `prototype-report.md`.

#### Phase 6 — UX DESIGN with /ui-ux-pro-max (Conditional)

1. **Check condition**: Run if `uiInvolved` is true (or feature touches views, templates, or visual components).
2. Load `ui-ux-pro-max` skill.
3. Generate design tokens (color palette, CSS variables, typography) and responsive layout rules (using mobile-first `min-*` breakpoints).
4. Append UX design rules to `walkthrough.md`.

#### Phase 7 — TO-SPEC with /to-spec

1. Load `to-spec` skill and consolidate all upstream evidence (`wayfinder.md`, `decision-log.md`, `domain-model.md`, `research-spike.md`, `prototype-report.md`).
2. Produce a structured `task.md` specification featuring executive context, UUIDv7 domain models, numbered task steps with target file paths, verification commands, and rollback playbooks.
3. Present `task.md` to user and **gate on explicit user approval**.

#### Phase 8 — BLUEPRINT CONTRACT with /workflow-blueprint

1. Load `workflow-blueprint` (or `plan-to-blueprint`).
2. Transform approved `task.md` into an executable workflow contract saved at `skills/<projectSlug>/workflows/<feature-slug>/SKILL.md`.
3. Wire the new feature workflow into the project's routing matrix.

#### Phase 9 — EXECUTE

1. Execute tasks in `task.md` sequentially.
2. Read files before editing; apply minimal, targeted edits.
3. Track progress by marking `[/]` (in progress) and `[x]` (completed) in `task.md`.
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
2. Update `task.md` and `walkthrough.md` with final verdict.
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

### Outputs

- `wayfinder.md`: Orientation brief.
- `decision-log.md`: Grilling decision log.
- `domain-model.md`: Domain entity & state machine specification (with UUIDv7 keys).
- `research-spike.md` / `prototype-report.md` (when triggered).
- `task.md`: Approved execution plan.
- `walkthrough.md`: Decision log, UX specs, review tables, fix history, and user changelog.
- `skills/<projectSlug>/workflows/<feature-slug>/SKILL.md`: Executable contract for the feature.
- Updated project skills and routing matrix.
- Clean git commit on local branch.

---

### Review gate

- [ ] Phase 1 Wayfinder orientation brief (`wayfinder.md`) created.
- [ ] Phase 2 Socratic grilling questions answered by user and recorded in `decision-log.md`.
- [ ] Phase 3 Domain model defined with UUIDv7 primary keys and Mermaid state/ER diagrams.
- [ ] Phase 4 Technical research spike completed (if technical risks existed).
- [ ] Phase 5 Prototype POC validated (if prototype requested).
- [ ] Phase 6 UX design system generated (if UI feature).
- [ ] Phase 7 Technical specification (`task.md`) approved by user.
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
