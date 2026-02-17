# Buntastic

A simple static site generator built with Bun.

## Why Buntastic?

- **Zero dependencies** - Uses only Bun's built-in APIs
- **Fast** - Powered by Bun's native markdown parser
- **Simple** - No complex configuration needed
- **Flexible** - Layouts with inheritance, collections, drafts

## Installation

### As a CLI tool (recommended)

```bash
# Install Bun (if needed)
curl -fsSL https://bun.sh/install | bash

# Install buntastic globally
bun install -g buntastic

# Or use npx without installing
npx buntastic build
```

### For development

```bash
# Clone the repo
git clone https://github.com/jsnchn/buntastic.git
cd buntastic

# Start the dev server
bun run dev
```

Visit `http://localhost:3000` to see your site.

## Project Structure

```
buntastic/
├── content/               # Your markdown content
│   ├── index.md          # → /
│   ├── about.md          # → /about
│   └── posts/
│       └── *.md          # → /posts/*
├── src/
│   ├── index.ts          # Build script
│   └── layouts/          # HTML templates
├── public/               # Static assets (CSS, images)
└── package.json
```

## Commands

| Command | Description |
|---------|-------------|
| `buntastic build` | Build for production (excludes drafts) |
| `buntastic build --drafts` | Build with drafts included |
| `buntastic dev` | Watch mode + dev server |
| `buntastic preview` | Serve the built `dist/` folder |

Or with bun run (if using from source):

| Command | Description |
|---------|-------------|
| `bun run build` | Build for production (excludes drafts) |
| `bun run build:drafts` | Build with drafts included |
| `bun run dev` | Watch mode + dev server |
| `bun run preview` | Serve the built `dist/` folder |

## Writing Content

Create markdown files in `content/`:

```markdown
---
title: My Post
date: 2024-01-15
layout: post
description: A short description
---

# Hello World

Your content here...
```

### Frontmatter Options

| Field | Description |
|-------|-------------|
| `title` | Page title |
| `date` | Publication date |
| `layout` | Layout to use (page, post, or custom) |
| `description` | Meta description |
| `draft` | Set `true` to exclude from production |

### File-Based Routing

| Content Path | Output URL |
|--------------|------------|
| `content/index.md` | `/` |
| `content/about.md` | `/about` |
| `content/posts/hello.md` | `/posts/hello` |

## Layouts

### Creating Layouts

Layouts live in `src/layouts/`. Create HTML files with frontmatter:

```html
<!-- layouts/post.html -->
---
extends: base.html
---
<article class="post">
  <h1>{{ title }}</h1>
  <time>{{ date }}</time>
  {{ content | safe }}
</article>
```

### Layout Variables

| Variable | Description |
|----------|-------------|
| `{{ title }}` | Page title |
| `{{ content \| safe }}` | Rendered markdown HTML |
| `{{ date }}` | Page date |
| `{{ description }}` | Meta description |
| `{{ url }}` | Current page URL |
| `{{ collection }}` | List of posts in current folder (for index pages) |

### Layout Inheritance

Use `extends:` to build on top of other layouts:

```html
<!-- layouts/base.html -->
<!DOCTYPE html>
<html>
<head>
  <title>{{ title }}</title>
</head>
<body>
  <main>{{ content | safe }}</main>
</body>
</html>
```

## Collections

For folder index pages (e.g., `content/posts/index.md`), use `{{ collection }}` to list all pages in that folder:

```html
<h1>Blog Posts</h1>
<ul>
{{ collection }}
</ul>
```

Renders as:

```html
<li><a href="/posts/hello">Hello World</a> - <time>2024-01-15</time></li>
<li><a href="/posts/other">Other Post</a></li>
```

## Static Assets

Place CSS, images, or other files in `public/`:

```
public/
└── style.css
```

They'll be available at `/style.css` in the built site.

You can also co-locate assets with content:

```
content/
└── posts/
    └── my-post/
        ├── index.md
        └── image.png
```

The image will be available at `/posts/my-post/image.png`.

## 404 Page

Create `content/404.md` to have a custom 404 page at `/404.html`.

## Drafts

Set `draft: true` in frontmatter to exclude a page from production builds:

```yaml
---
title: Work in Progress
draft: true
---
```

Drafts are included when running `bun run build:drafts`.

## Tech Stack

- [Bun](https://bun.sh) - JavaScript runtime
- Bun.markdown - Built-in GFM markdown parser
- Bun.serve - HTTP server
- Bun.watch - File watching

## License

MIT
