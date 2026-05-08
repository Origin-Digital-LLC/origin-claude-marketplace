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

## Installation

```bash
# Register this marketplace
/plugin marketplace add https://github.com/origin-digital-llc/origin-claude-marketplace

# Install a plugin
/plugin install github@origin-claude-marketplace
/plugin install github-projects@origin-claude-marketplace
```

## Requirements

- [Claude Code](https://claude.ai/code) installed
- Per-plugin requirements are listed above
