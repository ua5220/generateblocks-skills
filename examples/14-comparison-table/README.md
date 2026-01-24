# Comparison Table

Multiple comparison layouts for plans, products, or features.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | CSS Grid table | Feature-heavy comparisons |
| `output-2-stacked-cards.html` | Pricing cards with lists | Simple plan comparison |

## 1. Default Grid Table

Feature comparison table with grid layout.

**Features:**
- CSS Grid for aligned columns
- Highlighted "Most Popular" column
- Checkmarks and X marks for features
- Alternating row backgrounds
- Mobile: horizontal scroll

## 2. Stacked Cards

Pricing cards with feature checklists.

**Features:**
- 3-column card grid
- Dark featured card
- "Most Popular" badge
- Checkmark/X feature lists
- Hover lift effect
- Mobile: stacks vertically

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Update plan names, prices, and features
4. Switch to Visual Editor

## Customization

### Add/Remove Plans (Grid)

Adjust the grid columns:
```css
grid-template-columns: minmax(140px, 1.5fr) repeat(4, minmax(160px, 1fr));
```

### Change Highlighted Plan

Move the accent styling to a different column or card.

### Add More Feature Rows

Duplicate a row (Grid) or list item (Cards) and update content.

### Checkmark/X Icons

Checkmark (green):
```html
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#22c55e" stroke-width="2.5"><path d="M20 6L9 17l-5-5"/></svg>
```

X mark (gray):
```html
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#cbd5e1" stroke-width="2.5"><path d="M18 6L6 18M6 6l12 12"/></svg>
```

## Technical Notes

**Grid Table:**
- First column wider (`1.5fr`) for feature names
- Highlighted column has continuous border
- Uses `overflow-x: auto` for mobile
- `min-width: 640px` prevents collapse

**Stacked Cards:**
- 3-column grid, stacks on mobile
- Featured card uses dark background
- List items use flexbox with icons
