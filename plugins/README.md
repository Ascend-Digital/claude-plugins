# Plugins Directory

This directory contains plugin definitions for the Claude Plugins Marketplace.

## Plugin Structure

Each plugin should be defined in a JSON file with the following structure:

```json
{
  "id": "unique-plugin-id",
  "name": "Plugin Name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": "Author Name",
  "category": "category-name",
  "tags": ["tag1", "tag2"],
  "main": "index.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/username/repo"
  },
  "engines": {
    "claude": ">=1.0.0"
  },
  "permissions": [],
  "config": {}
}
```

## Categories

- `development`: Development tools and utilities
- `utilities`: General utility plugins
- `productivity`: Productivity enhancement plugins
- `integration`: Third-party integrations

## Adding a New Plugin

1. Create a new JSON file in this directory with your plugin definition
2. Ensure the plugin ID is unique
3. Follow the structure outlined above
4. Test your plugin configuration
