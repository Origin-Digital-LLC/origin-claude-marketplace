# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A Claude Code plugin marketplace managed by Origin Digital. It hosts plugins that can be installed into any project via the Claude Code plugin system. There is no build step, test suite, or dev server — the repo is purely declarative content (JSON manifests + Markdown skill files).

The marketplace is intentionally modular: each plugin targets a specific tool (source control host, issue tracker, etc.), so projects can install only what matches their stack. The current plugins cover GitHub workflows, but the pattern is designed to extend to other toolchains (e.g. Jira, GitLab) without touching existing plugins.

## Repository Structure

```
.claude-plugin/marketplace.json   # Marketplace manifest (lists all plugins)
plugins/
  <plugin-name>/
    .claude-plugin/plugin.json    # Plugin manifest (name, version, components)
    README.md                     # User-facing docs
    skills/
      <skill-name>/
        SKILL.md                  # Skill definition — frontmatter + instructions
```

## Adding a New Plugin

1. Create `plugins/<name>/.claude-plugin/plugin.json` with name, version, description, author, and components flags.
2. Create `plugins/<name>/README.md`.
3. Add skills under `plugins/<name>/skills/<skill-name>/SKILL.md`.
4. Register the plugin in `.claude-plugin/marketplace.json` under `"plugins"`.

## Skill File Format

`SKILL.md` files use YAML frontmatter followed by Markdown instructions:

```markdown
---
name: skill-name
description: Trigger condition for the skill (shown to Claude as the "when to use" hint)
argument-hint: what $ARGUMENTS contains (optional)
disable-model-invocation: true # prevents auto-invocation; skill runs only when explicitly called
---

# Skill Title

Markdown instructions...
```

Skills reference `$ARGUMENTS` for anything passed after the slash command name.

## Versioning

Both `marketplace.json` and each `plugin.json` carry a `version` field. Bump both when releasing changes — they should stay in sync within a plugin.

## Current Plugins

### `github`

Skills for any GitHub repository. Covers the PR review workflow: fetches unresolved inline review threads via GraphQL, contextualizes each comment against the actual code, triages by severity, and posts replies in-thread as changes are made.

Requires: `gh` CLI authenticated.

### `github-projects`

Skills for repos that use GitHub Projects as their issue tracker. Includes:

- **download-github-issues** — fetches the full project board via GraphQL and writes it to `project_mgmt/github-issues.json`
- **status-report** — reads that JSON and generates a dated markdown status report grouped by board column
- **functional-review** — fetches a GitHub issue and posts a collegial challenge comment from a product/UX angle (reads relevant frontend code before commenting)
- **tech-review** — same pattern but from an architecture/engineering angle (reads relevant backend code before commenting)

Requires: `gh` CLI authenticated, repository must use GitHub Projects.
