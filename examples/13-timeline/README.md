# Timeline

Multiple timeline layout variations for process steps or milestones.

## Variations

| File | Style | Best For |
|------|-------|----------|
| `output-1-default.html` | Vertical alternating | Detailed process steps |
| `output-2-horizontal.html` | Horizontal row | Company history, roadmaps |

## 1. Default Vertical

Vertical timeline with alternating left/right layout.

**Features:**
- Steps alternate left and right of center line
- Gradient vertical connecting line
- Numbered circles with color coding
- Date/time labels
- Mobile: collapses to left-aligned

## 2. Horizontal

Horizontal timeline row with numbered milestones.

**Features:**
- Horizontal connecting line
- Numbered step circles
- Year labels above each step
- Progress indication via colors
- Mobile: converts to vertical

## Usage

1. Copy the desired `output-*.html` file
2. Paste into WordPress block editor (Code Editor mode)
3. Update step content, dates, and descriptions
4. Switch to Visual Editor

## Customization

### Change Step Colors

Current/active step (accent):
```json
"backgroundColor":"#3b82f6"
```

Completed steps (green):
```json
"backgroundColor":"#22c55e"
```

Upcoming steps (gray):
```json
"backgroundColor":"#e2e8f0"
```

### Add More Steps

**Vertical:** Duplicate a step block. For left alignment, content goes in first grid column. For right, in third column.

**Horizontal:** Duplicate a step block and update the uniqueId values.

### Show Progress State

- Use accent color for current step
- Use green for completed steps
- Use gray for future/upcoming steps

## Technical Notes

**Vertical Timeline:**
- CSS Grid with `1fr auto 1fr` for balanced columns
- Center line is `::before` pseudo-element
- Circles have white box-shadow to cover line
- Mobile uses `order` property to reposition

**Horizontal Timeline:**
- Flexbox with `space-between` distribution
- Connecting line is positioned absolutely
- Mobile transforms to vertical with CSS
