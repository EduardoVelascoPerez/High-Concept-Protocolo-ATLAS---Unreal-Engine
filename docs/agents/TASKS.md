# Fila de tarefas dos agentes

Este arquivo é a fonte de verdade para Codex e Claude Code. Uma tarefa só pode
ser iniciada quando possuir responsável, branch, escopo e critérios de aceite.

## Estados

- `PRONTA`: pode ser iniciada pelo responsável.
- `EM_ANDAMENTO`: o responsável está executando a tarefa.
- `BLOQUEADA`: depende de decisão, acesso ou mudança externa documentada.
- `EM_REVISAO`: implementação concluída e aguardando integração.
- `CONCLUIDA`: integrada e validada.
- `FUTURA`: ainda não autorizada para execução.

Cada agente pode alterar somente o estado da própria tarefa. Mudanças de escopo,
responsável ou critérios de aceite são feitas pelo coordenador na branch de
integração.

## Quadro atual

| ID | Responsável | Branch | Estado | Resultado esperado |
| --- | --- | --- | --- | --- |
| SGA-CLAUDE-001 | Claude Code | `claude/sga-next` | `PRONTA` | Auditoria técnica e backlog seguro do SGA |
| SGA-CODEX-001 | Codex | `chore/ai-parallel-workflow` | `EM_REVISAO` | Infraestrutura de trabalho paralelo |

---

## SGA-CLAUDE-001 — Auditoria técnica e backlog seguro

**Responsável:** Claude Code

**Branch:** `claude/sga-next`

**Estado inicial:** `PRONTA`

**Tipo:** somente documentação e inspeção de leitura

### Objetivo

Entender o estado técnico do SGA/Protocolo ATLAS e produzir um mapa verificável
do projeto, com um backlog que possa ser dividido entre agentes sem colisão de
ativos binários.

### Escopo permitido

- Ler `README.md`, `AGENTS.md`, `CLAUDE.md`, configurações `.ini`, `.uproject`,
  histórico Git e nomes/tamanhos de arquivos rastreados.
- Inspecionar a organização de `ProtocoloATLAS/Content/` sem modificar ativos.
- Criar `docs/audits/SGA_REPOSITORY_AUDIT.md`.
- Atualizar apenas o estado desta tarefa neste arquivo.

### Fora do escopo

- Não editar `.uasset`, `.umap`, `.ubulk`, `.uexp` ou arquivos do Unreal.
- Não abrir o Unreal Editor para salvar, migrar ou corrigir ativos.
- Não remover artefatos rastreados nem migrar o histórico para Git LFS.
- Não instalar ferramentas/dependências, fazer push, abrir PR ou publicar.
- Não implementar itens encontrados durante a auditoria.

### Passos obrigatórios

1. Confirmar `claude/sga-next` e worktree limpo.
2. Ler todos os arquivos importados por `CLAUDE.md`.
3. Executar `scripts/asset-claim list`; esta tarefa não deve criar reservas.
4. Mapear versão do engine, plugins, mapas, GameMode, input, UI, personagem,
   interação ATLAS, escudo, disparo e recarga com as evidências disponíveis.
5. Identificar hotspots compartilhados, especialmente
   `BP_FirstPersonCharacter.uasset`, e os riscos de merge.
6. Propor backlog priorizado com tarefas pequenas. Para cada tarefa, indicar
   arquivos/pastas previstos e quais podem rodar em paralelo.
7. Validar somente Markdown e estado Git; não declarar testes do Unreal que não
   tenham sido realmente executados.
8. Produzir o handoff no formato obrigatório e mudar o estado para `EM_REVISAO`.

### Critérios de aceite

O arquivo `docs/audits/SGA_REPOSITORY_AUDIT.md` deve conter:

- resumo executivo e limites da inspeção;
- arquitetura e mapa de subsistemas com caminhos reais;
- linha do tempo dos sistemas identificados no histórico Git;
- inventário de ativos binários e artefatos gerados;
- matriz de hotspots/risco de colisão;
- backlog de 8 a 12 tarefas priorizadas, cada uma com escopo de arquivos,
  dependências, possibilidade de paralelismo e critério de aceite;
- lacunas que exigem inspeção humana no Unreal Editor;
- comandos/evidências usados, sem dados sensíveis.

Antes da entrega, executar:

```bash
git diff --check
git status --short --branch
git diff --stat
```

Commit sugerido: `docs: auditar arquitetura e backlog do SGA`.

---

## SGA-CODEX-001 — Infraestrutura paralela

**Responsável:** Codex

**Branch:** `chore/ai-parallel-workflow`

**Estado:** `EM_REVISAO`

Entregar worktrees isolados, reserva compartilhada de ativos, instruções
automáticas para Claude Code e documentação operacional. Não inclui publicação,
instalação do Claude Code, Git LFS ou limpeza do histórico.

---

## Tarefas futuras — não executar

Os itens a seguir são apenas candidatos e dependem da auditoria
`SGA-CLAUDE-001` e de autorização:

- limpeza dos diretórios gerados já rastreados;
- adoção/migração para Git LFS;
- testes de gameplay no Unreal Editor;
- refatoração do Blueprint central do personagem;
- separação de sistemas de disparo, recarga e escudo.
