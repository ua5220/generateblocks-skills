# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains GenerateBlocks WordPress plugin development and skills for creating/converting layouts.

**Contents:**
- `generateblocks/` - Free plugin source code (V2.2.0)
- `generateblocks-pro/` - Pro plugin source code
- `skills/` - Claude Code skills for GenerateBlocks workflows

## Skills

All skills follow Claude Code skill folder convention with `SKILL.md` as the main entry point.

### Available Skills

| Skill | Folder | Purpose |
|-------|--------|---------|
| **GenerateBlocks Layouts** | `skills/generateblocks-layouts/` | Build layouts using GenerateBlocks V2 blocks |
| **HTML to GenerateBlocks** | `skills/html-to-generateblocks/` | Convert HTML/CSS to GenerateBlocks format |
| **Elementor to GenerateBlocks** | `skills/elementor-to-generateblocks/` | Migrate Elementor layouts to clean GB blocks |
| **Figma to GenerateBlocks** | `skills/figma-to-generateblocks/` | Convert Figma designs to GB blocks |

### Skill Structure

```
skills/
├── generateblocks-layouts/
│   ├── SKILL.md              # Main skill entry (V2 blocks, styling, patterns)
│   ├── references/           # Detailed documentation
│   │   ├── block-types.md    # Element, Text, Media, Shape specs
│   │   ├── css-patterns.md   # Hover, transitions, gradients
│   │   ├── responsive.md     # Media queries, breakpoints
│   │   ├── svg-icons.md      # Shape block, inline SVG
│   │   ├── troubleshooting.md# Complex layouts, chunking
│   │   ├── query-loops.md    # Dynamic content loops
│   │   ├── dynamic-content.md# Dynamic tags, ACF, Pro templates
│   │   ├── global-styles.md  # Design tokens, theme.json
│   │   ├── patterns.md       # Block pattern registration
│   │   ├── performance.md    # CSS delivery optimization
│   │   └── migrations.md     # V1 to V2 migration guide
│   └── examples/             # Copy-paste ready blocks
│       ├── basic/            # Single blocks (buttons, containers)
│       ├── compound/         # Combined blocks (cards, features)
│       ├── layouts/          # Full sections (hero, services)
│       └── svg/              # Icons and decorative shapes
├── html-to-generateblocks/
│   ├── SKILL.md              # Conversion workflow, patterns
│   └── CLAUDE.md             # Trigger conditions
├── elementor-to-generateblocks/
│   ├── SKILL.md              # DIVception cleanup, widget mapping
│   └── CLAUDE.md             # Trigger conditions
└── figma-to-generateblocks/
    ├── SKILL.md              # Figma CSS mapping, design inference
    └── CLAUDE.md             # Trigger conditions
```

### Using Skills

**Trigger phrases:**
- "GenerateBlocks", "GB blocks", "GB layouts" → `generateblocks-layouts`
- "HTML to GenerateBlocks", "convert to GB" → `html-to-generateblocks`
- "Elementor to GenerateBlocks", "convert Elementor" → `elementor-to-generateblocks`
- "Figma to GenerateBlocks", "convert Figma design" → `figma-to-generateblocks`

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

**IMPORTANT V2 Naming:**
- Use `generateblocks/element` (NOT `/container`)
- Use `generateblocks/text` (NOT `/headline` or `/button`)
- Classes MUST be: `gb-element gb-element-{uniqueId}` for element blocks
- Classes MUST be: `gb-text gb-text-{uniqueId}` for text blocks
- Classes MUST be: `gb-media gb-media-{uniqueId}` for media blocks
- Classes MUST be: `gb-shape gb-shape-{uniqueId}` for shape blocks

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

## Unique ID Convention

Format: `{section}{number}{letter}`
- Section: 3-4 chars (hero, serv, tool, blog)
- Number: 001-999
- Letter: Optional for nested elements (a, b, c)

Examples: `hero001a`, `serv023`, `card014b`
