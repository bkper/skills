> **⚠️ Work in Progress**
>
> This repository is under active development and is **not ready for external use**.
> The skills, APIs, and documentation may change significantly without notice.
> Please do not depend on this repository until it reaches a stable release.

---

# Skills

AI agent skills for Bkper development. These skills provide procedural knowledge to AI coding assistants (Claude Code, OpenCode, Gemini CLI) when working on Bkper projects.

## What are Skills?

Skills are self-contained capability packages that teach AI assistants how to work with specific technologies and patterns. Unlike static project documentation (`AGENTS.md`), skills contain procedural knowledge that can be loaded on-demand.

This repository contains a **single unified skill** derived from the validated `bkper-cli` agent configuration. It replaces the previous collection of theoretical, URL-only skills with one self-contained package that bundles all reference documentation.

## Available Skill

| Skill        | Description                                                                                                          |
| ------------ | -------------------------------------------------------------------------------------------------------------------- |
| `bkper-dev`  | Comprehensive Bkper development: CLI usage, SDK code (bkper-js), data management, financial reporting, app lifecycle |

## How This Skill Is Generated

The `bkper-dev` skill is **auto-generated** from the `bkper-cli` agent source. It is not hand-edited.

```
bkper-cli/
├── src/agent/system-prompt.ts   ← Bkper context, operating principles, routing
├── docs/*.md                    ← Reference documentation (core-concepts, data-management, etc.)
└── scripts/generate-skill.ts    ← Derives ../skills/skills/bkper-dev/
```

When the CLI agent evolves:

1. Edit `src/agent/system-prompt.ts` or `docs/*.md` in `bkper-cli`
2. Run `bun run generate:skill`
3. The test suite validates the output
4. Commit both repositories

This keeps the skill in sync with the **actually validated** agent behavior, instead of drifting into theory.

## Skill Structure

```
skills/
└── bkper-dev/
    ├── SKILL.md              ← Frontmatter + Bkper context + routing instructions
    └── references/           ← Bundled docs (self-contained, no remote fetches needed)
        ├── core-concepts.md
        ├── index.md
        ├── data-management.md
        ├── app-management.md
        ├── financial-statements.md
        ├── bkper-js.md
        └── bkper-api-types.md
```

## Canonical Experience vs. Skill Fallback

| Capability                  | `bkper agent` (CLI)     | `bkper-dev` skill (other agents) |
| --------------------------- | ----------------------- | -------------------------------- |
| System prompt               | Always in context       | Loaded on-demand via description |
| Core concepts enforcement   | Runtime tool blocking   | Plain-text instruction only      |
| Doc paths                   | Resolved at runtime     | Relative `references/*.md`       |
| Startup banner / maintenance | CLI harness hooks       | Not available                    |

The `bkper agent` CLI mode is the **canonical validated path** because it has runtime enforcement (e.g. blocking non-read tools until `core-concepts.md` is loaded). The `bkper-dev` skill is a **best-effort fallback** for agents that cannot run the CLI harness.

## Distribution

There is no automatic distribution mechanism yet. To use this skill:

1. Clone or pull this repository
2. Copy `skills/bkper-dev/` to your agent's skills directory:
   - Claude Code: `~/.claude/skills/bkper-dev/`
   - OpenCode: `~/.claude/skills/bkper-dev/`
   - pi: `.pi/skills/bkper-dev/` (project-local) or `~/.pi/agent/skills/bkper-dev/` (global)

## Contributing

**Do not edit `SKILL.md` or files under `references/` directly.** They are overwritten by the generator.

If you want to change the skill content, edit the source in `bkper-cli` instead:

- `src/agent/system-prompt.ts` for context, principles, and routing
- `docs/*.md` for reference documentation

Then run `bun run generate:skill` in the CLI repo and commit both repositories.

## Skill Format

Follows the [Agent Skills specification](https://agentskills.io):

- `SKILL.md` with YAML frontmatter (`name`, `description`)
- Relative paths resolved against the skill directory
- Bundled assets in subdirectories (e.g. `references/`)

## Compatibility

Compatible with any agent that implements the Agent Skills standard:

- Claude Code
- OpenCode
- pi (`/skill:name` commands)
- Other Agent Skills-compatible tools
