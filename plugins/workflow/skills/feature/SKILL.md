---
name: feature
description: Use this skill when adding a feature to an existing application. Turns a user story or instruction into a complete, approved implementation plan — research, architecture, and a task breakdown with parallelism markings, all signed off before any code is written. Hands off to implement when ready.
---

# Feature

Turn a user story or instruction into a complete, approved implementation plan that Developer subagents can execute — research, architecture, and a task breakdown with parallelism markings, all signed off before any code is written. Use it when adding a feature to an *existing* application. The bridge between a requirement and working code; it owns research, architecture, and planning, then hands off to implement.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Understand the problem** — take the user story or instruction, ask clarifying questions, review the existing codebase for current state and patterns, and identify constraints and dependencies.
2. **Research and design** — deploy researcher-claude / researcher-copilot for approach research, then the Architect agent for the technical design (vertical slice architecture, reusable components, containerization and logging where applicable).
3. **Create the plan** — must include:

   | Section | Covers |
   | --- | --- |
   | **Feature Overview** | What is being built and why |
   | **Technical Design** | Architecture and component design |
   | **Task Breakdown** | Numbered implementation tasks |
   | **Parallelism** | Which tasks run concurrently vs. blocking/sequential |
   | **Testing Strategy** | How to verify the feature works |
   | **Integration Points** | How this connects to existing code |

4. **User review and approval** — present the plan and iterate on feedback. It is **not** implemented until explicitly approved.
5. **Store or implement** — once approved, save to `plans/`, file as a GitHub user story/issue, or run immediately via `/implement`.

## Pairs with

- **brainstorm** — when the idea needs business-level shaping first.
- **implement** — the execution step; parallelism markings drive how Developer subagents are deployed.
- **build** — use instead for greenfield apps rather than features on an existing one.

## Rules

- Ask questions first to fully understand the problem before designing.
- Never implement without user approval of the plan.
- Explicitly define concurrent vs. sequential tasks in every plan.
- Follow all **Architecture & Development Requirements** in `principles.md`.
- Use `/implement` for execution — this skill plans, it does not write code.
