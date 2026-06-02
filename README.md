# Origin Claude Marketplace

Origin Digital's Claude Code plugin marketplace. Install individual plugins based on your project's tooling.

## Plugins

### [github](./plugins/github)

Skills for any GitHub repository. Requires `gh` CLI authenticated.

| Skill             | Description                                                                                                                                                             |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fetch-pr-review` | Fetches unresolved inline review threads for your PR, contextualizes each against the actual code, triages by severity, and posts replies in-thread as you address them |

### [github-projects](./plugins/github-projects)

Skills for repos that use GitHub Projects for issue tracking. Requires `gh` CLI authenticated.

| Skill                    | Description                                                                                           |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| `download-github-issues` | Fetches the full project board via GraphQL and writes it to `project_mgmt/github-issues.json`         |
| `status-report`          | Reads the issues JSON and generates a dated markdown status report grouped by board column            |
| `functional-review`      | Fetches a GitHub issue and posts a collegial challenge comment from a product/UX angle                |
| `tech-review`            | Fetches a GitHub issue and posts a collegial challenge comment from an architecture/engineering angle |

### [workflow](./plugins/workflow)

A human-AI augmented development lifecycle: specialist agents plus a chained skill pipeline that drives a feature from a rough idea to a PR. Ships **agents** and a shared **principles** doc, not just skills.

| Component | Name | Description |
| --------- | ---- | ----------- |
| Skill | `brainstorm`   | Turns rough ideas into approved business-level plans via research and requirement writing |
| Skill | `build`        | Creates a new greenfield application from scratch; orchestrates brainstorm, feature, implement |
| Skill | `feature`      | Designs and plans a feature on an existing application, then hands off to implement |
| Skill | `implement`    | Writes code via Developer subagents in parallel; tests, fixes, and iterates |
| Skill | `fix`          | Diagnoses and corrects a failed implementation or bug, then updates memory |
| Skill | `workflow-run` | Runs the whole pipeline autonomously, from backlog item to PR |
| Agent | `architect`, `developer`, `devops`, `project-manager`, `researcher-claude`, `researcher-copilot` | Specialist subagents the skills delegate to |

Optional: `gh` CLI (GitHub story/PR management), Docker (used where the project is containerized), and the GitHub Copilot CLI (only for `researcher-copilot`).

## Installation

```bash
# Register this marketplace
/plugin marketplace add https://github.com/origin-digital-llc/origin-claude-marketplace

# Install a plugin
/plugin install github@origin-claude-marketplace
/plugin install github-projects@origin-claude-marketplace
/plugin install workflow@origin-claude-marketplace
```

## Requirements

- [Claude Code](https://claude.ai/code) installed
- Per-plugin requirements are listed above
