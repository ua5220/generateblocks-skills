# GenerateBlocks Skills

LLM-optimized skill documentation and development resources for [GenerateBlocks](https://generateblocks.com/) WordPress plugin.

## Start Here

**Want to build GenerateBlocks layouts with AI?** Pick your path:

### Quick Start: Copy-Paste Examples

Browse the [`examples/`](examples/) folder for 10 ready-to-use section templates:

| Section | Description | Copy |
|---------|-------------|------|
| [Hero Section](examples/01-hero/) | Stats bar, dual CTAs | [output.html](examples/01-hero/output.html) |
| [Pricing Table](examples/02-pricing/) | 3-tier with "Popular" badge | [output.html](examples/02-pricing/output.html) |
| [Card Grid](examples/03-card-grid/) | Blog posts, portfolio | [output.html](examples/03-card-grid/output.html) |
| [Feature List](examples/04-feature-list/) | 6 features with icons | [output.html](examples/04-feature-list/output.html) |
| [FAQ Section](examples/05-faq/) | Numbered Q&A, two columns | [output.html](examples/05-faq/output.html) |
| [Testimonials](examples/06-testimonials/) | Quotes, avatars, stars | [output.html](examples/06-testimonials/output.html) |
| [Sticky CTA](examples/07-sticky-cta/) | Dark banner, dual buttons | [output.html](examples/07-sticky-cta/output.html) |
| [Post Grid](examples/08-post-grid/) | Featured + small posts | [output.html](examples/08-post-grid/output.html) |
| [Stats Section](examples/09-stats/) | 4 metrics on dark bg | [output.html](examples/09-stats/output.html) |
| [Services Grid](examples/10-services/) | Bento layout | [output.html](examples/10-services/output.html) |

Each example includes the prompt that generated it, so you can learn and modify.

### Full Skill: Teach Your AI

For custom layouts, load the skill file into your AI.

**⚠️ IMPORTANT: You must upload the skill file at the START of every new conversation/thread.**

| Skill | Claude | ChatGPT / Gemini / Others |
|-------|--------|---------------------------|
| **GenerateBlocks Layouts** | [.skill](importable/generateblocks-layouts.skill) | [.zip](importable/generateblocks-layouts.zip) |
| **HTML to GenerateBlocks** | [.skill](importable/html-to-generateblocks.skill) | [.zip](importable/html-to-generateblocks.zip) |
| **Elementor to GenerateBlocks** | [.skill](importable/elementor-to-generateblocks.skill) | [.zip](importable/elementor-to-generateblocks.zip) |
| **Figma to GenerateBlocks** | [.skill](importable/figma-to-generateblocks.skill) | [.zip](importable/figma-to-generateblocks.zip) |

**Direct skill links (for manual copy-paste):**
- [`skills/generateblocks-layouts/SKILL.md`](skills/generateblocks-layouts/SKILL.md)
- [`skills/html-to-generateblocks/SKILL.md`](skills/html-to-generateblocks/SKILL.md)
- [`skills/elementor-to-generateblocks/SKILL.md`](skills/elementor-to-generateblocks/SKILL.md)
- [`skills/figma-to-generateblocks/SKILL.md`](skills/figma-to-generateblocks/SKILL.md)

---

## How to Use

### Option 1: Copy an Example

1. Open any [`examples/`](examples/) folder
2. Copy the `output.html` content
3. In WordPress, open your page/post
4. Switch to Code Editor (three dots menu > Code Editor)
5. Paste the blocks
6. Switch back to Visual Editor

### Option 2: Generate Custom Layouts

**With Claude Code / VS Code:**
```
Read skills/generateblocks-layouts/SKILL.md and create a testimonial slider with 3 cards
```

**With Claude.ai:**
1. Download the [.skill file](importable/generateblocks-layouts.skill)
2. Upload it at the start of a new chat
3. Ask: "Create a pricing table with 3 tiers"

**With ChatGPT / Gemini / Others:**
1. Download the [.zip file](importable/generateblocks-layouts.zip)
2. Upload it at the start of a new chat
3. Ask: "Create a pricing table with 3 tiers"

### Option 3: Clone This Repo

```bash
git clone https://github.com/wpgaurav/generateblocks-skills.git
cd generateblocks-skills
```

With Claude Code: Just run `claude` and it reads `CLAUDE.md` automatically.

---

## What's Included

### Golden Examples (`examples/`)

10 canonical sections with:
- **`prompt.md`** — The exact prompt that generated it
- **`output.html`** — Copy-paste ready GenerateBlocks markup
- **`README.md`** — Usage notes and customization tips

### Skills (`skills/`) and Importable Files (`importable/`)

| Skill | Folder | Claude | ChatGPT/Others |
|-------|--------|--------|----------------|
| **GenerateBlocks Layouts** | [`skills/generateblocks-layouts/`](skills/generateblocks-layouts/) | [.skill](importable/generateblocks-layouts.skill) | [.zip](importable/generateblocks-layouts.zip) |
| **HTML to GenerateBlocks** | [`skills/html-to-generateblocks/`](skills/html-to-generateblocks/) | [.skill](importable/html-to-generateblocks.skill) | [.zip](importable/html-to-generateblocks.zip) |
| **Elementor to GenerateBlocks** | [`skills/elementor-to-generateblocks/`](skills/elementor-to-generateblocks/) | [.skill](importable/elementor-to-generateblocks.skill) | [.zip](importable/elementor-to-generateblocks.zip) |
| **Figma to GenerateBlocks** | [`skills/figma-to-generateblocks/`](skills/figma-to-generateblocks/) | [.skill](importable/figma-to-generateblocks.skill) | [.zip](importable/figma-to-generateblocks.zip) |

#### GenerateBlocks Layouts Skill Structure

```
skills/generateblocks-layouts/
├── SKILL.md              # Main entry point — START HERE
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
└── examples/             # Basic building blocks
    ├── basic/            # Single blocks (buttons, containers)
    ├── compound/         # Combined blocks (cards, features)
    ├── layouts/          # Full sections (hero, services)
    └── svg/              # Icons and decorative shapes
```

### Plugin Source (`generateblocks/`)

Full GenerateBlocks free plugin source (V2.2.0) for reference.

---

## GenerateBlocks V2 Quick Reference

Four block types:

```
generateblocks/element  → Containers (div, section, nav, etc.)
generateblocks/text     → Text (h1-h6, p, span, a, button)
generateblocks/media    → Images
generateblocks/shape    → SVG icons
```

**IMPORTANT V2 Naming:**
- Use `generateblocks/element` (NOT `/container`)
- Use `generateblocks/text` (NOT `/headline` or `/button`)
- Classes MUST be: `gb-element gb-element-{id}` and `gb-text gb-text-{id}`

Block format:

```html
<!-- wp:generateblocks/element {"uniqueId":"hero001","tagName":"section","styles":{...},"css":"..."} -->
<section class="gb-element gb-element-hero001">
    <!-- content -->
</section>
<!-- /wp:generateblocks/element -->

<!-- wp:generateblocks/text {"uniqueId":"hero002","tagName":"h1","styles":{...},"css":"..."} -->
<h1 class="gb-text gb-text-hero002">Heading</h1>
<!-- /wp:generateblocks/text -->
```

Key attributes:
- `uniqueId` — Required for CSS targeting (format: `hero001`, `card023`)
- `tagName` — HTML element type
- `styles` — CSS properties as JSON (camelCase)
- `css` — Complex CSS string (hovers, media queries, pseudo-elements)
- `htmlAttributes` — Additional HTML attributes (href, target, aria-*)

---

## Example Prompts

> "Create a hero section with headline, subheadline, two CTA buttons, and a 4-stat bar"

> "Build a 3-column pricing table with a highlighted middle tier"

> "Make a testimonial grid with avatars, star ratings, and quote marks"

> "Convert this HTML to GenerateBlocks: [paste HTML]"

---

## Limitations

- **No external URLs** — Provide HTML, screenshots, or descriptions
- **Static output** — CSS-only, no JavaScript interactions
- **Placeholder images** — Replace with real images after generation
- **Hover inference** — Interactive states inferred from static designs

---

## Development

```bash
cd generateblocks

npm run build          # Production build
npm run start          # Watch mode
npm run test:unit      # Jest unit tests
npm run test:e2e       # Playwright E2E
npm run wp-env:start   # Local WordPress
```

---

## Other LLMs

For non-Claude assistants (GPT, Gemini, etc.), see **`AGENTS.md`** for universal instructions.

## License

- Plugin source code: GPL-2.0-or-later
- Skill documentation: MIT
