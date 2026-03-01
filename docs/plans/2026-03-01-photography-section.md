# Photography Section Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a photography page with masonry grid gallery and lightbox, plus a site-wide navigation menu.

**Architecture:** New standalone page at `/photography/` driven by `_data/photography.yml`. CSS `columns` masonry layout with vanilla JS lightbox. Minimal top-bar nav added to `_layouts/default.html` appearing on all pages.

**Tech Stack:** Jekyll, Liquid, SCSS, vanilla JS

---

### Task 1: Create navigation SCSS

**Files:**
- Create: `_sass/_nav.scss`

**Step 1: Create the nav stylesheet**

```scss
.nav {
  max-width: $site-max-width;
  margin: 0 auto;
  padding: $space-sm $space-md;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid var(--color-border);

  @include mobile {
    padding: $space-sm;
  }

  &__home {
    font-family: $font-serif;
    font-size: $font-size-lg;
    font-weight: $font-weight-semibold;
    color: var(--color-body-text);
    text-decoration: none;

    &:hover {
      color: var(--color-accent);
    }
  }

  &__links {
    display: flex;
    gap: $space-md;

    a {
      font-family: $font-sans;
      font-size: $font-size-sm;
      font-weight: $font-weight-medium;
      color: var(--color-muted);
      text-decoration: none;
      letter-spacing: 0.5px;

      &:hover {
        color: var(--color-accent);
      }
    }
  }
}
```

**Step 2: Import in main stylesheet**

In `assets/css/main.scss`, add `@import 'nav';` after the `'globals'` import (line 4), before `'base'`:

```scss
@import 'globals';
@import 'nav';
@import 'base';
```

**Step 3: Build to verify SCSS compiles**

Run: `bundle exec jekyll build`
Expected: Clean build, no errors

**Step 4: Commit**

```bash
git add _sass/_nav.scss assets/css/main.scss
git commit -m "Add navigation bar styles"
```

---

### Task 2: Add navigation to default layout

**Files:**
- Modify: `_layouts/default.html:7-10`

**Step 1: Add nav markup inside the body, before `.page`**

Replace lines 7-10 of `_layouts/default.html`:

```html
  <body>
    <nav class="nav">
      <a href="{{ site.baseurl }}/" class="nav__home">Christian Sherland</a>
      <div class="nav__links">
        <a href="{{ site.baseurl }}/photography/">Photography</a>
        <a href="{{ site.baseurl }}/assets/resume.pdf" target="_blank" rel="noopener">Resume</a>
      </div>
    </nav>
    <div class="page">
      {{ content }}
    </div>
```

**Step 2: Build and verify**

Run: `bundle exec jekyll build`
Expected: Clean build, no errors

**Step 3: Serve locally and verify nav appears**

Run: `bundle exec jekyll serve --livereload`
Expected: Nav bar visible at top of homepage with "Christian Sherland" on left, "Photography" and "Resume" links on right. Resume link opens PDF in new tab.

**Step 4: Commit**

```bash
git add _layouts/default.html
git commit -m "Add navigation bar to default layout"
```

---

### Task 3: Create photography data file

**Files:**
- Create: `_data/photography.yml`
- Create: `assets/img/photography/` (directory)

**Step 1: Create the photo directory**

```bash
mkdir -p assets/img/photography
```

**Step 2: Create the data file with placeholder entries**

Create `_data/photography.yml`:

```yaml
# Add photos here. Each entry needs:
#   image: path relative to site root (e.g., assets/img/photography/my-photo.jpg)
#   alt: descriptive alt text (required for accessibility)
#   title: optional title shown in lightbox
#
# Before adding photos, resize to max 1600px wide:
#   sips --resizeW 1600 *.jpg
#
# Example:
# - image: assets/img/photography/sunset-over-bay.jpg
#   alt: Golden sunset reflecting over a calm bay
#   title: Sunset Over the Bay
```

**Step 3: Commit**

```bash
git add _data/photography.yml
git commit -m "Add photography data file and image directory"
```

---

### Task 4: Create photography page

**Files:**
- Create: `photography.html`

**Step 1: Create the page**

Create `photography.html` at the project root:

```html
---
layout: default
title: Photography | Christian Sherland
description: Photography by Christian Sherland
permalink: /photography/
---

<section class="section photography fade-in">
  <div class="section__title">Photography</div>
  <div class="section__content">
    {% if site.data.photography.size > 0 %}
    <div class="photo-grid">
      {% for photo in site.data.photography %}
      <div class="photo-grid__item">
        <img
          src="{{ site.baseurl }}/{{ photo.image }}"
          alt="{{ photo.alt }}"
          {% if photo.title %}data-title="{{ photo.title }}"{% endif %}
          loading="lazy"
          class="photo-grid__img"
          onclick="openLightbox(this)"
        >
      </div>
      {% endfor %}
    </div>
    {% else %}
    <p>Photos coming soon.</p>
    {% endif %}
  </div>
</section>

<!-- Lightbox -->
<div class="lightbox" id="lightbox" onclick="closeLightbox(event)">
  <button class="lightbox__close" onclick="closeLightbox(event)" aria-label="Close">&times;</button>
  <img class="lightbox__img" id="lightbox-img" src="" alt="">
  <div class="lightbox__title" id="lightbox-title"></div>
</div>

<script>
  function openLightbox(img) {
    var lightbox = document.getElementById('lightbox');
    var lightboxImg = document.getElementById('lightbox-img');
    var lightboxTitle = document.getElementById('lightbox-title');
    lightboxImg.src = img.src;
    lightboxImg.alt = img.alt;
    var title = img.getAttribute('data-title');
    lightboxTitle.textContent = title || '';
    lightboxTitle.style.display = title ? 'block' : 'none';
    lightbox.classList.add('is-active');
    document.body.style.overflow = 'hidden';
  }

  function closeLightbox(e) {
    if (e.target.classList.contains('lightbox') || e.target.classList.contains('lightbox__close')) {
      var lightbox = document.getElementById('lightbox');
      lightbox.classList.remove('is-active');
      document.body.style.overflow = '';
    }
  }

  document.addEventListener('keydown', function (e) {
    if (e.key === 'Escape') {
      var lightbox = document.getElementById('lightbox');
      lightbox.classList.remove('is-active');
      document.body.style.overflow = '';
    }
  });
</script>
```

**Step 2: Build and verify**

Run: `bundle exec jekyll build`
Expected: Clean build, `_site/photography/index.html` exists

**Step 3: Commit**

```bash
git add photography.html
git commit -m "Add photography page with masonry grid and lightbox"
```

---

### Task 5: Create photography SCSS

**Files:**
- Create: `_sass/_photography.scss`
- Modify: `assets/css/main.scss`

**Step 1: Create the photography stylesheet**

Create `_sass/_photography.scss`:

```scss
// Masonry grid
.photo-grid {
  columns: 3;
  column-gap: $space-sm;

  @include tablet {
    columns: 2;
  }

  @include mobile {
    columns: 1;
  }

  &__item {
    break-inside: avoid;
    margin-bottom: $space-sm;
  }

  &__img {
    width: 100%;
    display: block;
    border-radius: 4px;
    cursor: pointer;
    @include transition;

    &:hover {
      opacity: 0.85;
      transform: scale(1.02);
    }
  }
}

// Lightbox overlay
.lightbox {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.9);
  z-index: 1000;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  &.is-active {
    display: flex;
  }

  &__close {
    position: absolute;
    top: $space-sm;
    right: $space-md;
    background: none;
    border: none;
    color: white;
    font-size: 2rem;
    cursor: pointer;
    padding: $space-xs;
    line-height: 1;

    &:hover {
      opacity: 0.7;
    }
  }

  &__img {
    max-width: 90vw;
    max-height: 80vh;
    object-fit: contain;
    border-radius: 4px;
  }

  &__title {
    color: white;
    font-family: $font-serif;
    font-size: $font-size-md;
    margin-top: $space-sm;
    text-align: center;
  }
}
```

**Step 2: Import in main stylesheet**

In `assets/css/main.scss`, add `@import 'photography';` before `'footer'`:

Final `main.scss`:
```scss
@import 'globals';
@import 'nav';
@import 'base';
@import 'intro';
@import 'experience';
@import 'projects';
@import 'photography';
@import 'footer';
```

**Step 3: Build and verify**

Run: `bundle exec jekyll build`
Expected: Clean build, no SCSS errors

**Step 4: Commit**

```bash
git add _sass/_photography.scss assets/css/main.scss
git commit -m "Add photography grid and lightbox styles"
```

---

### Task 6: Add exclude for docs directory

**Files:**
- Modify: `_config.yml:23-29`

**Step 1: Add docs to the exclude list**

Add `'docs'` to the `exclude` array in `_config.yml` so the plans directory is not published:

```yaml
exclude:
  - 'Gemfile'
  - 'Gemfile.lock'
  - '*.gemspec'
  - 'README.md'
  - 'LICENSE.md'
  - 'vendor'
  - 'docs'
```

**Step 2: Build and verify**

Run: `bundle exec jekyll build`
Expected: Clean build, no `docs/` directory in `_site/`

**Step 3: Commit**

```bash
git add _config.yml
git commit -m "Exclude docs directory from build"
```

---

### Task 7: End-to-end verification

**Step 1: Full build**

Run: `bundle exec jekyll build`
Expected: Clean build with no errors or warnings

**Step 2: Serve and manually verify**

Run: `bundle exec jekyll serve --livereload`

Verify:
- [ ] Nav bar appears on homepage with "Christian Sherland" (left) and "Photography" / "Resume" (right)
- [ ] Clicking "Christian Sherland" goes to homepage
- [ ] Clicking "Photography" goes to `/photography/`
- [ ] Clicking "Resume" opens PDF in new tab
- [ ] Photography page shows "Photos coming soon." (no photos added yet)
- [ ] Nav bar appears on photography page too
- [ ] Dark mode works correctly for nav and photography page
- [ ] Mobile responsive: nav links stay inline, grid collapses to 1 column

**Step 3: Final commit (if any fixes needed)**

```bash
git add -A
git commit -m "Fix any issues found during verification"
```
