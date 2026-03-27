---
name: implementation-review
description: Review Claude-generated code changes against the intended plan. Use for diffs, working tree reviews, PR-style reviews, and requests to validate an implementation before merge.
---

# Implementation Review

Use this skill when the user wants a second set of eyes on code that Claude already wrote.

Primary goal: find behavioral bugs, regressions, and plan drift.

Review priorities:

- correctness bugs
- broken edge cases
- missing or weak tests
- mismatch between implementation and approved plan
- unsafe refactors or migrations
- docs drift, especially when `shipped.md` or `plan.md` no longer matches the code
- accidental scope expansion

Suggested workflow:

1. Read the intended source of truth if it exists:
   `plan.md`, `idea.md`, issue notes, or user instructions.
2. Inspect the changed files or requested implementation area.
3. Compare what changed against what was supposed to change.
4. Check whether tests exist for the risky paths.
5. Produce a review with findings first, ordered by severity.

Output rules:

- Default to a code review mindset.
- Findings must lead the response.
- Reference specific files and lines when possible.
- Focus on bugs, regressions, and unverified assumptions before style concerns.
- If no findings are present, say that explicitly and mention remaining test or verification gaps.

Good review questions:

- What user-visible behavior changed without matching tests?
- Which code path now depends on an assumption the plan never justified?
- What did the implementation change that the plan did not authorize?
- Is there any claim of completion that is not backed by code or tests?
