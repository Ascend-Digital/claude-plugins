# Claude Plugins Marketplace

A marketplace for Claude Code plugins, providing a centralized repository for discovering and managing plugins.

## Overview

This repository serves as a plugin marketplace for Claude Code, allowing developers to share and discover plugins that extend Claude's functionality.

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