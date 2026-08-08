## agentic-workflows-blueprint.workflow.implementing-syslog-centralization-with-rsyslog

### Goal

Implement centralized syslog collection with rsyslog using encrypted transport, structured routing, and reliability controls for security operations.

### Scope

- Applies to: rsyslog-based server/client log forwarding architecture and operational hardening.
- Does not cover: full SIEM content engineering or long-term log analytics platform design.

### Triggers

- "Implement centralized syslog with rsyslog"
- "Enable TLS-protected log forwarding"
- "Deploy per-host log routing and reliable queues"
- "Standardize secure log ingestion pipeline"

### Inputs

- `collectorTopology`: log server/client host mapping
- `tlsMaterial`: CA/server/client certificate strategy
- `portPolicy`: ingestion ports and transport protocol choices
- `retentionPolicy`: file paths, rotation, and storage constraints
- `availabilityPolicy`: queue/retry/backpressure requirements
- `deploymentMethod` (optional): automation/SSH/config-management approach

### Invariants

- Log transport must be authenticated and encrypted in transit.
- Client forwarding must tolerate transient outages using durable queue strategy.
- Collector outputs must preserve host/source segregation.
- Configuration changes require validation before production rollout.
- Critical logging paths must avoid silent data loss behavior.

### Procedure

1. **Design centralization topology**
   - Define collector and client roles, expected log sources, and network paths.
   - Align retention and segregation structure with SOC/forensics needs.

2. **Provision trust and TLS**
   - Generate and distribute CA/server/client certificates.
   - Configure TLS auth mode and peer validation settings.

3. **Configure rsyslog collector**
   - Enable secure TCP input modules and listener ports.
   - Define templates for per-host/per-program output routing.

4. **Configure rsyslog clients**
   - Configure secure forwarding actions to collector endpoint.
   - Enable disk-assisted queues, retry, and resume behavior.

5. **Deploy and validate**
   - Apply configurations through selected deployment method.
   - Validate handshake, forwarding continuity, and routing correctness.

6. **Harden and operationalize**
   - Verify rotation and storage controls.
   - Add health checks and alerting for forwarding failures.

### Outputs

- Hardened rsyslog server/client configuration set.
- TLS validation and log delivery verification report.
- Reliability test evidence (queue, retry, outage behavior).
- Operations handoff notes for monitoring and maintenance.

### Review gate

- [ ] TLS-protected log transport is enforced and validated.
- [ ] Per-host/per-source routing works as expected.
- [ ] Queue/retry settings prevent silent data loss on outages.
- [ ] Deployment and rollback steps are documented.
- [ ] Log pipeline monitoring checks are defined.

### References

- `../../SKILL.md`
- `../infra-operations/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
