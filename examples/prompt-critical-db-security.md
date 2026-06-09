# Exemplo — tarefa crítica com banco e segurança

Comando inicial:

```bash
opencode --agent planner
```

Prompt:

```txt
Analise esta mudança crítica antes de qualquer implementação.

Contexto:
- Siga AGENTS.md.
- Não edite arquivos nesta etapa.
- Não rode migration nem schema change.
- A mudança envolve banco, permissões e dados sensíveis.

Objetivo:
Adicionar controle de acesso por organização em um recurso já existente.

O que preciso:
1. Classificar risco.
2. Separar o que é banco/RLS, o que é API/action e o que é UI.
3. Indicar se db-migration e security-auditor devem entrar.
4. Gerar plano mínimo e prompts para os próximos agentes.

Resposta final:
- risco;
- agentes recomendados em ordem;
- riscos de segurança;
- prompt para db-migration ou build.
```
