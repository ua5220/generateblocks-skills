# CTA Banners

Multiple CTA banner variations for promotions and conversions.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Dark inline banner | Page sections |
| `output-2-floating-bar.html` | Fixed bottom bar | Site-wide promos |
| `output-3-countdown.html` | Timer with urgency | Flash sales |

## 1. Default Inline Banner

Full-width dark CTA banner with gradient accent bar.

**Features:**
- Dark background with gradient accent bar
- Horizontal layout on desktop
- Primary and secondary CTA buttons
- Mobile: stacked, centered layout
- High contrast for visibility

## 2. Floating Bar

Fixed bottom bar with discount offer and dismiss button.

**Features:**
- Fixed position at bottom
- Slide-up animation on load
- Icon + text + CTA layout
- Dismiss (X) button
- White/light theme
- Promo code display

## 3. Countdown Timer

Urgency-driven CTA with animated countdown.

**Features:**
- Animated rotating background
- Days/Hours/Mins/Secs display
- Glass-morphism timer boxes
- Purple/indigo theme
- Prominent discount badge

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Update headline, description, and button links
4. Switch to Visual Editor

## Making It Sticky

To make variation 1 sticky at the bottom:

```css
.gb-element-cta001 {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 100;
}
```

Or use WordPress Interactivity API for show/hide behavior.

## Customization

- **Change gradient:** Edit the `::before` background
- **Different colors:** Replace color values in styles
- **Single button:** Remove the secondary button block
- **Add close button:** Include an X icon with JS to dismiss
- **Live countdown:** Add JavaScript to update timer values
