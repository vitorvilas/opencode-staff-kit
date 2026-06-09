# Exemplo — auditoria externa com Codex

```bash
git diff --staged | codex exec "Audite este diff staged sem alterar arquivos. Classifique achados em BLOCKER, HIGH, MEDIUM e LOW. Foque em bug, segurança, tipos, build, testes, dados e deploy. Ignore estilo cosmético."
```
