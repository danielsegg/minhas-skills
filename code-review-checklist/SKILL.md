---
name: code-review-checklist
description: Revisar diffs, commits, branches, Pull Requests e alterações locais com foco em defeitos, regressões, segurança, desempenho, testes e manutenibilidade. Usar quando Codex precisar realizar revisão ou auditoria de código, avaliar a qualidade de uma mudança, fornecer feedback técnico ou definir critérios de aprovação, especialmente em projetos PHP/CakePHP e aplicações web.
---

# Revisar código

Realizar revisões baseadas em evidências e no contexto do projeto. Priorizar comportamento incorreto, risco real e regressões; não gerar observações apenas para preencher um checklist.

## Respeitar o escopo e a segurança

- Tratar a revisão como somente leitura. Não editar código, trocar branch, instalar ou atualizar dependências, aplicar correções, fazer commit, publicar comentários ou executar deploy sem solicitação explícita.
- Poder executar verificações locais não mutáveis quando forem seguras e relevantes. Antes de executar código vindo de uma mudança não confiável, avaliar o comando e seus efeitos.
- Não enviar código, segredos ou metadados a serviços externos sem autorização.
- Diferenciar revisão de implementação: quando o pedido for apenas revisar, relatar os achados sem corrigi-los.
- Preservar alterações existentes do usuário e evitar comandos destrutivos.

## Construir o contexto

1. Ler integralmente `AGENTS.md` e as instruções aplicáveis ao diretório revisado.
2. Identificar o objetivo, os requisitos e o limite da mudança por meio do pedido, issue, descrição do PR, diff e histórico relevante.
3. Inspecionar todos os arquivos alterados e o código diretamente relacionado antes de concluir que algo é defeito.
4. Verificar configurações, versões, arquitetura e convenções reais do projeto. Não aplicar padrões de outra linguagem ou framework por analogia.
5. Investigar informações disponíveis localmente antes de pedir esclarecimentos. Perguntar apenas quando uma lacuna impedir uma conclusão responsável.
6. Registrar limitações da análise, como testes não executados, dependências indisponíveis ou ausência dos requisitos.

## Executar a revisão

Avaliar cada alteração nas dimensões aplicáveis:

### Funcionalidade e regressões

- Confirmar que a implementação atende ao comportamento solicitado e preserva fluxos existentes.
- Examinar condições de contorno, valores vazios ou nulos, falhas parciais, repetição da operação, concorrência e transições de estado.
- Conferir tratamento de erros, rollback, idempotência e efeitos colaterais.
- Verificar contratos públicos, compatibilidade retroativa, migrations e impacto em consumidores.

### Segurança e privacidade

- Verificar validação de entrada e codificação de saída nos limites corretos.
- Procurar falhas de autorização, autenticação, CSRF, XSS, injeção, path traversal, SSRF, upload inseguro e mass assignment.
- Conferir exposição de segredos ou dados pessoais em código, respostas, URLs e logs.
- Avaliar permissões pelo caso de uso e pelo recurso; autenticação isolada não substitui autorização.
- Tratar alertas de dependências como evidência a confirmar, considerando explorabilidade e compatibilidade da correção.

### Dados, desempenho e confiabilidade

- Identificar N+1, consultas repetidas, carregamento excessivo, loops custosos e algoritmos inadequados ao volume esperado.
- Conferir transações, locks, restrições, índices e integridade referencial quando a mudança tocar persistência.
- Verificar paginação, limites, timeouts, retries e consumo de memória em caminhos relevantes.
- Recomendar otimização ou cache somente quando houver um gargalo plausível; considerar invalidação e consistência.
- Em cálculos monetários, verificar precisão decimal, arredondamento e atomicidade.

### Design e manutenção

- Avaliar coesão, separação de responsabilidades, direção das dependências e linguagem do domínio.
- Preferir código simples e explícito. Não exigir funções arbitrariamente pequenas nem abstrações prematuras para eliminar toda repetição.
- Verificar nomes, contratos, tratamento de tipos e comentários que expliquem decisões não óbvias.
- Deixar estilo mecânico para os formatadores e linters do projeto, salvo quando afetar correção ou compreensão.

### Testes e documentação

- Verificar testes proporcionais ao risco, cobrindo comportamento principal, limites, erros e regressões relevantes.
- Avaliar a qualidade das asserções, o isolamento e a capacidade do teste de falhar diante do defeito pretendido; não usar apenas percentual de cobertura.
- Conferir documentação, contratos de API e instruções operacionais quando o comportamento público mudar.
- Antes de executar verificações, seguir os comandos e o ambiente prescritos pelo repositório. Relatar com precisão o que foi ou não executado.

## Aplicar verificações para PHP e CakePHP

Quando a stack envolver PHP ou CakePHP, também:

- Conferir compatibilidade com a versão de PHP e usar as ferramentas configuradas pelo projeto, como PHPUnit, PHPCS, PHPStan, Psalm e `composer audit`.
- Manter Controllers focados na entrada HTTP e na orquestração; localizar regras de negócio na camada adotada pelo projeto, normalmente Services.
- Examinar Policies e checagens de autorização para cada ação e recurso.
- Avaliar Validators, RulesChecker, `patchEntity()`, campos acessíveis da Entity e proteção contra mass assignment.
- Preferir ORM, query builder e parâmetros vinculados a SQL concatenado; conferir associations, `contain()`, seleção de campos e risco de N+1.
- Verificar transações em operações com múltiplas gravações e distinguir falha de persistência de exceção inesperada.
- Conferir escaping nas templates, campos explicitamente não escapados, CSRF e respostas que possam vazar detalhes internos.
- Revisar migrations quanto a reversibilidade prática, valores existentes, locks, índices, foreign keys e compatibilidade com o banco suportado.
- Respeitar `composer.lock`; não recomendar atualização indiscriminada de dependências.

Para CakePHP no Núcleo Lavanderias, respeitar especialmente o fluxo Controller → Service → Table/Entity, o contrato de `AbstractService::run()`, as Policies por perfil, MariaDB, PHPUnit e a execução de comandos dentro dos containers, sempre sem `-u root` nos testes.

## Aplicar verificações ao frontend web

- Conferir escaping contextual, manipulação segura do DOM, CSRF, validação no servidor e exposição de dados.
- Verificar estados de carregamento, erro, requisições concorrentes, cancelamento e respostas fora de ordem.
- Respeitar a stack e as convenções locais; em código novo do Núcleo Lavanderias, preferir StimulusJS e Axios e limitar jQuery à manutenção do legado.
- Avaliar acessibilidade e comportamento responsivo quando a alteração afetar a interface.

## Validar os achados

Antes de relatar um problema:

1. Confirmar que ele foi introduzido ou afetado pelo escopo revisado.
2. Localizar a menor região de código que demonstra o problema.
3. Descrever um cenário concreto em que o comportamento ocorre.
4. Verificar se outra camada, configuração ou teste já mitiga o risco.
5. Ajustar a severidade pela probabilidade e pelo impacto, não pelo tipo abstrato da regra.

Ausência no diff não comprova ausência no sistema. Se o repositório estiver disponível, inspecionar as camadas relacionadas antes de relatar falta de autorização, validação, transação ou proteção equivalente. Se houver apenas um fragmento isolado, apresentar o ponto como risco condicional ou dúvida, não como defeito confirmado.

Não apresentar hipótese remota como defeito confirmado. Quando uma incerteza relevante permanecer, identificá-la explicitamente e dizer qual evidência resolveria a dúvida.

## Produzir o relatório

Apresentar primeiro os achados, em ordem de severidade:

- **Bloqueante:** corrupção de dados, vulnerabilidade explorável, indisponibilidade grave ou comportamento central inviável.
- **Alta:** defeito provável com impacto relevante ou regressão importante.
- **Média:** problema real com impacto limitado, dependente de condição específica.
- **Baixa:** melhoria concreta e localizada, sem risco significativo imediato.

Para cada achado, incluir:

```markdown
### [Severidade] Título objetivo

- Local: `caminho/arquivo.ext:linha`
- Evidência: comportamento demonstrável no código.
- Impacto: cenário afetado e consequência.
- Recomendação: direção de correção proporcional, sem reescrever toda a solução.
```

Usar sempre a linha local do arquivo, nunca a posição global do trecho em um arquivo de diff. Se o diff não fornecer numeração de hunk suficiente, citar o símbolo ou método e informar que a linha não está disponível. Manter o intervalo curto e diretamente relacionado ao achado. Agrupar ocorrências equivalentes em vez de repetir comentários.

Depois dos achados, registrar brevemente:

- dúvidas ou premissas que influenciam a conclusão;
- verificações executadas e seus resultados;
- riscos residuais ou lacunas de teste.

Se não houver achados, declarar isso claramente e ainda informar limitações e verificações realizadas. Não substituir achados por um resumo genérico da mudança, salvo se o usuário também solicitar o resumo.
