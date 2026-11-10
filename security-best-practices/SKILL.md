---
name: security-best-practices
description: Evidence-based secure coding, threat-aware web application security review, vulnerability reporting, and remediation guidance across languages and frameworks. Use only when the user explicitly requests security best practices, secure-by-default implementation, a security review or report, vulnerability analysis, threat analysis, or security fixes. Do not trigger for general code review, debugging, or non-security work.
---

# Security Best Practices

Apply security guidance using repository evidence, current authoritative sources, and the actual deployment context. Avoid claiming that a review proves an application secure.

## Establish Context

1. Read the applicable repository instructions and security or architecture documentation.
2. Identify the in-scope frontend, backend, API, data store, authentication, proxy, deployment, and third-party integration surfaces.
3. Determine the requested mode: secure implementation, targeted guidance, review/report, or remediation.
4. Identify assets, actors, sensitive data, trust boundaries, exposed entry points, and material abuse cases before prioritizing controls.
5. State material scope gaps, assumptions, unavailable evidence, and unsupported technology coverage.

Treat repository conventions as implementation constraints, not automatic security exemptions. When a project requirement conflicts with a security control, explain the residual risk and propose a documented exception, compensating control, owner, and review condition where appropriate.

## Load Guidance

- For web applications, read [references/general-web-security.md](references/general-web-security.md) completely.
- For a review, report, or remediation task, also read [references/reporting-and-remediation.md](references/reporting-and-remediation.md) completely.
- Inspect `references/` for language- or framework-specific guidance and read only every file relevant to the in-scope stack.
- If specific guidance is absent or may be stale, consult current primary documentation from the framework, language, OWASP, NIST, or another authoritative standards body when browsing is available.
- State reduced confidence or coverage when authoritative stack-specific guidance cannot be verified. Do not fabricate support.
- Prefer versioned requirements and record the version used when referencing a standard such as OWASP ASVS.

## Operating Modes

### Secure implementation

- Follow the repository's established security mechanisms before creating custom controls.
- Use secure defaults while preserving required local development behavior through explicit environment-aware configuration.
- Add tests for authorization, validation, failure paths, and security invariants affected by the change.

### Targeted guidance

- Answer the requested security question without expanding into an unsolicited full audit.
- Distinguish requirements, defense in depth, operational hardening, and optional risk reductions.

### Review or report

- Operate read-only unless the user also requested changes.
- Trace reachable flows and trust boundaries; do not infer a vulnerability from a suspicious primitive alone.
- Report confirmed findings separately from hypotheses or hardening opportunities.
- Prioritize exploitable, high-impact issues over exhaustive low-value observations.
- Return findings in the conversation by default. Write a report file only when explicitly requested, using the requested path or the repository's documented convention.

### Remediation

- Modify code only when the user requested fixes or separately authorized remediation.
- Address root causes and preserve existing execution, error, authorization, transaction, logging, and integration contracts unless changing one is necessary and disclosed.
- Group fixes that share a cause, boundary, and verification path; isolate unrelated or high-risk changes.
- Explain behavior and regression risks before changes that may break clients, sessions, integrations, stored data, or deployment.
- Verify each fix with focused tests and the repository's normal quality checks.
- Comment non-obvious security invariants, not the generic fact that code follows a best practice.

## Safety Boundaries

- Default to source inspection, configuration review, dependency metadata inspection, and local tests.
- Do not scan, probe, exploit, load-test, or access external or production systems without explicit authorization and a clearly bounded target.
- Do not perform destructive tests, persistence, privilege escalation, exfiltration, or denial-of-service activity.
- Use minimal, non-destructive proofs of concept in local or designated test environments.
- Never reproduce secrets, credentials, tokens, private keys, or unnecessary personal data in tool output or reports. Redact evidence while preserving its diagnostic value.
- Treat dependency analysis as read-only unless updates were explicitly requested.
- Do not weaken controls merely to make a test pass. Identify the compatibility issue and propose a safe migration path.

## Core Review Principles

- Require server-side authorization for every protected action and object; unpredictable identifiers are not authorization controls.
- Validate at trust boundaries and encode output for its destination context; avoid generic sanitization as a universal defense.
- Use framework-provided parameterization, session, CSRF, escaping, cryptography, and secret-management mechanisms where appropriate.
- Analyze authentication, access control, sessions, injection, browser defenses, files, outbound requests, APIs, integrations, secrets, logging, dependencies, data protection, and operational configuration according to scope.
- Evaluate transactions, concurrency, idempotency, retries, replay protection, and partial failures for multi-step or externally integrated operations.
- Evaluate TLS, secure cookies, proxy trust, and HSTS against the production topology. Roll HSTS out carefully; do not reject it categorically or enable preload without validating all affected domains.
- Measure performance or availability claims and distinguish security defects from hardening opportunities.

## Communication

- Use precise, non-alarmist language.
- Separate observed evidence, inference, assumptions, and recommendations.
- Include file and line references for code findings whenever stable local evidence exists.
- Do not assign CVSS scores without enough environmental and exploitability data.
- Make risk acceptance a user or business decision; provide the technical consequences and compensating options.
