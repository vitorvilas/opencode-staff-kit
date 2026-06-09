# Auditoria externa com Codex CLI

A staff do OpenCode implementa, valida e revisa. O Codex pode entrar como auditor externo final.

## Por que usar auditor externo

Um auditor externo reduz o viés de confirmação do mesmo agente que implementou a mudança.

## Auditoria de diff

```bash
git diff --staged | codex exec "Audite este diff staged sem alterar arquivos. Classifique achados em BLOCKER, HIGH, MEDIUM e LOW. Foque em bug, segurança, tipos, build, testes, dados e deploy. Ignore estilo cosmético."
```

## Auditoria da branch

```bash
codex exec "Audite a branch atual contra a base principal sem alterar arquivos. Foque em bugs, segurança, permissões, tipos, build, testes e riscos de deploy."
```

## Formato recomendado

```txt
# Auditoria Codex

## Veredito
APROVADO / APROVADO COM RESSALVAS / REPROVADO

## Achados
### [SEVERIDADE] Título
- Evidência:
- Impacto:
- Correção recomendada:
- Confiança:

## Conclusão
Dizer se pode commitar, mergear ou deployar.
```
