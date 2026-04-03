---
name: plan-review
description: Review feature ideas and plans with a findings-first lens via GitHub PR review. Use for plan reviews before implementation starts.
---

# Plan Review

Use this skill when the user wants Codex to review a proposed plan before coding begins.

Primary goal: find weak assumptions early, not rewrite the whole plan.

## Finding the PR

The user will provide a feature ID or PR URL. Find the PR:

```bash
gh pr list --head feature/<feature-id> --json number,url,title,body --jq '.[0]'
```

## Review Context

1. Read the PR description and diff:
   ```bash
   gh pr view <pr-number> --json body,title
   gh pr diff <pr-number>
   ```

2. Read the feature artifacts:
   - `docs/features/<feature-id>/idea.md` — original problem
   - `docs/features/<feature-id>/plan.md` — the plan under review

## Review Priorities

- missing acceptance criteria
- hidden dependencies or sequencing errors
- risky migrations, data changes, or integration steps
- missing rollback or failure handling
- weak or absent test strategy
- scope creep hidden inside implementation bullets
- mismatch between the stated outcome and the listed steps

## Output

Post a PR review using `gh`:

```bash
gh pr review <pr-number> --comment --body "## Codex Plan Review

### Verdict: [PASS / CONDITIONAL PASS / FAIL]

### Critical Findings
- [Blocking issues ordered by severity]

### Recommendations
- [Non-blocking suggestions]"
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
