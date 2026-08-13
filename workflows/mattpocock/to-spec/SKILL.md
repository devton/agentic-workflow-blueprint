---
name: to-spec
description: Specification synthesis protocol to transform wayfinder orientation, grilling logs, domain models, research spikes, and prototypes into actionable, executable technical plans.
---

## agentic-workflows-blueprint.workflow.mattpocock.to-spec

### Goal

### Goal

Synthesize orientation briefs, grilling decision logs, domain models, technical research spikes, and prototype learnings into a rigorous, unambiguous executable technical specification saved at `docs/tasks/task.<reference>.md`.

---

### Scope

- **Applies to**: Transforming conceptual discovery and research artifacts into executable implementation specifications and task breakdowns.
- **Does not cover**: Direct code execution or preliminary discovery (use upstream Matt Pocock skills or execution phase).

---

### Triggers

- "/to-spec"
- "Convert findings to executable spec"
- "Generate specification from prototype and domain model"
- "To-spec conversion"

---

### Inputs

- `wayfinderBrief`: Orientation brief (`wayfinder.md`).
- `decisionLog`: Grilling decision log (`decision-log.md`).
- `domainModel`: Domain entity & state machine specification (`domain-model.md`).
- `researchReport` (optional): Technical research spike report (`research-spike.md`).
- `prototypeReport` (optional): Prototype retrospective (`prototype-report.md`).
- `projectSlug`: Short identifier for the repository.

---

### Invariants (Guardrails)

1. **Unambiguous Task Mapping**: Every task in `docs/tasks/task.<reference>.md` must specify exact target file paths, explicit dependencies, step-by-step code changes, and verification commands.
2. **Enforce UUIDv7 Identity Standards**: Ensure all data models, schemas, and resource IDs in the spec mandate UUIDv7 for time-ordered sorting.
3. **Strict Phase Gating**: The generated specification (`docs/tasks/task.<reference>.md`) must obtain explicit user approval before execution (Phase 9 of embed-aihero-radioactive).
4. **Isolated Task File Path**: Save specifications under `docs/tasks/task.<reference>.md` to avoid cluttering root and prevent overwriting tasks from other sessions.

---

### Procedure

#### 1) Consolidate Upstream Evidence

1. Gather all upstream artifacts: `wayfinder.md`, `decision-log.md`, `domain-model.md`, `research-spike.md`, and `prototype-report.md`.
2. Verify that all critical unknowns have been resolved and domain entities have UUIDv7 primary keys.

#### 2) Structure Technical Specification (`docs/tasks/task.<reference>.md`)

Produce a structured task specification file at `docs/tasks/task.<reference>.md` formatted as follows:

```markdown
# Technical Specification & Execution Plan: <Feature Title>

## 1. Executive Context & Objectives
- **Goal**: Clear 1-2 sentence objective.
- **Scope**: Explicit In-Scope vs. Out-of-Scope boundaries.
- **Key Invariants**: UUIDv7 primary keys, security rules, performance SLA.

## 2. Architecture & Domain Model
- Summary of entities, UUIDv7 schemas, state machines, and API endpoints.
- Embedded Mermaid ER / State diagrams from Phase 3.

## 3. Sequential Task Breakdown
- [ ] Task 1: <Task Title>
  - **Target Files**: `path/to/file.ext`
  - **Dependencies**: None / Task X
  - **Changes Required**: Detailed description of code/schema edits.
  - **Verification Command**: `npm run test` or `bun test path/to/spec`

- [ ] Task 2: <Task Title>
  - **Target Files**: `path/to/file.ext`
  - **Dependencies**: Task 1
  - **Changes Required**: Detailed description of edits.
  - **Verification Command**: `npm run test`

## 4. Quality Assurance & Test Strategy
- Unit test requirements, integration tests, and performance/security verification steps.

## 5. Rollback & Operational Safety Playbook
- Pre-change snapshot commands, change window constraints, and step-by-step rollback procedures.
```

#### 3) Review & Sanity-Check Specification

1. Verify that all target file paths exist or have parent directories defined.
2. Ensure task dependencies form a directed acyclic graph (DAG) without circular loops.
3. Confirm that verification commands are non-interactive and testable.

#### 4) Present Specification & Gate on User Approval

1. Present `docs/tasks/task.<reference>.md` to the user.
2. **Gate on explicit user approval** before proceeding to Phase 8 (Blueprint Contract) and Phase 9 (Execute).

---

### Outputs

- `docs/tasks/task.<reference>.md`: Comprehensive technical specification and execution breakdown ready for implementation.

---

### Review gate

- [ ] All upstream artifacts (wayfinder, decision log, domain model, research, prototype) integrated.
- [ ] Tasks contain exact file paths, explicit edits, and verification commands.
- [ ] UUIDv7 primary key standard enforced for all data models.
- [ ] Specification presented to user and explicit approval obtained.

---

### References

- `../../../SKILL.md`
- `../domain-modeling/SKILL.md`
- `../prototype/SKILL.md`
- `../../plan-to-blueprint/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
