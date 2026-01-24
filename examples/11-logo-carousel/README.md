# Logo Showcase

Multiple logo display variations for client/partner social proof.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Infinite scroll marquee | Many logos, dynamic feel |
| `output-2-static-grid.html` | 6-column responsive grid | Fewer logos, clean layout |

## 1. Default Marquee

Continuous scrolling logo carousel with CSS animation.

**Features:**
- Infinite scroll animation using CSS keyframes
- Grayscale to color hover effect
- Gradient edge fades for seamless effect
- Pause on hover for better UX
- Responsive speed adjustment

## 2. Static Grid

Simple responsive grid of logos.

**Features:**
- 6-column grid (4 tablet, 3 mobile, 2 small)
- Grayscale to color on hover
- Scale up effect on hover
- Simple, clean layout
- No animation/JavaScript needed

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Replace logo images with your client logos
4. Switch to Visual Editor

## Customization

### Replace Logos

Update the `src` attribute in each `generateblocks/media` block:

```html
"src":"https://your-domain.com/logo.png"
```

### Adjust Marquee Speed (Variation 1)

Change the animation duration in the CSS:

```css
animation: logoScroll 30s linear infinite  /* slower */
animation: logoScroll 15s linear infinite  /* faster */
```

### Add More Logos

**Marquee:** Duplicate a logo block and update uniqueId, then duplicate in second set for seamless looping.

**Grid:** Simply add more logo wrapper elements.

## Technical Notes

- Marquee: Logos duplicated for seamless infinite loop
- Animation translates by -50% of total width
- Edge gradients use pseudo-elements
- `pointer-events: none` allows hover on logos through gradient
