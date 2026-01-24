# Card Grid

Multiple card grid layout variations for portfolios, services, and features.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Standard 3-column with badges | Blog posts, portfolios |
| `output-2-hover-reveal.html` | Image cards with hover reveal | Creative portfolios |
| `output-3-masonry-style.html` | CSS Grid masonry layout | Agency portfolios |

## 1. Default Grid

Standard 3-column card grid with category badges and hover effects.

**Features:**
- 3-column grid (2 on tablet, 1 on mobile)
- Featured image with zoom on hover
- Category badge overlay
- Animated accent border reveal from left
- "Read more" link with arrow animation

## 2. Hover Reveal

Image-based cards with content revealed on hover.

**Features:**
- Full-bleed images with 4:5 aspect ratio
- Gradient overlay on hover
- Content slides up from bottom
- Category badges with color coding
- Smooth cubic-bezier transitions

## 3. Masonry Style

CSS Grid-based masonry layout with varying card sizes.

**Features:**
- Grid with `grid-auto-rows` for masonry effect
- Featured card spans 2x2
- Labels reveal on hover
- Consistent hover behavior across all cards
- Responsive breakpoints

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Replace placeholder images and content
4. Switch to Visual Editor

## Customization

- **Columns:** Adjust `grid-template-columns` values
- **Card sizes:** Change `grid-column` and `grid-row` span values
- **Colors:** Update category badge colors
- **Images:** Replace Unsplash URLs with your images
- **Hover effects:** Modify transition timing and transforms
