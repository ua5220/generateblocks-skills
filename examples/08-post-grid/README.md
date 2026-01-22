# Post Grid

An asymmetric blog post grid with a featured large post and smaller posts.

## Screenshot

![Post Grid](screenshot.png)

*Screenshot placeholder - paste your rendered output here*

## Features

- Asymmetric layout: 1 large (2 rows) + 2 small
- Featured post with gradient overlay
- Horizontal card layout for small posts
- Image zoom on hover
- "View all posts" link in header
- Mobile: single column stack

## Usage

1. Copy `output.html`
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

- Equal grid: Change to `grid-template-columns: repeat(3, 1fr)`
- Different featured size: Adjust `grid-row: span 2`
- Remove header link: Delete the "View all posts" span
