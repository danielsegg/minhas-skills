# General Web Security Baseline

Use this baseline for web applications regardless of language. Adapt every control to the framework, application exposure, data sensitivity, deployment topology, and threat model.

## Authoritative Baseline

- Use OWASP Application Security Verification Standard (ASVS) as the primary application-control baseline. Record the adopted version; version 5.0.0 was current when this reference was written.
- Use OWASP Cheat Sheet Series for implementation details and OWASP Web Security Testing Guide for verification techniques.
- Use NIST SP 800-218 Secure Software Development Framework for lifecycle, build environment, provenance, and vulnerability-management practices.
- Verify current versions and framework guidance when accuracy may have changed.

Primary sources:

- https://owasp.org/www-project-application-security-verification-standard/
- https://cheatsheetseries.owasp.org/
- https://owasp.org/www-project-web-security-testing-guide/
- https://csrc.nist.gov/pubs/sp/800/218/final

## Threat and Scope Model

Identify before reviewing controls:

- protected assets and sensitive data;
- anonymous, authenticated, privileged, service, and administrative actors;
- tenant, store, organization, or account boundaries;
- browsers, mobile clients, APIs, jobs, webhooks, queues, and administrative interfaces;
- trust boundaries among client, application, database, proxy, identity provider, storage, and third parties;
- abuse cases with material confidentiality, integrity, availability, financial, privacy, or safety impact.

Do not treat the absence of a complete threat model as proof of a vulnerability. State the uncertainty and focus on observable trust boundaries.

## Review Coverage

Select relevant areas rather than mechanically reporting every item:

- architecture, security requirements, and threat modeling;
- authentication, credential recovery, MFA, and account lifecycle;
- session lifecycle, cookie attributes, rotation, invalidation, and fixation;
- authorization at function, object, field, tenant, and administrative levels;
- input validation, contextual output encoding, injection, XSS, and template safety;
- CSRF, CORS, CSP, clickjacking, redirects, and browser security headers;
- database queries, mass assignment, deserialization, and command execution;
- file upload, archive extraction, storage permissions, downloads, and path traversal;
- SSRF, outbound requests, URL validation, DNS behavior, and egress restrictions;
- API authentication, BOLA/IDOR, rate limits, resource consumption, and schema validation;
- webhook authenticity, replay protection, retries, idempotency, and partial failures;
- cryptography, key management, secrets, tokens, and randomness;
- sensitive data collection, retention, caching, logs, errors, exports, and backups;
- dependency provenance, lockfiles, build integrity, vulnerable components, and update policy;
- TLS termination, trusted proxies, production flags, debug exposure, and environment separation;
- monitoring, auditability, alerting, incident response, and security regression tests.

## Control Nuance

### Object identifiers

Enforce authorization on every object operation. Treat UUIDs, random tokens, slugs, and indirect references as defense in depth against enumeration or metadata disclosure, not as substitutes for authorization. Recommend opaque public identifiers only when justified by the threat model and after considering migration, indexing, storage, and compatibility.

### Transport, cookies, and HSTS

Do not report missing local-development TLS as a production defect without evidence about deployment. Determine where TLS terminates and how proxy headers are trusted. Require `Secure` cookies in HTTPS environments and choose `HttpOnly`, `SameSite`, domain, path, and lifetime attributes according to session behavior.

Use HSTS for HTTPS-only production sites through a staged rollout: start with a short `max-age`, monitor, then increase it. Add `includeSubDomains` only when every affected subdomain supports HTTPS. Treat preload as a separate, deliberate commitment with rollback implications.

### Authorization and multi-tenancy

Check authorization server-side at the action and resource boundary. Scope queries to the authenticated principal or tenant where practical, deny by default, and test horizontal and vertical access. UI hiding and unpredictable URLs are not controls.

### Input and output

Validate type, shape, range, size, and business invariants at trust boundaries. Parameterize database and command interfaces. Encode untrusted output for the exact HTML, attribute, URL, JavaScript, CSS, JSON, SQL, shell, or log context. Avoid treating one sanitizer as safe for every sink.

### External integrations

Authenticate webhooks and service calls, limit permissions, protect credentials, validate destinations and payloads, bound timeouts and response sizes, and design retries to avoid duplicate effects. Consider replay, confused-deputy, SSRF, dependency outage, and partial-success scenarios.

### Dependencies and supply chain

Inspect manifests, lockfiles, registries, build scripts, integrity metadata, and update policy. A version match alone does not establish exploitability; consider whether vulnerable code is reachable and whether compensating controls exist. Do not update dependencies during a review unless explicitly requested.
