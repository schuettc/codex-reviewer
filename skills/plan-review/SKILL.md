---
name: plan-review
description: Review feature ideas and plans with a findings-first lens. Use for docs/features/*/idea.md, plan.md, .claude/plans/*.md, and similar planning artifacts before implementation starts.
---

# Plan Review

Use this skill when the user wants Codex to review a proposed plan before coding begins.

Primary goal: find weak assumptions early, not rewrite the whole plan.

## Review Context

Before reviewing, check for implementer-provided context:

1. Find the latest `docs/features/<feature-id>/reviews/context-round-N.md` file
2. Read it to understand the implementer's rationale and any areas of concern
3. Determine the current round number N from the latest context file
4. If no context file exists, determine N by counting existing `codex-review-round-*.md` files + 1

## Review Priorities

- missing acceptance criteria
- hidden dependencies or sequencing errors
- risky migrations, data changes, or integration steps
- missing rollback or failure handling
- weak or absent test strategy
- scope creep hidden inside implementation bullets
- mismatch between the stated outcome and the listed steps

## Suggested Workflow

1. Read the review context file (`context-round-N.md`) if it exists.
2. Read the plan file the user named.
3. Read adjacent context only as needed:
   `idea.md`, `shipped.md`, design docs, or touched code.
4. Identify what the plan claims it will change.
5. Check whether each major claim has:
   scope boundaries, implementation steps, verification, and risk handling.
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

# Codex Plan Review: Round N — [Feature Name]

## Verdict: [PASS / CONDITIONAL PASS / FAIL]

## Critical Findings
- [Blocking issues ordered by severity]

## Recommendations
- [Non-blocking suggestions]

## Areas of Concern Response
- [Direct response to implementer's flagged concerns, if any]
```

## Output Rules

- Findings are the main deliverable.
- Use concrete file references when possible.
- Be explicit about what is missing and why it matters.
- If the plan is sound, say so clearly and then list residual risks or assumptions.
- Do not rewrite the entire plan unless the user asks.

## Good Review Questions

- What can fail at rollout time even if implementation succeeds locally?
- What dependency is assumed but never validated?
- Which step cannot be verified from the current testing plan?
- Does this plan quietly expand scope beyond the stated goal?
