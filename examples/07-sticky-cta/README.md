# Sticky CTA Banner

A full-width CTA banner for inline use or sticky positioning.

## Screenshot

![Sticky CTA](screenshot.png)

*Screenshot placeholder - paste your rendered output here*

## Features

- Dark background with gradient accent bar
- Horizontal layout on desktop
- Primary and secondary CTA buttons
- Mobile: stacked, centered layout
- High contrast for visibility

## Usage

1. Copy `output.html`
2. Paste into WordPress block editor (Code Editor mode)
3. Update headline, description, and button links
4. Switch to Visual Editor

## Making It Sticky

To make this banner sticky at the bottom:

1. Add this CSS to your theme:
```css
.gb-element-cta001 {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 100;
}
```

2. Or use WordPress Interactivity API for show/hide behavior

## Customization

- Change gradient: Edit the `::before` background
- Different colors: Replace `#0a0a0a` and `#c0392b`
- Single button: Remove the secondary button block
- Add close button: Include an X icon with JS to dismiss
