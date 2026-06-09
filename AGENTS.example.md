# Regras raiz de agentes

Este arquivo define instruções compartilhadas para OpenCode, Codex CLI e outras ferramentas que leem `AGENTS.md` neste checkout.

Adapte este arquivo ao seu produto antes de usar em produção.

## Contexto do projeto

Substitua esta seção pelo contexto real do seu projeto:

- Produto: `NOME_DO_PRODUTO`.
- Stack: `Next.js`, `TypeScript`, `Supabase`, `PostgreSQL`, `Tailwind`, ou a stack real.
- Público-alvo: descreva em uma frase.
- Princípio de produto: descreva o que deve guiar decisões de UX/engenharia.
- Idioma de UI: português do Brasil, se aplicável.

## Ferramentas e agentes

- OpenCode usa agentes executáveis em `.opencode/agents/`.
- Agentes do OpenCode não devem ser assumidos por outras ferramentas, salvo pedido explícito.
- Codex CLI deve atuar como auditor externo quando usado, sem depender de agentes do OpenCode.
- Quando houver documentação atualizada de biblioteca/framework/SDK/API, use Context7 MCP se estiver configurado.

## Stack e comandos

A fonte de verdade de versões deve ser o arquivo do projeto, como `package.json`, `pyproject.toml`, `go.mod`, etc.

Exemplo para Node/TypeScript:

- Package manager: `pnpm`.
- Não usar `npm` ou `yarn` se o projeto padroniza `pnpm`.
- TypeScript strict, quando aplicável.
- Testes conforme stack do projeto.

Comandos úteis, ajuste conforme seu projeto:

```bash
pnpm dev
pnpm build
pnpm typecheck
pnpm lint
pnpm test
```

Não instalar novas dependências, runners ou ferramentas sem aprovação explícita.

## Arquitetura

Defina aqui os padrões do seu projeto.

Exemplo para Next.js App Router:

```text
src/app/(app)/[modulo]/
  page.tsx     # Server Component: auth, fetch, redirects e gates de acesso
  tela.tsx     # Client Component: estado local e UI
src/app/actions/[modulo].ts
src/components/[modulo]/
src/hooks/
```

Regras recomendadas:

- Server Components fazem auth, fetch inicial, redirects e gates de acesso.
- Client Components concentram estado local, interação, modais e composição visual.
- Mutações internas usam Server Actions, quando esse for o padrão do projeto.
- Route Handlers ficam para webhooks, integrações externas e endpoints públicos.
- Hooks são para estado/data fetching client-side, não para autorização server-side.

## Segurança

Adapte ao seu stack. Regras genéricas:

- Nunca confiar em `user_id`, role, email, permissão ou flag de admin vindos do cliente.
- Mutação crítica deve validar usuário autenticado e ownership no servidor.
- Nunca retornar erro bruto de banco ao cliente.
- Segredos, tokens e credenciais nunca devem ser gravados em docs, logs, arquivos versionados ou respostas de auditoria.
- Service-role/admin keys devem ficar apenas no servidor e com escopo mínimo.
- UI disabled não substitui autorização server-side.

## Banco de dados

- Não rodar migrations, `db push`, alterações de schema ou scripts destrutivos sem pedido explícito e confirmação aplicável.
- Ler o código e o histórico antes de alterar migrations.
- Preferir validação pequena antes de bulk operations.
- Não criar tabelas, índices ou policies sem necessidade explícita.

## Frontend e UX

- Mobile-first.
- Considere largura mínima de 375px, quando aplicável.
- Preserve acessibilidade, foco, teclado móvel, dark mode e responsividade quando tocar UI.
- Não criar redesign ou refatoração visual ampla quando o pedido é cirúrgico.
- Mensagens para usuário devem estar no idioma definido pelo projeto.

## Validação

Escolha a menor validação suficiente para o risco da mudança.

Exemplos:

- Docs apenas: `git diff --check`.
- TypeScript: `pnpm typecheck`.
- UI/componentes: teste componente quando houver e verificação responsiva.
- API/auth/banco: typecheck, testes relevantes, build e revisão de ownership/permissões/erros amigáveis.
- Deploy/PR: teste de CI ou equivalente antes de liberar.

Se não puder validar, informe claramente o que não foi validado e por quê.

## Confirmação obrigatória

Antes de qualquer ação abaixo, pedir confirmação usando exatamente:

`APLIQUE`

Ações que exigem essa confirmação:

- remover ou mover arquivos/pastas;
- reestruturar diretórios;
- alterar configuração de build, TypeScript, bundler ou CSS global;
- rodar migrations ou schema changes;
- `git push`, `git pull`, `git merge`, `git checkout`;
- instalar bibliotecas;
- qualquer script que modifique `.env`;
- qualquer ação destrutiva ou irreversível.

## Comandos proibidos

- `rm -rf /`
- `rm -rf *`
- `rm -rf .`
- `git reset --hard`
- `git push --force`
- comandos que apaguem dados ou migrations
- scripts que modifiquem `.env` automaticamente

## Git

- Preservar alterações locais do usuário.
- Nunca reverter mudanças não feitas por você sem pedido explícito.
- Não fazer commit se o usuário disser para não commitar.
- Não fazer push sem aprovação explícita.

## Documentação incremental

Para trabalhos longos, multi-etapa ou com handoff, registre uma nota em arquivo definido pelo projeto, por exemplo:

```txt
docs/agent-sessions.md
```

Tarefas curtas, perguntas pontuais, comandos simples e inspeções read-only estreitas podem omitir esse registro para reduzir ruído.

## Idioma obrigatório dos agentes

Todos os agentes, resumos, handoffs, relatórios, listas de tarefas, diagnósticos, mensagens finais e observações devem ser escritos em português do Brasil, salvo se o projeto definir outro idioma.

Exceções permitidas:

- nomes técnicos reais de comandos, arquivos, funções, variáveis, branches, pacotes e erros;
- saída bruta de terminal, logs, stack traces e mensagens originais de ferramentas;
- termos técnicos que não devem ser traduzidos por clareza, como `typecheck`, `build`, `lint`, `server action`, `route handler`, `diff`, `commit`, `push`.

Não responder em inglês por padrão.
Não usar títulos como `Work Completed`, `Remaining Tasks`, `Recommendations` ou `Maximum Steps Reached - Task Summary`.

Usar equivalentes em pt-BR:

- Trabalho realizado
- Tarefas pendentes
- Recomendações
- Resumo ao atingir limite de passos

## Modo de execução

Estas regras orientam todos os agentes e CLIs. Elas priorizam precisão, simplicidade e diffs pequenos.

### Antes de alterar código

- Entenda o pedido antes de agir.
- Se houver ambiguidade real, pare e pergunte.
- Se houver uma solução mais simples, prefira a solução mais simples.
- Em tarefa média ou longa, apresente um plano curto antes de executar.
- Não peça confirmação para tarefas normais quando o escopo estiver claro.
- Peça confirmação apenas nos casos listados em **Confirmação obrigatória** ou quando houver risco real de perda de dados, alteração estrutural ou ação irreversível.

### Simplicidade

- Implemente apenas o que foi pedido.
- Não crie abstrações para uso único.
- Não adicione flexibilidade especulativa.
- Não instale dependências sem aprovação.
- Se a solução ficar grande demais para o problema, simplifique antes de concluir.

### Mudanças cirúrgicas

- Altere somente os arquivos necessários.
- Não refatore código adjacente sem necessidade.
- Não reorganize diretórios sem confirmação.
- Preserve o estilo existente do arquivo.
- Remova apenas imports, variáveis e funções que ficaram inutilizados por causa da sua própria alteração.
- Se encontrar problema não relacionado, mencione no relatório final; não corrija sem pedido.

### Critério de sucesso

Toda tarefa deve terminar com um critério verificável.

Exemplos:

- Validação adicionada: entradas inválidas testadas ou justificativa se teste não foi viável.
- Bug corrigido: causa provável explicada e validação executada.
- Refatoração: comportamento preservado e checagem executada.
- UI alterada: estados principais e responsividade considerados.
- API/banco/auth: ownership, erro amigável e tipos revisados.

### Limite de tentativas

- Não entre em loop infinito de correções.
- Após 2 tentativas falhas na mesma validação, pare, resuma o erro, explique a hipótese e peça orientação.
