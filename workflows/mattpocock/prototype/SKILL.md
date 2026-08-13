---
name: prototype
description: Rapid proof-of-concept (POC) prototyping and disposable spike implementation to validate UI/logic hypotheses before specification.
---

## agentic-workflows-blueprint.workflow.mattpocock.prototype

### Goal

Build rapid, lightweight proof-of-concept (POC) prototypes to validate complex user interface or architectural hypotheses and gather empirical feedback prior to formal specification.

---

### Scope

- **Applies to**: High-risk UI interactions, complex algorithmic logic, innovative features, dynamic workflows, or untried system flows.
- **Does not cover**: Final production-ready implementations or comprehensive test suite writing (use Phase 9 EXECUTE and Phase 10 TESTS).

---

### Triggers

- "/prototype"
- "Build rapid prototype spike"
- "Validate POC hypothesis"
- "Create prototype implementation"

---

### Inputs

- `hypothesisToValidate`: Target feature hypothesis, UI interaction, or algorithm to test.
- `domainModel`: Domain entity model specification.
- `researchReport`: Technical research findings from Phase 4.
- `uiInvolved`: Boolean flag indicating if frontend/UI components are involved.

---

### Invariants (Guardrails)

1. **Time-Bounded Disposable Implementation**: Prototypes must focus strictly on proving core hypotheses; refrain from premature polish or production refactoring.
2. **Non-Destructive Feature Branching**: Build prototypes in temporary scratch files or isolated feature branches.
3. **Empirical UX/Logic Validation**: Run smoke tests or visual generation (e.g., using `generate_image` or dev server preview) to verify behavior.
4. **Prototype Retrospective**: Document key learnings, technical debt to avoid, and refined requirements for `/to-spec`.

---

### Procedure

#### 1) Define Hypothesis & Validation Criteria

1. Clearly articulate the specific hypothesis or UX interaction being validated.
2. Establish explicit pass/fail criteria for the prototype (e.g., response time under X ms, intuitive layout flow, zero layout shifts).

#### 2) Build Lightweight POC

1. Implement minimal viable logic or UI mock using simplified state or mock data providers.
2. If UI is involved, apply rapid styling (e.g. CSS flexbox/grid, glassmorphism, or modern web guidance patterns).

#### 3) Execute & Test Prototype

1. Launch local dev server (`npm run dev` or equivalent) or execute test script to verify real-time execution.
2. If UI components are created, generate visual screenshots or inspect rendered DOM elements to validate visual hierarchy.
3. Conduct interactive smoke testing to verify core user interactions and edge cases.

#### 4) Evaluate Results & Solicit Feedback

1. Review prototype performance, usability, responsive layout behavior, and failure modes.
2. Record user or reviewer feedback regarding prototype ergonomics.

#### 5) Compile Prototype Learnings

1. Document findings in `prototype-report.md`, summarizing:
   - Hypotheses confirmed vs. refuted.
   - Refined requirements and edge cases discovered.
   - Design patterns or code structures to carry forward into `/to-spec`.

---

### Outputs

- `prototype-report.md`: Prototype retrospective report detailing validated hypotheses, UI/UX feedback, and refined specification requirements.
- Lightweight disposable code spike.

---

### Review gate

- [ ] Core hypothesis tested with empirical runtime or visual proof.
- [ ] Prototype kept isolated from production main branch.
- [ ] Retrospective learnings recorded in `prototype-report.md` to feed `/to-spec`.

---

### References

- `../../../SKILL.md`
- `../research/SKILL.md`
- `../to-spec/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
