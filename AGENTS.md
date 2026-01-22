# AGENTS.md

Universal agent instructions for working with GenerateBlocks skills. Compatible with Claude, GPT, Gemini, and other LLM assistants.

## Overview

This repository contains skills for generating WordPress block markup using GenerateBlocks V2. These skills teach you how to create layouts, convert designs, and migrate from other builders.

## Available Skills

| Skill | Location | Purpose |
|-------|----------|---------|
| GenerateBlocks Layouts | `skills/generateblocks-layouts/SKILL.md` | Build layouts with V2 blocks |
| HTML to GenerateBlocks | `skills/html-to-generateblocks/SKILL.md` | Convert HTML/CSS to GB format |
| Elementor to GenerateBlocks | `skills/elementor-to-generateblocks/SKILL.md` | Migrate Elementor layouts |
| Figma to GenerateBlocks | `skills/figma-to-generateblocks/SKILL.md` | Convert Figma designs |

## How to Use

1. **Read the relevant SKILL.md file** based on the task
2. **Follow the patterns and rules** defined in the skill
3. **Reference the examples** in `skills/generateblocks-layouts/examples/`
4. **Use the reference docs** in `skills/generateblocks-layouts/references/`

## GenerateBlocks V2 Quick Reference

### Four Block Types

```
generateblocks/element  - Containers (div, section, header, footer, nav)
generateblocks/text     - Text content (p, h1-h6, span, a, button)
generateblocks/media    - Images
generateblocks/shape    - SVG icons and shapes
```

### Block Structure

```html
<!-- wp:generateblocks/{type} {"uniqueId":"id","tagName":"tag","styles":{...},"css":"..."} -->
<tag class="gb-{type} gb-{type}-id">
    content
</tag>
<!-- /wp:generateblocks/{type} -->
```

### Required Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `uniqueId` | string | Unique identifier (e.g., `hero001`, `card023a`) |
| `tagName` | string | HTML element type |
| `styles` | object | CSS properties in camelCase |
| `css` | string | Minified CSS string with selector |

### Styling Pattern

Always include BOTH `styles` and `css`:

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

### Complex CSS (hover, media queries)

Put in `css` attribute only:

```css
.gb-element-card001{...base...}.gb-element-card001:hover{transform:translateY(-6px)}@media(max-width:768px){.gb-element-card001{padding:1rem}}
```

## Critical Rules

1. **No HTML comments** - Only `<!-- wp:... -->` block comments allowed
2. **Minify CSS** - No line breaks in `css` attribute
3. **Duplicate styles** - Put properties in both `styles` and `css`
4. **Add transitions** - `transition:all 0.3s` for interactive elements
5. **Responsive media queries** - Always add tablet (1024px) and mobile (768px)

## Unique ID Convention

Format: `{section}{number}{letter}`

- Section: 3-4 chars (`hero`, `serv`, `card`, `feat`, `blog`)
- Number: 001-999
- Letter: Optional for nested elements (a, b, c)

Examples: `hero001`, `hero001a`, `card023`, `feat007b`

## Class Patterns

| Block Type | Class Pattern |
|------------|---------------|
| element | `.gb-element-{uniqueId}` |
| text | `.gb-text-{uniqueId}` |
| media | `.gb-media-{uniqueId}` |
| shape | `.gb-shape-{uniqueId}` |

## Default Design System

When no specific styles provided, use:

**Colors:**
- Primary: `#c0392b`
- Text: `#0a0a0a`
- Muted: `#5c5c5c`
- Background: `#ffffff`
- Light BG: `#f5f5f3`
- Border: `#e5e5e5`

**Typography:**
- H1: `clamp(2rem, 5vw, 3.5rem)`, weight `900`
- H2: `clamp(1.5rem, 3vw, 2.5rem)`, weight `700`
- Body: `1rem`, line-height `1.7`
- Letter-spacing headings: `-0.03em`

**Spacing:**
- Section padding: `4rem` (desktop), `2rem` (mobile)
- Container max-width: `1200px`
- Gap: `1rem` to `2rem`

**Effects:**
- Card radius: `1rem`
- Button radius: `2rem`
- Hover lift: `translateY(-6px)`
- Shadow: `0 20px 60px rgba(0,0,0,0.15)`
- Transition: `all 0.3s`

## Task Selection

| User Request | Skill to Use |
|--------------|--------------|
| "Create a hero section" | generateblocks-layouts |
| "Build a card grid" | generateblocks-layouts |
| "Convert this HTML" | html-to-generateblocks |
| "Convert this Elementor page" | elementor-to-generateblocks |
| "Implement this Figma design" | figma-to-generateblocks |
| "Make this responsive" | generateblocks-layouts + references/responsive.md |
| "Add hover effects" | references/css-patterns.md |
| "Create a blog loop" | references/query-loops.md |

## Detailed References

For comprehensive documentation, read these files:

- `skills/generateblocks-layouts/references/block-types.md` - All block specs
- `skills/generateblocks-layouts/references/css-patterns.md` - Hover, gradients, animations
- `skills/generateblocks-layouts/references/responsive.md` - Media queries
- `skills/generateblocks-layouts/references/svg-icons.md` - Shape block usage
- `skills/generateblocks-layouts/references/query-loops.md` - Dynamic content
- `skills/generateblocks-layouts/references/troubleshooting.md` - Common issues

## Examples

Copy-paste ready examples in:

- `skills/generateblocks-layouts/examples/basic/` - Single blocks
- `skills/generateblocks-layouts/examples/compound/` - Cards, features
- `skills/generateblocks-layouts/examples/layouts/` - Full sections
- `skills/generateblocks-layouts/examples/svg/` - Icons

## Limitations

- **Cannot access external URLs** - User must provide content/screenshots
- **Cannot execute CSS** - Output is static markup
- **Image placeholders** - Use placeholder URLs, user replaces with real images
- **Font availability** - Assume common web fonts or Google Fonts
- **No JavaScript** - GenerateBlocks is CSS-only (no JS interactions)
