---
name: functional-review
description: Use when asked to do a functional review of a GitHub issue — fetches the issue, investigates the relevant area of the app, and posts a collegial challenge comment focused on end-user usability.
argument-hint: issue number
---

# Functional Review

Challenge a GitHub issue from a product, business and usability angle: investigate what the app currently does in the relevant area, then post a peer-review comment that probes whether the feature works for the end user — scope, user value, and UX edge cases only.

---

## Phase 1 — Investigate

### Step 1: Fetch the issue

```bash
gh issue view $ARGUMENTS --repo Nimble-Insurance/digital-quoting-platform
```

Read the title and body carefully. Identify the area of the product being changed.

### Step 2: Find relevant code

Based on the issue, identify which parts of the codebase are likely affected and read them:

- `frontend/src/features/` — one folder per page/feature (`quote-form`, `quote-result`)
- `frontend/src/shared/` — reusable components and forms
- `backend/apps/nimble_server/` — FastAPI routes and request/response models
- `backend/pkgs/` — domain logic (`quotes`, `pdf_builder`, `email_support`, …)

Use `find` or `grep` to locate the specific components, routes, or domain logic the issue touches. Read those files to understand what the app currently does in that area — frontend and backend both, wherever the issue's scope reaches.

---

## Phase 2 — Challenge

### Step 3: Synthesize the comment

Write a comment that probes the issue from a functional and usability perspective. Key things to examine:

| Lens                       | Questions to ask                                                               |
| -------------------------- | ------------------------------------------------------------------------------ |
| **User value**             | Is the benefit to the user clearly articulated? Who is this for?               |
| **Overlap/conflict**       | Does this duplicate or conflict with existing functionality?                   |
| **Scope definition**       | Is the scope tight enough to implement without ambiguity?                      |
| **UX edge cases**          | What happens in edge cases the issue doesn't address?                          |
| **Validation/flow impact** | Does this change affect required fields, validation logic, or happy-path flow? |

Tone: collegial peer reviewer. Acknowledge what's well-scoped. Flag what needs clarification. Don't write a rigid checklist — write a paragraph or two as a thoughtful colleague would.

**Example output:**

> The value here is clear and the scope is tight. One thing to nail down before implementation: the form currently treats address lookup as required — if this field is optional it needs a fallback path for the validation logic. Otherwise looks well-defined.

### Step 4: Show the draft and wait for approval

Append this footer to the comment body:

```
---
*🔍 Functional Review — product, UX & usability challenge via `/functional-review`*
```

Then display the full comment body to the user in a markdown block and ask: **"Ready to post this, or would you like to tweak anything?"**

Do NOT post until the user explicitly says to. Once they approve (or give you edits to incorporate), post with:

```bash
gh issue comment $ARGUMENTS --repo Nimble-Insurance/digital-quoting-platform --body "..."
```

---

## Common Mistakes

- **Generic feedback** — read the actual code before commenting; surface specifics from the current implementation
- **Only finding problems** — if the issue is well-scoped, say so; approval is a valid outcome
- **Posting without reading the relevant files** — Phase 1 is mandatory, not optional
- **Straying into technical territory** — implementation details, data model choices, API design, and encoding strategies belong in `/tech-review`, not here. If the only concerns you can find are technical, this review has nothing to add. Say "this looks well-scoped from a functional and usability angle" and stop. Never manufacture technical questions to fill space.
