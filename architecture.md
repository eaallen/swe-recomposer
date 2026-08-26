# Architecture

This file is the source of truth for how **swe-recomposer** is structured and how artifacts are produced.

## Purpose

Keep the Cursor **swe** skill and **swe-module-builder** subagent as the canonical orchestration for modular TDD builds. From that same source, recompose into:

1. **Skills only** — one Cursor skill (no named subagents) that a single agent can follow.
2. **Single prompt** — one portable prompt a human can paste into Grok, Meta.ai, or similar.

The methodology does not change across targets. Only the *runtime assumptions* change (worktrees, named subagents, files on disk).

## Canonical source

| Artifact | Path | Role |
|---|---|---|
| SWE orchestrator | `.cursor/skills/swe/SKILL.md` | Tech-lead workflow, gates, module plans, assembly |
| Module builder | `.cursor/agents/swe-module-builder.md` | Isolated TDD implementation of one module |
| Review rubric | `source/review/thermo-nuclear-code-quality-review.md` | Quality bar after a module is built |

Do not paraphrase the canonical skill or agent when editing them. Change source, then recompose.

## Generated outputs

| Target | Path | Runtime |
|---|---|---|
| Skills only | `dist/skills-only/swe/SKILL.md` | Cursor, no `agents/` required |
| Portable prompt | `dist/prompts/portable.md` | Any capable chat model |
| Grok prompt | `dist/prompts/grok.md` | grok.com / xAI |
| Meta.ai prompt | `dist/prompts/meta-ai.md` | Meta.ai |

Regenerate these by invoking the **recompose** skill. Do not hand-edit `dist/` unless you are fixing a one-off export; prefer regenerating.

## Invariants that must survive recomposition

- User thinks through modules; the agent does not seed product preferences.
- Architecture document exists before implementation.
- Explicit user permission / sign-off before the next phase.
- Each module plan includes: Purpose, Public API, Dependencies (interfaces only), Data Models, Acceptance Criteria, Non-Goals.
- TDD is red-green-refactor with no pause for the user during a module build.
- A module may only edit itself. Other modules are off-limits.
- After two failed attempts on the same tests, write a brief `BLOCKERS.md`.
- Review uses the thermo-nuclear quality bar. Failures loop back into a rebuild.
- After modules pass: DRY utilities, integration tests, then assemble the system.

## Runtime translation

| Canonical (Cursor) | Skills only | Single prompt |
|---|---|---|
| Named `swe-module-builder` subagent in a worktree | Same agent runs the Module Builder procedure; isolate by directory (worktree if available) | One module at a time in the conversation; emit files as markdown the user can save |
| Parallel module builds | Parallel only if isolated tasks exist; otherwise sequential | Always sequential |
| `thermo-nuclear-code-quality-review` subagent | Inline review section in the skill | Inline review section in the prompt |
| Project `architecture.md` + Cursor rule | Same, if a repo exists | Treat architecture as a pinned document in-chat |

## Non-goals

- This repo is not a compiler or a package that “runs” SWE. It is source + recomposition.
- Do not invent a different software process for Grok/Meta.ai. Translate the same process.
- Do not silently drop sign-off gates to make the prompt shorter.
