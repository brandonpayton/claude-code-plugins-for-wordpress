---
name: wordpress-blueprint
description: Use when generating WordPress Playground Blueprint JSON files, creating WordPress demo configurations, or automating WordPress site setup
---

# WordPress Blueprint Generation

Generate Blueprint JSON files that configure WordPress Playground instances.

## Blueprint Structure

```json
{
  "$schema": "https://playground.wordpress.net/blueprint-schema.json",
  "landingPage": "/wp-admin/",
  "preferredVersions": {
    "php": "8.3",
    "wp": "latest"
  },
  "features": {
    "networking": true
  },
  "steps": []
}
```

**Critical paths:** All WordPress paths start with `/wordpress/` (e.g., `/wordpress/wp-content/mu-plugins/`).

## Common Steps

### Install Plugin from wordpress.org

```json
{
  "step": "installPlugin",
  "pluginData": {
    "resource": "wordpress.org/plugins",
    "slug": "contact-form-7"
  }
}
```

### Install Theme from wordpress.org

```json
{
  "step": "installTheme",
  "themeData": {
    "resource": "wordpress.org/themes",
    "slug": "flavor"
  }
}
```

**Note:** Plugin/theme slugs must exist on wordpress.org or the step will fail.

### Install from URL

```json
{
  "step": "installPlugin",
  "pluginData": {
    "resource": "url",
    "url": "https://example.com/plugin.zip"
  }
}
```

### Write File

```json
{
  "step": "writeFile",
  "path": "/wordpress/wp-content/mu-plugins/custom.php",
  "data": "<?php // PHP code here"
}
```

### Set Site Options

```json
{
  "step": "setSiteOptions",
  "options": {
    "blogname": "My Site",
    "blogdescription": "Site tagline"
  }
}
```

### Run PHP Code

**Must load wp-load.php first for WordPress functions:**

```json
{
  "step": "runPHP",
  "code": "<?php require_once '/wordpress/wp-load.php'; wp_insert_post(['post_title' => 'Hello', 'post_status' => 'publish']);"
}
```

### Login

```json
{
  "step": "login",
  "username": "admin",
  "password": "password"
}
```

Or simply `{"step": "login"}` for default admin.

### WP-CLI

```json
{
  "step": "wp-cli",
  "command": "wp post create --post_title='Test' --post_status=publish"
}
```

### Import Content (WXR)

```json
{
  "step": "importWxr",
  "file": {
    "resource": "url",
    "url": "https://example.com/content.xml"
  }
}
```

## Complete Example

Site with plugin, custom mu-plugin, and configuration:

```json
{
  "$schema": "https://playground.wordpress.net/blueprint-schema.json",
  "landingPage": "/wp-admin/",
  "preferredVersions": {
    "php": "8.3",
    "wp": "latest"
  },
  "features": {
    "networking": true
  },
  "steps": [
    {
      "step": "installPlugin",
      "pluginData": {
        "resource": "wordpress.org/plugins",
        "slug": "woocommerce"
      }
    },
    {
      "step": "installTheme",
      "themeData": {
        "resource": "wordpress.org/themes",
        "slug": "flavor"
      }
    },
    {
      "step": "writeFile",
      "path": "/wordpress/wp-content/mu-plugins/customizations.php",
      "data": "<?php add_filter('comments_open', '__return_false');"
    },
    {
      "step": "setSiteOptions",
      "options": {
        "blogname": "Demo Store",
        "blogdescription": "A WooCommerce demo"
      }
    },
    {
      "step": "login"
    }
  ]
}
```

## Testing Blueprints

Use the wordpress-playground skill with `--blueprint`:

```bash
npx @wp-playground/cli server --blueprint ./blueprint.json --login
```

## Common Mistakes

| Wrong | Correct |
|-------|---------|
| `phpVersion: "8.3"` | `preferredVersions: { php: "8.3" }` |
| `pluginZipUrl` | `pluginData: { resource, slug/url }` |
| `contents` | `data` |
| `/wp-content/...` | `/wordpress/wp-content/...` |
| `runPHP` without wp-load | Always `require_once '/wordpress/wp-load.php'` first |
