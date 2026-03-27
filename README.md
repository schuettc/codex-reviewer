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

## Install

Codex supports two practical local install patterns for plugins:

- a personal marketplace at `~/.agents/plugins/marketplace.json`
- a repo marketplace at `$REPO_ROOT/.agents/plugins/marketplace.json`

For most users, the personal marketplace is the right default.

### Personal install

1. Clone this repository somewhere local:

```bash
git clone git@github.com:schuettc/codex-reviewer.git
```

2. Copy it into your Codex plugins directory:

```bash
mkdir -p ~/.codex/plugins
cp -R ./codex-reviewer ~/.codex/plugins/codex-reviewer
```

3. Create or update `~/.agents/plugins/marketplace.json`:

```json
{
  "name": "local-personal",
  "interface": {
    "displayName": "Local Personal"
  },
  "plugins": [
    {
      "name": "codex-reviewer",
      "source": {
        "source": "local",
        "path": "./plugins/codex-reviewer"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

4. Restart Codex.

5. In Codex CLI, open the plugin surface:

```text
codex
/plugins
```

### Repo install

If you want the plugin available only for one repository:

1. Copy this repo into your project under `$REPO_ROOT/plugins/codex-reviewer`
2. Create or update `$REPO_ROOT/.agents/plugins/marketplace.json`
3. Point the plugin entry at `./plugins/codex-reviewer`
4. Restart Codex

Example repo marketplace:

```json
{
  "name": "local-repo",
  "interface": {
    "displayName": "Local Repo"
  },
  "plugins": [
    {
      "name": "codex-reviewer",
      "source": {
        "source": "local",
        "path": "./plugins/codex-reviewer"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

### Updating

After you change the plugin, update the plugin directory your marketplace points to and restart Codex so it reloads the local install.

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
