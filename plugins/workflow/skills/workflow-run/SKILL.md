---
name: workflow-run
description: Use this skill for fully autonomous feature delivery. Runs the entire pipeline end-to-end from backlog item to pull request — scans the todo list for the next unstarted item, then executes Brainstorm → Build/Feature → Implement → Fix in sequence until the work is complete and a PR is ready for review.
argument-hint: optional backlog item to run; otherwise picks up the next unstarted item
---

# Workflow Run

Run the entire workflow pipeline end-to-end, autonomously, from backlog item to pull request. Scans the todo list for the next unstarted item, then executes Brainstorm → Build/Feature → Implement → Fix in sequence until the work is complete and a PR is ready for review. Use for fully autonomous feature delivery.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Find work** — scan the backlog for the next unstarted item (or use the item given in `$ARGUMENTS`). For GitHub projects, check the project board; for local, check the `plans/` folder.
2. **Brainstorm phase** — invoke `/brainstorm` to research and define requirements into a business-level plan. Abbreviate if the item already has clear requirements. Present for user approval before proceeding.
3. **Design phase** — invoke `/build` (new application) or `/feature` (existing application); the Architect agent designs the plan and marks parallel vs. sequential tasks. Present for user approval before proceeding.
4. **Implementation phase** — invoke `/implement` with the approved plan; deploy Developer subagents, maximize parallel work, and test and iterate until all requirements are met.
5. **Fix phase (if needed)** — if implementation hits issues, invoke `/fix` to diagnose root causes, verify with sources, correct, and update memory; then return to implementation.
6. **Delivery** — deploy the DevOps agent to build and run locally, verify end to end, and have the Project Manager agent create a PR. The PR awaits human review.
7. **Repeat** — after the PR is created, return to Step 1 and pick up the next unstarted item, until the backlog is clear or the user stops the workflow.

## Pairs with

- Orchestrates every other skill — brainstorm, build, feature, implement, fix — and the full agent roster.

## Rules

- Follow the full pipeline: Brainstorm → Build/Feature → Implement → Fix.
- User approval is required at the brainstorm and design phases before implementation.
- All code changes performed by subagents.
- Build and run locally before creating the PR.
- Create a PR when work is complete — never commit directly to main.
- This skill orchestrates the other skills — it does not implement code directly.
