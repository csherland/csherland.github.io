# Hamburger Nav Design

## Overview

Replace the inline mobile nav with a hamburger toggle that reveals a slide-down link panel on mobile (480px and below). Desktop stays unchanged.

## Approach

Classic hamburger pattern: a toggle button hidden on desktop, visible on mobile. Tapping it adds an `is-open` class to the nav, which reveals the links stacked vertically with a `max-height` slide-down transition. Pure CSS animation + ~5 lines of inline JS.

## Behavior

**Desktop (above 480px):** No change. Links inline, toggle button hidden via `display: none`.

**Mobile (480px and below):**
- `nav__links` hidden by default (`max-height: 0; overflow: hidden`)
- `nav__toggle` button visible: shows `☰` when closed, `✕` when open
- Toggle button: no background, accent-colored text, same height as nav text
- On `is-open`: `nav__links` slides down below the nav row
- Links are full-width, vertically stacked, padded for tap targets (~44px min height), separated by `var(--color-border)` top borders
- Tapping a link closes the menu (JS removes `is-open`)

## Files Modified

- `_layouts/default.html` — add `<button class="nav__toggle">` to nav markup, add ~5 lines of toggle JS
- `_sass/_nav.scss` — add toggle button styles, mobile media query for hide/show, `max-height` transition

No new files created.
