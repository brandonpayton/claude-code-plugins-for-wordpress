---
name: wordpress-playground-server
description: Test code with WordPress by starting a WordPress server. Use when testing changes to a WordPress plugin, a WordPress theme, WordPress source code, verifying WordPress behavior, or needing a running WordPress instance to validate work
user-invocable: true
allowed-tools:
  - Bash
  - KillShell
  - mcp__playwright__browser_navigate
  - mcp__playwright__browser_snapshot
  - mcp__playwright__browser_click
  - mcp__playwright__browser_type
  - mcp__playwright__browser_fill_form
  - mcp__playwright__browser_hover
  - mcp__playwright__browser_select_option
  - mcp__playwright__browser_press_key
  - mcp__playwright__browser_wait_for
  - mcp__playwright__browser_take_screenshot
  - mcp__playwright__browser_close
---

# WordPress Playground

Run a local WordPress instance with your plugin, theme, wp-content directory, or whole WordPress directory mounted for testing.

**Requires:** Node.js 20.18+

## Quick Start

Use the helper scripts in `scripts/` relative to this skill's base directory:

```bash
# Start server and get PID + URL
./scripts/start-server.sh --login --auto-mount /path/to/plugin
# Output on success:
#   pid:12345
#   url:http://127.0.0.1:9400

# Stop server by PID
./scripts/stop-server.sh 12345
# Output: stopped:12345
```

| Flag | Purpose |
|------|---------|
| `--login` | Auto-login to wp-admin (required for Playwright) |
| `--auto-mount <path>` | Auto-detect and mount (see Path Detection) |
| `--port N` | Use custom port (default: 9400) |
| `--blueprint <path>` | Optional Blueprint JSON to run |

Run `npx @wp-playground/cli server --help` for all options.

## Path Detection

`--auto-mount` detects path type by file signatures:

| Type | Detection Rule |
|------|----------------|
| Plugin | PHP file with `Plugin Name:` header |
| Theme | `style.css` with `Theme Name:` header |
| wp-content | Directory named `wp-content` |
| WordPress | Contains `wp-includes/` directory |

The principle: detection looks for WordPress-standard markers.

## Workflow

### Using Helper Scripts (Recommended)

1. Start server with `start-server.sh` - it waits for ready and returns PID + URL
2. Parse the output to get the URL
3. Navigate and interact via Playwright MCP tools
4. Stop server with `stop-server.sh <pid>` when done

```bash
# From skill base directory
result=$(./scripts/start-server.sh --login --auto-mount /path/to/plugin)
pid=$(echo "$result" | grep '^pid:' | cut -d: -f2)
url=$(echo "$result" | grep '^url:' | cut -d: -f2)
# Use $url for Playwright, then:
./scripts/stop-server.sh "$pid"
```

### Using npx Directly

1. Start server with `run_in_background: true` (server runs continuously; blocking call would hang)
2. Read task output frequently until you see: `WordPress is running on http://127.0.0.1:<port>`
   **STOP.** Do not call any Playwright tool until this message appears.
3. Navigate and interact via Playwright MCP tools using the URL from step 2
4. Kill server with `KillShell` when done

<example type="CORRECT">
Bash(run_in_background=true): npx @wp-playground/cli server --login --auto-mount /path
TaskOutput(block=true): wait for "WordPress is running on http://127.0.0.1:9400"
browser_navigate: http://127.0.0.1:9400/wp-admin/
</example>

<example type="INCORRECT">
Bash(run_in_background=true): npx @wp-playground/cli server --login --auto-mount /path
browser_navigate: http://127.0.0.1:9400/wp-admin/
# Connection refused—server not ready yet
</example>

## Admin Testing

<example type="CORRECT">
npx @wp-playground/cli server --login --auto-mount /path/to/plugin
# Playwright can access /wp-admin/ immediately
</example>

<example type="INCORRECT">
npx @wp-playground/cli server --auto-mount /path/to/plugin
# Playwright blocked by login screen at /wp-admin/
</example>

## Troubleshooting

**Recoverable errors** (retry automatically):

| Error | Action |
|-------|--------|
| `EADDRINUSE` / port in use | Use `--port <port>` to choose a subsequent port that is not in use |
| No ready signal in 60s | Read task output for errors, retry once |

**Configuration errors** (verify setup):

| Error | Action |
|-------|--------|
| Plugin not in admin list | Verify path contains PHP file with `Plugin Name:` header |
| Playwright tools unavailable | Ask user to install Playwright MCP server |
