# Post Grid

Multiple blog post grid layouts for content sections.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Asymmetric featured grid | Homepage blog sections |
| `output-2-magazine.html` | Magazine layout with sidebar | News/blog archives |
| `output-3-minimal-list.html` | Simple date-based list | Personal blogs, changelogs |

## 1. Default Asymmetric Grid

Featured large post with smaller horizontal cards.

**Features:**
- Asymmetric layout: 1 large (2 rows) + 2 small
- Featured post with gradient overlay
- Horizontal card layout for small posts
- Image zoom on hover
- "View all posts" link in header
- Mobile: single column stack

## 2. Magazine Layout

Professional magazine-style with category navigation.

**Features:**
- 2-column layout (featured + sidebar)
- Category tab navigation
- Featured badge on main post
- Small thumbnail article list
- Category labels per article
- Newspaper aesthetic

## 3. Minimal List

Clean text-focused article list.

**Features:**
- Simple date + title + excerpt layout
- Monospace date formatting
- Reading time indicator
- Full-row hover effect
- Centered, narrow container
- Responsive grid to stack

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Replace images, titles, dates, and excerpts
4. Switch to Visual Editor

## Dynamic Content

For dynamic posts using GenerateBlocks Pro:

1. Replace static content with Query Loop block
2. Use Dynamic Tags for post title, excerpt, date
3. Use Dynamic Image for featured image

See [Query Loops reference](../../skills/generateblocks-layouts/references/query-loops.md) for details.

## Customization

- **Equal grid:** Change to `grid-template-columns: repeat(3, 1fr)`
- **Different featured size:** Adjust `grid-row: span 2`
- **Remove header link:** Delete the "View all posts" span
- **Add pagination:** Include pagination block below grid
