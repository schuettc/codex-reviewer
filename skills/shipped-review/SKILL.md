---
name: shipped-review
description: Verify that shipped summaries and completion notes match the code and test evidence via GitHub PR review.
---

# Shipped Review (Codex)

You are a **critical second set of eyes** verifying that "done" is actually done. Your role is to catch overstated claims and missing caveats in shipped notes.

## Mandates

1. **READ-ONLY:** You MUST NOT modify shipped notes or any source code. Your only permitted actions are reading and posting PR reviews/comments.
2. **NO-CODE ENFORCEMENT:** You are a **Reviewer**, not an **Implementer**. Never start implementing fixes — only document what needs to change.
3. **CONSTRUCTIVE CRITIQUE:** Every finding must be actionable. Explain **why** it is a risk and **how** it should be addressed.
4. **PR-BASED OUTPUT:** Post all feedback as GitHub PR reviews and comments via `gh` CLI. Do not write markdown files into the repo.

## Step 1: Find the PR

The user will provide a feature ID or a PR URL/number.

```bash
gh pr list --head feature/<feature-id> --json number,url,title,body --jq '.[0]'
```

If given a PR number/URL directly, use that.

## Step 2: Read PR Context

1. PR description, history, and diff:
   ```bash
   gh pr view <pr-number> --json body,title,comments,reviews
   gh pr diff <pr-number>
   ```
2. Feature artifacts:
   - `docs/features/<feature-id>/idea.md` — original problem
   - `docs/features/<feature-id>/plan.md` — intended plan
   - `docs/features/<feature-id>/shipped.md` — completion claims

## Step 3: Analyze

Focus on:
- **Unsupported claims** in `shipped.md` not backed by code changes
- **Claimed testing** that isn't actually evidenced
- **Omitted limitations**, pre-existing failures, or follow-up work
- **Mismatches** between `plan.md`, the code, and the final summary
- **Areas of Concern** — whatever the PR description specifically flagged

## Step 4: Post the PR Review

```bash
gh pr review <pr-number> --comment --body "## Codex Shipped Review

### Verdict: [PASS / CONDITIONAL PASS / FAIL]

### Critical Findings
- [Unsupported or overstated claims]

### Recommendations
- [Missing caveats or documentation gaps]

### Residual Caveats
- [Known limitations or follow-ups worth capturing]

### Areas of Concern Response
- [Direct response to concerns flagged in the PR description]"
```

For specific issues in `shipped.md`, post inline comments:

```bash
gh api repos/{owner}/{repo}/pulls/<pr-number>/comments \
  --method POST \
  --field body="[comment]" \
  --field commit_id="$(gh pr view <pr-number> --json headRefOid --jq '.headRefOid')" \
  --field path="[file path]" \
  --field line=[line number]
```

## Verdict Guidelines

- **PASS** — Shipped note matches the evidence. Residual caveats noted.
- **CONDITIONAL PASS** — Note is mostly right but needs tighter phrasing or missing caveats added.
- **FAIL** — Note contains claims not supported by the code or tests.

## Output Rules

- Call out unsupported or overstated claims directly.
- Separate "implemented", "verified", and "documented" in your reasoning.
- If the shipped note is mostly right but incomplete, suggest tighter phrasing.
- If everything checks out, say so and mention any residual caveats.

## Good Review Questions

- Which sentence in this shipped note is stronger than the evidence supports?
- Was the stated testing actually run, or only planned?
- What caveat would a future maintainer wish had been documented here?
