---
name: recompose
description: Recompose the canonical SWE skill + swe-module-builder agent into skills-only or a single portable prompt for Grok, Meta.ai, and similar. Use when exporting SWE, flattening subagents, or producing a prompt for another platform.
---

# Recompose SWE

Read [architecture.md](../../../architecture.md) first. Canonical source is never paraphrased; only runtime assumptions are translated.

## When to use

The user asks to recompose, flatten, export, make a skills-only pack, or produce a single prompt for Grok, Meta.ai, ChatGPT, or another platform.

## Inputs (do not edit unless the user is changing SWE itself)

1. `src/swe/skills/swe/SKILL.md`
2. `src/swe/agents/swe-module-builder.md`
3. `src/review/thermo-nuclear-code-quality-review.md`

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
- Inline the module-builder body as a **Module Builder** procedure the same agent must follow.
- Inline a **Review** section from the thermo-nuclear rubric (keep the approval bar and output order; drop plugin/Task orchestration).
- Replace “launch swe-module-builder subagents in worktrees” with: if isolated worktrees/tasks exist, use them with this procedure; otherwise build sequentially and only edit that module’s files.
- Keep sign-off gates, plan sections, TDD, BLOCKERS.md template, DRY, integration tests, assembly.
- Frontmatter: keep `disable-model-invocation: true`. Description must say this is the skills-only recomposition (no named subagents).

**Single prompt (portable / Grok / Meta.ai)**

- No YAML frontmatter, no Cursor tool names, no MCP, no worktrees.
- You are the tech lead in one conversation with the user.
- Modules are sequential. Emit each file as a markdown block the user can save.
- If the host has no repo, keep `architecture.md`, module plans, and `BLOCKERS.md` as in-chat documents and ask the user to persist them.
- Sign-off: do not write implementation until the user explicitly approves plans.
- TDD: write failing tests, then implementation, then refactor, without waiting during a module — but still stop between *phases* (discovery, architecture, plans, build).
- Grok: allow web search when facts are needed; prefer long, complete file dumps; user can save into a project.
- Meta.ai: one module (or one file) per message when output would be huge; remind the user to save artifacts; do not assume a filesystem.

## After writing

Summarize what changed in each target and list the output paths. Do not commit unless asked.
