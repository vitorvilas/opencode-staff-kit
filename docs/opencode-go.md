# OpenCode Go e limites

OpenCode Go pode ser usado como provider de modelos no OpenCode.

Este kit usa IDs como `opencode-go/qwen3.7-plus` nos agentes, mas isso é apenas sugestão.

## Pontos importantes

- Confira os modelos disponíveis com `opencode models opencode-go`.
- Os modelos disponíveis podem mudar.
- Os limites de uso são definidos por valor, então modelos diferentes consomem a franquia em velocidades diferentes.
- Modelos baratos podem ser bons para tarefas operacionais.
- Modelos caros devem ser reservados para tarefas críticas.

## Uso responsável

Use o provider conforme a documentação oficial do OpenCode e do serviço de modelos escolhido. Este kit documenta apenas a organização da staff, o roteamento por risco e a seleção técnica de modelos por tipo de tarefa.

## Estratégia sugerida

```txt
modelo caro/forte       -> risco alto
modelo médio de coding  -> desenvolvimento diário
modelo barato           -> comandos, logs, deploy, documentação
```
