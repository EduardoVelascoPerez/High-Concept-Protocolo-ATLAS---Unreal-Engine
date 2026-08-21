# SGA — Protocolo ATLAS

Protótipo de jogo em Unreal Engine 5.7, desenvolvido principalmente com
Blueprints. O projeto está em `ProtocoloATLAS/ProtocoloATLAS.uproject`.

O estado atual inclui personagem e animações, interação com o módulo ATLAS,
escudo do jogador, disparo, recarga e HUD.

## Desenvolvimento com agentes

O fluxo seguro para Codex e Claude Code trabalharem ao mesmo tempo usa branches,
`git worktree` e reservas de ativos binários. Consulte
[`docs/PARALLEL_WORKFLOW.md`](docs/PARALLEL_WORKFLOW.md) antes de iniciar uma
tarefa.

As atribuições autorizadas e seus critérios de aceite ficam em
[`docs/agents/TASKS.md`](docs/agents/TASKS.md). O `CLAUDE.md` importa essa fila
automaticamente quando o Claude Code é iniciado na raiz do worktree.

Antes de commits feitos por qualquer agente, rode `scripts/agent-preflight` para
validar branch, tarefa, arquivos gerados e reservas de ativos Unreal.
