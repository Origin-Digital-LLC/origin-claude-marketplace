# github-projects

Skills for repos using [GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects) for issue tracking. Requires the [GitHub CLI](https://cli.github.com/) (`gh`) to be installed and authenticated.

## Skills

### download-github-issues

Downloads the full GitHub Projects backlog and writes it to `project_mgmt/github-issues.json`. Can be invoked standalone or is called automatically by `status-report`.

### status-report

Reads `project_mgmt/github-issues.json` and writes a dated markdown status report grouped by board column.

### functional-review

Fetches a GitHub issue and posts a collegial challenge comment focused on product, business, and usability angles.

### tech-review

Fetches a GitHub issue and posts a collegial challenge comment focused on architecture and engineering concerns.

## Requirements

- `gh` CLI installed and authenticated (`gh auth login`)
- Repository must use GitHub Projects for issue tracking
