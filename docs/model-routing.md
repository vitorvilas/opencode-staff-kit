# Roteamento de modelos

Este kit usa modelos por função. Os IDs são sugestões baseadas em uso com `opencode-go` e devem ser adaptados ao seu provider.

## Princípio

Não escolha o modelo só pelo benchmark. Escolha pelo custo do erro.

```txt
Orquestração       -> modelo bom em contexto longo e planejamento
Código diário      -> modelo bom em TypeScript/coding e diffs concisos
Risco crítico      -> modelo forte em raciocínio e auditoria
Operacional        -> modelo barato e rápido para comandos/logs
Documentação       -> modelo barato, claro e consistente em texto
Auditoria externa  -> ferramenta/modelo separado da cadeia principal
```

## Sugestão inicial

| Papel | Modelo sugerido | Motivo |
|---|---|---|
| Planner/orquestrador | `opencode-go/kimi-k2.6` | Bom para coordenação e tarefas multi-step. |
| Build/código diário | `opencode-go/qwen3.7-plus` | Equilíbrio entre qualidade de código e custo. |
| Banco/segurança/arquitetura | `opencode-go/deepseek-v4-pro` | Reservado para risco alto. |
| Validação/logs/deploy | `opencode-go/deepseek-v4-flash` | Operacional, barato e rápido. |
| Documentação/changelog | `opencode-go/minimax-m2.5` | Texto claro e custo menor. |
| Tipos leves | `opencode-go/mimo-v2.5` | Código leve/repetitivo com custo baixo, se disponível. |
| Auditoria externa | Codex CLI | Segunda opinião fora do swarm. |

## Como adaptar

Rode:

```bash
opencode models
```

ou, se usar OpenCode Go:

```bash
opencode models opencode-go
```

Depois substitua os campos `model:` nos arquivos `.opencode/agents/*.md`.

## Regras práticas

- Não use modelo caro para documentação simples.
- Não use modelo operacional/fraco para segurança, banco ou review crítico.
- Não rode vários agentes fortes em sequência quando um agente resolve.
- Se o provider não tiver cache efetivo, reduza chamadas de contexto longo.
- Se um agente só roda comandos e resume logs, ele não precisa do melhor modelo.
