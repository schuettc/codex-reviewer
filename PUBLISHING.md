# Publishing Codex Reviewer

This plugin can be published as a standalone GitHub repository.

## Repository shape

This repository is already in the correct standalone shape:

```text
codex-reviewer/
├── .codex-plugin/
│   └── plugin.json
├── skills/
│   ├── implementation-review/
│   │   └── SKILL.md
│   ├── plan-review/
│   │   └── SKILL.md
│   └── shipped-review/
│       └── SKILL.md
├── LICENSE
├── README.md
└── PUBLISHING.md
```

## Push to GitHub

Add a remote and push normally:

```bash
git remote add origin git@github.com:schuettc/codex-reviewer.git
git branch -M main
git push -u origin main
```

## Before pushing

- Update `.codex-plugin/plugin.json` if you choose a different GitHub owner or repo name.
- Add assets later if you want a branded icon or screenshots.
