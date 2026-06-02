# workflow

A human-AI augmented development lifecycle for Claude Code. Specialist **agents** delegate tasks, supply domain expertise, and manage context; chained **skills** drive a feature from a rough idea to a pull request. The workflow constantly checks with the user on approach and asks questions to avoid incorrect assumptions, then runs off and drives a feature to completion on its own — involving the human only at kick-off and review.

The ultimate goal is to **build in parallel as much as possible**, accomplishing as much as possible with the human in the loop only at the start (kick-off/approval) and end (review before merge).

## The pipeline

```
Brainstorm  -->  Build / Feature  -->  Implement  -->  Fix  -->  PR review
```

## Skills

| Skill          | Description                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------ |
| `brainstorm`   | Turns rough ideas into approved business-level plans via competitive research and requirement writing        |
| `build`        | Creates a new greenfield application or solution from scratch; orchestrates brainstorm, feature, and implement |
| `feature`      | Designs and plans a feature on top of an existing application, then hands off to implement                    |
| `implement`    | Writes the code via Developer subagents, in parallel where possible; tests, fixes, and iterates              |
| `fix`          | Diagnoses and corrects a failed implementation or existing bug, then updates memory to prevent recurrence    |
| `workflow-run` | Runs the whole pipeline autonomously, from backlog item to PR                                                |

## Agents

| Agent                | Model | Role                                                                                       |
| -------------------- | ----- | ------------------------------------------------------------------------------------------ |
| `architect`          | Opus  | Designs applications and features; produces implementation plans for the Developer         |
| `developer`          | Sonnet | Implements code from the Architect's plans or direct instruction                          |
| `devops`             | Sonnet | Builds, runs, deploys, and manages applications locally and in deployment                  |
| `project-manager`    | Sonnet | Git operations, backlog/kanban management, PR creation, processing transcripts into stories |
| `researcher-claude`  | Haiku | Gathers information and a second opinion from a different Claude model                      |
| `researcher-copilot` | Haiku | Gathers an independent perspective by prompting GitHub Copilot in headless mode            |

## Principles

[`principles.md`](./principles.md) holds the guiding principles every agent and skill obeys — **No Tech Debt**, **Minimum Necessary Change**, **Architecture & Development Requirements**, **User Stories**, and the **Workflow** rules. Each agent and skill reads it before acting.

> **Optional — orchestrator-level enforcement:** plugins cannot write to your memory, so these principles bind the plugin's agents and skills but not your top-level Orchestrator session. To have the Orchestrator obey them too, copy the contents of `principles.md` into your project or global `CLAUDE.md`.

## Requirements

- [Claude Code](https://claude.ai/code) installed.
- `gh` CLI authenticated — for GitHub-integrated story/PR management (the `project-manager` agent).
- **Optional:** Docker / Docker Desktop — used where the project is containerized; the workflow falls back to the project's build/run setup otherwise.
- **Optional:** GitHub Copilot CLI (`copilot`) — required only by the `researcher-copilot` agent.

## PR review

This plugin focuses on the build pipeline. For working reviewer comments on an open PR, install the [`github`](../github) plugin, which provides the `fetch-pr-review` skill.
