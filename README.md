> **⚠️ Work in Progress**
>
> This repository is under active development and is **not ready for external use**.
> The skills, APIs, and documentation may change significantly without notice.
> Please do not depend on this repository until it reaches a stable release.

---

# Skills

AI agent skills for Bkper development. These skills provide procedural knowledge to AI coding assistants (Claude Code, OpenCode, Gemini CLI) when working on Bkper projects.

## What are Skills?

Skills are markdown files that teach AI assistants how to work with specific technologies and patterns. Unlike static project documentation (`AGENTS.md`), skills contain dynamic, procedural knowledge that can be automatically updated.

These Bkper skills are intentionally **docs-first**: each skill stays lightweight and points to public `bkper.com/docs/*.md` pages as the primary source of truth.

## Available Skills

| Skill                      | Description                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------- |
| `bkper-support-specialist` | Broad product/support routing using `https://bkper.com/llms.txt` and docs `.md` pages           |
| `bkper-app-manager`        | App lifecycle operations: init, dev, build, sync, deploy, secrets, and install/uninstall        |
| `bkper-app-dev`            | Platform app implementation references: architecture, configuration, events, menu, and deploy    |
| `bkper-web-dev`            | Web interface references with `@bkper/web-auth`, `@bkper/web-design`, and dev workflow          |
| `bkper-script-dev`         | Automation references for CLI pipelines, Node.js scripts, and direct REST usage                  |

## Distribution

Skills are distributed **globally** to `~/.claude/skills/` and managed automatically by the Bkper CLI.

### Automatic Updates

The CLI checks for updates and syncs all skills when running:

- `bkper app init <name>` - when creating a new app
- `bkper login` - when authenticating

### How It Works

1. CLI fetches the latest commit SHA that touched `skills/` folder via GitHub API
2. Compares with local commit in `~/.config/bkper/skills.yaml`
3. If commit differs (or skills are missing), downloads all `bkper-*` skills
4. Skills are available to all projects via the global location

### Local State

```
~/.claude/skills/
├── bkper-support-specialist/
│   └── SKILL.md
├── bkper-app-manager/
│   └── SKILL.md
├── bkper-app-dev/
│   └── SKILL.md
├── bkper-web-dev/
│   └── SKILL.md
└── bkper-script-dev/
    └── SKILL.md

~/.config/bkper/skills.yaml
└── commit: "abc123..."
```

## Skill Format

Each skill follows the [Agent Skills specification](https://agentskills.io):

```
skills/
└── skill-name/
    └── SKILL.md
```

The `SKILL.md` file contains:

- Procedural knowledge and patterns
- Code examples and best practices

## Contributing

When updating skills:

1. Edit the relevant `SKILL.md` file in `skills/`
2. Commit and push to `main`
3. Changes propagate to users instantly on next CLI command

## Compatibility

Skills are compatible with:

- Claude Code (`~/.claude/skills/`)
- OpenCode (`~/.claude/skills/`)
- Other Agent Skills-compatible tools
