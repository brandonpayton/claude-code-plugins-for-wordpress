---
name: wordpress-playground
description: Use when testing WordPress plugin changes, verifying plugin behavior, or needing a running WordPress instance to validate work
---

# WordPress Playground

Run a local WordPress instance with your plugin mounted for testing.

**Requires:** Node.js 20.18+

## Quick Start

From your plugin directory:

```bash
npx @wp-playground/cli server --auto-mount --login
```

## Server Details

| Property | Value |
|----------|-------|
| Default port | 9400 |
| Ready signal | `WordPress is running on http://127.0.0.1:9400 with N worker(s)` |
| Admin URL | `http://127.0.0.1:9400/wp-admin/` |
| Stop | Kill the process |

## Workflow

1. Start server in background
2. Wait for ready signal in output
3. Use Playwright to interact with WordPress (pages, admin, plugin UI)
4. Verify plugin behavior via HTTP requests or browser automation
5. Kill server when done

## Flags

| Flag | Purpose |
|------|---------|
| `--auto-mount` | Mount current directory as plugin/theme |
| `--login` | Auto-login to wp-admin |
| `--port <port>` | Use custom port (default: 9400) |
