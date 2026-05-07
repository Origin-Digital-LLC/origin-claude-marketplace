# github-projects

Skills for repos using [GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects) for issue tracking. Requires the [GitHub CLI](https://cli.github.com/) (`gh`) to be installed and authenticated.

## Skills

### functional-review

Fetches GitHub Project issues so Claude can review the codebase against them and ask clarifying questions.

### status-report

Downloads the full GitHub Projects backlog and writes a status report on it.

## Requirements

- `gh` CLI installed and authenticated (`gh auth login`)
- Repository must use GitHub Projects for issue tracking
