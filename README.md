# GenerateBlocks Skills

LLM-optimized skill documentation and development resources for the [GenerateBlocks](https://generateblocks.com/) WordPress plugin.

**Source-verified against GenerateBlocks free 2.3 and GB Pro 2.6** (June 2026). Every block schema, attribute order, and dynamic-tag in these skills was checked against the plugin source in this repo — not recalled from training data.

## What the skills can build

- **Static sections** — heroes, pricing, cards, FAQs, CTAs, full landing pages
- **Dynamic content** — query loops (blog grids, related posts, archives), 27 dynamic tags with exact syntax
- **Custom fields** — ACF text/image/link/group fields, repeater loops, options pages, meta queries
- **Animations** — hover micro-interactions, keyframe entrances, CSS scroll-driven reveals, reduced-motion guards
- **Conditions** — GB Pro block/menu conditions, form-field conditions, free-plugin alternatives
- **GB Pro blocks** — accordion, tabs, carousel, navigation, site header, overlays/mega menus, the 2.6 Forms system
- **Full-site templates** — GeneratePress Elements (loop templates, page heroes, hooks, display rules), FSE block themes

## Quick Install

Install skills for your AI coding assistant with one command:

```bash
git clone https://github.com/wpgaurav/generateblocks-skills.git
cd generateblocks-skills
chmod +x install.sh
./install.sh
```

This launches an interactive installer that sets up skills for your preferred tools.

### Supported Tools

| Tool | Install Location | Format |
|------|-----------------|--------|
| **Claude Code** | `~/.claude/skills/` | Skill directories |
| **Cursor** | `~/.cursor/skills/` | Skill directories |
| **Windsurf** | `~/.windsurf/rules/` | Combined markdown |
| **OpenAI Codex CLI** | `~/.codex/instructions.md` | Combined markdown |
| **Gemini CLI** | `GEMINI.md` (project root) | Combined markdown |
| **GitHub Copilot** | `.github/copilot-instructions.md` | Combined markdown |
| **Cline / Roo Code** | `.clinerules` (project root) | Combined markdown |
| **Aider** | `CONVENTIONS.md` (project root) | Combined markdown |

### Install Options

```bash
./install.sh              # Interactive mode (choose tools)
./install.sh --all        # Install for all tools
./install.sh claude       # Claude Code only
./install.sh cursor codex # Multiple tools at once
./install.sh --help       # Show help
```

---

## Alternative: Manual Setup

If you prefer not to use the installer, you have two options:

### Option A: Upload a Skill File

Download a `.skill` file and upload it at the start of a new chat in Claude.ai, ChatGPT, Gemini, or any AI assistant.

| Skill | Download |
|-------|----------|
| **GenerateBlocks Layouts** | [.skill](importable/generateblocks-layouts.skill) |
| **HTML to GenerateBlocks** | [.skill](importable/html-to-generateblocks.skill) |
| **Elementor to GenerateBlocks** | [.skill](importable/elementor-to-generateblocks.skill) |
| **Figma to GenerateBlocks** | [.skill](importable/figma-to-generateblocks.skill) |

### Option B: Point Your AI to the Skill File

With Claude Code, Cursor, or Windsurf, just reference the skill directly:

```
Read skills/generateblocks-layouts/SKILL.md and create a testimonial slider with 3 cards
```

Direct links to skill files:
- [`skills/generateblocks-layouts/SKILL.md`](skills/generateblocks-layouts/SKILL.md)
- [`skills/html-to-generateblocks/SKILL.md`](skills/html-to-generateblocks/SKILL.md)
- [`skills/elementor-to-generateblocks/SKILL.md`](skills/elementor-to-generateblocks/SKILL.md)
- [`skills/figma-to-generateblocks/SKILL.md`](skills/figma-to-generateblocks/SKILL.md)

---

## Copy-Paste Examples

Browse the [`examples/`](examples/) folder for **38 ready-to-use templates** across 14 section types:

| Section | Description |
|---------|-------------|
| [Hero Section](examples/01-hero/) | Stats bar, dual CTAs (5 variations) |
| [Pricing Table](examples/02-pricing/) | 3-tier with "Popular" badge |
| [Card Grid](examples/03-card-grid/) | Blog posts, portfolio |
| [Feature List](examples/04-feature-list/) | 6 features with icons |
| [FAQ Section](examples/05-faq/) | Numbered Q&A, two columns |
| [Testimonials](examples/06-testimonials/) | Quotes, avatars, stars |
| [Sticky CTA](examples/07-sticky-cta/) | Dark banner, dual buttons |
| [Post Grid](examples/08-post-grid/) | Featured + small posts |
| [Stats Section](examples/09-stats/) | 4 metrics on dark bg |
| [Services Grid](examples/10-services/) | Bento layout |
| [Logo Carousel](examples/11-logo-carousel/) | Client/partner logos |
| [Team Grid](examples/12-team-grid/) | Team member cards |
| [Timeline](examples/13-timeline/) | Vertical timeline |
| [Comparison Table](examples/14-comparison-table/) | Feature comparison |

Each folder contains multiple variations plus the prompt that generated them. A fully dynamic query-loop blog grid lives at [`skills/generateblocks-layouts/examples/layouts/query-blog-grid.html`](skills/generateblocks-layouts/examples/layouts/query-blog-grid.html).

**Bonus:** Check out [`examples/from-gauravtiwari-org/`](examples/from-gauravtiwari-org/) for real-world sections exported from production sites.

### Using Examples

1. Open any [`examples/`](examples/) folder
2. Copy any `output-*.html` file content
3. In WordPress, open your page/post
4. Switch to Code Editor (three dots menu > Code Editor)
5. Paste the blocks
6. Switch back to Visual Editor

---

## What's Included

```
generateblocks-skills/
├── install.sh                 # Multi-tool skill installer
├── CLAUDE.md                  # Claude Code project instructions
├── AGENTS.md                  # Universal LLM instructions
├── skills/                    # Skill source files
│   ├── generateblocks-layouts/
│   │   ├── SKILL.md           # Main entry point (slim — depth in references/)
│   │   ├── references/        # 23 reference files (see map below)
│   │   └── examples/          # Basic, compound, layout, SVG examples
│   ├── html-to-generateblocks/
│   ├── elementor-to-generateblocks/
│   └── figma-to-generateblocks/
├── importable/                # .skill and .zip files for upload
├── examples/                  # 38 golden examples across 14 sections
├── generateblocks/            # Plugin source (2.3.0-rc) for reference
└── generateblocks-pro/        # Pro plugin source (2.6.0-rc) for reference
```

### Skills

| Skill | Purpose |
|-------|---------|
| **GenerateBlocks Layouts** | Build anything with GB V2: static sections, query loops, dynamic tags, ACF data, animations, conditions, Pro forms/interactive blocks, full-site templates |
| **HTML to GenerateBlocks** | Convert HTML/CSS to GenerateBlocks block markup |
| **Elementor to GenerateBlocks** | Migrate Elementor layouts to clean GB blocks |
| **Figma to GenerateBlocks** | Convert Figma designs to GB blocks |

### Reference map (generateblocks-layouts/references/)

| File | Covers |
|------|--------|
| `_index.md` | Task router — which file to load for which job |
| `recovery-rules.md` | Every known cause of "Attempt Recovery" errors + exact fixes |
| `field-notes.md` | Real-conversion lessons: escaping workflow, validation scripts |
| `block-types.md` | Element/Text/Media/Shape verified attribute schemas |
| `dynamic-tags.md` | Canonical catalog of all 27 dynamic tags + exact syntax |
| `query-block.md` | Query/Looper/Loop-Item + Pro query extensions |
| `acf-and-custom-fields.md` | ACF fields, repeater loops, options pages, Meta Box |
| `conditions.md` | Pro block/menu/form conditions + free alternatives |
| `template-authoring.md` | Full sites: GeneratePress Elements, FSE, archive templates |
| `animations.md` | Hover, keyframes, scroll-driven animation, reduced motion |
| `gb-pro.md` | Pro feature map (28 blocks, global classes, version timeline) |
| `pro-forms.md` | Pro 2.6 Forms: fields, validation, ESP integrations, Turnstile |
| `pro-interactive.md` | Accordion, Tabs, Carousel, Navigation, Site Header, Overlays |
| `css-patterns.md` · `svg-icons.md` · `responsive.md` | Styling patterns |
| `global-styles.md` · `patterns.md` · `performance.md` · `migrations.md` · `troubleshooting.md` | Supporting guides |

### Importable Formats

The `importable/` folder contains two formats for each skill:
- **`.skill`** — Upload to any AI chat (Claude.ai, ChatGPT, Gemini)
- **`.zip`** — Compressed skill with references included

---

## GenerateBlocks V2 Quick Reference

Four core blocks plus the query family:

```
generateblocks/element    → Containers (div, section, nav, figure, a, ul/ol/li, dl/dt/dd)
generateblocks/text       → Text (h1-h6, p, span, div, a, button, figcaption, li)
generateblocks/media      → Images (img only — static and dynamic)
generateblocks/shape      → SVG icons
generateblocks/query      → Dynamic lists (+ looper, loop-item, query-no-results, query-page-numbers)
```

**The rules that matter most:**

- Use `generateblocks/element` (NOT `/container`), `generateblocks/text` (NOT `/headline` or `/button`)
- `htmlAttributes` is a plain object (`{"href":"https://example.com/about/"}`) — never an array
- **Links**: element `<a>` wrapping a text `span` child. Text `<a>` strips its href on save; element `<a>` with raw text triggers recovery
- JSON attribute order follows each block's `block.json` declaration order, `className` last
- The `css` string is single-line, minified, alphabetized — hover states live in the `styles` object

Block format (Option A — the plugin auto-injects the id-class):

```html
<!-- wp:generateblocks/element {"uniqueId":"hero001","tagName":"section","styles":{...},"css":"...","className":"gb-element"} -->
<section class="gb-element-hero001 gb-element">
    <!-- content -->
</section>
<!-- /wp:generateblocks/element -->
```

**Dynamic tags** — space after the tag name, pipe-separated options, no quotes:

```
{{post_title link:post}}
{{post_permalink}}
{{featured_image size:large}}
{{post_excerpt length:25}}
{{post_date dateFormat:M j, Y}}
{{term_list tax:category|sep:, }}
{{post_meta key:my_acf_field}}
{{post_meta key:repeater.0.subfield}}
```

`{{post_url}}`, `{{featured_image_url}}`, `{{post_terms}}`, `{{acf}}`, and any `key="quoted"` form **do not exist** — they save fine and render as literal text. The full catalog is in [`references/dynamic-tags.md`](skills/generateblocks-layouts/references/dynamic-tags.md).

---

## Example Prompts

> "Create a hero section with headline, subheadline, two CTA buttons, and a 4-stat bar"

> "Build a 3-column pricing table with a highlighted middle tier"

> "Build a related-posts section: 3 posts from the same category, current post excluded"

> "Loop my ACF repeater team_members into a 3-column team grid"

> "Build an archive loop template for GeneratePress Elements that inherits the query"

> "Add a scroll-reveal animation to this section with a reduced-motion fallback"

> "Convert this HTML to GenerateBlocks: [paste HTML]"

---

## Limitations

- **No external URLs** — Provide HTML, screenshots, or descriptions
- **No custom JavaScript** — interactions are CSS-only, plus GB Pro's built-in
  JS blocks (accordion, tabs, carousel, overlays, instant pagination)
- **Placeholder images** — Replace with real images after generation
- **Hover inference** — Interactive states inferred from static designs
- **UI-managed settings** — condition rules, form actions, overlay triggers,
  and display rules are configured in wp-admin, not in block markup; the
  skills list these as manual steps
- **Pro 2.6 / free 2.3 features** (Forms, CSS Mode) are beta as of June 2026 —
  sites on stable releases won't have them yet

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

For non-Claude assistants (GPT, Gemini, etc.), see **`AGENTS.md`** for universal instructions — or just run `./install.sh` to set up your tool automatically.

## License

- Plugin source code: GPL-2.0-or-later
- Skill documentation: MIT
