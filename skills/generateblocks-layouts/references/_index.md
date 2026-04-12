---
title: Skill router (read first)
description: Tells Claude which reference file to open for which task. Always check this before generating GenerateBlocks markup.
---

# GenerateBlocks Skill Router

When this skill loads, read this file FIRST. It tells you which reference to
load for the task at hand. Loading the wrong file (or loading too much) wastes
context — be precise.

## Before doing anything

**Always read these two files at the start of every task, no matter the task:**

1. `references/recovery-rules.md` — every cause of "Attempt Recovery" errors
   plus the exact fix. This is the bug-prevention manual. Skip it and you
   will produce broken markup.
2. The relevant task file from the table below.

The base `SKILL.md` has the high-level conventions. The references below
have the depth.

---

## Task → file routing

| If the user is asking for... | Read |
|---|---|
| A static section / hero / cards / grid (no dynamic data) | `block-types.md`, `css-patterns.md` |
| Hover effects, transitions, gradients, pseudo-elements | `css-patterns.md`, `recovery-rules.md` §2 |
| SVG icons or decorative shapes | `svg-icons.md` |
| Responsive layout / breakpoints | `responsive.md`, `recovery-rules.md` §6 |
| **A blog grid, archive page, related posts, or any dynamic post list** | **`query-block.md`** |
| Pagination on a query | `query-block.md` §1.5, §4 |
| Custom post type loop | `query-block.md` §5.3 |
| Accordion / tabs / carousel / sticky header | `gb-pro.md` |
| ACF fields, dynamic tags, conditional visibility | `gb-pro.md` §3, §5 |
| Migrating from V1 (`generateblocks/container`, `/headline`, `/grid`) | `migrations.md` |
| Block patterns / pattern registration | `patterns.md` |
| Global styles, design tokens, theme.json | `global-styles.md` |
| Performance / CSS delivery | `performance.md` |
| Already produced markup that's failing | `troubleshooting.md`, `recovery-rules.md` |

---

## File map

```
references/
├── _index.md              ← you are here
├── recovery-rules.md      ← MUST read every task. Recovery error catalog.
├── block-types.md         ← Element/Text/Media/Shape attribute specs
├── query-block.md         ← V2 Query/Looper/Loop-Item dynamic content
├── gb-pro.md              ← Pro-only blocks, dynamic tags, conditions
├── css-patterns.md        ← Hover, transitions, gradients, pseudo-elements
├── svg-icons.md           ← Shape block + inline SVG patterns
├── responsive.md          ← Media queries, breakpoints (V2)
├── responsive-legacy.md   ← Older breakpoint patterns (reference only)
├── dynamic-content.md     ← Free-plugin dynamic tag basics
├── global-styles.md       ← Design tokens, theme.json bridge
├── patterns.md            ← Block pattern registration
├── performance.md         ← CSS delivery optimization
├── migrations.md          ← V1 → V2 migration guide
├── query-loops.md         ← LEGACY core/query patterns (only if user explicitly wants core blocks)
└── troubleshooting.md     ← Debug recipes for known failures
```

---

## Output rules (always apply)

1. **Output to a file**, never inline in chat. Filename: `{section}-section.html`
   or `{slug}.html`. Place in `output/` if working in this repo, otherwise
   wherever the user wants.
2. **Run the pre-flight checklist** from `recovery-rules.md` §7 against your
   output before saving the file. Every item.
3. **Summarize in chat** what you built — purpose, block count, anything that
   needs Pro, anything you skipped due to a recovery rule.

---

## Decision shortcuts

- **Static image with caption?** → `core/image`, not `generateblocks/media`.
- **Dynamic image inside a loop?** → `generateblocks/media` (it can resolve
  loop context).
- **Action link / button with text?** → `generateblocks/element` `tagName:"a"`
  wrapping a `generateblocks/text` `span` child. Never text `<a>` (strips
  href). Never element `<a>` with raw text (recovery error).
- **List?** → `core/list` with `className:"list"`.
- **Emoji?** → `core/paragraph`. GB renders emoji glyphs incorrectly.
- **Tabs / accordion / carousel?** → GB Pro block. Tell the user it requires
  Pro.
- **Inheriting an archive query?** → `inheritQuery:true` with empty
  `"query":{}`.
- **Need a CSS variable in JSON?** → escape it: `var(\u002d\u002dgb-foo)`.
- **Need a CSS variable in inline `style=""`?** → literal: `var(--gb-foo)`.

If you're not sure which file to load, default to: `recovery-rules.md` +
`block-types.md` + the task-specific file from the table above. That's
the minimum viable context for any GenerateBlocks task.
