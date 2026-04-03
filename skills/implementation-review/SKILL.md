---
name: implementation-review
description: Review Claude-generated code changes against the intended plan via GitHub PR review. Use for PR reviews, diff reviews, and requests to validate an implementation before merge.
---

# Implementation Review

Use this skill when the user wants a second set of eyes on code that Claude already wrote.

Primary goal: find behavioral bugs, regressions, and plan drift.

## Finding the PR

The user will provide a feature ID or PR URL. Find the PR:

```bash
gh pr list --head feature/<feature-id> --json number,url,title,body --jq '.[0]'
```

## Review Context

1. Read the PR description for what was done, why, and areas of concern:
   ```bash
   gh pr view <pr-number> --json body,title,additions,deletions,changedFiles
   ```

2. Read the full diff:
   ```bash
   gh pr diff <pr-number>
   ```

3. Read the feature artifacts:
   - `docs/features/<feature-id>/idea.md` — original problem
   - `docs/features/<feature-id>/plan.md` — intended plan

## Review Priorities

- correctness bugs
- broken edge cases
- missing or weak tests
- mismatch between implementation and approved plan
- unsafe refactors or migrations
- docs drift, especially when `plan.md` no longer matches the code
- accidental scope expansion
- areas of concern flagged in the PR description

## Output

Post a PR review using `gh`:

```bash
gh pr review <pr-number> --comment --body "## Codex Implementation Review

### Verdict: [PASS / CONDITIONAL PASS / FAIL]

### Critical Findings
- [Blocking issues ordered by severity]

### Recommendations
- [Non-blocking suggestions]

### Areas of Concern Response
- [Direct response to concerns flagged in PR description]"
```

For specific code issues, post inline comments:

```bash
gh api repos/{owner}/{repo}/pulls/<pr-number>/comments \
  --method POST \
  --field body="[comment]" \
  --field commit_id="$(gh pr view <pr-number> --json headRefOid --jq '.headRefOid')" \
  --field path="[file]" \
  --field line=[line]
```

## Output Rules

- Findings must lead the response.
- Reference specific files and lines when possible.
- Focus on bugs, regressions, and unverified assumptions before style concerns.
- If no findings are present, say that explicitly and mention remaining test or verification gaps.

## Good Review Questions

- What user-visible behavior changed without matching tests?
- Which code path now depends on an assumption the plan never justified?
- What did the implementation change that the plan did not authorize?
- Is there any claim of completion that is not backed by code or tests?
