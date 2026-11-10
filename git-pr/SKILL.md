---
name: git-pr
description: Analyze a Git branch and produce or create a complete Pull Request title and Markdown description in Brazilian Portuguese. Use when the user asks Codex to generate a PR, write a Pull Request description, create a PR template, summarize branch changes for review, open a pull request with gh/glab, or similar PR documentation workflows.
---

# Git PR

Use this skill to inspect the current branch and generate a professional Pull Request title and description ready for GitHub, GitLab, or Bitbucket.

## Workflow

1. Determine the user's intent:
   - If the user asks only for PR text, return the title and Markdown description; do not open a PR.
   - If the user explicitly asks to create/open the PR, generate the title/body, then use the repository's available CLI (`gh`, `glab`, or equivalent) when configured.
   - If intent is ambiguous, provide the PR text only.
2. Inspect repository state:
   - Run `git status --short --branch`.
   - Identify the current branch with `git branch --show-current`.
   - Identify the likely base branch from upstream metadata, `origin/main`, `origin/master`, `main`, or `master`.
3. Analyze branch-only changes:
   - Use `git log --oneline <base>..HEAD` for commits unique to the branch.
   - Use `git diff --stat <base>...HEAD` for the file summary.
   - Use `git diff <base>...HEAD` when more detail is needed.
   - If no base is available, fall back to staged and unstaged diffs and clearly note the assumption.
4. Read relevant files when the diff alone does not explain intent, tests, or user-facing behavior.
5. Respect local work:
   - Do not revert, delete, normalize, stage, or commit unrelated user changes.
   - Do not include uncommitted changes in the PR summary unless the user explicitly asks or they are clearly part of the requested PR.
6. If the branch contains unrelated changes, suggest splitting the PR or clearly separate the topics.

## Output Format

Return the PR title first, then the PR body in Markdown.

Use a concise title in Brazilian Portuguese:

```text
<tipo>: <resumo curto do PR>
```

Prefer Conventional Commit-style types when they fit: `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `chore`, or `perf`.

Use this exact body template:

```markdown
### Descrição
[Explique de forma concisa e clara o que foi feito e o porquê, em um parágrafo de 2 a 4 frases]

### Tipo de mudança
- [ ] Bug fix (correção de um erro)
- [ ] Nova feature (funcionalidade inédita)
- [ ] Breaking change (mudança que quebra compatibilidade)
- [ ] Refactor (melhoria ou reestruturação de código)
- [ ] Documentação (alterações em documentação ou README)
- [ ] Outros

### O que foi alterado
- [Lista dos principais arquivos/módulos alterados e o motivo de cada alteração]

### Como testar
1. [Passo a passo sequencial para o revisor validar as mudanças localmente ou em ambiente de teste]

### Checklist
- [ ] Código revisado pelo autor
- [ ] Testes adicionados/atualizados
- [ ] Sem console.log ou código temporário de debug
- [ ] Documentação atualizada (se aplicável)
```

## Language And Style

- Write the title, description, changed-items list, and test steps in Brazilian Portuguese.
- Provide copy-ready Markdown with no greeting or extra model commentary.
- Mark only the PR type checkbox(es) that are clearly supported by the diff.
- Keep the description factual; do not claim tests were run unless evidence exists or the user says so.
- In `Como testar`, prefer commands from repository documentation, package scripts, Makefiles, CI config, or prior user instructions.
- If testing cannot be inferred, include a practical manual review step and mention that automated tests were not identified.

## Creating The PR

Only create/open the PR when the user explicitly asks for it.

Before running a PR creation command:

1. Show or determine the base branch and target remote.
2. Ensure the branch is pushed or ask/push only when the user requested that workflow.
3. Use non-interactive commands when possible, for example:

```bash
gh pr create --base main --head current-branch --title "feat: resumo" --body-file /tmp/pr-body.md
```

After creating the PR, report the PR URL returned by the CLI.
