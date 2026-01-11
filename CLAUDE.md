# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

```bash
# Local development server with live reload (includes drafts and future posts)
hugo server -D --buildFuture

# Build static site to public/
hugo

# Build with drafts and future posts (matches CI behavior)
hugo --buildDrafts --buildFuture --gc
```

## Architecture

This is a Hugo blog based on the PaperMod theme, customized for a personal blog about AI, technology, philosophy, and life. It deploys automatically to GitHub Pages via `.github/workflows/gh-pages.yml` on push to main.

### Content Structure

- **content/posts/** - Blog posts as Markdown files
- Posts use standard Hugo front matter:
  ```yaml
  ---
  title: "Post Title"
  date: 2025-01-06
  draft: false
  description: "SEO description"
  tags: ["Tag1", "Tag2"]
  categories: ["Category"]
  ---
  ```

### Theme Customization

This repo contains a customized version of the PaperMod theme directly in the repository (not as a submodule):

- **layouts/** - Hugo templates overriding/extending PaperMod defaults
- **layouts/partials/** - Reusable template components including `related_posts.html` for article recommendations
- **layouts/shortcodes/** - Custom shortcodes for content
- **assets/css/extended/blank.css** - Custom CSS (related posts styling, enhanced tags)
- **i18n/** - Translation strings for multilingual support

### Features Enabled

- **Related posts** - Shows up to 3 related articles based on tags/categories (configured in `[related]` section)
- **Tags/Categories** - Taxonomy pages auto-generated at `/tags/` and `/categories/`
- **Reading time & word count** - Displayed on posts
- **Table of contents** - Auto-generated for posts
- **Edit on GitHub** - "Suggest Changes" link on each post
- **Social sharing** - Share buttons on posts
- **Breadcrumbs** - Navigation path shown on posts

### Configuration

- **hugo.toml** - Main Hugo configuration (site params, taxonomies, related content settings)
- **theme.toml** - Theme metadata

### Deployment

GitHub Actions workflow (`.github/workflows/gh-pages.yml`) builds and deploys on push to main. Uses Hugo version 0.125.7 by default.

### Relation to Personal Website

This blog is linked from the main personal website (Jack5316.github.io) navigation under "Blog".
