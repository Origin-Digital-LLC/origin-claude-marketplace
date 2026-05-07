---
name: status-report
description: Use when generating a project status report from GitHub project issues
---

# Status Report

## Overview

Fetches the current GitHub project board state and writes a dated markdown status report grouped by issue status.

## Steps

1. **Download issues** from the GitHub project board by invoking the `download-github-issues` skill.

   This writes `project_mgmt/github-issues.json`.

2. **Read the JSON** at `project_mgmt/github-issues.json`. The relevant path is:
   `data.organization.projectV2.items.nodes[]`
   Each node has:
   - `status.name` — the board column (e.g. "In Progress", "Done", "Todo", "Backlog")
   - `content.number`, `content.title`, `content.url`, `content.state`
   - `content.assignees.nodes[].login`
   - `content.labels.nodes[].name`
   - `content.comments.nodes[]` — `{ author.login, body, createdAt }`

3. **Determine today's date** (YYYY-MM-DD) and set the output path:
   `project_mgmt/status_reports/status-report-YYYY-MM-DD.md`
   Create the `status_reports/` directory if it does not exist.

4. **Write the report** using the format below.

## Report Format

```markdown
# Status Report — YYYY-MM-DD

## Summary

One or two sentences describing overall project health and notable highlights.

## Recent Activity

Bullet list of notable comments or updates from the past week, drawn from
`content.comments.nodes` where `createdAt` is within the last 7 days.
Format: `- [#NNN](url): @author (YYYY-MM-DD) — brief summary of comment`

## In Progress

| #           | Title | Assignee | Labels |
| ----------- | ----- | -------- | ------ |
| [#123](url) | Title | @login   | label  |

## Todo / Ready

| #           | Title | Labels |
| ----------- | ----- | ------ |
| [#115](url) | Title | label  |

## Backlog

| #           | Title |
| ----------- | ----- |
| [#110](url) | Title |

## Done

| #           | Title | Assignee |
| ----------- | ----- | -------- |
| [#120](url) | Title | @login   |
```

## Status Grouping Rules

- Map `status.name` to sections: `"In progress"` → **In Progress**, `"Done"` → **Done**, `"Ready"` → **Todo / Ready**, `"Backlog"` / null → **Backlog**
- Note: the board uses `"In progress"` (lowercase p) — match exactly when filtering
- Issues where `content` is null (non-issue project items) — skip entirely
- Closed issues (`content.state == "CLOSED"`) with no status → omit unless they appear under "Done"
- Within each section, sort by issue number ascending

## Common Mistakes

| Mistake                                    | Fix                                                                                         |
| ------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Forgetting to create `status_reports/` dir | Run `mkdir -p project_mgmt/status_reports` before writing                                   |
| Using today's date wrong                   | Use the `currentDate` from context or `date +%Y-%m-%d` via Bash                             |
| Omitting the summary paragraph             | Always write 1–2 sentences of prose at the top                                              |
| Including items where `content` is null    | Skip nodes with null content                                                                |
| Temporal language in Recent Activity       | Never write "today", "yesterday", or "this week" — always use the ISO date from `createdAt` |
