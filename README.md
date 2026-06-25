# Bkper CLI Skill

Portable Bkper CLI context for external AI coding agents.

Use this when you want Claude Code, Codex, OpenCode, OpenClaw, Hermes Agent, Cursor, or another skill-compatible agent to work with the local `bkper` CLI safely.

## Install

Install the CLI and authenticate first when the agent needs live Bkper access:

```bash
npm i -g bkper
bkper auth login
```

Then install the skill:

```bash
npx skills add bkper/skills --skill bkper-cli
```

This installs `bkper-cli` to your agent's skills directory. Works with Claude Code, pi, Codex, and any agent that supports the [Agent Skills standard](https://agentskills.io).

Previously installed the old `bkper` skill? Remove it and install `bkper-cli` instead:

```bash
npx skills remove bkper
npx skills add bkper/skills --skill bkper-cli
```

## Use

Mention Bkper or the `bkper` CLI in your prompt and the skill loads automatically.

```text
"List transactions for January using the bkper CLI"
"Create draft transactions from this CSV"
"Build and deploy my Bkper app"
```

The skill teaches the agent Bkper's from-to accounting model, routes it to the right reference doc, and adds safety guardrails for CLI operations.

For general Bkper Q&A without local tool access, use the published docs instead:

```text
https://bkper.com/llms.txt
https://bkper.com/docs/core-concepts.md
```

## What it covers

- **CLI** — books, accounts, transactions, groups, balances, queries, collections
- **Apps** — init, dev, build, deploy, secrets
- **SDK references** — bkper-js and REST API types when CLI work needs code
- **Reporting guardrails** — deterministic balance, statement, and tax workflows

## Update

```bash
npx skills update bkper-cli
```
