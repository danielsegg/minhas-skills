---
name: sdd-spec-writer
description: Create repository-grounded, executable Spec-Driven Development (SDD) implementation specifications as .spec.md files or Markdown contracts by analyzing project instructions, documentation, code, tests, and conventions. Use when the user asks Codex to create a specification, write a spec, define an implementation contract, plan a development task, produce an executable task brief for humans or agents, or write/update a .spec.md file.
---

# SDD Spec Writer

Use this skill to write precise SDD specifications that act as implementation contracts for human developers and AI agents.

## Core Rule

Write the spec so that implementation should not require follow-up questions. If a capable agent would fail because of ambiguity, the spec is not specific enough.

## Spec File Location

For every new spec file, locate the project root before choosing the output path:

- If a `tasks/` directory exists directly under the project root, create the spec as `tasks/<descriptive-name>.spec.md`.
- If the project-root `tasks/` directory does not exist, create the spec as `<descriptive-name>.spec.md` directly in the project root.
- Do not create a `tasks/` directory solely to store the spec.
- When updating an existing spec, preserve its current path instead of relocating it.
- Treat the Git worktree root as the project root when available. Otherwise, use the detected project root from repository instructions or project configuration.

## Project Evidence

Before drafting any spec, gather evidence proportionally to the task:

1. Read project instructions:
   - Locate and read all applicable `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and equivalent instruction files from the project root through the affected scope.
   - Follow their language, architecture, tooling, testing, file-placement, and workflow rules.
   - Treat applicable repository instructions as authoritative for project conventions unless they conflict with higher-priority instructions.
2. Read relevant documentation:
   - Locate the documentation index and inspect the `README`, architecture, domain, database, integration, testing, deployment, and contribution documents relevant to the task.
   - Use targeted reads; do not load unrelated documentation merely for completeness.
3. Trace the affected implementation flow:
   - Inspect the relevant routes or entry points, controllers or handlers, services or use cases, domain models, persistence, schema and migrations, authorization, integrations, UI, configuration, and tests.
   - Identify established patterns, dependencies, side effects, validation boundaries, non-regression constraints, and executable project commands.
4. Inspect existing specs and history when useful:
   - Reuse the structure and terminology of relevant existing specs without carrying stale facts forward.
   - Inspect Git history only when it helps explain intent, conventions, or a suspected regression.
5. Preserve evidence integrity:
   - Support codebase-specific claims with inspected repository files and cite the most relevant repository-relative paths in `Context & Impact`.
   - Do not invent paths, symbols, commands, schemas, business rules, dependencies, or behavior.
   - If the user request, repository instructions, documentation, and current code materially disagree, expose the discrepancy in `Open Questions` instead of silently choosing one interpretation.
   - If the repository or necessary evidence is unavailable, state the limitation and avoid pretending the spec is repository-grounded.

## Workflow

1. Determine the target output:
   - If the user asks for a file, create or update a `.spec.md` file.
   - For a new file, resolve the project root and apply the `Spec File Location` rule before writing.
   - For an existing file, update it in place.
   - If the user asks only for text, return the spec in Markdown.
   - Always use the `.spec.md` extension for spec files, for example `create-order.spec.md`.
2. Gather repository context before writing:
   - Complete the `Project Evidence` workflow for the affected scope.
   - Prefer `rg`, `rg --files`, `git status --short`, and targeted file reads.
   - Record the evidence needed to justify the implementation contract and verification commands.
3. Identify whether the task is appropriate for an agent or a human:
   - Use `agent` for CRUD, validation, mapping, infrastructure, application-layer tasks, or well-bounded work expected to take less than 8 hours.
   - Use `human` for subjective UI/UX judgment, code review, undocumented legacy behavior, unclear product decisions, or new architecture decisions.
4. Resolve ambiguity conservatively:
   - Make reasonable assumptions only when low risk and state them in the spec.
   - If a missing decision would materially change the implementation, include an explicit `Open Questions` section instead of pretending certainty.
5. Produce concrete contracts:
   - Use exact types, field names, status codes, error formats, paths, commands, and test data.
   - Define side effects and non-regression requirements.
   - Include verification commands that can run without manual arguments.

## Required Spec Structure

Every spec must follow this structure and keep the headings in this order:

```markdown
# <Task Title>

## Metadata
- developer_type: agent|human
- estimated_complexity: low|medium|high
- languages:
  - <language/framework>

## Objective
<One paragraph explaining what must be built and why.>

## Context & Impact
<Existing code, applicable instructions and documentation, patterns to follow, integrations, affected modules, non-regression analysis, and repository-relative paths supporting these claims.>

## Implementation Contract
### Inputs
<Exact input types, schemas, DTOs, request bodies, CLI args, config keys, or events.>

### Outputs
<Exact success return structures, response codes, files, UI states, emitted events, or generated artifacts.>

### Side Effects
<Database writes, queues, files, caches, global state, network calls, logs, metrics, or "None".>

### Error Handling & Edge Cases
<Expected errors, exceptions, HTTP statuses, validation rules, limits, empty states, concurrency, retries, permissions, and rollback behavior.>

## Files to Create/Modify & Dependencies
### Files
- `<path>`: <required change>

### Dependencies
- <package/library/tool or "No new dependencies">

## Required Tests
1. <Concrete success test with inputs and expected result>
2. <Concrete error or edge-case test with inputs and expected result>
3. <Concrete regression or integration test with inputs and expected result>

## Acceptance Criteria
- [ ] <Automatically or objectively verifiable criterion>
- [ ] <Automatically or objectively verifiable criterion>

## Verification Commands
```bash
<command that can run without manual placeholders>
```
```

Add `## Open Questions` at the end only when implementation-critical information is missing.

## Quality Checklist

Before delivering the spec, verify:

- All applicable project instruction files were read and followed.
- Relevant project documentation was inspected without substituting assumptions for documented rules.
- The affected code flow and existing tests were traced far enough to define implementation and regression boundaries.
- `Context & Impact` cites the key repository-relative instruction, documentation, code, and test paths supporting the spec.
- Material conflicts among the request, instructions, documentation, and code are captured in `Open Questions`.
- Exact types are named, for example `OrderDto` instead of "object".
- Error handling includes exceptions, HTTP statuses, validation messages, or error codes when applicable.
- Edge cases and boundary limits are explicit.
- New dependencies are listed with environment-specific installation notes, including Docker/container commands when relevant.
- File paths are complete and repository-relative.
- Every new spec file is stored in the project-root `tasks/` directory when it exists, or directly in the project root when it does not.
- Test cases include concrete data and at least one success case plus one error or edge case.
- Acceptance criteria are objective and checkable.
- Verification commands are real commands discovered from the repository or clearly justified.
- The spec does not require the implementer to infer business rules that should have been written down.
