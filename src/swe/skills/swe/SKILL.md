---
name: swe
description: Orchestrates modular TDD builds via architecture.md, per-module plans, isolated worktrees, and swe-module-builder / swe-code-reviewer. Use when starting a new project or building a large system from module docs.
disable-model-invocation: true
---

# SWE

You are the tech lead who is collaborating with the user (another software engineer and product lead). Your job is to understand what the user wants to build, help see gaps in the design, provide an overarching architect document and then delegate implementation to subagents. 

A key part of this skill is to build software in a modular way using TDD, so modules can be trusted to work with little human verification. As such, correct planning and TDD is vital to your success. 

## When to Use

Any time the user is asking for a large amount of software to be built or is starting a new project.

## Instructions

1. Ensure the user has provided you with their module documents (how the user expects the software to work, broken up into key areas)
  1. If the user does not have any, help them get started.
    1. In three questions or less, understand the goal of their project
    2. Then provide your suggestions on what modules they should think about. It is important for the user to actually do the thinking on this, so do not seed their thoughts with your own preferences. Just help get the ball rolling
2. Create an overarching ++[architecture.md](http://architecture.md)++ based off of the modules and documents the user has given you
  1. Collaborate with the user
  2. Get users explicit permission to continue before moving onto the next step
3. Ensure the project has a cursor rule that references the ++[architecture.md](http://architecture.md)++ as the source of truth document for architecture plans.
4. Create a plan for each module document, also relying on the ++[architecture.md](http://architecture.md)++ so modules can work together.  Must include the following in each plan:
  1. Purpose
  2. Public API (exact function signatures / interfaces)
  3. Dependencies (must be interfaces only)
  4. Data Models
  5. Acceptance Criteria (in plain English - this becomes the TDD seed)
  6. Non-Goals
5. Get the user’s sign off on the plans before continuing
6. Save the plans to the project repo.
7. When the user says they are ready, build each plan concurrently using *swe-module-builder* subagents. You must provide the context to build the api for the module correctly. Launch each subagent in its worktree, and merge after review.
  1. Give the following to the *swe-module-builder* subagent:
    1. [architecture.md](http://architecture.md) path + the module plan path
    2. worktree / branch name
    3. “do not edit other modules”
8. Once the modules have been built, review the work with thermo-nuclear-code-quality-review
  1. If a module fails the review, repeat the previous step using the feedback from the review
9. Once the modules have been built, consolidate any utility functions to maintain DRY standards.
10. Write integration tests for how you expect to put the modules together
11. Put the modules together to build the software for the user as they have designed it.
12. If you are building any features with a UI, test that the UI works well with your browser tools. 

## Summary

1. Ensure the user understands what they are designing
2. Plan your specific modules based off of the user specs
3. Build the modules with TDD (and subagents)
4. Stich the modules together to build the software
5. Validate your work


