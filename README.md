# Claude Code Plugin Marketplace

A demo marketplace showcasing Claude Code plugin capabilities including skills, agents, commands, hooks, and MCP servers.

## Overview

This repository serves as a **Claude Code plugin marketplace** — a catalog that distributes plugins to Claude Code users via `/plugin marketplace add`. It demonstrates the full spectrum of Claude Code's extensibility features through three example plugins.

## Quick Start

### Install the Marketplace

```bash
# Add the marketplace (from within Claude Code)
/plugin marketplace add https://github.com/your-org/marketplace

# Install a specific plugin
/plugin install code-review@demo-marketplace
```

### Local Development

```bash
# Test a single plugin locally
claude --plugin-dir ./plugins/code-review

# Test multiple plugins
claude --plugin-dir ./plugins/code-review --plugin-dir ./plugins/git-toolkit

# Load the entire marketplace locally (from within Claude Code)
/plugin marketplace add ./
/plugin install code-review@demo-marketplace
```

## Available Plugins

### 🔍 code-review (v1.1.0)
Code review skills and agents for catching bugs, security issues, and style problems.

**Features:**
- **Skill**: `review` - Automatic code review on file changes
- **Agent**: `security-reviewer` - Specialized security analysis
- **Command**: `/code-review:quick-review` - On-demand code reviews

**Tags**: review, security, quality

### 🔧 git-toolkit (v1.1.0)
Git workflow automation with conventional commits, branch naming, and pre-commit hooks.

**Features:**
- **Skill**: `conventional-commit` - Enforces conventional commit standards
- **Commands**:
  - `/git-toolkit:smart-commit [type|scope]` - Intelligent commit creation
  - `/git-toolkit:branch` - Smart branch management
- **Hooks**: Pre-commit validation and formatting

**Tags**: git, commits, automation

### 📚 doc-generator (v1.1.0)
Documentation generation agents and skills for code explanation and API docs.

**Features:**
- **Skill**: `explain-code` - Contextual code explanations
- **Agent**: `doc-writer` - Comprehensive documentation specialist
- **Commands**:
  - `/doc-generator:api-docs` - Generate API documentation
  - `/doc-generator:readme` - Create project README files

**Tags**: docs, documentation, explanation

### 🚀 compound-engineering (v2.33.0)
AI-powered development tools with 29 agents, 22 commands, 19 skills, and 1 MCP server.

**External Plugin**: Sourced from [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin)

**Tags**: ai-powered, compound-engineering, workflow-automation

## Repository Structure

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json        # Marketplace catalog (root manifest)
├── plugins/
│   ├── code-review/           # Code review plugin
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/review/SKILL.md
│   │   ├── agents/security-reviewer.md
│   │   └── commands/quick-review.md
│   ├── git-toolkit/           # Git workflow plugin
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/conventional-commit/SKILL.md
│   │   ├── commands/smart-commit.md
│   │   ├── commands/branch.md
│   │   └── hooks/hooks.json
│   └── doc-generator/         # Documentation plugin
│       ├── .claude-plugin/plugin.json
│       ├── skills/explain-code/SKILL.md
│       ├── agents/doc-writer.md
│       ├── commands/api-docs.md
│       └── commands/readme.md
└── CLAUDE.md                  # Development guidelines
```

## Plugin Development

### Key Concepts

- **Skills** (`skills/*/SKILL.md`) - Auto-invoked by Claude based on task context
- **Commands** (`commands/*.md`) - User-invoked via slash commands (`/plugin:command`)
- **Agents** (`agents/*.md`) - Specialized Claude personas for complex tasks
- **Hooks** (`hooks/hooks.json`) - Event handlers for git operations and file changes

### Plugin Structure

Each plugin follows this structure:
```
plugin-name/
├── .claude-plugin/plugin.json  # Plugin manifest (name, version, description)
├── skills/                     # Auto-invoked capabilities
│   └── skill-name/SKILL.md
├── commands/                   # User-invoked slash commands
│   └── command-name.md
├── agents/                     # Specialized Claude personas
│   └── agent-name.md
├── hooks/                      # Event handlers
│   └── hooks.json
└── scripts/                    # Shell scripts for hooks
```

### Adding a New Plugin

1. **Create plugin manifest**:
   ```bash
   mkdir -p plugins/my-plugin/.claude-plugin
   # Edit plugins/my-plugin/.claude-plugin/plugin.json
   ```

2. **Add capabilities**:
   ```bash
   mkdir -p plugins/my-plugin/{skills,commands,agents}
   # Create your .md files following the examples
   ```

3. **Register in marketplace**:
   ```json
   // Edit .claude-plugin/marketplace.json
   {
     "plugins": [
       {
         "name": "my-plugin",
         "source": "./plugins/my-plugin",
         "description": "My awesome plugin",
         "version": "1.0.0",
         "category": "productivity",
         "tags": ["example"]
       }
     ]
   }
   ```

4. **Validate**:
   ```bash
   claude plugin validate .
   # Or from Claude Code: /plugin validate .
   ```

### Important Rules

- **`.claude-plugin/` only holds manifests** - Never put capabilities inside `.claude-plugin/`
- **Plugin names must be kebab-case** with no spaces
- **All marketplace.json paths are relative** to the repository root
- **Use `${CLAUDE_PLUGIN_ROOT}`** in hooks/scripts to reference plugin files
- **No path traversal** - plugins cannot reference `../` outside their directory

## Validation

```bash
# Validate entire marketplace
claude plugin validate .

# Validate specific plugin
claude plugin validate ./plugins/code-review

# From within Claude Code
/plugin validate .
```

## Contributing

1. Fork this repository
2. Create a new plugin following the structure above
3. Add it to `.claude-plugin/marketplace.json`
4. Validate with `claude plugin validate .`
5. Submit a pull request

## License

MIT - See individual plugins for their specific licenses.

## Related

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Plugin Development Guide](https://docs.anthropic.com/en/docs/claude-code/plugins)
- [Compound Engineering Plugin](https://github.com/EveryInc/compound-engineering-plugin)