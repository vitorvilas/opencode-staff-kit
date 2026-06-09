# YAML/frontmatter dos agentes

Os agentes do OpenCode são arquivos Markdown com um bloco YAML no topo. No OpenCode Staff Kit, esse bloco é usado para configurar o papel operacional de cada agente: modelo, modo, limite de passos e permissões.

Este arquivo existe porque muitos problemas de uso não vêm do prompt em si, mas de configurações pequenas no frontmatter.

## Estrutura básica

```md
---
description: Executor principal para tarefas full-stack claras.
mode: all
model: opencode-go/qwen3.7-plus
steps: 35
permission:
  edit: allow
  bash: allow
---

# Build

Instruções detalhadas do agente aqui.
```

Tudo entre o primeiro `---` e o segundo `---` é YAML. Tudo abaixo disso é o corpo Markdown do agente.

## O que deve ficar no YAML

Use o YAML para configuração curta:

- descrição do agente;
- modelo;
- modo;
- limite de passos;
- permissões.

Não use YAML para prompt longo, regras extensas, exemplos grandes ou documentação. Coloque isso no corpo Markdown.

## Campos usados neste kit

### `description`

Descrição curta do papel do agente.

```yaml
---
description: Executor principal para tarefas full-stack claras.
---
```

Mantenha a descrição objetiva. Ela ajuda a identificar o agente, mas não deve tentar substituir as instruções completas do corpo Markdown.

### `mode`

Define o modo de disponibilidade do agente.

Este kit usa:

```yaml
mode: all
```

Isso foi escolhido por compatibilidade operacional. Não significa que o agente deve fazer tudo. O papel real do agente continua definido pelo corpo Markdown e pelas permissões.

### `model`

Define o modelo usado pelo agente.

```yaml
model: opencode-go/qwen3.7-plus
```

O modelo definido no agente sobrescreve o modelo global do OpenCode para aquele agente. Os IDs dos modelos são sugestões e podem não existir no seu provider, conta ou versão do OpenCode.

Antes de usar, confira:

```bash
opencode models
```

ou, para OpenCode Go:

```bash
opencode models opencode-go
```

### `steps`

Controla o limite de iterações do agente.

```yaml
steps: 35
```

`steps` não é um indicador de qualidade. É um limite operacional.

- `steps` baixo economiza, mas pode parar antes da implementação.
- `steps` alto dá autonomia, mas pode consumir mais e permitir exploração longa demais.

Sugestão inicial deste kit:

| Agente | Faixa sugerida |
|---|---:|
| `planner` | 4-6 |
| `build` | 30-40 |
| `test-runner` | 10-15 |
| `reviewer` | 12-18 |
| `security-auditor` | 12-18 |
| `documentador` | 8-12 |
| `deploy` | 8-12 |

Ajuste conforme o tamanho do seu projeto e o comportamento real do agente.

### `permission`

Controla ferramentas que o agente pode usar.

Exemplo de agente executor:

```yaml
permission:
  edit: allow
  bash: allow
```

Exemplo de agente read-only:

```yaml
permission:
  edit: deny
  bash: deny
```

Use permissões para reforçar papéis:

- `planner`: normalmente `edit: deny` e `bash: deny`.
- `build`: normalmente pode editar e rodar comandos.
- `test-runner`: pode rodar comandos, mas não precisa editar.
- `reviewer`, `security-auditor` e `qa-tester`: preferencialmente read-only.
- `deploy`: pode ter permissões controladas, mas deve exigir autorização para ações de Git relevantes.

Permissões não substituem revisão humana nem regras do `AGENTS.md`.

## Cuidados com YAML

YAML é sensível a indentação e tipos.

Evite:

```yaml
permission:
 edit: allow
  bash: allow
```

Prefira:

```yaml
permission:
  edit: allow
  bash: allow
```

Se uma string tiver caracteres especiais, use aspas:

```yaml
description: "Agente de QA: valida fluxo, UX e regressão."
```

Não coloque comentários com segredos, tokens, nomes internos sensíveis ou dados privados no frontmatter.

## Sintomas comuns e causas prováveis

### O agente parou antes de editar

Possíveis causas:

- `steps` baixo demais;
- tarefa exigiu muita exploração antes da implementação;
- prompt pediu inspeção ampla demais;
- agente escolhido não era executor.

Correções possíveis:

- aumentar `steps` do agente executor;
- enviar tarefa clara diretamente para `build`;
- usar `planner` apenas quando houver incerteza real;
- dividir descoberta e implementação em duas execuções.

### O agente explorou demais e não implementou

Possíveis causas:

- prompt amplo demais;
- `steps` consumidos com leitura e grep;
- agente tentando confirmar padrões demais antes de agir.

Correções possíveis:

- informar arquivos exatos;
- limitar escopo no prompt;
- pedir para não refazer exploração ampla;
- reduzir `steps` em agentes de planejamento;
- reforçar no corpo do agente que, quando a tarefa vier clara, ele deve executar.

### O agente respondeu em inglês

Inclua regra explícita no `AGENTS.md` e no corpo dos agentes:

```md
Responda sempre em português do Brasil. Preserve em inglês apenas nomes técnicos, comandos, arquivos, funções, pacotes, logs e saídas brutas de ferramentas.
```

### O agente editou quando deveria apenas revisar

Verifique:

```yaml
permission:
  edit: deny
```

E reforce no corpo Markdown:

```md
Este agente é read-only. Não altere arquivos.
```

### O agente rodou comandos quando não deveria

Verifique:

```yaml
permission:
  bash: deny
```

E reforce no corpo Markdown:

```md
Não rode comandos. Apenas analise os arquivos e reporte achados.
```

## Relação com `AGENTS.md`

O frontmatter configura o agente. O `AGENTS.md` define regras globais do projeto.

Use o `AGENTS.md` para:

- stack do projeto;
- comandos permitidos;
- ações que exigem confirmação;
- idioma;
- segurança;
- regras de Git;
- padrões de arquitetura;
- validação.

Use o frontmatter do agente para:

- modelo;
- permissões;
- limite de passos;
- modo.

## Regra prática

Mantenha o YAML pequeno e previsível.

Se a instrução parece uma regra de comportamento, coloque no corpo Markdown. Se parece uma configuração do OpenCode, coloque no YAML.
