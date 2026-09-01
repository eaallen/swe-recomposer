# Architecture

This file is the source of truth for how **skill-recomposer** is structured and how artifacts are produced.

## Purpose

Hold Cursor skills (and companion agents) as canonical source, then recompose them for other runtimes. Currently that skill is **swe**: the same agent plans and builds modules with TDD; a code-quality review subagent runs only after modules are built. From that same source, recompose into:

1. **Skills only** — one Cursor skill (no named subagents) that a single agent can follow.
2. **Single prompt** — one portable prompt a human can paste into Grok, Meta.ai, or similar.

The methodology does not change across targets. Only the *runtime assumptions* change (named review subagent vs inline review, files on disk).

## Canonical source

| Artifact | Path | Role |
|---|---|---|
| SWE orchestrator | `src/swe/skills/swe/SKILL.md` | Tech-lead workflow, gates, in-process TDD module builds, assembly |
| Review rubric | `src/review/thermo-nuclear-code-quality-review.md` | Quality bar after modules are built (subagent in Cursor) |

Do not paraphrase the canonical skill when editing it. Change source, then recompose.

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
- A work-named architecture document exists before implementation (not a generic project-wide `architecture.md` unless this work *is* the whole project and the user agrees).
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
| Same agent builds each module with TDD (no implementation subagents); one module at a time; only edit that module | Same | One module at a time in the conversation; emit files as markdown the user can save |
| `thermo-nuclear-code-quality-review` subagent after modules are built | Inline review section in the skill | Inline review section in the prompt |
| Work-named architecture document (no project-wide cursor rule) | Same, if a repo exists | Treat that architecture document as pinned in-chat |

## Non-goals

- This repo is not a compiler or a package that “runs” a skill. It is source + recomposition.
- Do not invent a different software process for Grok/Meta.ai. Translate the same process.
- Do not silently drop sign-off gates to make the prompt shorter.
