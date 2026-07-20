# Contributing

Thanks for your interest in contributing to the Cadence Claude Code plugin!

## Where to make changes

Skill content (the `SKILL.md` and everything under `knowledge/`) is maintained in the canonical upstream repo: [`cadence-workflow/ai-skills`](https://github.com/cadence-workflow/ai-skills). **Changes to skill content must be made there, not in this repo.**

This repo (`claude-code-plugin`) only packages that content for distribution as a Claude Code plugin. The `skills/` directory here is a vendored copy that is refreshed automatically (see below). Edits made directly to `skills/` in this repo will be overwritten on the next sync.

Changes here should be limited to:

- Plugin and marketplace configuration (`.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`).
- The sync tooling (`scripts/sync-skills.sh`, `.github/workflows/sync-skills.yml`).
- Repo docs (`README.md`, this file).

## Making a change

### Skill content

1. Open a PR against [`cadence-workflow/ai-skills`](https://github.com/cadence-workflow/ai-skills).
2. Once merged and released upstream, this plugin picks it up via the sync workflow.

### Plugin configuration or tooling

1. Fork or clone this repo.
2. Create a feature branch from `main`.
3. Make your changes.
4. Open a PR targeting `main`.

## Syncing skills manually

To pull the latest skill content yourself:

```bash
# Latest upstream release:
./scripts/sync-skills.sh

# A specific tag, branch, or commit:
./scripts/sync-skills.sh v0.6.2
```

The script refreshes `skills/`, updates `.claude-plugin/skill-source.json`, and leaves the changes staged for you to review and commit. It does not bump the plugin version — bump `version` in `.claude-plugin/plugin.json` (and the matching entry in `.claude-plugin/marketplace.json`) in the same PR when appropriate.

The same script runs in CI via the **Sync skills from ai-skills** workflow, which opens the sync PR for you (triggered manually or by an upstream release).

## Common pitfalls

- **Editing `skills/` directly.** That directory is a vendored copy of the
  upstream skill. Any change you make here is overwritten on the next sync. Make
  skill content changes in [`ai-skills`](https://github.com/cadence-workflow/ai-skills) instead.
- **Expecting the release trigger to work without a token.** The `repository_dispatch`
  trigger fires from the `ai-skills` repo, which needs a `PLUGIN_DISPATCH_TOKEN`
  secret with dispatch access to this repo. The default `GITHUB_TOKEN` cannot
  dispatch across repos. Until that secret is configured, use the manual
  `workflow_dispatch` run instead.
- **Forgetting to bump the plugin version.** The sync script intentionally does
  not touch `version` in `.claude-plugin/plugin.json`. Bump it (and the version
  in `.claude-plugin/marketplace.json`) in the sync PR when the change warrants
  a new plugin release.
- **Misplacing component folders.** `skills/`, `agents/`, `hooks/`, and other
  component directories must live at the plugin root, not inside
  `.claude-plugin/`. Only `plugin.json`, `marketplace.json`, and
  `skill-source.json` belong there.

## Questions?

Open an issue on this repo for plugin/packaging questions, or on [`ai-skills`](https://github.com/cadence-workflow/ai-skills) for skill content questions.
