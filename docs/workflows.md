# Fluxos de trabalho

## Tarefa pequena

Use para copy, ajuste visual isolado, correção local simples ou componente sem regra de negócio.

```bash
opencode --agent build
```

Fluxo:

```txt
build
-> test-runner, se houve código
-> deploy, se houver autorização para commit
```

Não use `planner` por padrão.

## Tarefa média

Use para nova tela, formulário, integração simples, action/API, tipos ou fluxo relevante.

```bash
opencode --agent planner
```

ou direto:

```bash
opencode --agent build
```

Regra:

- se o prompt já está claro, use `build`;
- se há dúvida de abordagem, use `planner`.

Fluxo:

```txt
planner
-> build
-> types-sync, se necessário
-> test-runner
-> reviewer
-> documentador, se houver mudança funcional
-> deploy
-> Codex, se for merge/deploy relevante
```

## Tarefa crítica

Use para banco, RLS, auth, permissões, dados sensíveis, pagamentos, migrations, segurança ou regra central.

```bash
opencode --agent planner
```

Fluxo:

```txt
planner
-> tech-lead, se houver arquitetura
-> db-migration, se houver banco/schema/RLS
-> types-generator/types-sync, se houver tipos
-> build
-> test-builder, se precisar de teste novo
-> test-runner
-> reviewer
-> security-auditor
-> qa-tester
-> documentador
-> deploy
-> Codex obrigatório antes de merge/deploy
```

## Validação isolada

```bash
opencode --agent test-runner
```

Prompt exemplo:

```txt
Rode a menor validação suficiente para o diff atual. Comece por typecheck. Se falhar, resuma a causa provável e indique o próximo agente. Não altere arquivos.
```

## Auditoria externa

```bash
codex exec "Audite o diff atual sem alterar arquivos. Foque em bugs, segurança, tipos, build, testes e riscos de deploy."
```
