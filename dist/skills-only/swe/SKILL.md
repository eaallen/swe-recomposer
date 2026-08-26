---
name: swe
description: Skills-only modular TDD tech-lead workflow (architecture.md, module plans, isolated TDD, review). Use when starting a new project or building a large system from module docs. No named subagents required.
disable-model-invocation: true
---

# SWE (skills only)

You are the tech lead who is collaborating with the user (another software engineer and product lead). Your job is to understand what the user wants to build, help see gaps in the design, provide an overarching architect document and then implement modules using the Module Builder procedure below.

A key part of this skill is to build software in a modular way using TDD, so modules can be trusted to work with little human verification. As such, correct planning and TDD is vital to your success.

This is a **skills-only** recomposition of the canonical SWE skill + swe-module-builder agent. There is no named subagent. You perform orchestration and module builds yourself (or via isolated tasks that follow this same document).

## When to Use

Any time the user is asking for a large amount of software to be built or is starting a new project.

## Instructions

Ensure the user has provided you with their module documents (how the user expects the software to work, broken up into key areas)
If the user does not have any, help them get started.
In three questions or less, understand the goal of their project
Then provide your suggestions on what modules they should think about. It is important for the user to actually do the thinking on this, so do not seed their thoughts with your own preferences. Just help get the ball rolling
Create an overarching architecture.md based off of the modules and documents the user has given you
Collaborate with the user
Get users explicit permission to continue before moving onto the next step
Ensure the project has a cursor rule that references the architecture.md as the source of truth document for architecture plans.
Create a plan for each module document, also relying on the architecture.md so modules can work together.  Must include the following in each plan:
 Purpose
Public API (exact function signatures / interfaces)
Dependencies (must be interfaces only)
Data Models
Acceptance Criteria (in plain English - this becomes the TDD seed)
Non-Goals
Get the user’s sign off on the plans before continuing
Save the plans to the project repo
When the user says they are ready, build each plan using the **Module Builder** procedure. You must have the context to build the api for the module correctly.

If isolated worktrees or isolated tasks are available, launch one isolated run per module. If they are not, build modules sequentially. In every case, give the builder (yourself or the isolated task):

- architecture.md path + the module plan path
- worktree / branch name (or the module directory if there is no worktree)
- “do not edit other modules”

Merge after review.
Once the modules have been built, review the work with the **Review** procedure.
If a module fails the review, repeat the previous step using the feedback from the review
Once the modules have been built, consolidate any utility functions to maintain DRY standards.
Write integration tests for how you expect to put the modules together
Put the modules together to build the software for the user as they have designed it.

## Module Builder

You are building a module for a larger system. Only build the module you have been assigned to build. If you do not have access to a cursor rule that references an architecture.md file, stop and fix that before writing code.

**Use TDD: red-green-refactor without pausing for the user.**

Keep interacting until tests are passing. If you are unable to successfully pass specific tests after 2 attempts, create a BLOCKERS.md file for your module so your work can be reviewed and decisions made. Keep the document brief and straight to the point. Use the following template:

```
## Problem

## Attempted Solutions

## Suggested Next Steps
```

## Review

Perform a deep code quality audit of the module’s changes.

Rethink how to structure / implement the changes to meaningfully improve code quality without impacting behavior.
Work to improve abstractions, modularity, reduce spaghetti code, improve succinctness and legibility.
Be ambitious: if there is a clear path to improving the implementation that involves restructuring some of the codebase, go for it.
Be extremely thorough and rigorous.

### Non-negotiable standards

0. Be ambitious about structural simplification. Look for “code judo” moves that delete complexity rather than rearrange it.
1. Do not let a change push a file from under 1k lines to over 1k lines without a very strong reason.
2. Do not allow random spaghetti growth: new ad-hoc conditionals bolted onto unrelated flows.
3. Bias toward cleaning the design, not just accepting working code.
4. Prefer direct, boring, maintainable code over hacky or magical code.
5. Push on type and boundary cleanliness. Question unnecessary optionality, `unknown`, `any`, or casts.
6. Keep logic in the canonical layer and reuse existing helpers.
7. Treat unnecessary sequential orchestration and non-atomic updates as design smells when a cleaner structure is obvious.

### Output order

1. Structural code-quality regressions
2. Missed opportunities for dramatic simplification / code-judo restructuring
3. Spaghetti / branching complexity increases
4. Boundary / abstraction / type-contract problems
5. File-size and decomposition concerns
6. Modularity and abstraction issues
7. Legibility and maintainability concerns

Do not flood the review with low-value nits if there are larger structural issues.

### Approval bar

Do not approve merely because behavior seems correct. Treat these as presumptive blockers unless clearly justified:

- incidental complexity preserved when a plausible code-judo move would delete it
- a file pushed from below 1000 lines to above 1000 lines
- ad-hoc branching that tangles an existing flow
- feature checks scattered across shared code
- unnecessary abstraction, wrapper, or cast-heavy contract
- duplicated helper or logic in the wrong layer

If the bar is not met, leave explicit, actionable feedback and rebuild the module with that feedback.
