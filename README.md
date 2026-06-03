# effect-kit

A cross-tool **Effect-TS** skill kit — repo setup, idiomatic Effect 3.x patterns, and Fastify / Next.js / React → Effect porting. Skills follow the [agentskills.io](https://agentskills.io) standard, so they surface in **Claude Code, Codex, and OpenCode** from the same source.

## Skills

| Skill | Use it to |
|---|---|
| **`effect-ts-setup`** | Bootstrap Effect in a repo — detect the package manager, install `effect` (+ `@effect/platform`/`@effect/cli` by project type), wire `@effect/language-service`, apply a strict tsconfig, add a `typecheck` script, and write an agent-instruction block. |
| **`effect-ts-specialist`** | Write idiomatic Effect — services & layers (`Context.Tag`/`Effect.Service`), tagged errors, `effect/Schema`, `Config`, `ManagedRuntime`, and `@effect/vitest` testing. A passive patterns reference. |
| **`effect-ts-port`** | Migrate an existing Fastify / Next.js App Router / React codebase to Effect — detect the framework, ask scope, write a tiered plan to `docs/plans/` (via `plan-manager`), then port one boundary at a time, test-gated. |

All three target **Effect 3.x stable** (the `effect` package). Key version facts baked in: `Schema` is `effect/Schema` (never the deprecated `@effect/schema`), HTTP uses `@effect/platform` HttpApi (not the deprecated effect-http), and React uses `@effect-atom/atom-react` (the successor to effect-rx). Best-practice content is **self-contained** in each skill's `references/`; the skills *opportunistically* use the `effect-solutions` CLI, context7, or a cloned `Effect-TS/effect` source tree when present, but never depend on them.

A typical flow: `effect-ts-setup` (once) → `effect-ts-specialist` (while writing) → `effect-ts-port` (to migrate existing code).

## Install (Claude Code)

```bash
# local/dev:
claude --plugin-dir ./plugins/effect-kit
# or add the marketplace, then install:
#   /plugin marketplace add <this-repo>
#   /plugin install effect-kit@effect-kit
```

Codex consumes the same skills via `.codex-plugin/plugin.json` + `.agents/plugins/marketplace.json`.

## Layout

```
plugins/effect-kit/
├── .claude-plugin/plugin.json     Claude manifest
├── .codex-plugin/plugin.json      Codex manifest
└── skills/
    ├── engineering/               effect-ts-setup · effect-ts-specialist · effect-ts-port
    └── productivity/              bundled plan-lifecycle + authoring skills
docs/plans/                        plan lifecycle (used by effect-ts-port)
scripts/                           author-side validators + ci.sh
```

The bundled `productivity/` skills (`plan-init`, `plan-manager`, `plan-review`, `write-skill`, `context-tree`) come from the [docks](https://github.com/DocksDocks/docks) scaffold and back the planning + authoring workflow.

## Develop

```bash
corepack enable && pnpm install --frozen-lockfile   # once
bash scripts/ci.sh                                   # manifests + guards + score floors + hash idempotency
```

After editing a skill body or its `references/`, re-sync its hash:

```bash
bash scripts/skills/content-hash.sh --backfill plugins/effect-kit/skills
```

## Provenance

Scaffolded from **docks**. Effect best practices grounded in [effect.website](https://effect.website), [effect.solutions](https://www.effect.solutions/) (Kit Langton), [@effect/language-service](https://github.com/Effect-TS/language-service), and [tim-smart/effect-atom](https://github.com/tim-smart/effect-atom). MIT.
