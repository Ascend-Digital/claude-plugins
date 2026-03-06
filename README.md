# Claude Plugins Marketplace

A marketplace for Claude Code plugins, providing a centralized repository for discovering and managing plugins.

## Overview

This repository serves as a plugin marketplace for Claude Code, allowing developers to share and discover plugins that extend Claude's functionality.

## How to install

Start up Claude Code and then run the following commands:

```
/plugin marketplace add git@github.com:ascend-digital/claude-plugins.git
/plugin install ascend@ascend-marketplace
```

### Wiki MCP Server

The plugin includes an integration with the Ascend Wiki MCP server. To enable it, set your auth token:

```bash
# Add to ~/.zshrc
export ASCEND_WIKI_MCP_TOKEN="your-token"
```

Restart your shell and Claude Code. The wiki tools (`search_wiki`, `read_page`, etc.) will then be available automatically.

You can now start using commands such as:

```
/ascend:memory               (ascend) View or edit the triage memory — domain glossary, repo map, team conventions, and known technical debt.
/ascend:clarify              (ascend) Analyze a Jira ticket for ambiguities and generate targeted clarifying questions to ask the reporter.
/ascend:register-repo        (ascend) Register a repository path for multi-repo triage. Adds a repo to the memory file so the triage system knows where to look during cross-repo investigation.
/ascend:spec                 (ascend) Generate or regenerate a technical specification for a Jira ticket, using codebase analysis and any new information from clarifying questions.
/ascend:triage               (ascend) Pull a Jira ticket, analyze it against the codebase, classify it, generate a technical spec, identify ambiguities, and output a structured triage markdown file for review.
/ascend:pr                   (ascend) Generate a PR description using the team's template, populated from git diff, commit history, Jira ticket, and any existing triage/spec files.
/ascend:wiki                 (ascend) Write or update wiki documentation for the current project. Writes articles to docs/wiki/ for automatic sync to the central wiki.
```

### PR Description Skill

The `/ascend:pr` command generates a PR description using the team's standard template. Run it from within a feature branch:

```
/ascend:pr
/ascend:pr develop
/ascend:pr CS-1234
```

The skill will:
- Analyze the git diff and commit history against the target branch (defaults to `main`)
- Auto-detect the Jira ticket key from the branch name (e.g., `feature/CS-1234-desc`)
- Pull motivation context from the Jira ticket and any existing triage/spec files
- Output the filled PR template for review — it never auto-creates the PR

It also activates automatically when you ask Claude to "create a PR" or "fill the PR template".

### Wiki Documentation Skill

The `/ascend:wiki` command writes and maintains internal wiki articles. Run it from within a source repository:

```
/ascend:wiki the new voucher system
/ascend:wiki update basket docs to reflect delivery options
/ascend:wiki architecture overview for this project
```

Articles are written to `docs/wiki/` in the current repo and automatically synced to the central Ascend wiki via the daily GitHub Action.

Claude will also offer to write documentation after implementing new features or making significant changes — no need to remember to run the command manually.

## Structure

- `marketplace.json` - Main marketplace configuration
- `plugins/` - Plugin definitions directory
  - Each plugin is defined in a separate JSON file
  - See `plugins/README.md` for plugin structure details

## Getting Started

### For Plugin Users

Browse the available plugins in the `plugins/` directory. Each plugin includes:
- Description and version information
- Required permissions
- Installation instructions
- Configuration options

### For Plugin Developers

To add a new plugin to the marketplace:

1. Create a new JSON file in the `plugins/` directory
2. Follow the plugin schema defined in `plugins/README.md`
3. Submit a pull request with your plugin definition

## Plugin Categories

- **Development**: Tools and utilities for software development
- **Utilities**: General-purpose utility plugins
- **Productivity**: Plugins that enhance productivity
- **Integration**: Third-party service integrations

## Contributing

Contributions are welcome! Please read our contributing guidelines before submitting plugins.

## License

See individual plugin licenses for terms of use.