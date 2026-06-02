---
name: implement
description: Use this skill when code needs to be written from a plan or direct instruction. Takes a detailed plan or instruction and delivers fully implemented, tested, working code to the highest quality — gathering context, delegating to Developer subagents in parallel where possible, testing, fixing, and iterating until the requirement is met.
---

# Implement

Take a detailed plan or direct instruction and deliver fully implemented, tested, working code to the highest quality. Gathers project context, delegates to Developer subagents (in parallel where possible), tests, fixes, and iterates until the requirement is met. The execution engine of the workflow.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Gather context** — review the project's tech stack and framework docs, existing codebase patterns and conventions, best practices for the technologies in use, and the plan or instructions thoroughly.
2. **Delegate to Developer subagents** — break the work into tasks per the plan's parallelism markings; deploy multiple Developers in parallel for concurrent tasks and sequentially for blocking ones. Each subagent gets clear, focused instructions for its slice.
3. **UI implementation** — for any UI, use `/frontend-design` for guidance and modern frameworks (shadcn/ui, Bootstrap, Material, Google Fonts). Ensure responsive, accessible, big-tech-quality design.
4. **Test and iterate** — run tests after each slice, review for correctness, fix failures, and repeat until the requirement is fully satisfied. Use `/review` to review code and `/simplify` to clean up and optimize.
5. **Quality assurance** — verify all code follows project patterns, logging is in place, and containerization (where used) is correct; run the full test suite.
6. **Deliver** — start the local environment, verify the feature works end to end, and present to the user for testing and approval.

## Pairs with

- **feature** / **build** — upstream, supplying the approved plan.
- **fix** — when implementation goes wrong and needs diagnosis and correction.

## Rules

- Delegate work to subagents for context management and efficient implementation.
- Use the plan's parallelism markings to maximize concurrent work.
- Follow all **Architecture & Development Requirements** and **Minimum Necessary Change** in `principles.md`; never ship a shortcut (**No Tech Debt**).
- Test and iterate until the requirement is fully met before delivery.
- All code changes performed by subagents, never in the Orchestrator session.
