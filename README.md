# GenerateBlocks Skills

LLM-optimized skill documentation and development resources for [GenerateBlocks](https://generateblocks.com/) WordPress plugin.

## How to Use These Skills

These skill files teach AI assistants how to work with GenerateBlocks. Here's how to use them with different tools:

### With Claude Code (Terminal/CLI)

If you're using [Claude Code](https://claude.ai/code) in your terminal:

1. Open this folder in your terminal
2. Run `claude` to start Claude Code
3. Claude automatically reads `CLAUDE.md` and understands the skills
4. Just ask: *"Create a hero section with GenerateBlocks"*

### With VS Code + Claude/Copilot Extensions

1. Open this folder in VS Code
2. The `CLAUDE.md` file provides context to AI extensions
3. For best results, open a skill file (e.g., `layout-skills/generateblocks-layouts.md`) in a tab
4. Ask your AI assistant to create GenerateBlocks layouts

### With Claude.ai, ChatGPT, or Gemini (Web)

1. **Copy the skill file content** - Open the skill you need (e.g., `generateblocks-layouts.md`)
2. **Paste into chat** - Start a new conversation and paste the skill content
3. **Ask your question** - Now the AI understands GenerateBlocks format

**Example workflow:**
```
1. Copy contents of layout-skills/generateblocks-layouts.md
2. Paste into Claude/ChatGPT/Gemini
3. Ask: "Create a pricing table with 3 tiers using GenerateBlocks"
4. Copy the output into WordPress block editor (Code view)
```

### With Cursor, Windsurf, or Other AI IDEs

1. Open this folder as your project
2. The AI will read `CLAUDE.md` for context
3. Reference skill files in your prompts: *"Using generateblocks-responsive.md, create a mobile-first grid"*

### Quick Start Example

**Want to create a card grid?**

1. Tell your AI: *"Read layout-skills/generateblocks-layouts.md and create a 3-column card grid with hover effects"*
2. Copy the generated block code
3. In WordPress, open your page/post, switch to Code Editor (three dots menu > Code Editor)
4. Paste the blocks
5. Switch back to Visual Editor to see your layout

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
