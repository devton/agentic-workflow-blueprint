---
name: research
description: Technical spikes, API feasibility studies, dependency investigations, and performance risk assessments.
---

## agentic-workflows-blueprint.workflow.mattpocock.research

### Goal

Conduct evidence-driven technical research, dependency analysis, and isolated code spikes to eliminate technical risk and validate architectural choices.

---

### Scope

- **Applies to**: Integration spikes, third-party API research, library evaluations, performance bottlenecks, and unresolved technical risks.
- **Does not cover**: Full production code implementation or user requirement discovery (use `/wayfinder` or `/grilling`).

---

### Triggers

- "/research"
- "Run technical research spike"
- "Investigate API and dependencies"
- "Assess technical feasibility"

---

### Inputs

- `technicalUnknowns`: List of technical risks, API contracts, or unknown library behaviors.
- `domainModel`: Draft domain model specification from Phase 3.
- `wayfinderBrief`: Orientation brief from Phase 1.
- `techStack`: Core technology stack details.

---

### Invariants (Guardrails)

1. **Empirical Evidence First**: Base findings strictly on concrete code inspection, test runs, or benchmark outputs; no guessing.
2. **Isolated Spike Execution**: Run temporary research scripts or spikes in `scratch/` directory without polluting production code.
3. **Security & Dependency Audit**: Evaluate license, vulnerabilities, and maintenance status for any proposed third-party package.
4. **Research Report Output**: Document findings, benchmarks, trade-offs, and clear implementation recommendations in `research-spike.md`.

---

### Procedure

#### 1) Extract Technical Questions & Risks

1. Parse `wayfinder.md`, `decision-log.md`, and `domain-model.md` to identify unresolved technical questions, API contracts, or performance targets.
2. Formulate explicit hypotheses to test during research.

#### 2) Perform Code & Dependency Investigation

1. Inspect package manifests (`package.json`, `Gemfile`, `Cargo.toml`, `go.mod`), source code, and official API documentation.
2. Verify API rate limits, authentication protocols, payload constraints, and backward compatibility.

#### 3) Execute Spike / Benchmarking

1. Create isolated research scripts or spikes in `<appDataDir>/scratch/` or workspace `scratch/` directory (e.g. `scratch/spike_api_test.ts` or `.py`).
2. Run benchmark tests or API integration probes to measure latency, throughput, memory footprint, or failure behavior.
3. Capture exact output logs and performance metrics.

#### 4) Formulate Implementation Recommendations

1. Compare alternative approaches, detailing Pros, Cons, Performance Data, and Maintenance Overhead.
2. Recommend the optimal technical path with code snippets and configuration flags.

#### 5) Publish Research Spike Report

1. Record findings, benchmark logs, and recommendations in `research-spike.md` or conversation context.

---

### Outputs

- `research-spike.md`: Technical research spike report detailing findings, benchmarks, API contracts, and implementation recommendations.
- Temporary research scripts in `scratch/`.

---

### Review gate

- [ ] Technical research conducted using empirical code/log evidence.
- [ ] Code spikes executed in `scratch/` without uncommitted production churn.
- [ ] Security and dependency risks evaluated.
- [ ] Actionable implementation recommendation published in `research-spike.md`.

---

### References

- `../../../SKILL.md`
- `../domain-modeling/SKILL.md`
- `../prototype/SKILL.md`
- `../to-spec/SKILL.md`
- `../../embed-aihero-radioactive/SKILL.md`
- [Interactive HTML View](./README.html)
