# AGENTS.md

Cross-tool Effect-TS skill kit: repo setup, idiomatic Effect 3.x patterns, and Fastify/Next.js/React → Effect porting

`effect-kit` is a cross-tool plugin built on the docks scaffold and meant to run **alongside the [docks](https://github.com/DocksDocks/docks) plugin** — skills surface in every agentskills.io runtime (Claude Code, Codex, OpenCode). The plan-lifecycle and authoring skills (`plan-init`, `plan-manager`, `plan-review`, `write-skill`, `context-tree`) that the Effect skills reference are provided by docks, not bundled here. This root file stays repo-wide; per-area conventions load lazily from nested `AGENTS.md` nodes (see Context tree).

## Repository scope

```
.
├── plugins/effect-kit/
│   ├── .claude-plugin/plugin.json    Claude plugin manifest
│   ├── .codex-plugin/plugin.json     Codex plugin manifest
│   └── skills/                       cross-tool skills (engineering/ — the Effect payload)
├── .claude-plugin/marketplace.json   Claude marketplace catalog
├── .agents/plugins/marketplace.json  Codex marketplace catalog
├── docs/plans/                       plan lifecycle (bootstrapped by plan-init)
└── scripts/                          author-side validators
```

## Effect skills (the payload)

Three cross-tool skills under `plugins/effect-kit/skills/engineering/`, all targeting **Effect 3.x stable** (`effect` package; Schema is `effect/Schema`, never the deprecated `@effect/schema`):

| Skill | Use when |
|---|---|
| `effect-ts-setup` | One-time repo bootstrap — install `effect` (+ `@effect/platform`/`@effect/cli` by type), wire `@effect/language-service`, recommended tsconfig, `typecheck` script, agent-instruction block. |
| `effect-ts-specialist` | Writing idiomatic Effect — services & layers, tagged errors, `Schema`, `Config`, testing (`@effect/vitest`), running effects (`ManagedRuntime`). |
| `effect-ts-port` | Porting Fastify / Next.js App Router / React to Effect — detect → question → plan (via `plan-manager`) → migrate at the boundary. |

## Context tree

Per-area conventions live in nested `AGENTS.md` nodes, each paired with a one-line `CLAUDE.md` (`@AGENTS.md`) so Claude Code's descendant walker finds them:

| Node | Covers |
|---|---|
| `docs/plans/AGENTS.md` | plan lifecycle + conventions |
| `plugins/effect-kit/skills/AGENTS.md` | skill authoring rules |
| `scripts/AGENTS.md` | validators + CI |

The `context-tree` skill maintains these nodes; `scripts/tree/guard.sh` enforces the pair convention.

## Tool-agnostic rules

- Run `corepack enable && pnpm install --frozen-lockfile` once, then `bash scripts/tree/guard.sh` and `bash scripts/skills/guard.sh plugins/effect-kit/skills` before commit
- Skill bodies stay ≤500 lines per agentskills.io spec (sweet spot 80–310)
- Manifest versions stay in lockstep across both `plugin.json`s and both marketplace catalogs
- Effect best practices are self-contained in each skill's `references/`; the skills opportunistically use the `effect-solutions` CLI, context7, or a cloned `effect` source tree when present, but never depend on them
- docks is a hard companion: the Claude manifests declare a cross-marketplace `dependencies` on `docks@docks` (gated by `allowCrossMarketplaceDependenciesOn: ["docks"]` in the root marketplace.json). Codex has no dependency field, so docks is a documented manual prerequisite there — keep both in sync if the docks marketplace/plugin name ever changes
