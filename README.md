# swe-recomposer

Canonical **SWE** skill + **swe-module-builder** subagent, plus Cursor-driven recomposition into:

- **Skills only** — one skill, no named subagents
- **Single prompt** — paste into Grok, Meta.ai, or similar

`architecture.md` is the source of truth for how this repo is laid out.

## Canonical (Cursor)

| What | Path |
|---|---|
| Tech-lead orchestrator | `.cursor/skills/swe/SKILL.md` |
| Isolated TDD builder | `.cursor/agents/swe-module-builder.md` |
| Recompose workflow | `.cursor/skills/recompose/SKILL.md` |
| Review rubric snapshot | `source/review/thermo-nuclear-code-quality-review.md` |

In Cursor, `@swe` (or open that skill) to run the full skill + subagent flow. The builder is a project agent named `swe-module-builder`.

## Recompose

Ask Cursor, for example:

- `recompose SWE to skills only`
- `recompose to a Grok prompt`
- `recompose to Meta.ai`
- `recompose both`

That reads the canonical files and overwrites:

| Target | Path |
|---|---|
| Skills only | `dist/skills-only/swe/SKILL.md` |
| Portable | `dist/prompts/portable.md` |
| Grok | `dist/prompts/grok.md` |
| Meta.ai | `dist/prompts/meta-ai.md` |

Copy `dist/skills-only/swe/` into another project’s `.cursor/skills/` to use SWE without the agent. Copy a prompt file into Grok or Meta.ai as the system/first message.

## Edit, then recompose

Change SWE itself only in `.cursor/skills/swe/SKILL.md` and `.cursor/agents/swe-module-builder.md`. Then recompose. Do not hand-maintain `dist/`.
