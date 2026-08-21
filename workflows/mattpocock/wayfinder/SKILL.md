---
name: wayfinder
description: Strategic orientation, codebase mapping, architectural entry point identification, and intent framing prior to grilling, research, or prototyping.
---

## agentic-workflows-blueprint.workflow.mattpocock.wayfinder

### Goal

Provide a structured codebase navigation and problem space orientation protocol to map systems, data flows, and architectural constraints before deep technical design.

---

### Scope

- **Applies to**: Feature discovery, legacy codebase navigation, architecture exploration, and initial intent framing.
- **Does not cover**: Direct code execution or implementation (use execution or prototyping workflows).

---

### Triggers

- "/wayfinder"
- "Run wayfinder orientation"
- "Map codebase context for feature"
- "Orient problem space"

---

### Inputs

- `userPrompt`: Description of the feature or architectural requirement.
- `projectSlug`: Short identifier for the repository (e.g. `my-backend`).
- `baseBranch`: Default integration branch (default: `main` or `develop`).
- `techStack`: Core technology stack details.
- `existingRootDoc`: Main project instruction document (`AGENTS.md`, `CLAUDE.md`).

---

### Invariants (Guardrails)

1. **UUIDv7 Standard**: Always specify UUIDv7 for all primary keys and entity IDs in resource mappings.
2. **Evidence-Based Codebase Mapping**: Read actual entry points, route handlers, models, and configs before framing problem scope; do NOT assume directory or system structure.
3. **Problem Space Framing**: Define explicit system boundaries, affected modules, entry/exit points, and critical unknowns.
4. **Orientation Summary**: Produce a clear orientation summary (`wayfinder.md` or conversation context) to anchor subsequent grilling, modeling, or research phases.

---

### Procedure

#### 1) Parse Intent & Target Surface

1. Analyze user prompt to identify target domain, operational context, and core goals.
2. Extract implicit assumptions and map primary entry channels (HTTP controllers, event subscribers, CLI commands, background workers).

#### 2) Codebase Exploration & Symbol Search

1. Search the repository for relevant routes, controllers, schemas, database models, background jobs, and existing tests.
2. Identify existing patterns, conventions, and reusable utilities in the codebase.

#### 3) Map Dependency & Data Flow

1. Trace request pathways from entry (HTTP/CLI/Event) through business logic layers down to storage and third-party services.
2. Map incoming payloads to domain models and outgoing responses.

#### 4) Highlight Technical Risks & Unknowns

1. Identify legacy technical debt, missing test coverage, breaking change vectors, and unconfirmed assumptions.
2. Highlight complex integration boundaries or potential performance bottlenecks.

#### 5) Produce Wayfinder Orientation Brief

1. Output a structured summary (`wayfinder.md`) covering:
   - **System Context**: Overview of affected subsystems.
   - **Affected Modules**: Explicit file paths and components.
   - **Key Architectural Constraints**: Security, data integrity, UUIDv7 primary keys, and performance boundaries.
   - **Critical Unknowns**: Specific questions to be resolved during grilling or technical research.

---

### Outputs

- `wayfinder.md`: Orientation brief detailing codebase mapping, entrypoints, data flows, and critical unknowns.

---

### Review gate

- [ ] Codebase entry points and file targets verified via code search and file viewing.
- [ ] Data flows and system boundaries mapped.
- [ ] UUIDv7 identity standard specified for entity identifiers.
- [ ] Critical unknowns documented to feed Phase 2 grilling and technical research.

---

### References

- `../../../SKILL.md`
- `../grilling/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
