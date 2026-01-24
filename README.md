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

## Available Skills

| Skill | Description |
|-------|-------------|
| `bkper-app-dev` | Core SDK, bkper-js, event handling, app configuration |
| `bkper-web-dev` | Web development, @bkper/web-* packages, Lit components, auth |
| `bkper-script-dev` | Local scripting, CLI usage, bulk operations |

## Distribution

Skills are distributed **globally** to `~/.claude/skills/` and managed automatically by the Bkper CLI.

### Automatic Updates

The CLI checks for updates and syncs all skills when running:
- `bkper apps init <name>` - when creating a new app
- `bkper mcp start` - when starting the MCP server

### How It Works

1. CLI fetches `version.txt` from this repository
2. Compares with local version in `~/.config/bkper/skills.yaml`
3. If version differs (or skills are missing), downloads all `bkper-*` skills
4. Skills are available to all projects via the global location

### Version Tracking

```
~/.claude/skills/
├── bkper-app-dev/
│   └── SKILL.md
├── bkper-web-dev/
│   └── SKILL.md
└── bkper-script-dev/
    └── SKILL.md

~/.config/bkper/skills.yaml
└── version: 1
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
3. GitHub Action auto-increments `version.txt`
4. Changes propagate to users on next CLI command

## Compatibility

Skills are compatible with:
- Claude Code (`~/.claude/skills/`)
- OpenCode (`~/.claude/skills/`)
- Other Agent Skills-compatible tools
