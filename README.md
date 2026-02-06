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

You can now start using commands such as:

```
/ascend:memory               (ascend) View or edit the triage memory — domain glossary, repo map, team conventions, and known technical debt.
/ascend:clarify              (ascend) Analyze a Jira ticket for ambiguities and generate targeted clarifying questions to ask the reporter.
/ascend:register-repo        (ascend) Register a repository path for multi-repo triage. Adds a repo to the memory file so the triage system knows where to look during cross-repo investigation.
/ascend:spec                 (ascend) Generate or regenerate a technical specification for a Jira ticket, using codebase analysis and any new information from clarifying questions.
/ascend:triage               (ascend) Pull a Jira ticket, analyze it against the codebase, classify it, generate a technical spec, identify ambiguities, and output a structured triage markdown file for review.
```

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