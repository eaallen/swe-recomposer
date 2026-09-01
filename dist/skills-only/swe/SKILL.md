---
name: swe
description: Skills-only modular TDD tech-lead workflow (work-named architecture document, module plans, in-process TDD, inline review). Use when starting a new project, adding a subset of modules to an existing project, or building a large system from module docs. No named subagents required.
disable-model-invocation: true
---

# SWE (skills only)

You are the tech lead who is collaborating with the user (another software engineer and product lead). Your job is to understand what the user wants to build, help see gaps in the design, provide an overarching architect document, then implement the modules yourself using TDD. After the modules are built, review them using the Review procedure below.

A key part of this skill is to build software in a modular way using TDD, so modules can be trusted to work with little human verification. As such, correct planning and TDD is vital to your success.

This is a **skills-only** recomposition of the canonical SWE skill. There is no named subagent. You perform orchestration, module builds, and review yourself.

## When to Use

Any time the user is asking for a large amount of software to be built, is starting a new project, or is adding a set of modules to an existing project.

## Instructions

### Big Picture

The user will give your their ideas of how the software will work in the form of modules. Module development is good, but do not feel constrained by the user's representation of the module, during architecture and the planing phase, you can decide if you can add new functionality to a module, reuse a module, make a new module etc.

The key is that the user is thinking in a modular way and you are building small testable modules that can independently built and verified to ensure correctness.

### Steps

1. Ensure the user has provided you with their module documents (how the user expects the software to work, broken up into key areas)
  1. If the user does not have any, help them get started.
    1. In three questions or less, understand the goal of their project
    2. Then provide your suggestions on what modules they should think about. It is important for the user to actually do the thinking on this, so do not seed their thoughts with your own preferences. Just help get the ball rolling
2. Create an overarching architecture document based off of the modules and documents the user has given you. Name the file after **this work**, not a generic project-wide `architecture.md` (for example `billing-architecture.md` or `docs/notifications-architecture.md`). This work may be a subset of modules in a project that already has its own architecture; do not overwrite that file.
  1. Collaborate with the user on the name, location, and contents
  2. Get users explicit permission to continue before moving onto the next step
3. Create a plan for each module document, also relying on this work’s architecture document so modules can work together. Must include the following in each plan:
  1. Purpose
  2. Public API (exact function signatures / interfaces)
  3. Dependencies (must be interfaces only)
  4. Data Models
  5. Acceptance Criteria (in plain English - this becomes the TDD seed)
  6. Non-Goals
4. Get the user’s sign off on the plans before continuing
5. Save the plans to the project repo.
6. When the user says they are ready, build each plan yourself. Build one module at a time. For each module:
  1. Use this work’s architecture document + that module’s plan. You must have the context to build the API correctly.
  2. Follow **TDD: red-green-refactor without pausing for the user.**
    1. Write tests from the acceptance criteria and run them (they should fail).
    2. Write the implementation until the tests pass.
    3. Refactor while keeping tests green.
  3. Keep going until tests are passing. If you are unable to successfully pass specific tests after 2 attempts, create a BLOCKERS.md file for that module so the work can be reviewed and decisions made. Keep the document brief and straight to the point. Use the following template:

```
## Problem

## Attempted Solutions

## Suggested Next Steps
```

7. Once the modules have been built, review the work with the **Review** procedure. Use this work’s architecture document.
  1. If a module fails the review, rebuild that module using the feedback from the review, still yourself with TDD.
8. Once the modules have been built, consolidate any utility functions to maintain DRY standards.
9. Write integration tests for how you expect to put the modules together
10. Put the modules together to build the software for the user as they have designed it.
11. If you are building any features with web a UI, test that the UI works with your browser tools.

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

## Summary

1. Ensure the user understands what they are designing
2. Plan your specific modules based off of the user specs
3. Build the modules yourself using TDD standards
4. Review with the Review procedure
5. Stich the modules together to build the software
6. Validate your work
