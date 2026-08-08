## agentic-workflows-blueprint.workflow.remediating-s3-bucket-misconfiguration

### Goal

Identify and remediate S3 bucket misconfigurations that can expose data, then enforce preventive controls to reduce recurrence risk.

### Scope

- Applies to: S3 public access, bucket policy/ACL, encryption, logging, and guardrail automation controls.
- Does not cover: non-AWS object storage platforms.

### Triggers

- "Remediate S3 bucket misconfiguration"
- "Fix public S3 exposure findings"
- "Enforce encryption and logging on S3 buckets"
- "Deploy preventive controls for S3 security posture"

### Inputs

- `awsAccountScope`: account/org scope and target regions
- `bucketInventory`: in-scope bucket list and data criticality tags
- `findingSources`: Config/Security Hub/Macie/Analyzer findings
- `encryptionPolicy`: SSE-KMS/SSE-S3 requirements
- `loggingPolicy`: server access logging and CloudTrail data-event requirements
- `changeWindow` (optional): execution window for disruptive policy changes
- `rollbackPlan`: rollback strategy for accidental access disruption

### Invariants

- Public exposure controls are enforced at account and bucket levels.
- Bucket policies and ACL posture must align with least-access principles.
- Sensitive buckets require encryption and access traceability by default.
- Preventive controls must be codified to avoid configuration drift.
- Incident evidence must be preserved before destructive remediations.

### Procedure

1. **Detect and classify misconfiguration**
   - Aggregate findings from Config, Access Analyzer, Security Hub, and policy/ACL inspection.
   - Classify buckets by exposure severity and data sensitivity.

2. **Contain immediate public exposure**
   - Enable account-level and bucket-level Block Public Access.
   - Confirm externally accessible paths are closed.

3. **Remediate policy and ownership controls**
   - Remove or replace overly permissive bucket policies.
   - Enforce bucket ownership controls to eliminate legacy ACL dependence where applicable.

4. **Enforce encryption and transport protections**
   - Apply default encryption according to policy.
   - Deny insecure transport and unencrypted uploads through bucket policy controls.

5. **Enable logging and detection telemetry**
   - Configure server access logging and S3 data event auditing.
   - Ensure alerting paths for suspicious or prohibited access behavior.

6. **Deploy preventive governance**
   - Add org/account guardrails (for example SCP and Config auto-remediation patterns).
   - Restrict unauthorized changes to public-access and critical bucket settings.

7. **Validate and document**
   - Re-scan bucket posture after remediation.
   - Produce exposure timeline, remediation evidence, and residual risk notes.

### Outputs

- S3 remediation report with bucket-by-bucket status.
- Applied policy/encryption/logging control evidence.
- Preventive guardrail deployment summary.
- Residual risk and follow-up action register.

### Review gate

- [ ] Public access exposure is eliminated for protected buckets.
- [ ] Policy/ACL posture follows least-access requirements.
- [ ] Encryption and logging standards are enforced.
- [ ] Preventive controls are enabled to reduce recurrence.
- [ ] Post-remediation validation confirms compliance state.

### References

- `../../SKILL.md`
- `../securing-aws-iam-permissions/SKILL.md`
- `../review/SKILL.md`
- [Interactive HTML View](./README.html)
