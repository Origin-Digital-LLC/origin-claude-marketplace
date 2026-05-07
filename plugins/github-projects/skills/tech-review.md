---
name: tech-review
description: Use when asked to do a technical/engineering review of a GitHub issue — fetches the issue, investigates relevant backend architecture, and posts a collegial challenge comment.
argument-hint: issue number
disable-model-invocation: true
---

# Tech Review

Challenge a GitHub issue from a technical/engineering angle: investigate the current architecture in the affected area, then post a peer-review comment that probes data model implications, architectural fit, edge cases, and failure modes.

---

## Phase 1 — Investigate

### Step 1: Fetch the issue

```bash
gh issue view $ARGUMENTS
```

Read the title and body carefully. Identify the area of the codebase being changed.

### Step 2: Find relevant code

Based on the issue, identify which parts of the codebase are likely affected and read them:

- `backend/pkgs/` — domain logic (`quotes`, `pdf_builder`, `email_support`, …)
- `backend/apps/nimble_server/` — FastAPI routes and Pydantic request/response models
- `database/versions/` — Alembic migration files (raw SQL)
- `frontend/src/features/` / `frontend/src/shared/` — if the change surfaces in the UI

Use `find` or `grep` to locate the specific models, routes, or domain logic the issue touches. Read those files to understand the current architecture in the affected area — backend first, but follow the issue's scope wherever it leads.

---

## Phase 2 — Challenge

### Step 3: Synthesize the comment

Write a comment that probes the issue from a technical perspective. Key things to examine:

| Lens                              | Questions to ask                                                                                             |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Data model implications**       | Does this require a schema change? Will existing rows break on read? Does the new field need to be nullable? |
| **Architectural fit**             | Does this match existing patterns? Does it belong in the right package/layer?                                |
| **Edge cases & failure modes**    | What happens when inputs are missing, invalid, or in unexpected states?                                      |
| **Complexity in the wrong layer** | Is business logic leaking into routes, or presentation logic leaking into domain packages?                   |
| **Backward compatibility**        | Does this break existing API consumers, stored data, or running instances during deploy?                     |

Tone: collegial senior engineer. Acknowledge what's well-thought-out. Flag what needs clarification. Don't write a rigid checklist — write a paragraph or two as a thoughtful colleague would.

**Example output:**

> The approach is solid. One thing to flag: this touches the quote creation path — make sure the new field is nullable in the migration or existing rows will break on read. Also worth considering whether this belongs in `quotes` pkg or a new one given it has its own lifecycle.

### Step 4: Show the draft and wait for approval

Append this footer to the comment body:

```
---
*🔧 Tech Review — architecture & engineering challenge via `/tech-review`*
```

Then display the full comment body to the user in a markdown block and ask: **"Ready to post this, or would you like to tweak anything?"**

Do NOT post until the user explicitly says to. Once they approve (or give you edits to incorporate), post with:

```bash
gh issue comment $ARGUMENTS --body "..."
```

---

## Common Mistakes

- **Generic feedback** — read the actual models and routes before commenting; surface specifics from the current implementation
- **Only finding problems** — if the technical approach is sound, say so; approval is a valid outcome
- **Posting without reading the relevant files** — Phase 1 is mandatory, not optional
- **Pedantry** — don't flag things that (a) are caught immediately by the type checker or linter, (b) are obviously implied by the change itself, or (c) any competent implementer would do automatically as part of the work. The bar for inclusion is: _would a careful engineer actually miss this, or does it require an explicit decision before implementation?_ Silent failures, policy conflicts, and non-obvious behavioral edge cases clear the bar. "Don't forget to update the interface too" does not.
