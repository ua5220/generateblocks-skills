# Team Grid

Multiple team section layouts for showcasing team members.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Hover-reveal bios | Interactive, detailed bios |
| `output-2-simple-cards.html` | Card grid with socials | Clean, simple team pages |

## 1. Default Hover Reveal

Sophisticated team cards with slide-up bio reveal on hover.

**Features:**
- Slide-up reveal animation for bio and social links
- Photo zoom effect on hover
- Staggered fade-in animations
- Social icons with hover color change
- 4 > 2 > 1 column responsive

## 2. Simple Cards

Clean card grid with photos, names, roles, and social links.

**Features:**
- Square photos with 1:1 aspect ratio
- Card lift effect on hover
- Social icons always visible
- LinkedIn and Twitter links
- Light background theme

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Replace photos, names, roles, and social URLs
4. Switch to Visual Editor

## Customization

### Replace Team Photos

Update the `src` attribute in each `generateblocks/media` block:

```html
"src":"https://your-domain.com/team/person.jpg"
```

### Add More Social Links

Duplicate a social link block and change:
- The `href` URL
- The `aria-label` text
- The SVG icon

### Add More Team Members

1. Duplicate an entire team member block
2. Update all uniqueId values with a new prefix
3. Update photo, name, role, and social URLs

## Technical Notes (Variation 1)

- Info panel uses `translateY(calc(100% - 5rem))` to show only name/role
- Hover triggers `translateY(0)` to reveal full content
- Bio text has 0.1s delay, social links have 0.2s delay
- Photo scales to 1.05 with 0.5s duration
- Uses `cubic-bezier(0.16, 1, 0.3, 1)` for smooth easing
