# Como pedir prompts para OpenCode

Quando você for criar um prompt para OpenCode, primeiro escolha o agente.

## Regra fixa

```txt
Prompt claro com arquivos/mudanças/critérios -> build
Incerteza/múltiplas rotas/arquitetura        -> planner
Banco/RLS/schema/migration                   -> db-migration
Auth/permissões/dados sensíveis              -> security-auditor no fluxo
Validação/comandos/logs                      -> test-runner
Auditoria final                              -> Codex CLI
```

## Estrutura de prompt recomendada

```txt
Agente recomendado:
Comando sugerido:
Contexto obrigatório:
Objetivo:
Escopo:
Restrições:
Tarefas:
Critérios de aceite:
Validação obrigatória:
Resposta final esperada:
```

## Exemplo para build

```txt
Execute a tarefa abaixo com mudanças cirúrgicas.

Contexto:
- Siga AGENTS.md.
- Use pnpm.
- Não instale dependências.
- Não altere schema, migrations ou .env.

Objetivo:
...

Arquivos prováveis:
...

Tarefas:
...

Validação:
- pnpm typecheck
- teste focado se existir

Resposta final:
- arquivos alterados
- resumo
- comandos executados
- riscos/pendências
```

## Exemplo para planner

```txt
Analise esta demanda e defina a rota mínima de execução.

Não edite arquivos.
Não rode comandos.
Classifique o risco.
Indique agentes necessários e agentes desnecessários.
Gere um prompt para o próximo agente.
```
