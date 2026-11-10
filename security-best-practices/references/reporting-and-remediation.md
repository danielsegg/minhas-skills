# Security Reporting and Remediation

Use this reference for security reviews, formal reports, and fixes.

## Classify Findings

Base priority on demonstrated impact, likelihood, exposure, required privileges, exploit preconditions, affected users or data, and existing controls.

- **Critical:** credible path to catastrophic impact such as broad compromise, high-value secret exposure, systemic tenant bypass, or destructive unauthenticated action requiring immediate coordination.
- **High:** realistic path to major confidentiality, integrity, financial, or availability impact.
- **Medium:** meaningful weakness with constrained impact, stronger preconditions, or effective partial mitigations.
- **Low:** limited-impact defect or defense-in-depth gap with a plausible security consequence.
- **Hardening:** useful improvement without enough evidence to call it a vulnerability.

Assign confidence independently:

- **High confidence:** directly evidenced and reachable, with the relevant control absent or bypassable.
- **Medium confidence:** strong evidence exists, but reachability, deployment, or a compensating control remains uncertain.
- **Low confidence:** suspicious pattern or missing evidence requiring validation; report as a hypothesis, not a confirmed vulnerability.

Do not inflate severity to prioritize uncertain work. Do not calculate CVSS unless the task requires it and enough metric evidence exists.

## Finding Format

Use stable IDs such as `SEC-001`. Include:

1. title;
2. severity, confidence, and status (`confirmed`, `potential`, or `hardening`);
3. affected component and surface;
4. evidence with file and line, configuration key, request flow, or reproducible local behavior;
5. preconditions and plausible attack path;
6. concrete impact;
7. root cause;
8. recommended remediation and alternatives;
9. compatibility, migration, and rollback considerations;
10. verification or regression test;
11. applicable CWE or versioned ASVS requirement when supported by evidence.

For Critical findings, lead with a one-sentence impact statement and an immediate containment option. Redact secrets and sensitive payloads.

## Evidence Rules

- Trace data and authorization decisions from source to sink when possible.
- Verify framework defaults and middleware ordering before declaring a missing control.
- Distinguish code that is reachable in production from examples, tests, dead code, and development-only configuration.
- Check compensating controls at reverse proxies, gateways, identity providers, database policies, and deployment configuration when they are in scope and observable.
- Cite the most specific evidence available and state what could not be verified.
- Avoid presenting a dependency advisory as exploitable without assessing version, configuration, reachability, and environmental controls.

## Report Structure

When a file report is explicitly requested, use:

1. executive summary;
2. scope, method, constraints, and excluded testing;
3. risk overview by severity and confidence;
4. findings ordered by priority;
5. hardening opportunities;
6. remediation sequence and dependencies;
7. unresolved questions and coverage gaps.

State that a source review is not equivalent to a penetration test or proof of complete security. Write the report only to the user-specified location or the repository's documented convention.

## Remediation Workflow

1. Confirm that remediation is authorized; a request for review alone does not authorize edits.
2. Reproduce or establish a failing security invariant with the safest practical local test.
3. Select the smallest root-cause fix compatible with the architecture.
4. Evaluate client, data, session, integration, and deployment migration effects.
5. Implement related fixes together only when they share a cause and verification path.
6. Run focused security regression tests and the repository's normal checks.
7. Re-evaluate the original attack path and document residual risk.

If a control must be deferred or bypassed, record the rationale, affected assets, risk owner, duration or review trigger, and compensating controls. Never describe an accepted risk as a secure implementation.
