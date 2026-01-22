---
name: generateblocks-layouts
version: 2.0.0
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
tags:
  - wordpress
  - generateblocks
  - layouts
  - blocks
references:
  - references/block-types.md
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

Build professional WordPress layouts using GenerateBlocks V2's four core blocks.

## Quick Start

GenerateBlocks V2 uses four block types:

| Block | Class Pattern | Use For |
|-------|--------------|---------|
| `generateblocks/element` | `.gb-element-{id}` | Containers (div, section, article, header, nav, footer) |
| `generateblocks/text` | `.gb-text-{id}` | Text content (h1-h6, p, span, a, button) |
| `generateblocks/media` | `.gb-media-{id}` | Images |
| `generateblocks/shape` | `.gb-shape-{id}` | SVG icons and decorative shapes |

## Block Template

```html
<!-- wp:generateblocks/{type} {json_attributes} -->
<{tag} class="gb-{type} gb-{type}-{uniqueId}">
    {content}
</{tag}>
<!-- /wp:generateblocks/{type} -->
```

## Required Attributes

Every block needs:
- `uniqueId` - Unique identifier (format: `{section}{number}` like `hero001`, `card023`)
- `tagName` - HTML element type
- `styles` - CSS properties as JSON object (camelCase)
- `css` - Generated CSS string (kebab-case, minified)

## Styling Approach

**Always use both `styles` AND `css` attributes:**

```json
{
  "uniqueId": "card001",
  "tagName": "div",
  "styles": {
    "display": "flex",
    "padding": "2rem",
    "backgroundColor": "#ffffff"
  },
  "css": ".gb-element-card001{display:flex;padding:2rem;background-color:#ffffff}"
}
```

**Complex features (hover, media queries) go in `css` only:**

```css
.gb-element-card001{...base styles...}.gb-element-card001:hover{transform:translateY(-6px)}@media(max-width:768px){.gb-element-card001{padding:1rem}}
```

## Unique ID Convention

Format: `{section}{number}{letter}`

- **Section prefix**: 3-4 chars (`hero`, `serv`, `card`, `feat`, `blog`)
- **Number**: 001-999 sequential
- **Letter**: Optional for nested elements (`a`, `b`, `c`)

Examples: `hero001`, `serv023a`, `card014`, `feat007b`

## References

For detailed documentation, see:

- **[Block Types](references/block-types.md)** - Complete attribute specs for all four blocks
- **[CSS Patterns](references/css-patterns.md)** - Hover effects, transitions, gradients, pseudo-elements
- **[SVG Icons](references/svg-icons.md)** - Shape block usage and inline SVG patterns
- **[Responsive](references/responsive.md)** - Media queries and breakpoint patterns
- **[Troubleshooting](references/troubleshooting.md)** - Complex layout handling, chunking, error recovery

## Examples

See `/examples/` folder for copy-paste ready blocks:

- **basic/** - Single blocks (text, buttons, images)
- **compound/** - Combined blocks (cards, features, stats)
- **layouts/** - Full sections (hero, services, grid)
- **svg/** - Icons and decorative shapes

## Key Rules

1. **No HTML comments** - Only WordPress block comments allowed (`<!-- wp:... -->` and `<!-- /wp:... -->`). All other HTML comments break the block editor.
2. **No custom CSS classes** - All styling in block attributes
3. **Minify CSS** - No line breaks in `css` attribute
4. **Include transitions** - Always add `transition:all 0.3s` for interactive elements
5. **Duplicate styles** - Put in both `styles` object AND `css` string
6. **Test responsive** - Add media queries for tablet (1024px) and mobile (768px)

## Design Inference (When CSS Not Provided)

When no CSS values are specified, infer styles based on context:

### GeneratePress Defaults
- Primary: `#0073e6`
- Text: `#222222`
- Body font: `17px`, line-height `1.7`
- H1: `42px`, H2: `35px`, H3: `29px`
- Section padding: `60px`
- Container max-width: `1200px`
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
