# Cadence Plugin for Claude Code

This repository provides a [Claude Code plugin](https://code.claude.com/docs/en/plugins) for working with [Cadence](https://cadenceworkflow.io/) — the fault-tolerant, stateful workflow orchestration platform. It packages the `cadence-developer` Agent Skill for distribution through Claude Code.

> **Status:** Early development. The bundled skill targets the Go, Java, and Python SDKs (the Python SDK is alpha, so its coverage calls out gaps explicitly).

## Installation

### Via the plugin marketplace (recommended)

This repo is itself a single-plugin marketplace, so no separate marketplace repo is needed.

1. Add the marketplace:

   ```
   /plugin marketplace add cadence-workflow/claude-code-plugin
   ```

2. Install the plugin:

   ```
   /plugin install cadence@cadence
   ```

3. Restart Claude Code, or run `/reload-plugins`.

### Local development

To try changes locally without installing from the marketplace:

```bash
git clone https://github.com/cadence-workflow/claude-code-plugin.git
claude --plugin-dir ./claude-code-plugin
```

Run `/reload-plugins` inside a session to pick up further changes without restarting.

## What's included

- **cadence-developer** skill — Comprehensive guidance for building, debugging, and operating Cadence applications: creating workflows, activities, and workers; handling signals, queries, and child workflows; debugging non-determinism errors; and implementing saga, versioning, and testing patterns across the Go, Java, and Python SDKs. Once installed, it's invoked automatically by Claude or manually via `/cadence:cadence-developer`.

## Standalone skill

The skill content is maintained upstream in [`cadence-workflow/ai-skills`](https://github.com/cadence-workflow/ai-skills). If you only need the skill without the Claude Code plugin wrapper, you can install it directly from that repo (for example with `npx skills add cadence-workflow/ai-skills`).

## How skill content stays in sync

The `skills/` directory in this repo is a **vendored copy** of the upstream skill. It is updated automatically:

- A maintainer can run the **Sync skills from ai-skills** workflow manually
  (`workflow_dispatch`), optionally pinning a specific upstream ref.
- A new release in `ai-skills` fires a `repository_dispatch` event that triggers
  the same workflow.

Either path runs [`scripts/sync-skills.sh`](scripts/sync-skills.sh), which fetches the upstream content, refreshes `skills/`, records provenance in [`.claude-plugin/skill-source.json`](.claude-plugin/skill-source.json), and opens a pull request for review.

Do not hand-edit files under `skills/` in this repo — they are overwritten on the next sync. See [common pitfalls](CONTRIBUTING.md#common-pitfalls) in the contributing guide.

## Other coding agents

Plugins for other coding agents follow this same template, each vendoring the skill from `ai-skills`. See [`cursor-plugin`](https://github.com/cadence-workflow/cursor-plugin) for the Cursor equivalent.

## License

Apache 2.0 — see [`LICENSE`](LICENSE).
