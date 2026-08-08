## agentic-workflows-blueprint.workflow.c4-architecture

### Goal

Produce clear, audience-appropriate architecture documentation using the C4 model with Mermaid diagrams and traceable narrative context.

### Scope

- Applies to: architecture documentation generation (context/container/component/deployment/dynamic views).
- Does not cover: implementation of system changes described by diagrams.

### Triggers

- "Generate C4 architecture diagrams"
- "Document system architecture with Mermaid"
- "Create context/container/component views"
- "Produce deployment architecture documentation"

### Inputs

- `systemScope`: product/system boundary to document
- `audience`: executive, product, architecture, developer, or operations
- `evidenceSources`: codebase structure, docs, infra metadata, and domain inputs
- `diagramLevels`: required C4 levels and optional dynamic views
- `outputTarget`: architecture docs path and naming strategy

### Invariants

- Context and container views are baseline deliverables unless explicitly out of scope.
- Diagram detail must match audience needs and avoid unnecessary complexity.
- Labels and relationships must be explicit, directional, and technology-aware.
- Generated docs must remain maintainable and linked to evidence sources.

### Procedure

1. **Define scope and audience**
   - Confirm system boundary, key actors, and target reader expectations.
   - Select required C4 levels for delivery.

2. **Collect architecture evidence**
   - Map services, data stores, integration points, and deployment topology.
   - Validate ownership and dependency boundaries.

3. **Draft C4 diagrams**
   - Build context and container views first.
   - Add component/deployment/dynamic diagrams where they add decision value.

4. **Apply diagram quality rules**
   - Keep naming consistent, relationships directional, and technology labels explicit.
   - Avoid overloaded diagrams by splitting concerns when needed.

5. **Document narrative context**
   - Add concise explanation for each diagram: purpose, boundaries, and key flows.
   - Highlight assumptions and open questions.

6. **Publish architecture artifacts**
   - Write outputs to agreed doc paths and naming conventions.
   - Link diagrams to related workflows and technical references.

### Outputs

- C4 architecture markdown artifacts with Mermaid diagrams.
- Context and container baseline documentation.
- Optional component/deployment/dynamic views by audience need.
- Assumption and gap notes for follow-up.

### Review gate

- [ ] Context and container diagrams exist and are coherent.
- [ ] Diagram level/depth matches intended audience.
- [ ] Relationships and technology labels are explicit and accurate.
- [ ] Documentation includes narrative context and assumptions.
- [ ] Output paths and naming are consistent.

### References

- `../../SKILL.md`
- `../document/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
