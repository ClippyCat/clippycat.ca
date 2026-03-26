# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A personal portfolio website built with [Zola](https://www.getzola.org/), a Rust-based static site generator. Content is written in Markdown, templates use Tera, and configuration uses TOML.

## Commands

```bash
# Local development server (live reload)
zola serve

# Build the site
zola build

# Deploy (runs on remote server via SSH)
# See sync.bat for the full SSH deploy command
```

Zola must be installed separately (not managed via npm or any package manager in this repo).

## Architecture

### Content → URL Mapping

Files in `content/` map directly to URLs:
- `content/_index.md` → `/`
- `content/resume.md` → `/resume/`
- `content/ideas/_index.md` → `/ideas/`
- `content/ideas/foo.md` → `/ideas/foo/`

Folders with `_index.md` are **sections**; sibling `.md` files are **pages** within that section.

### Template Hierarchy

```
base.html          ← root layout (html, head, header, footer, main block)
  ├── index.html   ← home page (extends base)
  ├── page.html    ← individual content pages (extends base)
  └── section.html ← sections with child page lists (extends base)

includes/
  ├── header.html  ← navigation (included in base)
  └── footer.html  ← footer links (included in base)

link.html          ← reusable external link component
```

### Navigation and Footer Data

Navigation and footer links are defined in TOML data files, not hardcoded in templates:

- `data/nav.toml` → loaded in `templates/includes/header.html`
- `data/foot.toml` → loaded in `templates/includes/footer.html`

Add or change nav/footer links by editing these TOML files.

### Deployment

The site is served by Caddy on a remote server (`arch@clippycat.ca`). `sync.bat` SSHs in, pulls main, runs `zola build`, and copies output to `/usr/share/caddy/public/clippycat/`.

## Accessibility

All markup must conform to WCAG 2.1 Level AAA. Existing patterns to maintain:
- Semantic landmarks: `<header>`, `<nav>`, `<main>`, `<footer>`
- `aria-current="page"` on active nav links
- `role="list"` on lists where CSS resets remove default list semantics
- External links: `target="_blank"` + `rel="noopener noreferrer"`
