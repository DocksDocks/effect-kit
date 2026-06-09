# Validator tooling (scripts/)

Author-side validators — never shipped to consumers. Each accepts a path argument so it targets this project's plugin directory.

- `skills/guard.sh <skills-dir>` — runs both Codex and Claude skill compatibility checks
- `skills/codex.sh <skills-dir>` — Codex loader checks (YAML, name, description cap)
- `skills/claude.sh <skills-dir>` — Claude skill checks (CSO, metadata, body cap)
- `skills/score.sh <skills-dir>` — quality score (per-category floor from `config/scoring.json`)
- `skills/content-hash.sh --backfill <skills-dir>` — sync `metadata.content_hash` after a body/reference edit
- `tree/guard.sh [repo-root]` — context-tree node-pair convention (AGENTS.md + one-line CLAUDE.md)
- `config/read-floor.sh skills <category>` — reads the per-category score floor

Run `corepack enable && pnpm install --frozen-lockfile` before the Node-backed skill guards. `ci.sh` is the aggregate gate — run `bash scripts/ci.sh` before every commit (manifests + version lockstep, structural guards, score floors, content-hash idempotency). The bundled validators default their root to `plugins/effect-kit/skills` (repointed from the docks scaffold); `ci.sh` passes it explicitly anyway.
