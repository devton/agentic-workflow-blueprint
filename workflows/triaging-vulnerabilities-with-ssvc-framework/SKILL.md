## agentic-workflows-blueprint.workflow.triaging-vulnerabilities-with-ssvc-framework

### Goal

Prioritize vulnerability remediation using SSVC decision logic by combining exploitation intelligence, technical impact, and mission context into deterministic response classes.

### Scope

- Applies to: vulnerability triage pipelines that need risk-based prioritization beyond CVSS-only scoring.
- Does not cover: direct patch deployment execution or vulnerability scanning itself.

### Triggers

- "Prioritize vulnerabilities with SSVC"
- "Triage CVEs into Act/Attend/Track outcomes"
- "Combine KEV/EPSS with operational risk context"
- "Create SLA-based remediation priorities"

### Inputs

- `vulnerabilityDataset`: source findings (scanner exports, SIEM feeds, asset mappings)
- `threatIntelSources`: KEV, EPSS, vendor advisories, exploit telemetry
- `missionContext`: business criticality and prevalence per asset/system
- `technicalAttributes`: CVSS vectors, exploitability hints, affected components
- `triagePolicy`: SLA mapping and escalation rules by SSVC outcome
- `reportTarget` (optional): output path/system for triage artifacts

### Invariants

- SSVC outcomes must be reproducible from explicit decision inputs.
- Exploitation evidence quality must be documented for each decision.
- Critical mission systems receive stricter decision posture.
- Every triage outcome maps to an actionable remediation deadline.
- Exceptions or overrides require rationale and owner approval.

### Procedure

1. **Ingest and normalize findings**
   - Import vulnerability records and normalize identifiers (CVE, asset, component, environment).
   - De-duplicate findings and retain traceability to source systems.

2. **Collect exploitation intelligence**
   - Enrich findings with KEV presence, EPSS scores, and known PoC/active exploitation signals.
   - Mark confidence level for each exploitation data point.

3. **Evaluate SSVC decision points**
   - Classify exploitation status, technical impact, automatability, mission prevalence, and public-wellbeing impact.
   - Record the evidence path for each classified field.

4. **Apply SSVC decision tree**
   - Derive one of `Act`, `Attend`, `Track*`, `Track` per vulnerability.
   - Resolve ties/ambiguities using predeclared triage policy rules.

5. **Map outcomes to actions and SLAs**
   - Assign remediation owner, due date, and escalation path based on outcome class.
   - Generate prioritized queues for operations and security teams.

6. **Validate and calibrate**
   - Run test cases on known CVEs to verify decision consistency.
   - Compare outcomes against historical incident lessons and adjust policy if needed.

7. **Publish triage report**
   - Export machine-readable and human-readable triage outputs.
   - Include unresolved data-quality gaps and recommended follow-ups.

### Outputs

- SSVC triage report with per-vulnerability decision outcomes.
- Prioritized remediation backlog with SLA targets.
- Evidence ledger for decision-point inputs and confidence.
- Exception/override register with owner and rationale.

### Review gate

- [ ] Every vulnerability has a deterministic SSVC outcome.
- [ ] Outcome-to-SLA mapping is explicit and actionable.
- [ ] Exploitation and impact inputs are traceable to evidence.
- [ ] Overrides/exceptions are documented and approved.
- [ ] Output supports both analyst review and automation ingestion.

### References

- `../../SKILL.md`
- `../review/SKILL.md`
- `../changelog/SKILL.md`
- [Interactive HTML View](./README.html)
