# Stats Section

A 4-column statistics section with large numbers on a dark background.

## Screenshot

![Stats Section](screenshot.png)

*Screenshot placeholder - paste your rendered output here*

## Features

- Dark background for impact
- 4-column grid (2x2 on mobile)
- Large, bold numbers with accent color
- Uppercase labels in muted color
- Cards with subtle glass effect
- Hover lift animation

## Usage

1. Copy `output.html`
2. Paste into WordPress block editor (Code Editor mode)
3. Update numbers and labels
4. Switch to Visual Editor

## Customization

- Light version: Change `#0a0a0a` to `#f5f5f3` and adjust text colors
- Remove cards: Delete `backgroundColor` and `border` from stat containers
- Add dividers: Use `border-right` instead of card backgrounds
- Animate numbers: Requires JavaScript (not included)

## Number Formatting

Use `font-variant-numeric: tabular-nums` for consistent number widths, especially if animating or changing values.
