# Bkper Skills (deprecated)

This repository is deprecated and should be archived.

The Bkper CLI skill now lives with the Bkper CLI, because it is generated from the CLI agent configuration and is meant to help external coding agents operate the local `bkper` CLI safely.

## Install from the new source

```bash
npx skills add bkper/bkper-cli --skill bkper-cli
```

Install and authenticate the CLI first when the agent needs live Bkper access:

```bash
npm i -g bkper
bkper auth login
```

## Migrating from the old repo

If you installed the old `bkper` or `bkper-cli` skill from this repository, remove it and reinstall from `bkper/bkper-cli`:

```bash
npx skills remove bkper
npx skills remove bkper-cli
npx skills add bkper/bkper-cli --skill bkper-cli
```

## General Bkper context

For general Bkper Q&A without local CLI/tool access, use the published Markdown docs instead:

```text
https://bkper.com/llms.txt
https://bkper.com/docs/core-concepts.md
```
