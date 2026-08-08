---
name: plan-writing
description: Generate structured implementation plans with task breakdowns, dependencies, and verification criteria.
---

## agentic-workflows-blueprint.workflow.plan-writing

### Goal

Transform a decision log and feature requirements into a structured, dependency-mapped task breakdown saved as `task.md`.

### Scope

- **Applies to**: Feature design, refactoring tasks, or multi-step execution.
- **Does not cover**: Direct code execution (use `radioactive` or execution workflow).

### Triggers

- "/plan-writing"
- "Create implementation plan"
- "Generate task breakdown"

### Inputs

- `decisionLog`: Confirmed decisions from discovery.
- `projectSlug`: Target repo identifier.

### Invariants

1. **Explicit File Targets**: Every task must specify target file paths.
2. **Dependency Mapping**: Clearly list task prerequisites.
3. **Verification Criteria**: Include explicit check criteria for each task.
4. **Operational Commands Required**: Infra/network/IaC/OS tasks must include concrete pre-check, apply/change, verification, and rollback commands.

### Procedure

1. Read decision log and context.
2. Break feature into discrete tasks (simple/medium/complex).
3. Specify dependencies and file paths.
4. Add verification criteria (which tests/checks to run).
   - For infra tasks, include command-level verification (`terraform plan`, connectivity checks, service health checks, policy/security checks).
5. Output plan to `task.md` artifact.
6. Present plan to user for approval.

### Outputs

- `task.md` artifact containing structured task list.

### Review gate

- Plan presents complete task breakdown with dependencies.
- Plan approved by user.

### References

- `../../SKILL.md`
- `../brainstorming/SKILL.md`
- `../radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
