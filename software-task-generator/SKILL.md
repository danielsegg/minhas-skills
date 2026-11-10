---
name: software-task-generator
description: Draft concise, repository-grounded software tasks for Jira or similar tools. Automatically propose an Epic with small, independently deliverable tasks when the scope is broad; do not implement the work.
---

# Software Task Generator

Create copy-ready software work items in Brazilian Portuguese. Optimize for fidelity to the request, clear completion, and the smallest useful scope.

## Workflow

1. **Understand the ask.** Extract the requested outcome, affected users, constraints, and explicit format. Separate confirmed requirements from assumptions. Do not turn proposal details, implementation ideas, or repository observations into requirements unless the request supports them.
2. **Ground proportionally.** If a repository is available, read its applicable instructions and inspect only the relevant docs, code, and tests needed to confirm current behavior and likely change boundaries. Cite repository-relative paths only when useful. Treat uncommitted work as context, never as authorization or new requirements.
3. **Choose the work-item shape.** Use one task for a bounded outcome with one coherent completion condition. Use an Epic when the request spans multiple independently deliverable outcomes or coordinated workstreams (for example, several domain areas or a user flow requiring separately shippable backend, interface, and reporting work). Do not split merely because many files or technical layers are involved.
4. **Draft the smallest complete result.** Include only requirements necessary to achieve the stated outcome, relevant observable acceptance criteria, and technical context supported by evidence. Keep optional ideas, deferred decisions, and out-of-scope items distinct.
5. **Check fidelity and size.** Remove duplicated rules, speculative requirements, exhaustive implementation inventories, generic boilerplate, and details that do not change delivery or QA. If uncertainty materially changes behavior, state one concise assumption or ask one focused question.

## Epic and task decomposition

When scope qualifies for an Epic, output:

- A short Epic with the business outcome, boundaries, and Epic-level completion condition.
- A numbered set of **3–7** child tasks grouped in delivery order, unless evidence supports fewer. Each task must have one primary outcome, a clear finish condition, and only necessary dependencies.
- A brief dependency note only where ordering matters. Avoid nested subtasks, duplicated descriptions, estimates, dates, and assignments unless requested.

Each child task should be independently understandable and reviewable. Split by user-visible outcome or cohesive capability, not by file, layer, or individual test. If the scope is too uncertain to split faithfully, make the Epic plus a short discovery task that resolves the key decisions; do not invent the remaining tasks.

## Task content

Use the format the user or destination explicitly requests. Otherwise keep a single task to:

- **Título**
- **Contexto e resultado esperado** (brief)
- **Regras / limites** (only when relevant)
- **Critérios de aceite** (usually 2–5 observable bullets)
- **Notas técnicas** (only when verified and useful; include relevant test expectations here)

For an Epic, use the same concise fields and list child tasks separately. Do not require a fixed business/technical taxonomy when it adds no clarity. If a user explicitly supplies a template, preserve it while keeping each section concise.

Acceptance criteria describe observable behavior and cover the main success flow plus only relevant failure, authorization, data, or regression cases. Keep test execution instructions separate from business acceptance unless the user asks for a combined checklist.

## Boundaries

- Never invent endpoints, fields, roles, calculations, paths, or current behavior.
- Do not add every conceivable edge case. Include an edge case only when the request, domain rule, or inspected code makes it material.
- Do not make medical assumptions about a person's abilities. Use accessibility-oriented task formatting only when requested or when the destination/team convention calls for it.
- Do not create, edit, or send work items in an external tracker unless explicitly asked and the necessary tool is available. Drafting a task is not permission to publish it.
- Do not modify the codebase or implement the described work.

## Final review

Before responding, verify that the title names the outcome; requirements trace to the request or evidence; assumptions are visible; every criterion can be checked; and the output is short enough to scan. If broad, confirm the Epic children are distinct, properly sized, and not a file-by-file checklist.
