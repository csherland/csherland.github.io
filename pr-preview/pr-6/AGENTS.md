# Repository Guidelines

This guide keeps contributions consistent for the Jekyll-powered personal site served via GitHub Pages. Follow these conventions to avoid regressions and keep local/production outputs aligned.

## Project Structure & Module Organization
- Content and layout: `_layouts/` for base templates, `_includes/` for partials, `_data/` for YAML-driven content such as experience, and `index.html` plus `404.md` for top-level pages.
- Styling: `_sass/` for partials and variables, `assets/css/main.scss` as the compiled entry (output handled by Jekyll/Pages).
- Assets: `assets/img/` for images and `assets/resume.pdf` for downloads; keep filenames kebab-case.
- Configuration: `_config.yml` controls site metadata, plugins (`jekyll-sitemap`, `jemoji`), Sass compression, and exclusion rules. Update here before adding new content buckets.

## Build, Test, and Development Commands
- `bundle install` — install Ruby gems (required once or after Gemfile changes).
- `bundle exec jekyll serve --livereload` — run the site locally with incremental rebuilds at `http://127.0.0.1:4000`.
- `bundle exec jekyll build` — produce the `_site/` output to confirm production parity; GitHub Pages runs an equivalent build.
- `bundle exec jekyll doctor` — sanity-check configuration when builds behave unexpectedly.

## Coding Style & Naming Conventions
- Use 2-space indentation for HTML, Liquid, YAML, and SCSS. Prefer Liquid filters over inline Ruby.
- Keep front matter minimal and ordered: `layout`, `title`, `description`, then custom fields.
- Favor kebab-case for files and asset names; keep Liquid variables snake_case for readability.
- SCSS: group variables/mixins in `_sass/`, avoid inline styles, and rely on compressed output already configured in `_config.yml`.

## Testing Guidelines
- No automated test suite; validate via `bundle exec jekyll serve` and manual checks for navigation, images, and social links.
- Before pushing, run `bundle exec jekyll build` to catch Liquid/YAML errors and confirm `_data` changes render as expected.
- When altering links or adding assets, spot-check with browser dev tools and consider Lighthouse for regressions.

## Commit & Pull Request Guidelines
- Follow the existing short, imperative style seen in history (e.g., `Update experience.yml`, `Update _config.yml`). One change-set per commit when possible.
- Pull requests should describe the user-facing impact, list key files touched, and include screenshots for visual changes.
- Reference related issues if applicable; note any content dependencies (e.g., new images) and confirm local build status in the description.
- GitHub CLI: `gh auth status` to confirm login, `gh repo view --web` to open the repo, and `gh pr create --fill --web` to open a PR after `git push`. Use `gh pr status` to verify checks before merging.

## Security & Configuration Notes
- Never commit credentials or private contact details; URLs and emails live in `_config.yml` and `_data/` and should stay public-ready.
- External embeds or scripts should be scoped to specific pages and reviewed for mixed-content or CSP concerns before adding.
