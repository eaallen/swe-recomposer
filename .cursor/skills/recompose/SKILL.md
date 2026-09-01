---
name: recompose
description: Recompose the canonical SWE skill into skills-only or a single portable prompt for Grok, Meta.ai, and similar. Use when exporting SWE, flattening the review subagent, or producing a prompt for another platform.
---

# Recompose SWE

Read [architecture.md](../../../architecture.md) first. Canonical source is never paraphrased; only runtime assumptions are translated.

## When to use

The user asks to recompose, flatten, export, make a skills-only pack, or produce a single prompt for Grok, Meta.ai, ChatGPT, or another platform.

## Inputs (do not edit unless the user is changing SWE itself)

1. `src/swe/skills/swe/SKILL.md`
2. `src/review/thermo-nuclear-code-quality-review.md`

## Targets

Ask which target if the user did not specify. Default is **both**.

| Target | Write to |
|---|---|
| Skills only | `dist/skills-only/swe/SKILL.md` |
| Portable | `dist/prompts/portable.md` |
| Grok | `dist/prompts/grok.md` |
| Meta.ai | `dist/prompts/meta-ai.md` |

Overwrite generated files. Do not hand-patch `dist/` across runs.

## Translation rules

Keep every invariant in architecture.md. Translate mechanics as follows:

**Skills only**

- One skill named `swe`. No `agents/` file required.
- Canonical already builds modules in-process with TDD (no implementation subagents). Keep that.
- Inline a **Review** section from the thermo-nuclear rubric (keep the approval bar and output order; drop plugin/Task orchestration). Replace “review with thermo-nuclear-code-quality-review” with that inline procedure.
- Keep sign-off gates, work-named architecture document, plan sections, TDD, BLOCKERS.md template, DRY, integration tests, assembly.
- Frontmatter: keep `disable-model-invocation: true`. Description must say this is the skills-only recomposition (no named subagents).

**Single prompt (portable / Grok / Meta.ai)**

- No YAML frontmatter, no Cursor tool names, no MCP, no worktrees.
- You are the tech lead in one conversation with the user.
- Modules are sequential. Emit each file as a markdown block the user can save.
- If the host has no repo, keep the work-named architecture document, module plans, and `BLOCKERS.md` as in-chat documents and ask the user to persist them.
- Sign-off: do not write implementation until the user explicitly approves plans.
- TDD: write failing tests, then implementation, then refactor, without waiting during a module — but still stop between *phases* (discovery, architecture, plans, build).
- Grok: allow web search when facts are needed; prefer long, complete file dumps; user can save into a project.
- Meta.ai: one module (or one file) per message when output would be huge; remind the user to save artifacts; do not assume a filesystem.

## After writing

Summarize what changed in each target and list the output paths. Do not commit unless asked.
