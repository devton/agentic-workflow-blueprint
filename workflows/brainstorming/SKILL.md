---
name: brainstorming
description: Socratic questioning protocol and user communication protocol to uncover requirements, edge cases, and scope.
---

## agentic-workflows-blueprint.workflow.brainstorming

### Goal

Uncover user requirements, trade-offs, scope boundaries, and edge cases through a structured Socratic discovery protocol before writing code.

### Scope

- **Applies to**: New features, ambiguous requests, or complex architecture changes.
- **Does not cover**: Direct code implementation or execution.

### Triggers

- "/brainstorming"
- "Brainstorm feature requirements"
- "Ask discovery questions"

### Inputs

- `userPrompt`: Initial request or topic.
- `existingRootDoc`: Project root doc.

### Invariants

1. **Socratic Gate**: Do NOT start coding until 3+ strategic questions are asked and answered.
2. **Context Before Content**: Understand architecture boundaries, security, and data models first.
3. **Decision Log**: Summarize answers into a persistent decision log.
4. **Operational Risk Discovery**: For infra/network/IaC/OS tasks, discovery must include blast radius, rollback path, and observability checks.

### Procedure

1. Parse request to identify domain, data model impact, security, and edge cases.
2. Formulate **3 to 5 targeted questions** with clear trade-offs (Pros, Cons, Default Option).
   - For infrastructure tasks, include at least one question in each category:
     - network/security boundaries;
     - operational window and rollback;
     - validation and monitoring evidence.
3. Present questions to user and **WAIT** for explicit response.
4. Synthesize answers into a **Decision Log** to drive the plan.

### Outputs

- Confirmed decision log stored in context and `walkthrough.md`.

### Review gate

- At least 3 discovery questions presented and answered by user.
- Decision log generated.

### References

- `../../SKILL.md`
- `../radioactive/SKILL.md`
- `../plan-writing/SKILL.md`
- [Interactive HTML View](./README.html)
