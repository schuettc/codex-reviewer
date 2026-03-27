---
name: plan-review
description: Review feature ideas and plans with a findings-first lens. Use for docs/features/*/idea.md, plan.md, .claude/plans/*.md, and similar planning artifacts before implementation starts.
---

# Plan Review

Use this skill when the user wants Codex to review a proposed plan before coding begins.

Primary goal: find weak assumptions early, not rewrite the whole plan.

Review priorities:

- missing acceptance criteria
- hidden dependencies or sequencing errors
- risky migrations, data changes, or integration steps
- missing rollback or failure handling
- weak or absent test strategy
- scope creep hidden inside implementation bullets
- mismatch between the stated outcome and the listed steps

Suggested workflow:

1. Read the plan file the user named.
2. Read adjacent context only as needed:
   `idea.md`, `shipped.md`, `CLAUDE.md`, design docs, or touched code.
3. Identify what the plan claims it will change.
4. Check whether each major claim has:
   scope boundaries, implementation steps, verification, and risk handling.
5. Produce a review with findings first, ordered by severity.

Output rules:

- Findings are the main deliverable.
- Use concrete file references when possible.
- Be explicit about what is missing and why it matters.
- If the plan is sound, say so clearly and then list residual risks or assumptions.
- Do not rewrite the entire plan unless the user asks.

Good review questions:

- What can fail at rollout time even if implementation succeeds locally?
- What dependency is assumed but never validated?
- Which step cannot be verified from the current testing plan?
- Does this plan quietly expand scope beyond the stated goal?
