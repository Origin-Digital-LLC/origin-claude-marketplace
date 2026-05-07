# Origin Claude Marketplace

Origin Digital's Claude Code plugin marketplace. Install individual plugins based on your project's tooling.

## Plugins

| Plugin | Description | Requires |
|--------|-------------|----------|
| [github](./plugins/github) | Skills for any GitHub repository | gh CLI |
| [github-projects](./plugins/github-projects) | Skills for repos using GitHub Projects | gh CLI + GitHub Projects |

## Installation

```bash
# Register this marketplace
/plugin marketplace add https://github.com/origindigital/origin-claude-marketplace

# Install a plugin
/plugin install github@origin-claude-marketplace
/plugin install github-projects@origin-claude-marketplace
```

## Requirements

- [Claude Code](https://claude.ai/code) installed
- [GitHub CLI](https://cli.github.com/) installed and authenticated (`gh auth login`)
