# Photography Section Design

## Overview

Add a photography page to the portfolio site with a masonry grid gallery, lightbox viewer, and site-wide navigation menu.

## Approach

Pure CSS masonry (CSS `columns`) + vanilla JS lightbox. No external dependencies.

## Navigation Menu

- Minimal top bar added to `_layouts/default.html`, appears on all pages
- Left: "Christian Sherland" linking to home
- Right: "Photography" and "Resume" text links
- Styled with existing font/color system (`$font-sans`, `var(--color-body-text)`, accent on hover)
- Subtle bottom border using `var(--color-border)`
- Mobile: links stay inline (only 2 links), font size shrinks via responsive mixins
- New `_sass/_nav.scss`, imported in `assets/css/main.scss`

## Photography Page

- New `photography.html` at root, uses `default` layout, permalink `/photography/`
- Section title "Photography" with masonry grid below

### Data Model (`_data/photography.yml`)

```yaml
- image: assets/img/photography/example-photo.jpg
  alt: Description of the photo
  title: Optional title
```

Flat list. Each entry needs image path and alt text. Title is optional, shown in lightbox.

### Masonry Grid

- CSS `columns`: 3 columns desktop, 2 tablet, 1 mobile
- `break-inside: avoid` on each item
- `column-gap` and `margin-bottom` for spacing
- Images `width: 100%` with natural aspect ratios
- `.fade-in` class for scroll animations
- Hover effect: subtle scale or opacity shift on thumbnails

### Lightbox

- Click photo to open full-screen dark overlay (`rgba(0,0,0,0.9)`)
- Image centered, scaled to fit (`max-width: 90vw`, `max-height: 90vh`, `object-fit: contain`)
- Title displayed below image if present
- Close via: clicking backdrop, pressing Escape, or X button
- Vanilla JS, inline in page or default layout

## Image Management

- Store optimized JPEGs in `assets/img/photography/`
- Resize to max 1600px wide before adding (`sips --resizeW 1600 *.jpg`)
- Compress at quality 80-85%
- Use kebab-case filenames

## Files

### New

- `photography.html` — page
- `_data/photography.yml` — photo data
- `_sass/_photography.scss` — grid and lightbox styles
- `_sass/_nav.scss` — navigation styles
- `assets/img/photography/` — photo directory

### Modified

- `_layouts/default.html` — add nav markup
- `assets/css/main.scss` — import new SCSS partials
