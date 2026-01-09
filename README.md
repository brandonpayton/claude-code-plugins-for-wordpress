# WordPress Playground Skills

A Claude Code plugin with skills for testing WordPress plugins, themes, and code using WordPress Playground.

## Installation

```bash
# Add the marketplace
/plugin marketplace add https://github.com/brandonpayton/claude-code-wordpress-playground-plugin.git

# Install the plugin
/plugin install wordpress-playground-skills@wordpress-playground-skills
```

## Skills Included

### wordpress-playground

Run a local WordPress instance with your plugin, theme, wp-content directory, or whole WordPress directory mounted for testing.

Invoke with `/wordpress-playground` or it will be automatically suggested when testing WordPress code.

### wordpress-blueprint

Generate Blueprint JSON files that configure WordPress Playground instances.

Invoke with `/wordpress-blueprint` or it will be automatically suggested when creating WordPress demo configurations.

## Requirements

- Node.js 20.18+
- Claude Code with plugin support

## Updating

```bash
/plugin marketplace update
```
