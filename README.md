# Codex Reviewer

Codex plugin for running a second-pass review over Claude-driven work.

This plugin is designed for a doc-driven workflow:

- Claude captures or updates `idea.md`, `plan.md`, and `shipped.md`.
- Claude implements the code change.
- Codex reviews the artifacts and the implementation before you treat the work as complete.

The plugin ships three focused skills:

- `plan-review`: review ideas and plans for missing scope, hidden dependencies, sequencing mistakes, and weak test strategy
- `implementation-review`: review code changes against the approved plan and call out bugs, regressions, and missing tests
- `shipped-review`: verify that `shipped.md` matches the code and test evidence instead of overstating what landed

Examples:

```text
Use plan-review on docs/features/my-feature/plan.md.
```

```text
Use implementation-review on the current changes and compare them to the approved plan.
```

```text
Use shipped-review on docs/features/my-feature/shipped.md and verify the claims against the codebase.
```

Recommended operating model:

1. Let Claude drive ideation, drafting, and first-pass implementation.
2. Invoke Codex Reviewer before merge, especially on risky plans or broad refactors.
3. Treat findings as quality gates. Fix or consciously waive them.

## Repository Layout

This repository is already laid out as a standalone plugin:

```text
.codex-plugin/plugin.json
skills/plan-review/SKILL.md
skills/implementation-review/SKILL.md
skills/shipped-review/SKILL.md
```
