---
name: fetch-pr-review
description: Use when you have received review comments on your own PR and need to fetch, contextualize, and work through them systematically.
argument-hint: PR number (optional — detected from current branch if omitted)
disable-model-invocation: true
---

# Fetch PR Review

Fetch and contextualize reviewer feedback on your own PR, triage by severity, and work through comments one at a time.

---

## Step 0: Resolve PR Number

If `$ARGUMENTS` was provided, use it directly as the PR number.

Otherwise, detect it from the current branch:

```bash
gh pr view --json number -q .number
```

If this fails (no PR exists for the current branch), ask the user for the PR number before continuing.

---

## Step 1: Fetch Unresolved Comments

Fetch inline review threads via GraphQL, filtering to only unresolved threads (the REST API does not expose resolution status):

```bash
gh api graphql -f query='
  query($pr: Int!) {
    repository(owner: "Nimble-Insurance", name: "digital-quoting-platform") {
      pullRequest(number: $pr) {
        reviewThreads(first: 100) {
          nodes {
            isResolved
            comments(first: 10) {
              nodes {
                databaseId
                path
                line
                body
                author { login }
              }
            }
          }
        }
      }
    }
  }
' -F pr=$PR_NUMBER \
  --jq '.data.repository.pullRequest.reviewThreads.nodes[] | select(.isResolved == false) | .comments.nodes[] | {id: .databaseId, path: .path, line: .line, body: .body, user: .author.login}'
```

Also fetch general PR conversation comments (these don't have resolution status):

```bash
gh api repos/Nimble-Insurance/digital-quoting-platform/issues/$ARGUMENTS/comments \
  --jq '.[] | {id: .id, body: .body, user: .user.login}'
```

---

## Step 2: Contextualize Inline Comments

For each inline comment, read the referenced file around the target line to show actual code context. Present as:

- **`path:line`** — the exact location
- Surrounding code (±5 lines)
- Reviewer's comment

This gives a complete picture before deciding how to respond.

---

## Step 3: Triage

Categorize all comments before acting on any of them:

| Priority       | What it looks like                          |
| -------------- | ------------------------------------------- |
| **Blocking**   | Bugs, correctness issues, security problems |
| **Required**   | Reviewer explicitly requests a change       |
| **Suggestion** | Style, alternatives, nitpicks               |

Address in that order.

---

## Step 4: Work Through One at a Time

For each comment, blocking first:

1. Re-read the code at the referenced location
2. If the request is unclear — **ask for clarification before implementing**
3. Make the change
4. Reply in the comment thread to close the loop (see Step 5)

Don't batch all changes before replying. Reply as you go so reviewers know what's been addressed.

---

## Step 5: Reply In-Thread

Reply directly on the inline comment thread — not as a top-level PR comment:

```bash
gh api repos/Nimble-Insurance/digital-quoting-platform/pulls/$ARGUMENTS/comments/{comment_id}/replies \
  -f body="Fixed — [brief description of what changed]"
```

For a response to a PR-level review:

```bash
gh pr comment $ARGUMENTS --body "..."
```

---

## Common Mistakes

- **Posting a top-level comment instead of a thread reply** — use the `/replies` endpoint for inline comments
- **Implementing before understanding** — if a comment is unclear, ask first
- **Batching all changes then replying** — reply per comment as you address it
