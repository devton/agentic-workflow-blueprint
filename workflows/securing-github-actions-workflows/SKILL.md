## agentic-workflows-blueprint.workflow.securing-github-actions-workflows

### Goal

Harden GitHub Actions workflows against supply chain abuse, token misuse, and unsafe workflow execution patterns through enforceable CI security controls.

### Scope

- Applies to: GitHub Actions workflow security posture in repositories and organizations.
- Does not cover: non-GitHub CI systems or application vulnerability scanning itself.

### Triggers

- "Secure GitHub Actions workflows"
- "Enforce SHA pinning and least-privilege workflow permissions"
- "Harden pull_request and pull_request_target behavior"
- "Add workflow change controls and secret handling safeguards"

### Inputs

- `repoScope`: repositories/workflows in scope
- `orgPolicyContext` (optional): organization-level Actions/security settings
- `tokenPolicy`: default and per-job permission model
- `thirdPartyActionPolicy`: approved actions and pinning strategy
- `forkPolicy`: external contributor and approval requirements
- `exceptionPolicy` (optional): approved deviations with owner/expiry

### Invariants

- Third-party actions must be pinned to immutable SHAs.
- `GITHUB_TOKEN` permissions default to least privilege.
- Untrusted input must never be directly interpolated into shell commands.
- Workflow changes require explicit ownership/review controls.
- Secrets exposure paths are blocked and auditable.

### Procedure

1. **Inventory and baseline**
   - Enumerate workflows, referenced actions, trigger events, and permission scopes.
   - Identify mutable version tags, broad permissions, and unguarded secret usage.

2. **Enforce action integrity**
   - Pin actions to immutable SHAs.
   - Introduce update automation for pinned SHAs (for example Dependabot strategy).

3. **Minimize token privileges**
   - Set restrictive workflow-level defaults.
   - Grant job-specific permissions only where required.

4. **Harden untrusted input handling**
   - Refactor vulnerable `run` interpolations to safe env/script patterns.
   - Validate PR metadata handling against injection risks.

5. **Secure fork/PR execution model**
   - Prefer safe trigger patterns for external contributions.
   - Constrain or gate privileged execution paths.

6. **Protect secrets and environments**
   - Enforce environment protection for sensitive jobs.
   - Validate no secret values are logged or passed insecurely.

7. **Apply change governance**
   - Require CODEOWNERS/reviewer controls for workflow/action paths.
   - Enforce org-level policy settings for Actions permissions and approvals.

8. **Validate and monitor**
   - Lint workflows, run dry checks, and verify policy enforcement behavior.
   - Publish security audit report with remediation backlog and ownership.

### Outputs

- Hardened workflow definitions with pinned actions and scoped permissions.
- Workflow security audit report with findings and remediation status.
- Governance controls for workflow path changes (owner/reviewer policy).
- Exception register for temporary deviations.

### Review gate

- [ ] All third-party actions are SHA-pinned.
- [ ] Workflow/job token permissions follow least-privilege model.
- [ ] No unsafe untrusted-input interpolation paths remain.
- [ ] Secret handling and environment protection controls are enforced.
- [ ] Workflow change governance and approval controls are active.

### References

- `../../SKILL.md`
- `../implementing-devsecops-security-scanning/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
