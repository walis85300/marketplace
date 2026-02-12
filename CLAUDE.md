# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is a **Claude Code plugin marketplace** — a catalog that distributes plugins to Claude Code users via `/plugin marketplace add`. It is NOT a traditional app codebase. There is no build step, no runtime, no tests. The "code" here is Markdown skills, JSON manifests, and shell scripts that Claude Code loads as extensions.

## Repository Structure

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json        ← Marketplace catalog (the root manifest)
└── plugins/
    ├── code-review/            ← Plugin: code review skills + security agent
    ├── git-toolkit/            ← Plugin: git workflow automation + hooks
    └── doc-generator/          ← Plugin: documentation generation agents
```

Each plugin follows this internal structure:
```
plugin-name/
├── .claude-plugin/plugin.json  ← Plugin manifest (name, version, description)
├── skills/                     ← Agent Skills (auto-invoked by Claude based on context)
│   └── skill-name/SKILL.md
├── commands/                   ← User-invoked slash commands (/plugin:command)
│   └── command-name.md
├── agents/                     ← Subagents (specialized Claude personas)
│   └── agent-name.md
├── hooks/                      ← Event handlers (hooks.json)
│   └── hooks.json
└── scripts/                    ← Shell scripts referenced by hooks
```

## Key Rules

- **`.claude-plugin/` only holds manifests.** Never put `commands/`, `agents/`, `skills/`, or `hooks/` inside `.claude-plugin/`. Only `plugin.json` (or `marketplace.json` at root) goes there.
- **All paths in marketplace.json are relative** to the repo root. The `metadata.pluginRoot` field (`./plugins`) is the base for relative source paths.
- **Plugin names must be kebab-case** with no spaces. The name becomes the namespace prefix for skills (e.g., `/code-review:quick-review`).
- **Skills vs Commands**: Skills (`skills/*/SKILL.md`) are auto-invoked by Claude based on task context. Commands (`commands/*.md`) are user-invoked via slash commands. Use skills for capabilities Claude should discover on its own; use commands for explicit user actions.
- **`${CLAUDE_PLUGIN_ROOT}`** — use this env var in hooks and MCP server configs to reference files within the plugin's installed directory. Plugins get copied to a cache on install, so absolute paths break.
- **No path traversal** — plugins cannot reference files outside their own directory (`../` is not allowed). If you need shared files, use symlinks or restructure.

## Validation

```bash
claude plugin validate .
```
Or from within Claude Code: `/plugin validate .`

## Testing Plugins Locally

```bash
# Load a single plugin for testing
claude --plugin-dir ./plugins/code-review

# Load multiple plugins
claude --plugin-dir ./plugins/code-review --plugin-dir ./plugins/git-toolkit

# Add the whole marketplace locally
# (from within Claude Code)
/plugin marketplace add ./
/plugin install code-review@demo-marketplace
```

## Adding a New Plugin

1. Create `plugins/<plugin-name>/.claude-plugin/plugin.json` with `name`, `description`, `version`
2. Add skill/command/agent/hook directories at the plugin root (NOT inside `.claude-plugin/`)
3. Add an entry to `.claude-plugin/marketplace.json` in the `plugins` array with `name`, `source`, and `description`
4. Validate with `claude plugin validate .`

## Marketplace JSON Schema (Quick Reference)

**Required in marketplace.json**: `name` (kebab-case), `owner.name`, `plugins[]` with each having `name` and `source`.

**Required in plugin.json**: `name` (kebab-case). Everything else is optional.

**SKILL.md frontmatter**: Must have `name` and `description`. Optional: `disable-model-invocation: true` (for user-only commands).

## TypeScript Convention

Never use `any` in TypeScript — if any plugin includes TS scripts, use proper types or `unknown`.
