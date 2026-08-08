## agentic-workflows-blueprint.workflow.network-engineering

### Goal

Define and execute network engineering changes with explicit design evidence, validation steps, and rollback readiness.

### Scope

- Applies to: network topology, segmentation, routing, DNS, firewall, load balancing, and connectivity updates.
- Does not cover: generic application feature implementation unrelated to network behavior.

### Triggers

- "Design this network change"
- "Update routing/firewall rules"
- "Validate connectivity after infra change"
- "Create network engineering workflow"

### Inputs

- `environment` (dev/stage/prod)
- `changeWindow`
- `networkIntent` (what is changing and why)
- `topologyContext` (subnets, zones, peerings, gateways, ACLs)
- `verificationPlan` (commands, probes, synthetic checks)
- `rollbackPlan`

### Invariants

- Every change must define blast radius and dependency boundaries before execution.
- Validation commands must be explicit and reproducible.
- Rollback path must be available before any state mutation.
- Security boundaries (ACL, security groups, firewall policy) must remain least-privilege.

### Procedure

1. Capture current-state topology and intended target-state.
2. Produce change plan with affected paths, ports/protocols, and dependency map.
3. Define pre-change checks (reachability, DNS, latency, packet loss, route tables).
4. Apply network changes in a controlled sequence aligned with `changeWindow`.
5. Execute post-change validation commands and compare against pre-change baseline.
6. If validation fails, execute rollback and record exact failure point.
7. Produce a handoff report with final state and residual risks.

### Outputs

- Network change plan artifact.
- Validation log (pre/post checks and outcomes).
- Rollback report (executed or confirmed-ready).
- Handoff summary for operations and documentation workflows.

### Review gate

- [ ] Blast radius, dependencies, and security boundaries are explicit.
- [ ] Pre and post validation commands are documented and executed.
- [ ] Rollback path is complete and actionable.
- [ ] Final state matches intended network intent.

### References

- `../../SKILL.md`
- `../infra-operations/SKILL.md`
- `../document/SKILL.md`
- [Interactive HTML View](./README.html)
