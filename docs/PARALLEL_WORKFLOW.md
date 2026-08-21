# Trabalho paralelo: Codex + Claude Code

## Objetivo

Cada agente trabalha em uma cópia física separada do projeto, mas todas as
cópias compartilham o mesmo banco Git. Isso evita troca acidental de branch e
permite abrir o Unreal Editor de cada tarefa isoladamente.

```text
main (somente integração)
├── codex/<tarefa>   -> .worktrees/codex-<tarefa>
└── claude/<tarefa>  -> .worktrees/claude-<tarefa>
```

## Preparar uma tarefa

Antes de criar a branch, registre a atribuição completa em
`docs/agents/TASKS.md`. Esse arquivo define quem pode agir, em qual branch, com
qual escopo e quais critérios de aceite. O Claude Code o recebe automaticamente
por meio dos imports do `CLAUDE.md`.

A partir da raiz deste repositório:

```bash
scripts/agent-worktree create claude <tarefa>
scripts/agent-worktree create codex <outra-tarefa>
scripts/agent-worktree list
```

O comando cria `.worktrees/<agente>-<tarefa>` e a branch
`<agente>/<tarefa>`, sempre a partir de `origin/main` por padrão. Para usar uma
base já revisada, passe-a como quarto argumento.

Inicie o Claude no diretório indicado pelo comando:

```bash
cd .worktrees/claude-<tarefa>
claude
```

Na primeira mensagem, basta dizer `execute a tarefa ativa desta branch`. O
Claude deve localizar a atribuição exata em `docs/agents/TASKS.md`; se não
encontrar uma tarefa `PRONTA` ou `EM_ANDAMENTO`, ele deve parar.

O executável `claude` precisa estar instalado e autenticado na máquina; o
repositório não instala ferramentas globais.

## Divisão recomendada

Divida por subsistema ou pasta, não por passos que editam o mesmo Blueprint.
Exemplo seguro:

| Agente | Escopo exclusivo |
| --- | --- |
| Claude | `Content/Widget/` e documentação da UI |
| Codex | `Content/Model/Items/Recharge/` e testes de recarga |

Exemplo inseguro: um agente alterar disparo e outro recarga quando ambos
precisam salvar `BP_FirstPersonCharacter.uasset`.

## Reservar ativos binários

As reservas ficam em uma área compartilhada dentro do Git e são vistas por
todos os worktrees, sem entrar em commits.

```bash
scripts/asset-claim list
scripts/asset-claim claim claude ProtocoloATLAS/Content/Widget
scripts/asset-claim claim codex ProtocoloATLAS/Content/Model/Items/Recharge
scripts/asset-claim release claude
```

Uma reserva de diretório também bloqueia seus descendentes. O comando rejeita
reservas sobrepostas. A reserva é coordenação, não um bloqueio do filesystem;
cada agente continua responsável por obedecê-la.

## Ciclo de integração

1. Atualize `origin/main`.
2. Registre responsável, branch, escopo e aceite em `docs/agents/TASKS.md`.
3. Crie o worktree a partir da base que contém essa atribuição.
4. Reserve os ativos binários antes de abrir o editor.
5. Faça commits pequenos e valide o fluxo dentro do worktree da tarefa.
6. Libere a reserva e entregue usando `docs/agents/HANDOFF_TEMPLATE.md`.
7. Integre uma branch por vez via PR. Após cada merge, atualize a segunda
   branch e valide novamente no Unreal antes do merge final.

Antes de cada commit, execute:

```bash
scripts/agent-preflight
```

O preflight rejeita trabalho direto em `main`, mudanças em diretórios gerados e
ativos Unreal modificados sem uma reserva pertencente à branch atual. Os
artefatos históricos já rastreados são contabilizados como baseline e só causam
erro se forem modificados novamente.

Para Blueprints centrais como `BP_FirstPersonCharacter`, serialize o trabalho:
somente um agente altera o ativo; o outro pode pesquisar, documentar ou atuar em
pastas independentes.

## Estado e riscos encontrados

- Repositório público, `main` única branch remota e sem PRs/issues registrados
  durante a preparação deste fluxo.
- Projeto em Unreal Engine 5.7, majoritariamente Blueprint, sem módulo C++.
- Há 508 `.uasset` rastreadas e 2 mapas `.umap`.
- `Saved/`, `Intermediate/` e `DerivedDataCache/` possuem conteúdo antigo já
  rastreado, apesar do `.gitignore`. Não inclua esse ruído em commits novos.
- Git LFS não está configurado e não está instalado nesta máquina. Uma migração
  para LFS e a remoção dos artefatos gerados do histórico devem ser tratadas em
  uma manutenção separada, com backup e autorização explícita.
- O Claude Code não estava instalado nesta máquina quando este fluxo foi criado;
  os worktrees e as instruções funcionam independentemente da instalação.

## Encerrar um worktree

Depois que a branch estiver integrada e não houver mudanças locais:

```bash
scripts/asset-claim release <agente>
git worktree remove .worktrees/<agente>-<tarefa>
git branch -d <agente>/<tarefa>
```

Revise `git status` no worktree antes de removê-lo. Esses comandos são
intencionalmente manuais para evitar perda acidental de trabalho.
