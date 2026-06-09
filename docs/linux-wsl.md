# Linux, WSL e ambiente de execução

Este kit assume OpenCode rodando em Linux.

## Por que Linux

Projetos Node/TypeScript grandes costumam ter melhor experiência em Linux por causa de:

- performance de filesystem;
- watchers de arquivos;
- permissões;
- symlinks;
- compatibilidade de CLIs;
- comportamento mais previsível de terminal.

## Windows

No Windows, prefira WSL para rodar OpenCode e o projeto.

Evite rodar agentes em uma pasta montada de forma lenta ou compartilhada entre dois ambientes ao mesmo tempo.

## Dois ambientes

Se usar Linux e Windows/WSL, prefira clones separados sincronizados por Git.

Evite dois OpenCode editando a mesma pasta simultaneamente.

Fluxo recomendado:

```txt
Ambiente A implementa
-> commit ou patch WIP
-> Ambiente B faz pull/aplica patch
-> continua em nova sessão
```
