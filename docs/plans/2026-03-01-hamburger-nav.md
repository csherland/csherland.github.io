# Hamburger Nav Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a hamburger toggle to the mobile nav that reveals links in a slide-down panel.

**Architecture:** Modify existing nav to wrap in a flex container with `flex-wrap: wrap`. On mobile, toggle button appears and links panel collapses/expands via `max-height` transition. ~5 lines of inline JS toggle an `is-open` class.

**Tech Stack:** Jekyll, SCSS, vanilla JS

---

### Task 1: Add hamburger toggle button to nav markup

**Files:**
- Modify: `_layouts/default.html:8-13`

**Step 1: Add toggle button inside the nav element**

Replace lines 8-13 of `_layouts/default.html`:

```html
    <nav class="nav" id="nav">
      <a href="{{ site.baseurl }}/" class="nav__home">Christian Sherland</a>
      <button class="nav__toggle" aria-label="Toggle menu" aria-expanded="false" onclick="toggleNav()">
        <span class="nav__toggle-icon"></span>
      </button>
      <div class="nav__links">
        <a href="{{ site.baseurl }}/photography/">Photography</a>
        <a href="{{ site.baseurl }}/assets/resume.pdf" target="_blank" rel="noopener">Resume</a>
      </div>
    </nav>
```

**Step 2: Add toggle JS before the closing `</body>` tag**

Add this script block after the existing IntersectionObserver script (before `</body>` on line 32):

```html
    <script>
      function toggleNav() {
        var nav = document.getElementById('nav');
        var toggle = nav.querySelector('.nav__toggle');
        var isOpen = nav.classList.toggle('is-open');
        toggle.setAttribute('aria-expanded', isOpen);
      }
    </script>
```

**Step 3: Commit**

```bash
git add _layouts/default.html
git commit -m "Add hamburger toggle button to nav markup"
```

---

### Task 2: Style the hamburger toggle and mobile slide-down

**Files:**
- Modify: `_sass/_nav.scss` (full rewrite)

**Step 1: Rewrite `_sass/_nav.scss` with the following content**

```scss
.nav {
  max-width: $site-max-width;
  margin: 0 auto;
  padding: $space-sm $space-md;
  display: flex;
  flex-wrap: wrap;
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

  &__toggle {
    display: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: $space-xs;
    width: 2rem;
    height: 2rem;
    position: relative;

    @include mobile {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }

  &__toggle-icon,
  &__toggle-icon::before,
  &__toggle-icon::after {
    display: block;
    width: 1.25rem;
    height: 2px;
    background: var(--color-body-text);
    border-radius: 1px;
    transition: transform 0.2s ease, opacity 0.2s ease;
  }

  &__toggle-icon {
    position: relative;

    &::before,
    &::after {
      content: '';
      position: absolute;
      left: 0;
    }

    &::before {
      top: -6px;
    }

    &::after {
      top: 6px;
    }
  }

  // Animate to X when open
  &.is-open &__toggle-icon {
    background: transparent;

    &::before {
      top: 0;
      transform: rotate(45deg);
    }

    &::after {
      top: 0;
      transform: rotate(-45deg);
    }
  }

  &__links {
    display: flex;
    gap: $space-md;

    @include mobile {
      flex-basis: 100%;
      flex-direction: column;
      gap: 0;
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.25s ease;
    }

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

      @include mobile {
        padding: $space-sm 0;
        border-top: 1px solid var(--color-border);
      }
    }
  }

  // Expand links when open
  &.is-open &__links {
    @include mobile {
      max-height: 10rem;
    }
  }
}
```

**Step 2: Build and verify**

Run: `bundle exec jekyll build`
Expected: Clean build, no SCSS errors

**Step 3: Commit**

```bash
git add _sass/_nav.scss
git commit -m "Style hamburger toggle and mobile slide-down nav"
```

---

### Task 3: Verify and push

**Step 1: Serve locally**

Run: `bundle exec jekyll serve --livereload`

Verify:
- [ ] Desktop: nav looks unchanged, no toggle button visible
- [ ] Mobile (resize to <480px): toggle button appears, links hidden
- [ ] Tap toggle: links slide down, icon animates to X
- [ ] Tap again: links slide up, icon returns to hamburger
- [ ] Links have border separators and adequate tap targets
- [ ] Dark mode: toggle icon and links render correctly

**Step 2: Push**

```bash
git push
```
