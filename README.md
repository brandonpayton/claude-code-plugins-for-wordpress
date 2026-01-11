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

**wordpress-playground-server** - Run a local WordPress instance with your plugin, theme, wp-content directory, or WordPress directory mounted for testing.

Invoke with `/wordpress-playground-server` or it will be automatically suggested when testing WordPress code.

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

NOTE: Initially, the wordpress-playground-server skill directly invoked `npx @wp-playground/cli` and waited for the "WordPress is running" message to know that Playground had started. Unfortunately, starting Playground this way required using Claude to examine Playground CLI output and was really slow, sometimes taking 1 minute to start up. By switching to start/stop scripts, we moved all the "has Playground finished starting?" to bash and stopped involving Claude in that process. With the start script, Playground startup takes about 8 seconds on my system.

## Requirements

- Node.js 20.18+
- Claude Code with plugin support

## Updating

```bash
/plugin marketplace update
```
