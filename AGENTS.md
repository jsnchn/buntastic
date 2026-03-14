# Buntastic

A simple static site generator built with Bun.

## Project Structure

```
buntastic/
├── content/               # File-based routing source
│   ├── index.md          # → /
│   ├── about.md          # → /about
│   ├── 404.md            # → /404.html
│   └── posts/
│       ├── index.md      # → /posts (with collection)
│       └── *.md          # → /posts/*
├── src/
│   ├── index.ts          # Main build script
│   └── layouts/          # HTML layouts
│       ├── base.html     # Root layout
│       ├── page.html     # extends: base.html
│       └── post.html     # extends: base.html
├── public/               # Static assets
└── package.json
```

## Commands

```bash
bun run build        # Production build (excludes drafts)
bun run build:drafts  # Build with drafts included
bun run dev          # Watch mode + dev server
bun run preview      # Serve dist folder
```

## Features

- **Zero dependencies** - Uses only Bun's built-in APIs
- **File-based routing** - `content/*.md` → `/`, `content/posts/*.md` → `/posts/*`
- **Layout inheritance** - Layouts can extend other layouts via `extends:` frontmatter
- **Collections** - `{{ collection }}` variable available in index pages
- **Draft mode** - `draft: true` in frontmatter, included with `--drafts` flag
- **404 page** - `content/404.md` automatically becomes `/404.html`
- **Asset co-location** - Non-markdown files in content/ are copied to dist

## Frontmatter

```yaml
---
title: Page Title
date: 2024-01-15
layout: post              # Uses layouts/post.html
description: Meta description
draft: false             # Set true to exclude from production build
---
```

## Layout Variables

| Variable | Description |
|----------|-------------|
| `{{ title }}` | Page title from frontmatter |
| `{{ content \| safe }}` | Rendered markdown HTML |
| `{{ date }}` | Page date |
| `{{ description }}` | Page description |
| `{{ url }}` | Current page URL |
| `{{ collection }}` | Array of posts in current folder (for index pages) |
| `{{ head \| safe }}` | Head content from child layouts (appended to parent) |

## Layout System

Layouts support inheritance via frontmatter:

```html
<!-- layouts/post.html -->
---
extends: base.html
---
<article class="post">
  <h1>{{ title }}</h1>
  {{ content | safe }}
</article>
```

The `{{ content | safe }}` placeholder is where child content gets injected.

## Head Block

Layouts can inject content into the `<head>` section using `{{ head | safe }}`. Content BEFORE the marker goes into `<head>`, content AFTER the marker is treated as the layout body:

```html
<!-- layouts/post.html -->
---
extends: base.html
---
<link rel="stylesheet" href="/post.css">
{{ head | safe }}
<article class="post">
  <h1>{{ title }}</h1>
  {{ content | safe }}
</article>
```

The `{{ head | safe }}` marker is where additional head content can be injected (from child layouts or page frontmatter). Content before the marker (e.g., the post.css link) is automatically placed in the parent's `<head>` section. Child layout head content is appended to parent head content (parent's head first, then child's).

## Development

1. Add content to `content/` folder
2. Run `bun run dev` for development
3. Run `bun run build` for production

## Tech Stack

- [Bun](https://bun.sh) - JavaScript runtime
- Bun.markdown - Built-in GFM markdown parser
- Bun.serve - HTTP server
- Bun.watch - File watching
