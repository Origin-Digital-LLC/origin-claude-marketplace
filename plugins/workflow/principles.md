# Workflow Principles

The guiding principles for this workflow. **Every agent and skill must read this file and obey every rule before acting.** These are non-negotiable and apply to the Orchestrator and all subagents.

---

## No Tech Debt

Never create tech debt or defer correct work as a follow-up. If a task, feature, or fix involves doing something the right way, handle it now — not as a TODO, a comment, a follow-up issue, or a "we'll refactor later."

There will be no time to revisit. Deferring correct architecture means it never gets done. If the right approach requires extra upfront effort — better abstractions, fixing the root cause instead of patching a symptom — do it as part of the current task.

**Never ship a known shortcut.**

This principle is non-negotiable. It works alongside **Minimum Necessary Change**: keep the change minimal in *scope*, but never minimal in *correctness*.

---

## Minimum Necessary Change

When writing code for any change, feature, or fix, use a minimal-change approach.

- **Minimal scope** — do not exceed what the task requires. No speculative additions, no refactors outside the task boundary.
- **Simple and readable** — prefer the obvious solution over the clever one.
- **Accurate, concise, working, and fully testable** — every unit of generated code must meet all four.
- **Follow existing project patterns** — before writing code, consult the project's `CLAUDE.md` and any `rules/` documentation for declared conventions, then read representative existing files to confirm the style, conventions, and frameworks in use. All generated code must match them exactly. Do not introduce new patterns, libraries, or conventions unless explicitly requested. If a pattern conflict is found, flag it rather than silently deviating.

Minimal in scope is not the same as minimal in correctness — see **No Tech Debt**. Keep the footprint small, but always do the work the right way.

---

## Architecture & Development Requirements

The core software principles every agent must apply on all projects and every feature.

- **Follow best practices for all code.**
- **Prefer vertical slice architecture** — when the project doesn't already follow an established structure, implement features as vertical slices so each subagent can focus its context on one slice rather than an entire domain layer (each slice owns its own models, handlers, services, and data access). If the project already uses another pattern (onion, repository, hexagonal, etc.), match it — see **Minimum Necessary Change**.
- **Encourage code reuse** — for common cross-project patterns (e.g. a Microsoft Foundry API call, an AWS Bedrock implementation), build reusable clients. Once a pattern is verified working, update memory so every future project repeats it correctly.
- **Run everything in Docker containers where the project uses them** — deploy via Docker Desktop where possible. If the project does not use Docker, follow the project's existing build and run setup.
- **All applications must have structured logs** — record where the logs are stored, for both AI and human review.

These requirements are referenced by the **Workflow** principle and enforced by the Architect, Developer, and DevOps agents.

---

## User Stories

User stories are the primary method of creating, working, and monitoring tasks and features.

### Storage

- If a project is GitHub-integrated, stories are created and stored as GitHub issues.
- If not, stories are stored in the local `plans/` folder.
- A locally stored story must carry the same properties as a GitHub story: status, title, description, acceptance criteria, etc.

### Working stories

- Every story is worked in its own **git worktree** — never on main or shared with another story.
- Stories are worked by Claude subagents autonomously until a human review step is required.
- This workflow is iterative and built to involve the human only at the **start** (kick-off/approval) and **end** (review before merge) of the development lifecycle.

### Kanban discipline

When starting work on a story:

1. Assign yourself to the story before beginning.
2. Move it to the **Ready** column when work begins.
3. Check all other in-progress items for scope or file clashes first.
4. If a clash is found, check with the user before continuing.

---

## Workflow

The agents, skills, and these principles together form a system designed to deliver software end to end from business requirements — either building new applications from scratch or adding features to existing ones. The ultimate goal is to **build in parallel as much as possible**, with the human involved only at kick-off and review.

### Agents

- **Project Manager** — git operations, backlog management, kanban, PR creation, processing transcripts into stories. Consults the Architect when technical decisions are needed.
- **Architect** — designs applications and features; produces implementation plans for the Developer.
- **Developer** — implements code from the Architect's plans or direct Orchestrator instruction.
- **DevOps** — manages the lifecycle of applications locally and deployed; invoked after a feature is complete for build, run, and testing.
- **Researcher - Claude** — gathers information and a second opinion from a different Claude model.
- **Researcher - Copilot** — gathers information and an independent perspective by prompting GitHub Copilot in headless mode.

### Skills

- **Brainstorm** — define and expand ideas via competitive research and requirement writing.
- **Build** — create a new greenfield application or solution from scratch.
- **Feature** — design and plan a feature on top of an existing application.
- **Implement** — implement a specific, usually smaller, piece of code.
- **Fix** — diagnose and correct a failed implementation or existing bug.
- **Workflow Run** — run the entire pipeline: Brainstorm → Build/Feature → Implement → Fix → PR.

### Workflow Rules

- **ALL code changes are performed by subagents.** Never change code in the Orchestrator session.
- **Delegate everything to subagents** to preserve the Orchestrator's context. This is a team.
- When development is finished and code is ready for review, **build and run locally** for testing.
- When working off a worktree, deploy to a **free port** that will not clash with the primary session or other worktrees.
- **User approval is required** at the brainstorm and design phases before implementation begins.
- **Never commit directly to main** — Once the implementation is done, build and run locally so the user can test and review. Ask if the user is ready to create a PR.
