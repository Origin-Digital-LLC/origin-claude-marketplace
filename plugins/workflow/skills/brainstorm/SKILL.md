---
name: brainstorm
description: Use this skill when the user wants to define, explore, or expand rough ideas into well-researched, business-level plans before engineering begins — for both a small feature and a full application. Performs competitive research, writes requirements, and recommends a stack, all approved before any code is written.
---

# Brainstorm

Take rough ideas and expand them into well-researched, business-level plans ready for the engineering workflow — competitive research, requirements, and a recommended stack, all approved before any code is written. Works for both a small feature and a full application. Decides *what* to build and *what tools to use*, not *how* to implement it.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Intake and understand** — take the idea or idea list, ask clarifying questions per idea, and identify scale (small feature, utility, or full application).
2. **Research** — deploy the researcher-claude or researcher-copilot agent for competitive research: existing solutions, tools, gaps, and relevant frameworks.
3. **Define requirements** — write business-level requirements, target audience, use cases, key features, and a justified stack recommendation.
4. **Produce the plan** — for each idea, deliver:

   | Section | Answers |
   | --- | --- |
   | **Problem Statement** | What problem does this solve? |
   | **Target Users** | Who is this for? |
   | **Competitive Landscape** | What exists today? |
   | **Proposed Solution** | High-level description of what to build |
   | **Key Features** | Prioritized feature list |
   | **Technology Recommendations** | Suggested stack and tools, justified |
   | **Scope** | What's in and out for the initial build |

5. **User review** — present the plan and iterate on feedback. The user **must** approve before it proceeds to the engineering workflow.

## Pairs with

- **build** — when the approved plan is a greenfield application.
- **feature** — when the plan adds to an existing application.

## Rules

- Manage the user's idea list — track which ideas are brainstormed and which are pending.
- Never proceed to implementation without user approval of the plan.
- Research before recommending — do not guess at the competitive landscape or technology choices.
- Keep plans at the business level — implementation detail is the Architect agent's job.
