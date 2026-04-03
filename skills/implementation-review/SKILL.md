---
name: implementation-review
description: Review Claude-generated code changes against the intended plan. Use for diffs, working tree reviews, PR-style reviews, and requests to validate an implementation before merge.
---

# Implementation Review

Use this skill when the user wants a second set of eyes on code that Claude already wrote.

Primary goal: find behavioral bugs, regressions, and plan drift.

## Review Context

Before reviewing, check for implementer-provided context:

1. Find the latest `docs/features/<feature-id>/reviews/context-round-N.md` file
2. Read it to understand what was implemented, why, and what areas need focus
3. Determine the current round number N from the latest context file
4. If no context file exists, determine N by counting existing `codex-review-round-*.md` files + 1

## Review Priorities

- correctness bugs
- broken edge cases
- missing or weak tests
- mismatch between implementation and approved plan
- unsafe refactors or migrations
- docs drift, especially when `shipped.md` or `plan.md` no longer matches the code
- accidental scope expansion
- areas of concern flagged by the implementer in the context file

## Suggested Workflow

1. Read the review context file (`context-round-N.md`) if it exists.
2. Read the intended source of truth:
   `plan.md`, `idea.md`, issue notes, or user instructions.
3. Inspect the changed files or requested implementation area.
4. Compare what changed against what was supposed to change.
5. Check whether tests exist for the risky paths.
6. Produce a review with findings first, ordered by severity.

## Output

Write the review to `docs/features/<feature-id>/reviews/codex-review-round-N.md` with this format:

```markdown
---
round: N
reviewer: codex
verdict: [pass|conditional-pass|fail]
reviewed: YYYY-MM-DD HH:MM:SS
---

# Codex Review: Round N — [Feature Name]

## Verdict: [PASS / CONDITIONAL PASS / FAIL]

## Critical Findings
- [Blocking issues ordered by severity]

## Recommendations
- [Non-blocking suggestions]

## Areas of Concern Response
- [Direct response to implementer's flagged concerns, if any]
```

## Output Rules

- Findings must lead the response.
- Reference specific files and lines when possible.
- Focus on bugs, regressions, and unverified assumptions before style concerns.
- If no findings are present, say that explicitly and mention remaining test or verification gaps.

## Verdict Guidelines

- **PASS**: No critical issues. Implementation is solid.
- **CONDITIONAL PASS**: Minor issues that should be addressed but don't block progress.
- **FAIL**: Critical issues that must be resolved.

## Good Review Questions

- What user-visible behavior changed without matching tests?
- Which code path now depends on an assumption the plan never justified?
- What did the implementation change that the plan did not authorize?
- Is there any claim of completion that is not backed by code or tests?
