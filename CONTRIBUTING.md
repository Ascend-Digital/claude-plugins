# Contributing to Claude Plugins Marketplace

Thank you for your interest in contributing to the Claude Plugins Marketplace!

## How to Contribute a Plugin

### 1. Prepare Your Plugin

Ensure your plugin:
- Has a unique ID (lowercase letters, numbers, and hyphens only)
- Follows semantic versioning (e.g., 1.0.0)
- Includes a clear description
- Specifies required permissions
- Has been tested

### 2. Create Plugin Definition

Create a JSON file in the `plugins/` directory following this structure:

```json
{
  "id": "your-plugin-id",
  "name": "Your Plugin Name",
  "version": "1.0.0",
  "description": "Clear description of what your plugin does",
  "author": "Your Name or Organization",
  "category": "development|utilities|productivity|integration",
  "tags": ["relevant", "tags"],
  "main": "index.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/your-plugin"
  },
  "engines": {
    "claude": ">=1.0.0"
  },
  "permissions": [],
  "config": {}
}
```

### 3. Validate Your Plugin

Ensure your plugin definition:
- Uses valid JSON syntax
- Conforms to the schema defined in `plugin-schema.json`
- Has a unique ID not used by other plugins
- Includes all required fields

### 4. Submit a Pull Request

1. Fork the repository
2. Create a new branch (`git checkout -b add-my-plugin`)
3. Add your plugin definition file to the `plugins/` directory
4. Commit your changes (`git commit -m 'Add my-plugin'`)
5. Push to the branch (`git push origin add-my-plugin`)
6. Open a Pull Request

## Plugin Guidelines

### Naming
- Use descriptive, lowercase IDs with hyphens
- Keep names concise but clear
- Avoid generic terms

### Categories
Choose the most appropriate category:
- **development**: Code editors, linters, formatters, build tools
- **utilities**: File management, text processing, data conversion
- **productivity**: Task management, note-taking, automation
- **integration**: API clients, service connectors, webhooks

### Permissions
Be explicit about required permissions:
- File system access
- Network access
- Environment variables
- System commands

### Documentation
Include:
- Clear description of functionality
- Usage examples
- Configuration options
- Any prerequisites or dependencies

## Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Follow best practices for security and privacy
- Test thoroughly before submitting

## Questions?

If you have questions or need help, please open an issue in this repository.
