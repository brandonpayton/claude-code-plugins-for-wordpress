# Cross-Tool Skill Discovery Design

Enable skills in this repo to be discovered by GitHub Copilot, Cursor, Codex CLI, and other tools that support the Agent Skills open standard.

## Background

Agent Skills became an open standard in December 2025, governed by the Agentic AI Foundation (Anthropic, OpenAI, Block as co-founders; AWS, Google, Microsoft as platinum members). Tools now look for skills in standardized locations:

- `.github/skills/` (project-level)
- `.claude/skills/` (project-level, also supported by Copilot)

The existing skill at `plugins/wordpress-playground/skills/wordpress-playground-server/` works for Claude Code's plugin system but isn't discoverable by other tools.

## Design

### 1. Add symlink for cross-tool discovery

```
.github/skills/wordpress-playground-server -> ../../plugins/wordpress-playground/skills/wordpress-playground-server
```

This provides a single source of truth while enabling discovery via the standard `.github/skills/` path.

### 2. Remove redundant frontmatter field

Remove `user-invocable: true` from `SKILL.md`. This field:
- Defaults to `true` when omitted
- Is Claude-specific (not in the open Agent Skills spec)

### 3. Add minimal plugin README

Create `plugins/wordpress-playground/README.md`:

```markdown
# WordPress Playground Plugin

Run a local WordPress instance for testing WordPress plugins, themes, and code.

## Documentation

See [`skills/wordpress-playground-server/SKILL.md`](skills/wordpress-playground-server/SKILL.md) for usage.

## Requirements

- Node.js 20.18+
```

## Result

- **GitHub Copilot, Cursor, Codex CLI** discover the skill via `.github/skills/`
- **Claude Code** continues working via the plugin system
- **Single source of truth** — all tools use the same SKILL.md

## References

- [Agent Skills Specification](https://agentskills.io/specification)
- [GitHub Copilot Agent Skills](https://docs.github.com/copilot/concepts/agents/about-agent-skills)
- [GitHub Changelog - Copilot Skills](https://github.blog/changelog/2025-12-18-github-copilot-now-supports-agent-skills/)
