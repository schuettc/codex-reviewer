---
name: shipped-review
description: Verify that shipped summaries and completion notes match the code and test evidence. Use for docs/features/*/shipped.md, release notes, and final completion reviews.
---

# Shipped Review

Use this skill when the user wants Codex to verify that "done" is actually done.

Primary goal: catch overstated claims and missing caveats in shipped notes.

## Review Context

Before reviewing, check for implementer-provided context:

1. Find the latest `docs/features/<feature-id>/reviews/context-round-N.md` file
2. Read it for any final context about what was shipped
3. Determine the current round number N from the latest context file
4. If no context file exists, determine N by counting existing `codex-review-round-*.md` files + 1

## Review Priorities

- claims in `shipped.md` that are not supported by code changes
- claims of testing that are not actually evidenced
- omitted limitations, pre-existing failures, or follow-up work
- mismatches between `plan.md`, the code, and the final summary

## Suggested Workflow

1. Read the review context file (`context-round-N.md`) if it exists.
2. Read the shipped note or release summary.
3. Read the related plan and implementation context.
4. Validate each concrete claim against the code and available tests.
5. Produce findings first.

## Output

Write the review to `docs/features/<feature-id>/reviews/codex-review-round-N.md` with this format:

```markdown
---
round: N
reviewer: codex
verdict: [pass|conditional-pass|fail]
reviewed: YYYY-MM-DD HH:MM:SS
---

# Codex Shipped Review: Round N — [Feature Name]

## Verdict: [PASS / CONDITIONAL PASS / FAIL]

## Critical Findings
- [Unsupported or overstated claims]

## Recommendations
- [Missing caveats or documentation gaps]
```

## Output Rules

- Call out unsupported or overstated claims directly.
- Separate "implemented", "verified", and "documented" in your reasoning.
- If the shipped note is mostly right but incomplete, suggest tighter phrasing.
- If everything checks out, say so and mention any residual caveats.

## Good Review Questions

- Which sentence in this shipped note is stronger than the evidence supports?
- Was the stated testing actually run, or only planned?
- What caveat would a future maintainer wish had been documented here?
