# Exemplo — tarefa pequena para build

Comando:

```bash
opencode --agent build
```

Prompt:

```txt
Corrija o texto do botão abaixo com mudança cirúrgica.

Contexto:
- Siga AGENTS.md.
- Use o padrão atual do projeto.
- Não altere comportamento.
- Não instale dependências.

Objetivo:
Alterar o rótulo do botão "Salvar dados" para "Salvar alterações".

Arquivos prováveis:
- src/components/...

Critérios de aceite:
- O novo texto aparece corretamente.
- Nenhuma lógica não relacionada foi alterada.

Validação:
- Rode pnpm typecheck se houver alteração em código TypeScript.
- Se não rodar, explique por quê.

Resposta final:
- arquivos alterados;
- resumo;
- comandos executados;
- riscos/pendências.
```
