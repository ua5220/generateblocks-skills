# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains GenerateBlocks WordPress plugin development and HTML-to-GenerateBlocks conversion skills/documentation.

**Contents:**
- `generateblocks/` - Free plugin source code (V2.2.0)
- `generateblocks-pro/` - Pro plugin source code
- `layout-skills/` - Skill documentation for working with GenerateBlocks

## Development Commands

### GenerateBlocks (Free)

Working directory: `generateblocks/`

```bash
# Build
npm run build              # Production build
npm run start              # Watch mode with hot reload
npm run clean              # Reset dist/ to git state

# Linting
npm run lint:js            # ESLint JavaScript
npm run lint:pkg-json      # Package.json validation

# Testing
npm run test:unit          # Jest unit tests
npm run test:e2e           # Playwright E2E tests
npm run test:e2e:67        # E2E with WordPress 6.7
npm run test:e2e:68        # E2E with WordPress 6.8
npm run test:e2e:trunk     # E2E with WordPress trunk

# Local WordPress
npm run wp-env:start       # Start @wordpress/env
npm run wp-env:stop        # Stop environment
npm run wp-env:clean       # Clean environment

# Package
npm run package            # Create plugin zip
npm run googleFonts        # Download Google Fonts
```

### GenerateBlocks Pro

Working directory: `generateblocks-pro/`

```bash
# Same commands as free version, plus:
npm run plugin-zip         # Create plugin zip
```

## Architecture

### GenerateBlocks V1 vs V2

**V1 (Legacy):** Specific block types
- `generateblocks/container`, `button`, `headline`, `grid`, `image`

**V2 (Current):** Generic element-based blocks
- `generateblocks/element` - Container (div, section, article, header, footer, nav, etc.)
- `generateblocks/text` - Text content (p, h1-h6, span, a, button)
- `generateblocks/media` - Images
- `generateblocks/shape` - SVG shapes/icons

### Plugin Structure

**Frontend (`src/`):**
- `blocks/` - Block implementations (React)
- `components/` - Reusable components (46+)
- `hooks/` - Custom hooks (15+)
- `hoc/` - Higher-order components
- `utils/` - Utilities
- `editor/`, `extend/`, `pattern-library/`, `dynamic-tags/`

**Backend (`includes/`):**
- `blocks/` - PHP block classes (server-side rendering)
- `class-do-css.php` - CSS generation
- `class-enqueue-css.php` - CSS enqueuing
- `class-dynamic-content.php` - Dynamic content
- `class-render-blocks.php` - Block rendering
- `class-query-loop.php` - Query loops
- `pattern-library/`, `dynamic-tags/`

**Build:**
- Webpack with `@wordpress/scripts`
- Custom config in `webpack.config.js`
- `@edge22/*` packages bundled separately
- Output: `dist/`

**Import aliases** (from `jsconfig.json`):
- `@utils/*`, `@components/*`, `@hooks/*`, `@hoc/*`

### Key Dependencies

- `@wordpress/block-editor`, `@wordpress/blocks`, `@wordpress/components`
- `@edge22/block-styles`, `@edge22/components`, `@edge22/styles-builder`
- `@tanstack/react-query`, `colord`, `react-select`, `uuid`

## Block Structure (V2)

```html
<!-- wp:generateblocks/element {"uniqueId":"abc123","tagName":"div","styles":{...},"css":"..."} -->
<div class="gb-element gb-element-abc123">
    <!-- Inner blocks -->
</div>
<!-- /wp:generateblocks/element -->
```

**Attributes:**
- `uniqueId` - Required for CSS targeting
- `tagName` - HTML element type
- `styles` - Object with basic CSS (padding, margin, colors, flex, grid)
- `css` - String with complex CSS (hovers, pseudo-elements, media queries)
- `globalClasses` - Array of global CSS classes
- `htmlAttributes` - Additional HTML attrs (href, target, data-*, aria-*)

## CSS Approaches

**1. PHP-Generated (Standard):** Styles stored as objects, PHP generates CSS at render time

**2. Inline Styles (V2 with `styles` + `css`):** Self-contained styling in block attributes

Use `styles` for: layout, spacing, colors, typography, borders
Use `css` for: hover states, pseudo-elements, media queries, transitions

## Skill References

All skills are in `layout-skills/` directory:

| Skill | File | Purpose |
|-------|------|---------|
| **Layouts** | `generateblocks-layouts.md` | V2 block specs, attributes, layout patterns (hero, cards, grids) |
| **HTML Conversion** | `html-to-generateblocks-v2.md` | Convert HTML/CSS to GenerateBlocks with inline styles |
| **Query Loops** | `generateblocks-query-loops.md` | Dynamic content loops, post grids, archives, pagination |
| **Dynamic Content** | `generateblocks-dynamic-content.md` | Dynamic tags, post meta, ACF fields, templates (Pro) |
| **Responsive** | `generateblocks-responsive.md` | Breakpoints, device-specific layouts, mobile-first design |
| **Global Styles** | `generateblocks-global-styles.md` | Design tokens, theme.json integration, global classes |
| **Patterns** | `generateblocks-patterns.md` | Reusable block patterns, pattern registration |
| **Performance** | `generateblocks-performance.md` | CSS delivery, optimization, critical CSS |
| **Migrations** | `generateblocks-migrations.md` | V1 to V2 migrations, deprecations, backward compatibility |

## Unique ID Convention

Format: `{section}{number}{letter}`
- Section: 3-4 chars (hero, serv, tool, blog)
- Number: 001-999
- Letter: Optional for nested elements (a, b, c)

Examples: `hero001a`, `serv023`, `card014b`
