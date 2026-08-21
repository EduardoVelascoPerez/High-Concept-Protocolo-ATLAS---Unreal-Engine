# Claude Code — SGA / Protocolo ATLAS

Leia primeiro `AGENTS.md` e `docs/PARALLEL_WORKFLOW.md`; ambos são obrigatórios.

Você está trabalhando em um projeto Unreal Engine 5.7 baseado principalmente em
Blueprints. Ativos `.uasset` e mapas `.umap` são binários: nunca tente resolver
um conflito desses escolhendo um dos lados sem revisão humana no Unreal Editor.

## Início da sessão

```bash
git status --short --branch
scripts/asset-claim list
```

Confirme que a branch começa com `claude/`. Antes de editar ativos:

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
- Ao entregar, rode `scripts/asset-claim release claude` e reporte validação,
  arquivos e commits.
