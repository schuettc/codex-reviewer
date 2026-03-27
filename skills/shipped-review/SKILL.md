---
name: shipped-review
description: Verify that shipped summaries and completion notes match the code and test evidence. Use for docs/features/*/shipped.md, release notes, and final completion reviews.
---

# Shipped Review

Use this skill when the user wants Codex to verify that "done" is actually done.

Primary goal: catch overstated claims and missing caveats in shipped notes.

Review priorities:

- claims in `shipped.md` that are not supported by code changes
- claims of testing that are not actually evidenced
- omitted limitations, pre-existing failures, or follow-up work
- mismatches between `plan.md`, the code, and the final summary

Suggested workflow:

1. Read the shipped note or release summary.
2. Read the related plan and implementation context.
3. Validate each concrete claim against the code and available tests.
4. Produce findings first.

Output rules:

- Call out unsupported or overstated claims directly.
- Separate "implemented", "verified", and "documented" in your reasoning.
- If the shipped note is mostly right but incomplete, suggest tighter phrasing.
- If everything checks out, say so and mention any residual caveats.

Good review questions:

- Which sentence in this shipped note is stronger than the evidence supports?
- Was the stated testing actually run, or only planned?
- What caveat would a future maintainer wish had been documented here?
