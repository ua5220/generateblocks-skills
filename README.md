# GenerateBlocks Skills

LLM-optimized skill documentation and development resources for [GenerateBlocks](https://generateblocks.com/) WordPress plugin.

## What's Included

### Plugin Source Code

- **`generateblocks/`** - GenerateBlocks free plugin (V2.2.0)

### Not included

- **`generateblocks-pro/`** - GenerateBlocks Pro plugin

### Skill Documentation

The `layout-skills/` directory contains comprehensive guides for working with GenerateBlocks:

| Skill                                                                                | Description                                                                   |
| ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| [generateblocks-layouts.md](layout-skills/generateblocks-layouts.md)                 | V2 block system, element/text/media/shape blocks, attributes, layout patterns |
| [html-to-generateblocks-v2.md](layout-skills/html-to-generateblocks-v2.md)           | Convert HTML/CSS to GenerateBlocks format with inline styles                  |
| [generateblocks-query-loops.md](layout-skills/generateblocks-query-loops.md)         | Dynamic content loops, post grids, archives, pagination                       |
| [generateblocks-dynamic-content.md](layout-skills/generateblocks-dynamic-content.md) | Dynamic tags, post meta, ACF fields, templates (Pro feature)                  |
| [generateblocks-responsive.md](layout-skills/generateblocks-responsive.md)           | Breakpoints, responsive design, mobile-first patterns                         |
| [generateblocks-global-styles.md](layout-skills/generateblocks-global-styles.md)     | Design tokens, theme.json integration, global classes                         |
| [generateblocks-patterns.md](layout-skills/generateblocks-patterns.md)               | Reusable block patterns, PHP registration                                     |
| [generateblocks-performance.md](layout-skills/generateblocks-performance.md)         | CSS delivery optimization, critical CSS strategies                            |
| [generateblocks-migrations.md](layout-skills/generateblocks-migrations.md)           | V1 to V2 migration, deprecations, backward compatibility                      |

## GenerateBlocks V2 Block System

GenerateBlocks V2 uses four generic blocks:

```
generateblocks/element  - Container (div, section, header, footer, nav, etc.)
generateblocks/text     - Text content (p, h1-h6, span, a, button)
generateblocks/media    - Images
generateblocks/shape    - SVG icons and shapes
```

### Block Format

```html
<!-- wp:generateblocks/element {"uniqueId":"abc123","tagName":"section","styles":{...},"css":"..."} -->
<section class="gb-element gb-element-abc123">
    <!-- Inner blocks -->
</section>
<!-- /wp:generateblocks/element -->
```

### Key Attributes

- `uniqueId` - Required identifier for CSS targeting
- `tagName` - HTML element type
- `styles` - Object with basic CSS properties
- `css` - String with complex CSS (hovers, media queries, pseudo-elements)
- `globalClasses` - Array of global CSS class names
- `htmlAttributes` - Additional HTML attributes (href, target, data-*, aria-*)

## Development

### Requirements

- Node.js 18+
- WordPress 6.5+
- PHP 7.2+

### Commands

```bash
cd generateblocks

# Build
npm run build          # Production build
npm run start          # Watch mode

# Test
npm run test:unit      # Jest unit tests
npm run test:e2e       # Playwright E2E tests

# Local WordPress
npm run wp-env:start   # Start local environment
npm run wp-env:stop    # Stop environment

# Package
npm run package        # Create plugin zip
```

## Using with LLMs

These skills are designed for use with LLM assistants (Claude, GPT, etc.) to:

1. **Generate layouts** - Create GenerateBlocks markup from design descriptions
2. **Convert HTML** - Transform existing HTML/CSS into GenerateBlocks format
3. **Build dynamic content** - Set up query loops and dynamic tags
4. **Optimize performance** - Apply best practices for CSS delivery
5. **Migrate content** - Update V1 blocks to V2 format

### Example Prompt

> "Create a 3-column responsive grid of feature cards using GenerateBlocks V2 elements. Each card should have an icon, heading, and description with hover effects."

The LLM will reference `generateblocks-layouts.md` and `generateblocks-responsive.md` to generate proper block markup.

## License

- Plugin source code: GPL-2.0-or-later
- Skill documentation: MIT
