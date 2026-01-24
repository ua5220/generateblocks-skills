# Stats Section

Multiple statistics section variations for showcasing metrics.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Dark 4-column grid | Homepage impact sections |
| `output-2-split-content.html` | Text + 2x2 stat cards | About pages, features |
| `output-3-gradient-banner.html` | Horizontal gradient bar | Footers, compact displays |

## 1. Default Dark Grid

4-column statistics on dark background with cards.

**Features:**
- Dark background for impact
- 4-column grid (2x2 on mobile)
- Large, bold numbers with accent color
- Uppercase labels in muted color
- Cards with subtle glass effect
- Hover lift animation

## 2. Split Content Layout

Text content on left with 2x2 stat card grid on right.

**Features:**
- Two-column layout
- Descriptive text with badge and CTA
- 4 stat cards in grid
- One highlighted card (accent color)
- White card shadows
- Light background theme

## 3. Gradient Banner

Horizontal stats bar with gradient background.

**Features:**
- Vibrant gradient background
- Subtle pattern overlay
- Inline horizontal layout
- Vertical dividers between stats
- Compact single-row design
- Mobile: centered, wrapping

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Update numbers and labels
4. Switch to Visual Editor

## Customization

- **Light version:** Change `#0a0a0a` to `#f5f5f3` and adjust text colors
- **Remove cards:** Delete `backgroundColor` and `border` from stat containers
- **Add dividers:** Use `border-right` instead of card backgrounds
- **Animate numbers:** Requires JavaScript (not included)

## Number Formatting

Use `font-variant-numeric: tabular-nums` for consistent number widths, especially if animating or changing values.
