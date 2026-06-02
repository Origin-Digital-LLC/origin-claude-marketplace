---
name: build
description: Use this skill when building something entirely new — a new greenfield application or solution from scratch, from a small utility to a full-scale web app with database, auth, and deployments. Orchestrates the full creation pipeline from zero to working software, invoking brainstorm, feature, and implement as needed.
---

# Build

Create a new greenfield application or solution from scratch — from a small utility to a full-scale web app with database, auth, and deployments. The one-shot build skill for quick prototyping and for laying the foundation of larger software. Orchestrates the full creation pipeline from zero to working software, invoking brainstorm, feature, and implement as needed.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Requirements gathering** — if requirements aren't defined yet, invoke `/brainstorm` first. Ensure a clear, approved plan exists and the scope (in and out) is verified before proceeding.
2. **Architecture design** — deploy the Architect agent to design the application: vertical slice architecture, containerization where the project uses it, reusable client patterns, and logging throughout. Review and approve the architecture plan.
3. **Implementation** — use `/implement` to execute the plan, deploying Developer subagents in parallel where the plan allows and following the Architect's sequential vs. parallel task breakdown.
4. **Assembly** — use `/feature` for the individual features that compose the application; assemble the vertical slices into the complete app and ensure all components integrate.
5. **Testing and delivery** — run the full test suite, deploy the DevOps agent to build and start the app locally, verify end to end, and present to the user for review.

## Pairs with

- **brainstorm** — upstream, to shape requirements before building.
- **feature** / **implement** — invoked during assembly and execution.

## Rules

- Follow all **Architecture & Development Requirements** in `principles.md`.
- Everything runs in Docker containers where the project uses them; otherwise follow the project's existing build and run setup.
- Use the brainstorm, feature, and implement skills to assemble the entire product.
- All code changes performed by subagents, never in the Orchestrator session.
- Build and run locally before reporting completion.
