---
name: shipped-review
description: Verify that shipped summaries and completion notes match the code and test evidence via GitHub PR review.
---

# Shipped Review

Use this skill when the user wants Codex to verify that "done" is actually done.

Primary goal: catch overstated claims and missing caveats in shipped notes.

## Finding the PR

The user will provide a feature ID or PR URL. Find the PR:

```bash
gh pr list --head feature/<feature-id> --json number,url,title,body --jq '.[0]'
```

## Review Context

1. Read the PR and its history:
   ```bash
   gh pr view <pr-number> --json body,title,comments,reviews
   gh pr diff <pr-number>
   ```

2. Read the feature artifacts:
   - `docs/features/<feature-id>/idea.md` — original problem
   - `docs/features/<feature-id>/plan.md` — intended plan
   - `docs/features/<feature-id>/shipped.md` — completion claims

## Review Priorities

- claims in `shipped.md` that are not supported by code changes
- claims of testing that are not actually evidenced
- omitted limitations, pre-existing failures, or follow-up work
- mismatches between `plan.md`, the code, and the final summary

## Output

Post a PR review using `gh`:

```bash
gh pr review <pr-number> --comment --body "## Codex Shipped Review

### Verdict: [PASS / CONDITIONAL PASS / FAIL]

### Critical Findings
- [Unsupported or overstated claims]

### Recommendations
- [Missing caveats or documentation gaps]"
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
