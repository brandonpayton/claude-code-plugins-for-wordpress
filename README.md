# WordPress Plugins for Claude Code

A marketplace of Claude Code plugins for WordPress development.

## Installation

```bash
# Add the marketplace
/plugin marketplace add brandonpayton/claude-code-plugins-for-wordpress

# Install the wordpress-playground plugin
/plugin install wordpress-playground@brandonpayton/claude-code-plugins-for-wordpress
```

## Plugins

### wordpress-playground

Skills for testing WordPress plugins, themes, and core code using WordPress Playground.

#### Skills

**wordpress-playground:server** - Run a local WordPress instance with your plugin, theme, wp-content directory, or WordPress directory mounted for testing.

Invoke with `/wordpress-playground:server` or it will be automatically suggested when testing WordPress code.

## Repository Structure

```
.claude-plugin/
  marketplace.json      # Marketplace definition
plugins/
  wordpress-playground/ # WordPress Playground plugin
    .claude-plugin/
      plugin.json       # Plugin metadata
    skills/
      server/           # Server skill
        SKILL.md
        scripts/
          start-server.sh
          stop-server.sh
```

## Requirements

- Node.js 20.18+
- Claude Code with plugin support

## Updating

```bash
/plugin marketplace update
```
