# SGA / Protocolo ATLAS — instruções para agentes

Este repositório contém um protótipo **Unreal Engine 5.7**, orientado a
Blueprints. O projeto fica em `ProtocoloATLAS/ProtocoloATLAS.uproject`.

Antes de alterar qualquer coisa, leia `docs/PARALLEL_WORKFLOW.md`.

## Regras obrigatórias

- Nunca trabalhe diretamente em `main`. Use um `git worktree` e uma branch
  própria (`codex/<tarefa>` ou `claude/<tarefa>`).
- Não edite a mesma `.uasset`, `.umap`, `.ubulk` ou `.uexp` que outro agente.
  Esses arquivos são binários e não têm merge textual confiável.
- Antes de tocar em ativos Unreal, reserve o menor diretório/arquivo possível:
  `scripts/asset-claim claim <agente> <caminho>...`.
- Consulte reservas com `scripts/asset-claim list` e libere-as ao terminar com
  `scripts/asset-claim release <agente>`.
- Não altere nem versione `Saved/`, `Intermediate/`, `DerivedDataCache/`,
  `Binaries/` ou arquivos locais do editor.
- Não mova nem renomeie ativos no filesystem. Faça isso no Unreal Editor para
  preservar referências e corrigir redirectors.
- Mantenha commits pequenos e com uma única responsabilidade. Não misture
  documentação/configuração com mudanças binárias de gameplay.
- Não faça push, merge, rebase destrutivo, publicação ou instalação de
  dependências sem autorização explícita.

## Contexto técnico rápido

- Mapa e GameMode padrão: First Person.
- Gameplay central: `BP_FirstPersonCharacter`.
- Sistemas identificados no histórico: personagem/animações, interação com o
  módulo ATLAS, escudo, disparo e recarga.
- UI: `ProtocoloATLAS/Content/Widget/`.
- Ações Enhanced Input: `ProtocoloATLAS/Content/Input/`.
- O repositório ainda contém artefatos gerados antigos já rastreados pelo Git;
  não os inclua em commits novos.

## Entrega de cada agente

Informe branch, commits, arquivos alterados, testes executados, reservas ainda
ativas e riscos conhecidos. Integração é feita por PR/cherry-pick após revisão.
