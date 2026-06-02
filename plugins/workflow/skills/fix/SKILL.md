---
name: fix
description: Use this skill when implementation has gone wrong — Developer subagents failed to achieve the correct result, or an existing bug needs solving. Reviews the work, identifies where the workflow went wrong, verifies the correct approach with authoritative sources, fixes the code, and updates memory to prevent the mistake from recurring.
---

# Fix

Invoked when implementation has gone wrong — Developer subagents failed to achieve the correct result, or an existing bug needs solving. Reviews the work, identifies where the workflow went wrong, verifies the correct approach with authoritative sources, fixes the code, and updates memory to prevent the mistake from recurring.

Read `principles.md` in the workflow plugin root and follow it throughout.

## Process

1. **Review the work** — examine what was implemented vs. the expected result; read the relevant code changes, test results, error messages, and logs; identify the gap between expected and actual behavior.
2. **Diagnose the root cause** — determine *where* the workflow went wrong: architecture/design (wrong approach), implementation (right approach, wrong code), configuration (right code, wrong setup), or dependencies (external issues).
3. **Verify with sources** — before fixing, confirm the correct approach using official documentation, Stack Overflow, and technical forums. Deploy the researcher-claude or researcher-copilot agent for deeper investigation if needed.
4. **Fix the code** — implement the fix following **Minimum Necessary Change** and **No Tech Debt**: change only what's necessary, match existing project patterns, fix the root cause, and run tests to verify.
5. **Update memory** — record what went wrong, why, and the correct approach for this type of problem, so all agents benefit going forward.
6. **Verify** — run the full test suite to ensure no regressions, confirm the original requirement is satisfied, and start the local environment for manual verification if needed.

## Pairs with

- **implement** — the upstream step Fix corrects and returns to.

## Rules

- Always verify findings with external authoritative sources before applying a fix.
- Never guess — diagnose first, then fix.
- Always update memory after a fix so the mistake is not repeated.
- Follow **Minimum Necessary Change** in `principles.md` — fix only what is broken.
- If the root cause is in the architecture/design, consult the Architect agent.
