# Bkper Skills

Skills for AI coding assistants working with Bkper.

## Install

```bash
npx skills add bkper/skills --skill bkper
```

This installs `bkper` to your agent's skills directory. Works with Claude Code, pi, Codex, and any agent that supports the [Agent Skills standard](https://agentskills.io).

## Use

Mention Bkper in your prompt and the skill loads automatically.

```
"Create a book with Brazilian settings"
"List transactions for January"
"Build and deploy my Bkper app"
```

The skill tells the agent to read `core-concepts.md` first, then routes to the right reference doc for the task.

## What it covers

- **CLI** — books, accounts, transactions, groups, balances, queries, collections
- **SDK** — bkper-js and REST API types
- **Apps** — init, dev, build, deploy, secrets
- **Reporting** — balance sheets, P&L workflows

## Update

```bash
npx skills update bkper
```
