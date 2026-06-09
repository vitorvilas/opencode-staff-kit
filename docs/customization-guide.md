# Guia de personalização

Antes de usar este kit em um projeto real, personalize os pontos abaixo.

## 1. AGENTS.md

Copie:

```bash
cp AGENTS.example.md AGENTS.md
```

Depois ajuste:

- nome do produto;
- stack real;
- package manager;
- comandos de validação;
- padrões de arquitetura;
- regras de segurança;
- idioma;
- palavra de confirmação;
- documentação incremental.

## 2. Modelos

Verifique modelos disponíveis:

```bash
opencode models
```

Substitua `model:` nos agentes se seu provider não tiver os modelos sugeridos.

## 3. Permissões

Revise `permission:` nos agentes.

Sugestão:

- `planner`, `reviewer`, `security-auditor`, `qa-tester`, `research-context`: read-only;
- `build`, `bug-fixer`, `types-sync`, `test-builder`: podem editar;
- `test-runner`: pode rodar comandos, mas não editar;
- `deploy`: pode rodar git, mas não alterar código.

## 4. Steps

Ajuste conforme seu custo e autonomia desejada.

Sugestão inicial:

```txt
planner: 6
build: 35
test-runner: 15
reviewer: 18
security-auditor: 18
documentador: 12
deploy: 12
```

Se o agente para antes de implementar, aumente `steps`. Se ele explora demais, reduza ou melhore o prompt.

## 5. Idioma

O kit vem em pt-BR. Para outro idioma, ajuste:

- `AGENTS.md`;
- todos os blocos `Idioma obrigatório`;
- README e docs;
- respostas finais dos agentes.
