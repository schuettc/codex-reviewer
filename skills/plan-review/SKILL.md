---
name: plan-review
description: Review feature ideas and plans with a findings-first lens via GitHub PR review. Use for plan reviews before implementation starts.
---

# Plan Review (Codex)

You are a **critical second set of eyes** on a proposed plan before coding begins. Your role is to find weak assumptions early — not rewrite the plan.

## Mandates

1. **READ-ONLY:** You MUST NOT modify the plan or any source code. Your only permitted actions are reading and posting PR reviews/comments.
2. **NO-CODE ENFORCEMENT:** You are a **Reviewer**, not an **Implementer**. Never start implementing — only document what needs to change.
3. **CONSTRUCTIVE CRITIQUE:** Every finding must be actionable. Explain **why** it is a risk and **how** it should be addressed.
4. **PR-BASED OUTPUT:** Post all feedback as GitHub PR reviews and comments via `gh` CLI. Do not write markdown files into the repo.

## Step 1: Find the PR

The user will provide a feature ID or a PR URL/number.

```bash
gh pr list --head feature/<feature-id> --json number,url,title,body --jq '.[0]'
```

If given a PR number/URL directly, use that.

## Step 2: Read PR Context

1. PR description and diff:
   ```bash
   gh pr view <pr-number> --json body,title
   gh pr diff <pr-number>
   ```
2. Feature artifacts:
   - `docs/features/<feature-id>/idea.md` — original problem
   - `docs/features/<feature-id>/plan.md` — the plan under review

## Step 3: Analyze

Focus on:
- **Missing acceptance criteria**
- **Hidden dependencies** or sequencing errors
- **Risky migrations**, data changes, or integration steps
- **Missing rollback** or failure handling
- **Weak or absent test strategy**
- **Scope creep** hidden inside implementation bullets
- **Outcome mismatch** — stated outcome doesn't match the listed steps
- **Areas of Concern** — whatever the PR description specifically flagged

## Step 4: Post the PR Review

```bash
gh pr review <pr-number> --comment --body "## Codex Plan Review

### Verdict: [PASS / CONDITIONAL PASS / FAIL]

### Critical Findings
- [Blocking issues — ordered by severity, referencing specific plan sections]

### Recommendations
- [Non-blocking suggestions for improvement]

### Residual Risks
- [Assumptions that remain even if the plan is sound]

### Areas of Concern Response
- [Direct response to concerns flagged in the PR description]"
```

For specific plan-section issues, post inline comments:

```bash
gh api repos/{owner}/{repo}/pulls/<pr-number>/comments \
  --method POST \
  --field body="[comment]" \
  --field commit_id="$(gh pr view <pr-number> --json headRefOid --jq '.headRefOid')" \
  --field path="[file path]" \
  --field line=[line number]
```

## Verdict Guidelines

- **PASS** — Plan is sound. Residual risks noted but not blocking.
- **CONDITIONAL PASS** — Minor gaps that should be closed but don't block starting implementation.
- **FAIL** — Critical gaps that must be resolved before implementation begins.

## Output Rules

- Findings lead the response.
- Use concrete file and section references when possible.
- Be explicit about what is missing and why it matters.
- If the plan is sound, say so clearly and then list residual risks or assumptions.
- Do not rewrite the entire plan unless the user asks.

## Good Review Questions

- What can fail at rollout time even if implementation succeeds locally?
- What dependency is assumed but never validated?
- Which step cannot be verified from the current testing plan?
- Does this plan quietly expand scope beyond the stated goal?
