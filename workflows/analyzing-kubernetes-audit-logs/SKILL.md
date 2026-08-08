## agentic-workflows-blueprint.workflow.analyzing-kubernetes-audit-logs

### Goal

Analyze Kubernetes API audit logs to detect high-risk behaviors (privilege escalation, secret access, lateral movement patterns) and produce actionable detection and response artifacts.

### Scope

- Applies to: Kubernetes API server audit event streams and exported log files.
- Does not cover: host-level syscall telemetry or network packet-level forensics.

### Triggers

- "Analyze Kubernetes audit logs"
- "Investigate suspicious Kubernetes API activity"
- "Create detections for exec/secrets/RBAC abuse"
- "Validate SOC coverage for Kubernetes attack techniques"

### Inputs

- `auditLogSource`: file path, stream, or SIEM index containing audit events
- `timeWindow`: interval under investigation
- `clusterContext`: cluster/environment and namespace scope
- `detectionProfile`: event patterns and severity mapping
- `knownIdentities` (optional): approved users/service accounts for suppression baselines
- `incidentContext` (optional): case ID, IOC list, and investigation hypotheses

### Invariants

- Parsing logic must preserve original event fields for traceability.
- High-risk event classes are explicitly monitored (`pods/exec`, secret access, RBAC mutations, privileged pod creation, unauthenticated access).
- Detection outputs must distinguish confirmed suspicious activity from expected admin operations.
- Investigation artifacts must be suitable for SOC handoff and replay.

### Procedure

1. **Ingest and normalize logs**
   - Parse JSON-lines audit entries and validate schema consistency.
   - Normalize key fields (verb, resource, user, namespace, source IP, response code, stage).

2. **Apply high-risk detection rules**
   - Detect shell access (`pods/exec`, `pods/attach`), secret enumeration/access, RBAC change events, and privileged workload creations.
   - Detect anonymous/unauthenticated API access and unusual identity behaviors.

3. **Correlate and enrich**
   - Correlate events by user, namespace, workload, and time sequence.
   - Enrich with identity criticality, known admin windows, and threat indicators.

4. **Classify findings**
   - Rank detections by severity and confidence.
   - Separate expected operational actions from anomalous activity.

5. **Generate detection artifacts**
   - Produce rule candidates or SIEM queries for recurring patterns.
   - Document event signatures and false-positive tuning guidance.

6. **Incident handoff and response support**
   - Build timeline of key suspicious actions.
   - Provide recommended containment/investigation next steps.

### Outputs

- Structured audit analysis report with severity-ranked findings.
- Event timeline highlighting suspicious activity chains.
- Detection rule/query package for SOC/SIEM integration.
- Tuning notes and suppression candidates for noisy expected behavior.

### Review gate

- [ ] Core high-risk event categories are covered by explicit detections.
- [ ] Findings include enough context for analyst action.
- [ ] Event evidence is traceable back to raw audit records.
- [ ] Detection package is reproducible and suitable for automation.
- [ ] False-positive handling guidance is documented.

### References

- `../../SKILL.md`
- `../triaging-vulnerabilities-with-ssvc-framework/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
