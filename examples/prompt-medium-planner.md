# Exemplo — tarefa média para planner

Comando:

```bash
opencode --agent planner
```

Prompt:

```txt
Analise esta demanda e defina a rota mínima de execução.

Contexto:
- Siga AGENTS.md.
- Não edite arquivos.
- Não rode comandos.
- Não chame agentes desnecessários.

Objetivo:
Criar um fluxo de edição de perfil com validação, mensagens amigáveis e persistência no backend.

O que preciso de você:
1. Classifique o risco.
2. Identifique arquivos prováveis.
3. Indique agentes necessários e desnecessários.
4. Defina plano curto.
5. Gere prompt para o próximo agente.

Resposta final:
- risco;
- rota mínima;
- agentes;
- prompt para execução.
```
