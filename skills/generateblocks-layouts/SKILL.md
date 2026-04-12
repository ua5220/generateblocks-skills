---
name: generateblocks-layouts
version: 2.1.0
description: Build layouts using GenerateBlocks V2 elements for WordPress
author: Gaurav Tiwari
trigger:
  - GenerateBlocks
  - GB blocks
  - GB layouts
  - HTML to GenerateBlocks
  - convert to GB
  - WordPress block layout
  - landing page section
  - blog grid
  - related posts
  - query loop with GenerateBlocks
tags:
  - wordpress
  - generateblocks
  - layouts
  - blocks
references:
  - references/_index.md
  - references/recovery-rules.md
  - references/block-types.md
  - references/query-block.md
  - references/gb-pro.md
  - references/css-patterns.md
  - references/svg-icons.md
  - references/responsive.md
  - references/troubleshooting.md
examples:
  - examples/basic/
  - examples/compound/
  - examples/layouts/
  - examples/svg/
---

# GenerateBlocks V2 Layout Builder

Build professional WordPress layouts using GenerateBlocks V2's four core blocks
plus the Query / Looper / Loop-Item family for dynamic content.

## Read first

**Before generating any markup**, read these in order:

1. `references/_index.md` — task router. Tells you which other files to load.
2. `references/recovery-rules.md` — every known cause of "Attempt Recovery"
   errors with the exact fix. This is non-negotiable. Skip it and you will
   produce broken markup.
3. The task-specific reference from `_index.md`.

The most common task-specific files:
- Static layout → `block-types.md`
- Blog grid / dynamic content → `query-block.md`
- Tabs / accordion / sticky header / ACF / conditions → `gb-pro.md`

## The meta-rule (read this twice)

> The WordPress block editor validates blocks by **re-serializing the
> attributes and string-comparing against the markup you pasted**. Any
> deviation — even semantically-equivalent JSON or HTML — is treated as
> corruption and triggers "Attempt Recovery".

This is the unifying principle. Every rule in `recovery-rules.md` exists to
make your output **byte-identical** to what the editor itself would emit.

That means matching:
- The JSON key order from `block.json` declarations
- The four substitutions (`--` `<` `>` `&` → `\u002d\u002d` `\u003c` `\u003e` `\u0026`) on every JSON string
- CSS minification and alphabetization
- The class list including any auto-injected duplicates
- The exact spacing and quoting

If you remember nothing else: **emit what the editor emits, not what you
think is correct**.

## Output Requirements

**ALWAYS output generated blocks to a file, never inline in the chat.**

- Output filename: `{section-name}.html` (e.g., `hero-section.html`, `services-grid.html`)
- For multiple sections: Create separate files or one combined file
- Include a brief summary in chat describing what was created

**Why file output?**
- Block code is often 100+ lines and breaks chat formatting
- Easier to copy/paste into WordPress
- Prevents truncation of long outputs
- Allows incremental building of complex layouts

## Quick Start

GenerateBlocks V2 uses four block types:

| Block | Class Pattern | Use For |
|-------|--------------|---------|
| `generateblocks/element` | `.gb-element-{id}` | Containers (div, section, article, header, nav, footer) |
| `generateblocks/text` | `.gb-text-{id}` | Text content (h1-h6, p, span, a, button) |
| `generateblocks/media` | `.gb-media-{id}` | Images (static only, no dynamic features) |
| `generateblocks/shape` | `.gb-shape-{id}` | SVG icons and decorative shapes |

## When to Use Core Blocks

For elements not available in GenerateBlocks or requiring advanced media features, use WordPress Core Blocks:

| Content Type | Use Core Block | Why |
|--------------|----------------|-----|
| Images with captions | `core/image` | Built-in caption support |
| Image galleries | `core/gallery` | Lightbox, columns, captions |
| Videos | `core/video` | Native video player, controls |
| Embedded media | `core/embed` | YouTube, Vimeo, Twitter, etc. |
| Audio files | `core/audio` | Native audio player |
| File downloads | `core/file` | Download links with filename |
| Tables | `core/table` | Structured data tables |
| Lists | `core/list` | Semantic ul/ol with `.list` class |
| Quotes | `core/quote` | Blockquote with citation |
| Code blocks | `core/code` | Preformatted code display |
| Separators | `core/separator` | Horizontal rules |
| Buttons (grouped) | `core/buttons` | Multiple button layouts |
| Columns (simple) | `core/columns` | Quick equal-width layouts |
| Cover images | `core/cover` | Background images with overlays |
| Dynamic post content | `core/post-*` | Post title, excerpt, featured image, etc. |
| Query loops | `core/query` | Dynamic content from posts |
| **Emojis** | `core/paragraph` | GenerateBlocks doesn't render emojis properly |

**Rule of thumb:** Use GenerateBlocks for layout structure and custom styling. Use Core Blocks for specialized content types and media with built-in functionality.

## Block Template

```html
<!-- wp:generateblocks/{type} {json_attributes} -->
<{tag} class="gb-{type} gb-{type}-{uniqueId}">
    {content}
</{tag}>
<!-- /wp:generateblocks/{type} -->
```

**Element blocks** use `"className":"gb-element"` (just the base class — the plugin auto-injects the id-class). The rendered HTML class is `gb-element-{id} gb-element`:
```html
<!-- wp:generateblocks/element {"uniqueId":"card001","tagName":"div","styles":{...},"css":"...","className":"gb-element"} -->
<div class="gb-element-card001 gb-element">...</div>
<!-- /wp:generateblocks/element -->
```

## Required Attributes

Every block needs (in this canonical key order, from `block.json`):

1. `uniqueId` — Unique identifier (`{section}{number}` like `hero001`, `card023`)
2. `tagName` — HTML element type (omitted for `shape`, which uses `html` instead)
3. `content` — **(text block only, position 3)** the rich text content
4. `styles` — CSS properties as JSON object (camelCase). Supports responsive keys like `"@media (max-width:1024px)":{...}`
5. `css` — Generated CSS string (kebab-case, minified, alphabetically sorted)
6. `globalClasses` — Array of global CSS class slugs (optional)
7. `htmlAttributes` — Plain object of attribute key-value pairs (for links, IDs, data attributes)
8. Block-specific extras: `align` (element), `mediaId` (media), `queryType, paginationType, query, inheritQuery` (query), `midSize` (query-page-numbers), `icon, iconLocation, iconOnly` (text)
9. `className` — **always last.** WordPress core attribute. Should NOT include the auto-injected `gb-{type}-{uniqueId}` class — the plugin adds that itself.

**Per-block exceptions**:
- **Text block**: `content` is at position 3, BEFORE `styles`. This is the only block that breaks the standard order.
- **Shape block**: no `tagName`. `html` is at position 2.
- **Query block**: `queryType, paginationType, query, inheritQuery` come after `htmlAttributes`.

See `references/recovery-rules.md` §3.4 for the verified per-block declaration order.

### className: do NOT include the id-class

The plugin auto-injects `gb-{type}-{uniqueId}` into the rendered HTML class
list whenever `styles` is non-empty. If you also put it in `className`, the
rendered HTML has it **twice** and you trigger recovery on save.

```json
// RIGHT — let the plugin auto-inject
"className":"gb-element"
"className":"gb-element alignfull"

// WRONG — duplicates the id-class
"className":"gb-element-card001 gb-element"
```

The rendered HTML class list always shows the id-class once (auto-injected
at the front) followed by whatever is in `className`:

```html
<div class="gb-element-card001 gb-element">           <!-- ✓ -->
<header class="gb-element-hero001 gb-element alignfull"> <!-- ✓ -->
```

## CRITICAL: htmlAttributes Format

**htmlAttributes MUST be a plain object, NOT an array:**

```json
// ✅ CORRECT - Plain object
"htmlAttributes": {"href": "https://example.com/page/", "target": "_blank", "id": "section-id"}

// ❌ WRONG - Array of objects (causes block editor recovery errors)
"htmlAttributes": [
  {"attribute": "href", "value": "/contact/"},
  {"attribute": "target", "value": "_blank"}
]
```

**Use full absolute URLs**, not relative paths. The block editor saves links as absolute URLs; relative paths get converted on save, causing a mismatch that triggers block recovery.

```json
// ✅ CORRECT
"htmlAttributes": {"href": "https://gauravtiwari.org/services/web-development/"}

// ❌ WRONG
"htmlAttributes": {"href": "/services/web-development/"}
```

### The link block pattern (read carefully)

There are two failure modes to avoid:

- **`generateblocks/text` with `tagName:"a"`** strips its `href` on save. The
  href does not survive the round trip. Don't use it for action links.
- **`generateblocks/element` with `tagName:"a"` containing raw text** triggers
  recovery — element blocks expect inner blocks, not text content.

**The correct pattern: element `<a>` wrapping a `text` span child.**

```html
<!-- wp:generateblocks/element {"uniqueId":"link1","tagName":"a","styles":{"display":"inline-block","color":"#c0392b"},"css":".gb-element-link1{color:#c0392b;display:inline-block}","htmlAttributes":{"href":"https://example.com/page/?a=1\u0026b=2"},"className":"gb-element"} -->
<a class="gb-element-link1 gb-element" href="https://example.com/page/?a=1&amp;b=2">
    <!-- wp:generateblocks/text {"uniqueId":"link2","tagName":"span","content":"Read more →","styles":{},"css":""} -->
    <span class="gb-text-link2 gb-text">Read more →</span>
    <!-- /wp:generateblocks/text -->
</a>
<!-- /wp:generateblocks/element -->
```

This works because:
- The element `<a>` has an inner block child → no "raw text" recovery
- `htmlAttributes.href` on the element block survives save → href preserved
- The text child is a `span`, not an `a` → no href stripping
- The arrow is a literal `→` in the text — never use a CSS `\2192` escape
- `&` in the JSON `htmlAttributes` is escaped as `\u0026`
- The same `&` in the rendered HTML `href` is escaped as `&amp;`
- `className` is just `"gb-element"` (no id-class duplicate) — the plugin
  auto-injects `gb-element-link1` so the rendered HTML has it once
- JSON key order: `uniqueId, tagName, styles, css, htmlAttributes, className`
  (canonical block.json order, with `className` last)

For inline links inside a paragraph, write the `<a>` directly in the rich text
content of a single `generateblocks/text` paragraph block — don't wrap each
inline link in its own block.

See `references/recovery-rules.md` §4 for the full explanation.

## Styling Approach

**Always use both `styles` AND `css` attributes:**

```json
{
  "uniqueId": "card001",
  "tagName": "div",
  "styles": {
    "backgroundColor": "#ffffff",
    "display": "flex",
    "padding": "2rem"
  },
  "css": ".gb-element-card001{background-color:#ffffff;display:flex;padding:2rem}"
}
```

**CSS rules:**
- The `css` attribute contains **only base styles** - no hover states, no transitions (the plugin generates those from the `styles` object)
- CSS properties must be **alphabetically sorted**
- Exceptions that go in `css`: pseudo-elements (::before/::after), media queries, animations, parent hover targeting children

```css
/* Base styles only (alphabetically sorted) + pseudo-elements + media queries */
.gb-element-card001{background-color:#ffffff;border-radius:1rem;display:flex;padding:2rem;position:relative}.gb-element-card001::after{content:'';position:absolute;bottom:0;left:0;width:100%;height:3px;background:#c0392b;transform:scaleX(0)}@media(max-width:768px){.gb-element-card001{padding:1rem}}
```

**Parent hover targeting children** is written in the child's `css`:
```css
.gb-element-card001:hover .gb-text-title001{color:#c0392b}
```

## Responsive Design

**Desktop-first approach with standard breakpoints:**

| Breakpoint | Width | Use For |
|------------|-------|---------|
| Desktop | 1025px+ | Default styles (no media query) |
| Tablet | 768px - 1024px | `@media(max-width:1024px)` |
| Mobile | < 768px | `@media(max-width:768px)` |

**Two approaches for responsive styles:**

1. **In `styles` object** (preferred for simple overrides):
```json
{
  "styles": {
    "display": "grid",
    "gridTemplateColumns": "minmax(0, 1fr) minmax(0, 1fr)",
    "gap": "4rem",
    "@media (max-width:1024px)": {
      "gridTemplateColumns": "minmax(0, 1fr)"
    }
  }
}
```

2. **In `css` string** (for complex responsive rules):
```css
.gb-element-hero001{display:grid;gap:4rem;grid-template-columns:minmax(0,1fr) minmax(0,1fr)}@media (max-width:1024px){.gb-element-hero001{grid-template-columns:minmax(0,1fr)}}
```

**Common responsive patterns:**
- Grid to single column: `grid-template-columns:minmax(0,1fr) minmax(0,1fr)` → `grid-template-columns:minmax(0,1fr)`
- Reduce padding: `padding:6rem 0` → `padding:4rem 0` → `padding:3rem 0`
- Reduce font sizes: Use `clamp()` for fluid typography
- Stack flex items: `flex-direction:row` → `flex-direction:column`
- Adjust gaps: `gap:4rem` → `gap:2rem`
- Center text on mobile: `text-align:left` → `text-align:center`

## Full-Width Section Pattern

For full-width sections with contained inner content:

```html
<!-- wp:generateblocks/element {"uniqueId":"hero001","tagName":"section","styles":{...},"css":"...","align":"full","className":"gb-element-hero001 gb-element alignfull"} -->
<section class="gb-element-hero001 gb-element alignfull">
    <!-- wp:generateblocks/element {"uniqueId":"hero002","tagName":"div","styles":{"maxWidth":"var(\u002d\u002dgb-container-width)","marginLeft":"auto","marginRight":"auto"},"css":".gb-element-hero002{margin-left:auto;margin-right:auto;max-width:var(\u002d\u002dgb-container-width)}","className":"gb-element-hero002 gb-element"} -->
    <div class="gb-element-hero002 gb-element">
        <!-- Inner content -->
    </div>
    <!-- /wp:generateblocks/element -->
</section>
<!-- /wp:generateblocks/element -->
```

**Key:**
- Outer section: `"align":"full"` + `"className":"gb-element-hero001 gb-element alignfull"`
- Inner container: `maxWidth: "var(\u002d\u002dgb-container-width)"` (unicode-escaped `--gb-container-width`)

## Unique ID Convention

Format: `{section}{number}{letter}`

- **Section prefix**: 3-4 chars (`hero`, `serv`, `card`, `feat`, `blog`)
- **Number**: 001-999 sequential
- **Letter**: Optional for nested elements (`a`, `b`, `c`)

Examples: `hero001`, `serv023a`, `card014`, `feat007b`

## References

Start with `_index.md` to figure out which file you need.

- **[Skill router](references/_index.md)** — task → file mapping. Read first.
- **[Recovery rules](references/recovery-rules.md)** — every cause of "Attempt Recovery" with the exact fix. **Read on every task.**
- **[Block types](references/block-types.md)** — attribute specs for element/text/media/shape
- **[Query block](references/query-block.md)** — V2 dynamic content: query, looper, loop-item, no-results, page-numbers
- **[GenerateBlocks Pro](references/gb-pro.md)** — Pro-only blocks, dynamic tags, conditions, transforms, effects
- **[CSS patterns](references/css-patterns.md)** — hover effects, gradients, pseudo-elements
- **[SVG icons](references/svg-icons.md)** — shape block usage and inline SVG
- **[Responsive](references/responsive.md)** — media queries and breakpoint patterns
- **[Troubleshooting](references/troubleshooting.md)** — debug recipes for known failures

## Examples

See `/examples/` folder for copy-paste ready blocks:

- **basic/** - Single blocks (text, buttons, images)
- **compound/** - Combined blocks (cards, features, stats)
- **layouts/** - Full sections (hero, services, grid)
- **svg/** - Icons and decorative shapes

## CRITICAL: No Extra HTML Comments

**⛔ NEVER add HTML comments other than WordPress block markers.**

The ONLY allowed comments are WordPress block delimiters:
- `<!-- wp:generateblocks/element {...} -->` and `<!-- /wp:generateblocks/element -->`
- `<!-- wp:generateblocks/text {...} -->` and `<!-- /wp:generateblocks/text -->`
- `<!-- wp:generateblocks/media {...} -->` and `<!-- /wp:generateblocks/media -->`
- `<!-- wp:generateblocks/shape {...} -->` and `<!-- /wp:generateblocks/shape -->`
- `<!-- wp:image {...} -->` and `<!-- /wp:image -->`
- `<!-- wp:video {...} -->` and `<!-- /wp:video -->`
- `<!-- wp:embed {...} -->` and `<!-- /wp:embed -->`
- Any other `<!-- wp:{namespace}/{block} -->` format

**WRONG - These will break the block editor:**
```html
<!-- Hero Section -->
<!-- Card container -->
<!-- Button wrapper -->
<!-- This is a heading -->
<!-- Content goes here -->
```

**CORRECT - Only block delimiters:**
```html
<!-- wp:generateblocks/element {"uniqueId":"hero001",...,"className":"gb-element"} -->
<section class="gb-element-hero001 gb-element">
    <!-- wp:generateblocks/text {"uniqueId":"hero002",...} -->
    <h1 class="gb-text gb-text-hero002">Heading</h1>
    <!-- /wp:generateblocks/text -->
</section>
<!-- /wp:generateblocks/element -->
```

Any extra HTML comments will **break the WordPress block editor** and cause parsing errors. This is non-negotiable.

## Key Rules

1. **No custom CSS classes** — all styling in block attributes
2. **Minify CSS** — no line breaks in `css` attribute, no spaces inside function args (`repeat(3,1fr)` not `repeat(3, 1fr)`)
3. **CSS = base styles only** — no `transition` declarations and no `:hover` rules in the `css` string. The plugin generates both from the `styles` object. Exceptions: pseudo-elements, media queries, animations, parent-hover targeting children
4. **Alphabetically sort CSS properties** inside each rule block in the `css` string
5. **Duplicate styles** — most properties go in both the `styles` object AND the `css` string
6. **Escape `--` as `\u002d\u002d`** anywhere it appears inside JSON strings (both `styles` values and `css` strings) — literal `--` in JSON triggers recovery on save. Inline `style=""` on rendered HTML can use literal `var(--foo)` because it's not JSON
7. **No descendant selectors in `css`** — selectors like `.gb-text-x small{}` or `.gb-text-x .u{}` get stripped and trigger recovery. Put inline `style=""` on the inner `<small>`, `<code>`, `<strong>`, `<span>` directly. Allowed exceptions: pseudo-elements on the block's own selector, and parent-hover targeting another GB block by its own generated class
8. **No CSS `\xxxx` escape sequences** — write literal characters: `content:'→'`, not `content:'\2192'`. Better: put arrows in the rendered text content, not in pseudo-elements
9. **Action links: element `<a>` wrapping a `text` span child** — text `<a>` strips its href on save, element `<a>` with raw text triggers recovery. The `element-a wraps text-span` pattern avoids both. See SKILL.md "The link block pattern" section
10. **`htmlAttributes` is always a plain object** — `{"href":"..."}` never `[{"attribute":"href","value":"..."}]`
11. **Full absolute URLs in `htmlAttributes.href`** — relative paths get canonicalized on save → recovery
12. **`&` encoding** — keep literal `&` in JSON `htmlAttributes`, encode as `&amp;` in the rendered HTML `<a href="...">` body
13. **`className` must include the uniqueId class** — `"gb-element-{uniqueId} gb-element"`, never just `"gb-element"`
14. **Compact nesting** — closing comment adjacent to closing tag, no blank lines inside empty blocks
15. **No HTML comments other than WP block delimiters** — no section labels, no TODOs
16. **Test responsive** — add media queries for tablet (1024px) and mobile (768px). Avoid responsive rules that would need descendant selectors in `css`
17. **Static images with captions → `core/image`**, NOT `generateblocks/media`. `generateblocks/media` is for dynamic loop images. Plain static images without captions can use either, but `core/image` is more reliable
18. **Lists → `core/list`** with `className:"list"`
19. **Emojis → `core/paragraph`** — GB renders emoji glyphs incorrectly
20. **Dynamic content → `query-block.md`** — use `generateblocks/query` + `looper` + `loop-item` for any post list, archive, or related-posts section
21. **Shape block** — use `styles.svg` for SVG-specific props OR simple `styles` with inline SVG attributes; both work
22. **Use `var(\u002d\u002dgb-container-width)` for inner containers** with `align:"full"` on the parent section for full-width layouts
23. **SVG attribute order** — the editor reorders attributes: `stroke-linejoin`, `stroke-linecap`, `stroke-width`, `stroke`, `fill`, `viewBox`, `height`, `width`. Write them in that order.

For the full list of recovery causes and the exact fix for each, see
`references/recovery-rules.md`.

## Design Inference (When CSS Not Provided)

When no CSS values are specified, infer styles based on context:

### GeneratePress Defaults
- Primary: `#0073e6`
- Text: `#222222`
- Body font: `17px`, line-height `1.7`
- H1: `42px`, H2: `35px`, H3: `29px`
- Section padding: `60px`
- Container max-width: `var(--gb-container-width)`
- Button padding: `15px 30px`

### gauravtiwari.org Design System
- Primary: `#c0392b`
- Text: `#0a0a0a`, Muted: `#5c5c5c`
- Background: `#ffffff`, Light: `#f5f5f3`
- Headings: font-weight `900`, tight letter-spacing
- Section padding: `4rem`
- Card radius: `1rem`, Button radius: `2rem`
- Hover lift: `translateY(-6px)`
- Shadow: `0 20px 60px rgba(0,0,0,0.15)`

## Complex Layout Strategy

For large sections (50+ blocks), break into chunks:

1. **Plan structure first** - Map components before coding
2. **Build bottom-up** - Start with innermost elements
3. **Test incrementally** - Verify each component works
4. **Use consistent IDs** - Same prefix for related elements

See [Troubleshooting](references/troubleshooting.md) for detailed guidance on complex layouts.
