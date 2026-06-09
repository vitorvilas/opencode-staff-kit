# OpenCode Staff Kit

Um kit prático de agentes para OpenCode, com roteamento por risco, modelos por função e fluxo enxuto para projetos web/full-stack.

> Este projeto não é uma configuração oficial do OpenCode, não é benchmark definitivo de modelos e não é uma pipeline universal. É um ponto de partida para organizar uma “staff” de agentes com papéis claros.

## Para quem é este kit

Este kit foi pensado para pessoas que usam OpenCode no terminal e querem evitar três problemas comuns:

1. mandar toda tarefa para um planner e gastar contexto sem necessidade;
2. criar muitos agentes especializados e chamar todos em cadeia;
3. usar modelos fortes em tarefas operacionais ou modelos fracos em tarefas críticas.

O foco inicial é desenvolvimento web/full-stack com Node.js, TypeScript, Next.js, Supabase ou stacks similares. Ainda assim, a estrutura é genérica e pode ser adaptada para outros projetos.

## Premissas

- OpenCode rodando em Linux.
- Uso em terminal/TUI.
- Projeto testado em ambiente Node/TypeScript.
- `pnpm` como package manager padrão nos exemplos.
- Agentes configurados em `.opencode/agents/`.
- Modelos definidos no provider `opencode-go` como sugestão inicial.
- Os modelos disponíveis podem mudar conforme provider, conta, região e versão do OpenCode.

No Windows, a recomendação prática é usar WSL/Linux para projetos Node grandes, principalmente por performance de filesystem, watchers, permissões e compatibilidade de ferramentas.

## Conceito principal

Este kit usa agentes por **fase de desenvolvimento**, não apenas por camada técnica.

Em vez de chamar um agente para frontend, outro para backend, outro para hook e outro para componente em toda tarefa, a staff trabalha assim:

```txt
planner           -> classifica risco e define rota mínima
build             -> executor principal full-stack
especialistas     -> entram apenas por risco ou necessidade real
codex             -> auditor externo fora do OpenCode
```

Ter muitos agentes disponíveis não significa chamar todos. Agente parado não consome. O consumo vem dos agentes invocados, porque cada um pode ler contexto, arquivos, diff, logs e gerar relatório.

## Staff recomendada

### Núcleo

| Agente | Modelo sugerido | Papel |
|---|---|---|
| `planner` | `opencode-go/kimi-k2.6` | Triagem, classificação de risco e rota mínima. Não implementa. |
| `build` | `opencode-go/qwen3.7-plus` | Executor principal full-stack. Implementa mudanças claras. |
| `test-runner` | `opencode-go/deepseek-v4-flash` | Roda validações, lê logs e reporta falhas. |
| `reviewer` | `opencode-go/qwen3.7-plus` | Revisão técnica de tarefa média/crítica. |
| `documentador` | `opencode-go/minimax-m2.5` | Handoff, changelog e pendências. |
| `deploy` | `opencode-go/deepseek-v4-flash` | Fechamento, checklist e commit quando autorizado. |

### Especialistas sob demanda

| Agente | Modelo sugerido | Quando usar |
|---|---|---|
| `tech-lead` | `opencode-go/deepseek-v4-pro` | Arquitetura, performance, decisões difíceis. |
| `db-migration` | `opencode-go/deepseek-v4-pro` | Banco, schema, migrations, RLS, policies. |
| `security-auditor` | `opencode-go/deepseek-v4-pro` | Auth, tenant, permissões, dados sensíveis, exposição. |
| `types-generator` | `opencode-go/mimo-v2.5` | Geração/ajuste operacional de tipos. |
| `types-sync` | `opencode-go/qwen3.7-plus` | Sincronizar schema, actions, API, UI e tipos. |
| `test-builder` | `opencode-go/qwen3.7-plus` | Criar testes para regra crítica ou regressão. |
| `qa-tester` | `opencode-go/qwen3.7-plus` | Validar fluxo funcional, UX, formulário e regressão. |
| `bug-fixer` | `opencode-go/qwen3.7-plus` | Investigar e corrigir bug com causa provável. |
| `refatorador` | `opencode-go/qwen3.7-plus` | Refatoração solicitada e controlada. |
| `prompt-specialist` | `opencode-go/qwen3.7-plus` | Prompts, agentes, IA, instruções e guardrails. |
| `research-context` | `opencode-go/qwen3.7-plus` | Pesquisa read-only em documentação atual, via Context7/MCP quando disponível. |

## Estratégia de modelos

A tabela acima é uma sugestão inicial. A escolha ideal depende do seu projeto, orçamento, provider e disponibilidade dos modelos.

Use a lógica abaixo para adaptar:

```txt
modelo de orquestração longa      -> planner
modelo bom de coding diário       -> build, reviewer, QA, tests inteligentes
modelo forte de raciocínio crítico -> banco, segurança, arquitetura
modelo barato operacional         -> test-runner, deploy, logs
modelo leve textual               -> documentação/changelog
modelo de auditoria externo       -> Codex CLI ou ferramenta equivalente
```

No OpenCode Go, os limites são definidos por valor de uso. Modelos mais baratos tendem a render mais chamadas; modelos caros devem ser reservados para tarefas onde erro custa caro.

## Roteamento por risco

| Situação | Agente inicial recomendado |
|---|---|
| Mudança pequena e clara | `build` |
| Prompt já traz arquivos, mudanças e critérios | `build` |
| Demanda aberta, incerta ou com múltiplas rotas | `planner` |
| Feature média full-stack | `planner` ou `build`, conforme clareza |
| Banco, schema, migrations, RLS | `db-migration` ou `planner` se a abordagem estiver incerta |
| Auth, permissões, tenant, dados sensíveis | `security-auditor` após implementação ou antes, se for desenho de segurança |
| Bug sem causa clara | `bug-fixer` |
| Validação, typecheck, build, testes, logs | `test-runner` |
| Documentação, changelog, handoff | `documentador` |
| Commit/fechamento | `deploy` |
| Auditoria final independente | Codex CLI fora do OpenCode |

## Regra mais importante

```txt
Não mande tudo automaticamente para o planner.
```

Use `planner` quando houver incerteza. Use `build` quando a tarefa já estiver clara.

### Exemplo de tarefa clara

```bash
opencode --agent build
```

Prompt:

```txt
Corrija o rótulo do botão X no arquivo Y. Não altere comportamento. Rode pnpm typecheck ao final.
```

### Exemplo de tarefa incerta

```bash
opencode --agent planner
```

Prompt:

```txt
Analise a melhor forma de implementar o fluxo X. Classifique risco, indique arquivos prováveis e defina a rota mínima de agentes. Não edite arquivos.
```

## Instalação

1. Copie a pasta `.opencode/agents/` para a raiz do seu projeto.
2. Copie `AGENTS.example.md` para `AGENTS.md` e adapte ao seu projeto.
3. Ajuste package manager, comandos, stack, regras de segurança e idioma.
4. Confirme os modelos disponíveis:

```bash
opencode models opencode-go
```

5. Ajuste os campos `model:` nos agentes, se necessário.
6. Inicie o OpenCode com o agente adequado:

```bash
opencode --agent build
```

ou:

```bash
opencode --agent planner
```

## Estrutura do repositório

```txt
.opencode/agents/      agentes prontos para copiar
AGENTS.example.md      regra raiz genérica para seu projeto
README.md              guia principal
docs/                  guias de uso e adaptação
examples/              prompts de exemplo
scripts/               scripts opcionais e seguros
```

## YAML/frontmatter dos agentes

Cada agente do OpenCode neste kit é um arquivo Markdown com um bloco YAML no topo. Esse bloco não é comentário: ele controla como o OpenCode carrega o agente, qual modelo usa, quais permissões ele tem e quantas iterações pode executar.

Exemplo simplificado:

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

Instruções do agente aqui.
```

Use o YAML apenas para configuração curta e objetiva. Coloque instruções longas, regras de comportamento e exemplos no corpo Markdown abaixo do frontmatter.

Campos usados neste kit:

- `description`: descrição curta do papel do agente.
- `mode`: modo de disponibilidade do agente. Este kit usa `all` por compatibilidade operacional.
- `model`: modelo usado pelo agente. Sobrescreve o modelo global do OpenCode.
- `steps`: limite de iterações do agente. Não mede qualidade; controla até onde ele pode avançar.
- `permission`: permissões de ferramentas, como edição de arquivos e terminal.

Cuidados importantes:

- YAML depende de indentação correta.
- Strings com caracteres especiais devem ser colocadas entre aspas quando necessário.
- Não coloque prompts longos dentro do YAML.
- Não coloque tokens, credenciais, segredos ou dados privados no frontmatter.
- `steps` baixo pode fazer o agente parar antes de implementar.
- `steps` alto pode aumentar consumo e permitir loops mais longos.
- Se `edit: deny`, o agente não deve alterar arquivos.
- Se `bash: deny`, o agente não deve rodar comandos.
- Permissões não substituem revisão humana nem as regras do `AGENTS.md`.

Leia o guia completo em [`docs/agent-yaml-frontmatter.md`](docs/agent-yaml-frontmatter.md).

## Fluxos sugeridos

### Tarefa pequena

```txt
build
-> test-runner se houve código
-> deploy se houver autorização para commit
-> Codex opcional no diff
```

### Tarefa média

```txt
planner
-> build
-> types-sync se necessário
-> test-runner
-> reviewer
-> documentador se houver mudança funcional
-> deploy
-> Codex auditor externo
```

### Tarefa crítica

```txt
planner
-> tech-lead se houver decisão arquitetural
-> db-migration se houver banco/RLS/schema
-> types-generator/types-sync se houver impacto de tipos
-> build
-> test-builder se houver regra crítica
-> test-runner
-> reviewer
-> security-auditor
-> qa-tester
-> documentador
-> deploy
-> Codex auditor externo obrigatório
```

## Idioma

Os agentes deste kit respondem em português do Brasil por padrão.

Exceções aceitáveis:

- nomes de arquivos;
- nomes de comandos;
- nomes de funções, variáveis, branches e pacotes;
- logs, erros e stack traces originais.

## Segurança operacional

A regra raiz `AGENTS.example.md` inclui travas para ações de risco:

- não instalar dependências sem aprovação;
- não alterar `.env`;
- não rodar migrations sem confirmação;
- não fazer `git push`, `pull`, `merge` ou `checkout` sem confirmação;
- não apagar ou mover arquivos/pastas sem confirmação;
- não reverter mudanças locais do usuário;
- não fazer refatoração ampla quando a tarefa pede mudança cirúrgica.

Por padrão, a palavra de confirmação usada nos exemplos é:

```txt
APLIQUE
```

Você pode trocar por outra palavra no seu projeto.

## Uso com OpenCode Go

Este kit usa `opencode-go/*` nos exemplos de modelos, mas você pode usar qualquer provider compatível com OpenCode.

Recomendações:

- confira os modelos disponíveis com `opencode models`;
- não assuma que os IDs deste repositório existem na sua conta;
- respeite a documentação e as regras do provider escolhido.

## Codex como auditor externo

A staff do OpenCode constrói e valida. O Codex, quando usado, entra como auditor externo final.

Exemplo:

```bash
codex exec "Audite o diff atual sem alterar arquivos. Foque em bugs, segurança, tipos, build, testes e riscos de deploy."
```

Isso reduz o risco de viés de confirmação do mesmo agente que implementou a mudança.

## O que este projeto não é

Este projeto não é:

- configuração oficial do OpenCode;
- garantia de menor custo;
- benchmark definitivo de modelos;
- substituto de revisão humana;
- pipeline obrigatória universal;
- documentação de gestão de credenciais ou provedores.

## Licença

MIT. Ajuste conforme sua preferência antes de publicar.
