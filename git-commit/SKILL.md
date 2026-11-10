---
name: git-commit
description: Analyze Git changes and produce or execute clear Conventional Commit messages in Brazilian Portuguese. Use when the user asks Codex to generate a commit message, write a commit, create a commit, commit staged or unstaged changes, review the current Git diff for a commit, or similar Git commit workflow requests.
---

# Git Commit

Use this skill to inspect repository changes and create concise, accurate commit messages following Conventional Commits in Brazilian Portuguese.

## Workflow

1. Determine the user's intent:
   - If the user asks only for a message, return the commit message and do not run `git commit`.
   - If the user explicitly asks to make/create/do the commit, inspect changes, stage only appropriate files when needed, then run `git commit`.
   - If intent is ambiguous, provide the message only.
2. Inspect repository state with `git status --short`.
3. Prefer staged changes for the commit message:
   - Use `git diff --cached` when staged changes exist.
   - Use `git diff` when nothing is staged and the user asked for a message for current changes.
4. Read relevant files when the diff alone does not explain the purpose.
5. Do not revert, delete, normalize, or stage unrelated user changes.
6. If multiple unrelated changes are present, either suggest separate commits or commit only the clearly requested subset.
7. Before committing, verify the final staged diff with `git diff --cached --stat` and, when useful, `git diff --cached`.

## Message Format

Use Conventional Commits:

```text
<tipo>(<escopo>): <descrição curta>
```

Allowed types:

- `feat`: nova funcionalidade
- `fix`: correcao de bug
- `docs`: documentacao
- `style`: formatacao que nao altera comportamento
- `refactor`: alteracao de codigo sem corrigir bug nem adicionar funcionalidade
- `perf`: melhoria de desempenho
- `test`: testes novos ou corrigidos
- `build`: build, empacotamento ou dependencias externas
- `ci`: configuracao de CI/CD
- `chore`: tarefas gerais sem alteracao de codigo de produto ou testes

Use a scope when it is clear from the changed area, for example `feat(auth): ...`, `fix(api): ...`, or `docs(readme): ...`.

## Language And Style

- Write subject, body, and footer in Brazilian Portuguese.
- Keep the subject at 72 characters or fewer.
- Start the subject description with a lowercase letter after the colon.
- Do not end the subject with a period.
- Prefer an imperative, direct description of the change.
- Use ASCII unless the repository or user specifically calls for accents.
- Add a body only when the change is complex, spans multiple areas, or needs rationale.
- Add a footer with `BREAKING CHANGE:` when the diff introduces an incompatible change.

## Output

When generating only the message, return it in a fenced code block:

```text
feat(escopo): descreve a mudanca
```

When making the commit, run a non-interactive command such as:

```bash
git commit -m "tipo(escopo): descricao"
```

For messages with a body or footer, use multiple `-m` arguments.

After committing, report the commit hash and subject from `git log -1 --oneline`.
