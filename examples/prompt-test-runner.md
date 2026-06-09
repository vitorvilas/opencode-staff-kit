# Exemplo — validação com test-runner

Comando:

```bash
opencode --agent test-runner
```

Prompt:

```txt
Valide o diff atual sem alterar arquivos.

Execute a menor validação suficiente:
1. git diff --check
2. pnpm typecheck
3. teste focado se for óbvio qual teste cobre a alteração

Se algum comando falhar:
- resuma a falha em pt-BR;
- aponte arquivos prováveis;
- indique próximo agente recomendado.

Não corrija código.
```
