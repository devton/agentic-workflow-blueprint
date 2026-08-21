---
name: grilling
description: Deep Socratic grilling and interrogation protocol to stress-test assumptions, clarify non-functional requirements, and resolve edge cases before design.
---

## agentic-workflows-blueprint.workflow.mattpocock.grilling

### Goal

Systematically interrogate assumptions, edge cases, trade-offs, and operational boundaries through intensive Socratic questioning before domain modeling or code implementation.

---

### Scope

- **Applies to**: Feature requirement clarification, ambiguous requests, architectural trade-offs, and high-impact changes.
- **Does not cover**: Direct code implementation or initial codebase mapping (use `/wayfinder`).

---

### Triggers

- "/grilling"
- "Grill user on feature details"
- "Challenge feature assumptions"
- "Socratic grilling session"

---

### Inputs

- `wayfinderBrief` or `userPrompt`: Orientation brief or initial feature request.
- `existingRootDoc`: Main project instruction document (`AGENTS.md`, `CLAUDE.md`).

---

### Invariants (Guardrails)

1. **Mandatory Socratic Interrogation Gate**: Present 3–7 strategic questions covering Scope, Edge Cases, Data Integrity, Security, and Operational Constraints. Do NOT proceed without explicit user responses.
2. **Option-Driven Questioning**: Present questions with clear trade-offs (Pros, Cons, Default Option) so choices are actionable.
3. **Strict UUIDv7 Ordering Invariant**: Enforce time-sortable UUIDv7 for resource identifiers in data model decisions.
4. **Binding Decision Log**: Record all user answers and trade-off resolutions into a binding decision log to govern domain modeling and spec creation.

---

### Procedure

#### 1) Analyze Wayfinder & Request Context

1. Review the orientation brief (`wayfinder.md`) and original request to extract ambiguous requirements, boundary decisions, and risk areas.
2. Identify implicit assumptions that must be explicitly confirmed.

#### 2) Formulate Categorized Grilling Questions

Formulate **3 to 7 targeted questions** across key dimensions:
- **Category A: Scope & Boundary Limits** — What is strictly in vs. out of scope for this iteration?
- **Category B: Edge Cases & Failure Modes** — How to handle network timeouts, concurrency conflicts, invalid payloads, or missing data?
- **Category C: Data Contracts & Invariants** — Permitted mutations, state transition guards, UUIDv7 identity rules, and audit logging.
- **Category D: Performance & Security Constraints** — Rate limiting, RBAC permissions, zero-downtime deployment requirements, and migration strategy.

Format each question with:
- Context & Problem Statement
- Option 1 (Pros / Cons)
- Option 2 (Pros / Cons)
- Recommended Default Option

#### 3) Present Grilling Interview

1. Present the formatted questions clearly to the user.
2. **Wait for explicit user answers.** Do NOT proceed to domain modeling or spec writing until grilling responses are recorded.

#### 4) Synthesize Binding Decision Log

1. Summarize user choices and confirmed trade-offs into a binding **Decision Log**.
2. Store decision log in conversation context and append to `walkthrough.md`.

---

### Outputs

- Binding `decision-log.md` stored in conversation context and `walkthrough.md`.

---

### Review gate

- [ ] At least 3 to 7 targeted grilling questions presented with trade-offs.
- [ ] Explicit user responses received and recorded.
- [ ] UUIDv7 identity standard confirmed for resource data models.
- [ ] Binding decision log generated to govern domain modeling and specification.

---

### References

- `../../../SKILL.md`
- `../wayfinder/SKILL.md`
- `../domain-modeling/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
