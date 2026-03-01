# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Jekyll-powered personal portfolio site for Christian Sherland, deployed via GitHub Pages. Content is data-driven through YAML files in `_data/`, with templates in `_includes/` and layouts in `_layouts/`.

## Development Commands

```bash
# Install dependencies (required once or after Gemfile changes)
bundle install

# Run local development server with live reload
bundle exec jekyll serve --livereload
# Site runs at http://127.0.0.1:4000

# Build production output (validates before deployment)
bundle exec jekyll build

# Sanity-check configuration
bundle exec jekyll doctor
```

## Architecture

### Content-Driven Design
- **Data sources**: `_data/experience.yml` and `_data/projects.yml` contain all structured content
- **Templates**: `_includes/` contains reusable partials (intro.html, experience.html, background.html, footer.html, etc.)
- **Page composition**: `index.html` uses `default` layout and includes multiple partials to build the single-page site
- **Styling**: SCSS partials in `_sass/` are compiled via `assets/css/main.scss`; Sass compression is enabled in `_config.yml`

### Jekyll Configuration
- Plugins: `jekyll-sitemap`, `jemoji`
- Markdown: kramdown
- Build excludes: Gemfile, Gemfile.lock, gemspecs, README.md, LICENSE.md
- Site metadata (name, title, description, social links) lives in `_config.yml`

### File Organization
- Layouts: `_layouts/default.html` (base template), `_layouts/project.html` (for project pages)
- Assets: `assets/img/` for images, `assets/resume.pdf` for downloads
- Use kebab-case for asset filenames

## Coding Conventions

- **Indentation**: 2 spaces for HTML, Liquid, YAML, and SCSS
- **Naming**: kebab-case for files/assets, snake_case for Liquid variables
- **Front matter**: minimal and ordered (layout, title, description, then custom fields)
- **SCSS**: group variables/mixins in `_sass/`, avoid inline styles
- **Liquid**: prefer filters over inline Ruby

## Testing

- No automated tests; validate by running `bundle exec jekyll serve` and manually checking navigation, images, and social links
- Before pushing, run `bundle exec jekyll build` to catch Liquid/YAML errors
- Verify `_data` changes render correctly in the local build

## Git Workflow

- **Commit style**: short, imperative messages (e.g., "Update experience.yml", "Update _config.yml")
- **GitHub CLI**:
  - `gh auth status` — confirm authentication
  - `gh repo view --web` — open repo in browser
  - `gh pr create --fill --web` — create PR after push
  - `gh pr status` — check CI status

## Important Notes

- This is a GitHub Pages site; changes pushed to `master` automatically deploy
- Site configuration (URLs, email, social links) is public-facing and stored in `_config.yml`
- All content in `_data/` drives the rendered site via Liquid templates in `_includes/`
