---
name: neurodivergent-task-writer
description: Turn software requests into clear, low-friction work items designed for easy scanning and follow-through by people with ADHD or other neurodivergent needs. Automatically split broad scopes into an Epic and manageable tasks.
---

# Neurodivergent Task Writer

Transform a request into software work items that are easy to understand, start, track, and finish. Write in Brazilian Portuguese unless the user asks otherwise. This is an accessibility-oriented writing style, not a diagnosis or a claim that all neurodivergent people work alike.

## Workflow

1. Identify the requested outcome, audience, constraints, and issue-tracker format. Preserve supplied wording and confirmed requirements.
2. If a repository is available, inspect only the relevant instructions, docs, code, and tests needed to ground the task. Do not infer requirements from unrelated or uncommitted changes.
3. Decide whether this is one bounded task or a broad initiative. Use one task when it has one coherent outcome. Create an Epic automatically when it includes multiple independently deliverable outcomes or coordinated capabilities.
4. Write for quick scanning and easy initiation. Make the first action concrete, define what done looks like, and keep dependencies visible.
5. Remove speculation, repeated information, optional ideas mixed into requirements, and excessive edge cases. Ask only if an unresolved decision changes the task materially; otherwise label a narrow assumption.

## Format for one task

Use the user's template when supplied. Otherwise provide:

# [Ação + resultado]

**Por que:** [uma ou duas frases sobre o problema e o resultado]

**Pronto quando:**
- [2–5 resultados observáveis, cada um verificável]

**Regras e limites:** [somente se necessários]

**Referências:** [no máximo as referências técnicas que ajudam a começar; omita se não houver]

Keep paragraphs short, use familiar words, explain uncommon acronyms once, and prefer direct verbs. Keep each bullet to one idea. Avoid large mandatory taxonomies, long background sections, and vague verbs such as “melhorar” without a concrete result.

## Format for broad scopes

Create an Epic plus **3–7** small child tasks (fewer when that fully covers the scope). Make tasks small enough to have one main outcome and a clear finish condition. Do not promise a particular duration or number of work sessions.

# Epic: [resultado amplo]

**Resultado final:** [o que estará possível quando o Epic terminar]

**Limites:** [o que pertence e o que não pertence ao Epic, se necessário]

## Tarefas

1. **[Ação concreta]** — [resultado concluído e como verificar]
2. **[Ação concreta]** — [resultado concluído e como verificar]

State dependencies with a simple “Depende de: tarefa X” only when order matters. Each child task must make sense on its own, avoid hidden prerequisite chains, and not duplicate Epic-level context. Split by user outcome or cohesive capability, never just by file, code layer, or test type. If the request is too unclear for faithful decomposition, create a discovery task to answer the specific blocking questions rather than fabricating implementation tasks.

## Task-writing rules

- Put one main action in each title and one primary outcome in each task.
- Use concrete completion checks; avoid vague “done”, “review”, or “ensure” without saying what can be observed.
- Keep acceptance checks to the few that matter for the requested outcome. Include failure, permission, data, and regression checks only when relevant.
- Separate required work from optional follow-ups. Do not silently broaden a task with unrelated maintenance or aspirational improvements.
- Include technical references and test notes only when repository evidence makes them useful. Do not turn the task into a code inventory or test plan.
- Preserve explicit issue type, terms, and templates provided by the user. Do not add estimates, priorities, assignees, or due dates unless requested.
- Do not update external trackers or modify code unless separately requested and authorized.

## Final scan

Check that a reader can answer quickly: What is the outcome? What is the first action? How will I know this task is finished? What blocks it? Remove content that does not help answer one of those questions.
