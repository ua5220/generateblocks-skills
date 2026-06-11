---
name: generateblocks-layouts
version: 3.0.0
description: Build any WordPress design with GenerateBlocks V2 — static sections, dynamic query loops, ACF data, animations, conditions, forms, and full-site templates. Source-verified against GB free 2.3 + Pro 2.6.
author: Gaurav Tiwari
trigger:
  - GenerateBlocks
  - GB blocks
  - GB layouts
  - WordPress block layout
  - landing page section
  - blog grid
  - related posts
  - query loop with GenerateBlocks
  - ACF with GenerateBlocks
  - GenerateBlocks template
tags:
  - wordpress
  - generateblocks
  - layouts
  - blocks
---

# GenerateBlocks V2 Layout Builder

Build professional WordPress layouts with GenerateBlocks V2: four core blocks,
the Query/Looper family for dynamic content, dynamic tags for data binding,
and GB Pro for interactive components, forms, and conditions.

## Read first — routing

This file holds only the non-negotiables. The depth lives in `references/`:

1. **`references/_index.md`** — task router. Tells you which files to load.
2. **`references/recovery-rules.md`** — every known cause of "Attempt
   Recovery" errors with the exact fix. **Read on every task.** Skip it and
   you will produce broken markup.
3. The task-specific reference(s) from the router — query loops, dynamic
   tags, ACF, animations, conditions, forms, template authoring, Pro blocks.

If the task involves dynamic data at all, `references/dynamic-tags.md` is
mandatory — the tag syntax is precise and wrong forms fail silently.

## The meta-rule (read this twice)

> The WordPress block editor validates blocks by **re-serializing and
> string-comparing against the markup you pasted**. Any deviation — even
> semantically-equivalent JSON or HTML — is treated as corruption and
> triggers "Attempt Recovery".

Emit what the editor emits, not what you think is correct. That means the
canonical JSON key order, the five string substitutions
(`--`→`\u002d\u002d`, `<`→`\u003c`, `>`→`\u003e`, `&`→`\u0026`,
`\"`→`\u0022`), minified alphabetized CSS, and the exact class lists.
Details: `recovery-rules.md`.

## The blocks

| Block | Class pattern | Use for |
|---|---|---|
| `generateblocks/element` | `gb-element-{id} gb-element` | Containers: div, section, article, header, footer, nav, main, figure, a, ul, ol, li, dl, dt, dd |
| `generateblocks/text` | `gb-text-{id} gb-text` | Text: p, span, div, h1–h6, a, button, figcaption, li |
| `generateblocks/media` | `gb-media-{id} gb-media` | Images (img only). Static AND dynamic loop images |
| `generateblocks/shape` | `gb-shape-{id} gb-shape` | Inline SVG icons/shapes |
| `generateblocks/query` + `looper` + `loop-item` (+ `query-no-results`, `query-page-numbers`) | `gb-query-{id}` etc. | All dynamic post lists — `query-block.md` |

GB Pro adds 28 more (accordion, tabs, carousel, navigation, site-header,
forms) — `gb-pro.md` has the map.

Use core blocks for specialized content: `core/image` (captions),
`core/list`, `core/table`, `core/video`, `core/embed`, `core/paragraph`
(emoji). Full table: `block-types.md` §5.

## The ten commandments

1. **Canonical attribute order per block** (block.json declaration order;
   `className` last). Text block puts `content` 3rd. Pro blocks differ —
   check `pro-interactive.md` before emitting Pro JSON.
2. **The five JSON substitutions** on every string value in block comments.
   Inline HTML `style=""` in the body is NOT JSON — literal characters there.
3. **`className` omits the id-class** (Option A): `"className":"gb-element"`,
   rendered HTML `class="gb-element-{id} gb-element"`. Never duplicate the
   id-class into `className`.
4. **`htmlAttributes` is a plain object**, never an array. Absolute URLs in
   `href`.
5. **`css` string**: single line, minified (`repeat(3,1fr)` not
   `repeat(3, 1fr)`), properties alphabetized, single quotes inside. No
   descendant selectors except pseudo-elements on the block's own selector
   and parent-hover targeting another GB block's own class. No hover/
   transition in `css` — those live in the `styles` object (`&:hover` keys).
6. **Links**: element `<a>` wrapping a text `span` child. Text `<a>` strips
   its href; element `<a>` with raw text triggers recovery. Inline links go
   inside a text block's rich-text content.
7. **Dynamic tags**: `{{tag option:value|option2:value}}` — space after the
   tag name, pipes between options, no quotes ever. `{{post_permalink}}`,
   `{{featured_image size:large}}`, `{{post_meta key:field}}`,
   `{{term_list tax:category}}`. Wrong tags save fine and render as literal
   text — worse than recovery. `dynamic-tags.md` is law.
8. **No HTML comments** other than `<!-- wp:... -->` delimiters. Compact
   nesting — closing comment adjacent to closing tag.
9. **Dynamic loop images** use `generateblocks/media` (+ tag in `src`);
   static captioned images use `core/image`.
10. **Responsive**: `@media (max-width:1024px)` / `(max-width:768px)` keys in
    the `styles` object (also `@supports`, `@container`), mirrored in `css`.
    Never a responsive rule that needs a stripped descendant selector.

## Output requirements

- **Always write generated blocks to a file** (`{section-name}.html`), never
  inline in chat — block code breaks chat formatting and truncates.
- Run the `recovery-rules.md` §7 pre-flight checklist before saving.
- Summarize in chat: what was built, block count, anything needing Pro.

## Unique ID convention

`{section}{number}{letter?}` — 3-4 char prefix + 001-999 + optional nesting
letter: `hero001`, `serv023a`, `card014`. Consistent prefix per section.

## Design inference (when no design is given)

**GeneratePress defaults:** primary `#0073e6`, text `#222222`, body 17px/1.7,
H1 42px / H2 35px / H3 29px, section padding 60px, container
`var(--gb-container-width)`, button padding 15px 30px.

**gauravtiwari.org system:** primary `#c0392b`, text `#0a0a0a`, muted
`#5c5c5c`, light bg `#f5f5f3`, headings weight 900 with tight tracking,
section padding 4rem, card radius 1rem, button radius 2rem, hover lift
`translateY(-6px)`, shadow `0 20px 60px rgba(0,0,0,0.15)`.

## Complex layout strategy

For 50+ block sections: map the structure first, build bottom-up, keep one
ID prefix per component, validate each chunk against the checklist before
assembling. See `troubleshooting.md` for failure recipes.

## Examples

Copy-paste-ready blocks in `examples/`: `basic/` (buttons, containers),
`compound/` (cards), `layouts/` (hero, query blog grid), `svg/` (icons).
Golden full sections live at the repo root `examples/` (14 section types +
production pages from gauravtiwari.org).
