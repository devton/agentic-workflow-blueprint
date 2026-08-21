---
name: domain-modeling
description: Domain entity definition, value objects, aggregate boundaries, state transition modeling, and business invariant contracts.
---

## agentic-workflows-blueprint.workflow.mattpocock.domain-modeling

### Goal

Formulate precise domain models, entity schemas, aggregate boundaries, state transition rules, and business invariants to ensure robust software architecture.

---

### Scope

- **Applies to**: Core data layer changes, state machine definitions, domain logic refactoring, schema migrations, and API payload modeling.
- **Does not cover**: Direct UI styling or raw technical spikes (use `/ui-ux-pro-max` or `/research`).

---

### Triggers

- "/domain-modeling"
- "Model domain entities"
- "Define business rules and state machines"
- "Schema and entity design"

---

### Inputs

- `decisionLog`: Confirmed decision log from Phase 2 grilling.
- `wayfinderBrief`: Orientation brief from Phase 1 wayfinder.
- `techStack`: Core technology stack details.
- `existingRootDoc`: Main project instruction document.

---

### Invariants (Guardrails)

1. **UUIDv7 Primary Key Mandatory Rule**: Every entity primary key or unique resource identifier MUST use UUIDv7 for time-ordered sorting and distributed uniqueness.
2. **Bounded Context & Aggregate Boundaries**: Explicitly define root aggregates and isolate domain responsibilities.
3. **Deterministic State Transitions**: State machines must define explicit allowed transitions, prohibited transitions, and transition triggers.
4. **Schema & Invariant Specifications**: Business rules, validation constraints, and database indexes must be fully specified.

---

### Procedure

#### 1) Parse Decision Log & Wayfinder Evidence

1. Extract entities, attributes, domain events, and state mutations from `decision-log.md` and `wayfinder.md`.
2. Identify core domain boundaries and bounded contexts.

#### 2) Define Aggregates & Entities

1. Identify Aggregate Roots, Entities, and Value Objects.
2. Specify entity attributes: field names, data types, nullability, default values, and foreign keys.
3. Enforce **UUIDv7** primary keys (`id: uuidv7`) for all new tables/entities.

#### 3) Model State Machines & Transitions

1. Define all permitted entity lifecycle states (e.g. `draft` ➔ `pending_approval` ➔ `published` ➔ `archived`).
2. Document permitted transitions, prohibited transitions, triggering events, and invariant guard conditions.
3. Generate a Mermaid state diagram (`stateDiagram-v2`) detailing state transitions.

#### 4) Draft Entity-Relationship (ER) Diagram

1. Generate a Mermaid ER diagram (`erDiagram`) mapping entity relationships (1:1, 1:N, N:M), foreign key constraints, and cascade policies.

#### 5) Compile Domain Model Specification

1. Document entities, value objects, invariants, schemas, state transition tables, and Mermaid diagrams into `domain-model.md`.

---

### Outputs

- `domain-model.md`: Domain model specification containing Mermaid ER & state diagrams, entity schemas, UUIDv7 primary key definitions, and business invariant contracts.

---

### Review gate

- [ ] UUIDv7 primary key standard enforced for all entities.
- [ ] Aggregate boundaries and entities clearly specified.
- [ ] State machine transitions documented with guards and side effects.
- [ ] Mermaid ER (`erDiagram`) and State (`stateDiagram-v2`) diagrams generated and syntactically valid.

---

### References

- `../../../SKILL.md`
- `../grilling/SKILL.md`
- `../research/SKILL.md`
- `../to-spec/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
