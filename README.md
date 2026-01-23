# Skills

AI agent skills for Bkper development. These skills provide procedural knowledge to AI coding assistants (Claude Code, OpenCode, Gemini CLI) when working on Bkper projects.

## What are Skills?

Skills are markdown files that teach AI assistants how to work with specific technologies and patterns. Unlike static project documentation (`AGENTS.md`), skills contain dynamic, procedural knowledge that can be automatically updated across projects.

## Available Skills

| Skill | Description |
|-------|-------------|
| `bkper-app-dev` | Core SDK, bkper-js, event handling, app configuration |
| `bkper-web-dev` | Web development, @bkper/web-* packages, Lit components, auth |
| `bkper-script-dev` | Local scripting, CLI usage, bulk operations |

## Usage

### In Bkper App Projects

Skills are automatically installed when you create a new app with `bkper init`. They are stored in `.claude/skills/` and configured in `bkperapp.yaml`:

```yaml
# bkperapp.yaml
skills:
  autoUpdate: true
  installed:
    - bkper-app-dev
    - bkper-web-dev
```

### Manual Installation

Copy the desired skill folder to your project:

```bash
cp -r skills/bkper-app-dev .claude/skills/
```

### Auto-Update

When `autoUpdate: true` (default), the Bkper CLI will check for skill updates when running `bkper dev` and sync them automatically.

To disable auto-update:

```yaml
skills:
  autoUpdate: false
```

## Skill Format

Each skill follows the [Agent Skills specification](https://agentskills.io):

```
skills/
└── skill-name/
    └── SKILL.md
```

The `SKILL.md` file contains:
- Skill metadata (name, description, version)
- Procedural knowledge and patterns
- Code examples and best practices

## Contributing

When updating skills:

1. Edit the relevant `SKILL.md` file
2. Update the version in the skill metadata
3. Test with a sample project
4. Commit and push

Changes will propagate to projects with `autoUpdate: true` on their next `bkper dev` run.

## Compatibility

Skills are compatible with:
- Claude Code (`.claude/skills/`)
- OpenCode (`.claude/skills/` or `.opencode/skills/`)
- Other Agent Skills-compatible tools
