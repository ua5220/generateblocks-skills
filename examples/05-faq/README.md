# FAQ Section

Multiple FAQ layout variations for different use cases.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Numbered two-column grid | Knowledge bases, help pages |
| `output-2-two-column.html` | Sticky sidebar + accordion | Pricing pages, product FAQs |

## 1. Default Numbered Grid

Two-column grid with numbered questions.

**Features:**
- Two-column layout (stacks on mobile)
- Numbered questions with accent color
- Light background cards
- Clean typography hierarchy
- Static display (no accordion)

## 2. Sticky Sidebar + Accordion

Sidebar with heading + native HTML `<details>` accordion.

**Features:**
- Sticky sidebar with heading and CTA
- Native `<details>`/`<summary>` elements (no JS needed)
- Plus icon rotates to X on open
- Border separators between items
- Contact support CTA in sidebar

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Replace questions and answers
4. Switch to Visual Editor

## Customization

- **Add questions:** Duplicate a FAQ block and update content
- **Layout:** Change `grid-template-columns` for different ratios
- **Icons:** Replace plus/minus SVG with chevrons or arrows
- **Open by default:** Add `open` attribute to `<details>` element

## Accordion Note

Variation 2 uses native HTML `<details>` element which provides accordion functionality without JavaScript. For custom animations or "only one open at a time" behavior, you'll need:
- WordPress Interactivity API
- Or JavaScript/jQuery
- Or a dedicated FAQ plugin like ACF FAQ block
