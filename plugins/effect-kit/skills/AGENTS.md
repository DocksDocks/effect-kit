# Authoring skills (plugins/effect-kit/skills/)

Skills are the cross-tool payload — each surfaces in Claude Code, Codex, and any agentskills.io runtime. Each skill is a directory `<category>/<name>/SKILL.md` (+ optional `references/`). Sole category: `engineering/` (the Effect payload). Plan-lifecycle and authoring skills come from the companion docks plugin — don't re-bundle them here.

- Description starts "Use when …" (CSO); ≤500 chars for full scorer credit.
- `name` matches the parent directory; kebab-case.
- Body ≤500 lines (sweet spot 80–310) — every line loads on activation.
- Run `corepack enable && pnpm install --frozen-lockfile` once, then `bash scripts/skills/guard.sh plugins/effect-kit/skills` before commit.

The Effect skills (`engineering/effect-ts-setup`, `engineering/effect-ts-port`, `engineering/effect-ts-specialist`) target **Effect 3.x stable**. Keep version-specific API claims (Schema in `effect/Schema`, `@effect/platform` HttpApi, `@effect-atom/atom-react`) grounded — verify against current docs before changing them.

Use the `write-skill` skill (from the companion docks plugin) to author new skills from scratch.
