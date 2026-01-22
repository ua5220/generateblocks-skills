# GenerateBlocks Skills

LLM-optimized skill documentation and development resources for [GenerateBlocks](https://generateblocks.com/) WordPress plugin.

## How to Use These Skills

> Just paste the link https://github.com/wpgaurav/generateblocks-skills to your AI assistant and tell it to read the skill files and it will take care of everything for you.

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
3. For best results, open a skill file (e.g., `skills/generateblocks-layouts/SKILL.md`) in a tab
4. Ask your AI assistant to create GenerateBlocks layouts

### With Claude.ai, ChatGPT, or Gemini (Web)

1. **Copy the skill file content** - Open the skill you need (e.g., `SKILL.md`)
2. **Paste into chat** - Start a new conversation and paste the skill content
3. **Ask your question** - Now the AI understands GenerateBlocks format

**Example workflow:**
```
1. Copy contents of skills/generateblocks-layouts/SKILL.md
2. Paste into Claude/ChatGPT/Gemini
3. Ask: "Create a pricing table with 3 tiers using GenerateBlocks"
4. Copy the output into WordPress block editor (Code view)
```

### With Cursor, Windsurf, or Other AI IDEs

1. Open this folder as your project
2. The AI will read `CLAUDE.md` for context
3. Reference skill files in your prompts: *"Using the responsive reference, create a mobile-first grid"*

### Quick Start Example

**Want to create a card grid?**

1. Tell your AI: *"Read skills/generateblocks-layouts/SKILL.md and create a 3-column card grid with hover effects"*
2. Copy the generated block code
3. In WordPress, open your page/post, switch to Code Editor (three dots menu > Code Editor)
4. Paste the blocks
5. Switch back to Visual Editor to see your layout

## What's Included

### Plugin Source Code

- **`generateblocks/`** - GenerateBlocks free plugin (V2.2.0)

### Not Included

- **`generateblocks-pro/`** - GenerateBlocks Pro plugin (proprietary)

### Skills

The `skills/` directory contains Claude Code skills following the standard folder convention:

| Skill | Folder | Description |
|-------|--------|-------------|
| **GenerateBlocks Layouts** | `skills/generateblocks-layouts/` | V2 blocks, attributes, layout patterns, styling |
| **HTML to GenerateBlocks** | `skills/html-to-generateblocks/` | Convert HTML/CSS to GenerateBlocks format |
| **Elementor to GenerateBlocks** | `skills/elementor-to-generateblocks/` | Migrate Elementor layouts to clean GB blocks |
| **Figma to GenerateBlocks** | `skills/figma-to-generateblocks/` | Convert Figma designs to GB blocks |

#### GenerateBlocks Layouts Skill Structure

```
skills/generateblocks-layouts/
├── SKILL.md              # Main entry point
├── references/           # Detailed documentation
│   ├── block-types.md    # Element, Text, Media, Shape block specs
│   ├── css-patterns.md   # Hover effects, transitions, gradients
│   ├── responsive.md     # Media queries, breakpoints
│   ├── svg-icons.md      # Shape block, inline SVG patterns
│   ├── troubleshooting.md# Complex layouts, error recovery
│   ├── query-loops.md    # Dynamic content loops, pagination
│   ├── dynamic-content.md# Dynamic tags, ACF, Pro templates
│   ├── global-styles.md  # Design tokens, theme.json integration
│   ├── patterns.md       # Block pattern registration
│   ├── performance.md    # CSS delivery optimization
│   └── migrations.md     # V1 to V2 migration guide
└── examples/             # Copy-paste ready blocks
    ├── basic/            # Single blocks (buttons, containers)
    ├── compound/         # Combined blocks (cards, features)
    ├── layouts/          # Full sections (hero, services)
    └── svg/              # Icons and decorative shapes
```

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
- `styles` - Object with basic CSS properties (camelCase)
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
3. **Migrate from Elementor** - Clean up DIVception and convert to semantic blocks
4. **Convert Figma designs** - Transform Figma screenshots/CSS to GB blocks
5. **Build dynamic content** - Set up query loops and dynamic tags
6. **Optimize performance** - Apply best practices for CSS delivery
7. **Migrate V1 to V2** - Update legacy blocks to new format

### Example Prompts

> "Create a 3-column responsive grid of feature cards using GenerateBlocks V2 elements. Each card should have an icon, heading, and description with hover effects."

> "Convert this Elementor hero section to clean GenerateBlocks markup."

> "Build a blog post grid with query loop that shows 6 posts with featured images."

> "Convert this Figma design to GenerateBlocks with responsive breakpoints."

The LLM will reference the appropriate skill files to generate proper block markup.

## Limitations

These skills have some inherent limitations:

- **No external URL access** - LLMs cannot fetch live URLs; provide screenshots, HTML, or descriptions
- **Static output only** - Generated markup is CSS-only; no JavaScript interactions
- **Image placeholders** - Real images must be replaced by user after generation
- **Font assumptions** - Skills assume common web fonts or Google Fonts availability
- **Figma limitations** - Cannot access Figma API directly; requires screenshots or exported CSS
- **Color approximation** - Colors extracted from screenshots may not be exact
- **Hover state inference** - Interactive states must be inferred from static designs

## Importable Skill Files

The `importable/` directory contains standalone `.skill` files that can be directly imported into Claude Code or other AI tools:

| File | Description |
|------|-------------|
| `generateblocks-layouts.skill` | Core layout building skill |
| `html-to-generateblocks.skill` | HTML/CSS conversion skill |
| `elementor-to-generateblocks.skill` | Elementor migration skill |
| `figma-to-generateblocks.skill` | Figma design conversion skill |

### Import into Claude Code

```bash
# Copy to your Claude Code skills directory
cp importable/*.skill ~/.claude/skills/
```

### Use with Other Tools

These `.skill` files are self-contained markdown with YAML frontmatter. They can be:
- Imported into any tool that supports skill files
- Copied and pasted into chat interfaces
- Used as context files in AI IDEs

## Other LLMs

For non-Claude assistants (GPT, Gemini, etc.), see **`AGENTS.md`** for universal instructions that work across all LLM platforms.

## License

- Plugin source code: GPL-2.0-or-later
- Skill documentation: MIT
