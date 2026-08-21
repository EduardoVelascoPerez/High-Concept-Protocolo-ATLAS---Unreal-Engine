# Claude Code — SGA / Protocolo ATLAS

Os arquivos abaixo fazem parte destas instruções e devem ser considerados em
toda sessão:

@AGENTS.md
@docs/PARALLEL_WORKFLOW.md
@docs/agents/TASKS.md
@docs/agents/HANDOFF_TEMPLATE.md

## Fonte de verdade da tarefa

`docs/agents/TASKS.md` é a única fila autorizada. Ao iniciar:

1. identifique a branch atual;
2. localize a tarefa cujo campo **Branch** seja exatamente essa branch;
3. trabalhe apenas se ela estiver `PRONTA` ou `EM_ANDAMENTO`;
4. execute somente o escopo permitido e os critérios de aceite descritos;
5. se não houver tarefa correspondente, pare e peça uma atribuição — não
   invente uma tarefa nem escolha itens futuros.

Na branch `claude/sga-next`, a tarefa inicial é `SGA-CLAUDE-001`.

Você está trabalhando em um projeto Unreal Engine 5.7 baseado principalmente em
Blueprints. Ativos `.uasset` e mapas `.umap` são binários: nunca tente resolver
um conflito desses escolhendo um dos lados sem revisão humana no Unreal Editor.

## Início da sessão

```bash
git status --short --branch
scripts/asset-claim list
sed -n '1,260p' docs/agents/TASKS.md
```

Confirme que a branch começa com `claude/` e corresponde à tarefa. Antes de
editar ativos:

```bash
scripts/asset-claim claim claude ProtocoloATLAS/Content/<escopo>
```

Escolha um escopo exclusivo e estreito. Se houver sobreposição com uma reserva
existente, pare e proponha uma divisão diferente. Mudanças somente em Markdown,
scripts ou configuração textual não exigem reserva, mas ainda devem ficar na
branch própria.

## Durante e no fim

- Não abra/edite o mesmo projeto físico usado por outro agente; use seu worktree.
- Ignore ruído de `Saved`, `Intermediate`, `DerivedDataCache` e `Binaries`.
- Revise `git diff --stat` e `git status` antes de cada commit.
- Faça commits atômicos; não publique nem integre sem autorização.
- Atualize apenas o status da sua própria tarefa em `docs/agents/TASKS.md`.
- Preencha a entrega usando exatamente `docs/agents/HANDOFF_TEMPLATE.md`.
- Ao entregar, rode `scripts/asset-claim release claude` e reporte validação,
  arquivos e commits.
