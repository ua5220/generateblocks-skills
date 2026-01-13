# HTML to GenerateBlocks V2 Conversion

Convert HTML/CSS layouts to GenerateBlocks V2 format using inline styles in block attributes.

## Core Principle

**Use both `styles` and `css` attributes:**
- `styles`: Basic properties (padding, margin, colors, display, flex, grid)
- `css`: Complex features (hover states, pseudo-elements, media queries, transitions, animations)

**Never use BEM or custom classes** - all styling goes in block attributes.

## Block Structure

### Standard Element Block

```html
<!-- wp:generateblocks/element {"uniqueId":"elem001","tagName":"div","styles":{"display":"flex","gap":"1rem","padding":"2rem"},"css":".gb-element-elem001{display:flex;gap:1rem;padding:2rem}@media(max-width:768px){.gb-element-elem001{flex-direction:column}}"} -->
<div class="gb-element gb-element-elem001">
    <!-- Inner content -->
</div>
<!-- /wp:generateblocks/element -->
```

### Text Block (for headings, paragraphs, links)

```html
<!-- wp:generateblocks/text {"uniqueId":"text001","tagName":"h2","styles":{"fontSize":"2rem","fontWeight":"900","color":"#0a0a0a"},"css":".gb-text-text001{font-size:2rem;font-weight:900;color:#0a0a0a}"} -->
<h2 class="gb-text gb-text-text001">Heading Text</h2>
<!-- /wp:generateblocks/text -->
```

### Link as Card Wrapper

```html
<!-- wp:generateblocks/text {"uniqueId":"card001","tagName":"a","htmlAttributes":{"href":"/services/"},"styles":{"display":"flex","flexDirection":"column","padding":"2rem","backgroundColor":"white","borderRadius":"1rem","textDecoration":"none"},"css":".gb-text-card001{display:flex;flex-direction:column;padding:2rem;background-color:white;border-radius:1rem;text-decoration:none;transition:all 0.3s}.gb-text-card001:hover{transform:translateY(-6px);box-shadow:0 20px 60px rgba(0,0,0,0.15)}"} -->
<a class="gb-text gb-text-card001" href="/services/">
    <!-- Card content -->
</a>
<!-- /wp:generateblocks/text -->
```

### Media/Image Block

```html
<!-- wp:generateblocks/media {"uniqueId":"img001","mediaType":"image","htmlAttributes":{"src":"https://example.com/image.jpg","alt":"Description","loading":"lazy","width":"600","height":"400"},"styles":{"display":"block","width":"100%"},"css":".gb-media-img001{display:block;width:100%}"} -->
<img class="gb-media gb-media-img001" src="https://example.com/image.jpg" alt="Description" loading="lazy" width="600" height="400" />
<!-- /wp:generateblocks/media -->
```

## Styles vs CSS Decision Matrix

| Feature | Use `styles` | Use `css` |
|---------|-------------|-----------|
| Layout (display, flex, grid) | ✅ | Also in CSS |
| Spacing (padding, margin, gap) | ✅ | Also in CSS |
| Colors (background, text) | ✅ | Also in CSS |
| Typography (font-size, weight) | ✅ | Also in CSS |
| Basic borders, border-radius | ✅ | Also in CSS |
| Hover states | ❌ | ✅ Only CSS |
| Pseudo-elements (::before, ::after) | ❌ | ✅ Only CSS |
| Media queries | ❌ | ✅ Only CSS |
| Transitions/animations | ❌ | ✅ Only CSS |
| Complex selectors | ❌ | ✅ Only CSS |

**Pattern**: Always put properties in `styles`, then replicate with proper CSS syntax in `css` attribute, adding hover/pseudo/media queries.

## Common Patterns

### Card with Hover Effect

```html
<!-- wp:generateblocks/text {"uniqueId":"card001","tagName":"a","htmlAttributes":{"href":"/link/"},"styles":{"display":"flex","flexDirection":"column","backgroundColor":"white","borderRadius":"1rem","padding":"2rem","border":"1px solid transparent","textDecoration":"none"},"css":".gb-text-card001{display:flex;flex-direction:column;background-color:white;border-radius:1rem;padding:2rem;border:1px solid transparent;text-decoration:none;transition:all 0.3s}.gb-text-card001::after{content:'';position:absolute;bottom:0;left:0;width:100%;height:3px;background:#c0392b;transform:scaleX(0);transform-origin:left;transition:transform 0.4s cubic-bezier(0.16, 1, 0.3, 1)}.gb-text-card001:hover{transform:translateY(-6px);box-shadow:0 20px 60px rgba(0,0,0,0.15);border-color:#e5e5e5}.gb-text-card001:hover::after{transform:scaleX(1)}"} -->
<a class="gb-text gb-text-card001" href="/link/">
    <!-- Card content -->
</a>
<!-- /wp:generateblocks/text -->
```

### Grid Layout (Responsive)

```html
<!-- wp:generateblocks/element {"uniqueId":"grid001","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"repeat(4, 1fr)","gap":"1rem"},"css":".gb-element-grid001{display:grid;grid-template-columns:repeat(4, 1fr);gap:1rem}@media(max-width:1024px){.gb-element-grid001{grid-template-columns:repeat(2, 1fr)!important}}@media(max-width:768px){.gb-element-grid001{grid-template-columns:1fr!important}}"} -->
<div class="gb-element gb-element-grid001">
    <!-- Grid items -->
</div>
<!-- /wp:generateblocks/element -->
```

### Icon Container with Hover

```html
<!-- wp:generateblocks/element {"uniqueId":"icon001","tagName":"div","styles":{"width":"3.5rem","height":"3.5rem","display":"flex","alignItems":"center","justifyContent":"center","backgroundColor":"#f5f5f3","borderRadius":"1rem","color":"#c0392b"},"css":".gb-element-icon001{width:3.5rem;height:3.5rem;display:flex;align-items:center;justify-content:center;background-color:#f5f5f3;border-radius:1rem;color:#c0392b;transition:all 0.3s}.gb-text-parentcard:hover .gb-element-icon001{background-color:#c0392b;color:white;transform:scale(1.05) rotate(-3deg)}"} -->
<div class="gb-element gb-element-icon001">
    <i class="md-icon-bolt" aria-hidden="true"></i>
</div>
<!-- /wp:generateblocks/element -->
```

### Featured Card (Dark, Span Multiple Columns)

```html
<!-- wp:generateblocks/element {"uniqueId":"feat001","tagName":"div","styles":{"gridColumn":"span 2","gridRow":"span 2","background":"linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%)","minHeight":"26rem","position":"relative","display":"flex","flexDirection":"column","gap":"1rem","borderRadius":"1rem","padding":"2rem"},"css":".gb-element-feat001{grid-column:span 2;grid-row:span 2;background:linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%);min-height:26rem;position:relative;display:flex;flex-direction:column;gap:1rem;border-radius:1rem;padding:2rem}.gb-element-feat001::before{content:'';position:absolute;top:0;right:0;width:60%;height:100%;background:radial-gradient(circle at 100% 0%, rgba(192, 57, 43, 0.2) 0%, transparent 60%);pointer-events:none}.gb-element-feat001>*{position:relative;z-index:1}@media(max-width:1024px){.gb-element-feat001{grid-column:span 2;grid-row:span 1;min-height:auto}}@media(max-width:768px){.gb-element-feat001{grid-column:span 1}}"} -->
<div class="gb-element gb-element-feat001">
    <!-- Featured card content -->
</div>
<!-- /wp:generateblocks/element -->
```

### Badge (Absolute Position)

```html
<!-- wp:generateblocks/text {"uniqueId":"badge001","tagName":"span","styles":{"position":"absolute","top":"1rem","right":"1rem","padding":"0.25rem 0.625rem","fontSize":"0.75rem","fontWeight":"600","letterSpacing":"0.05em","textTransform":"uppercase","backgroundColor":"#c0392b","color":"white","borderRadius":"2rem"},"css":".gb-text-badge001{position:absolute;top:1rem;right:1rem;padding:0.25rem 0.625rem;font-size:0.75rem;font-weight:600;letter-spacing:0.05em;text-transform:uppercase;background-color:#c0392b;color:white;border-radius:2rem}"} -->
<span class="gb-text gb-text-badge001">Recommended</span>
<!-- /wp:generateblocks/text -->
```

## Dynamic Content with Query Blocks

For sections with dynamic WordPress posts, use native query blocks with GenerateBlocks for styling:

```html
<!-- wp:query {"queryId":1,"query":{"perPage":12,"postType":"post","order":"desc","orderBy":"date","taxQuery":{"category":{"terms":[],"operator":"NOT IN"}}}} -->
<div class="wp-block-query">
    <!-- wp:post-template {"style":{"spacing":{"blockGap":"1rem"}}} -->

        <!-- Post Card -->
        <!-- wp:generateblocks/text {"uniqueId":"post001","tagName":"a","styles":{"display":"flex","flexDirection":"column","backgroundColor":"white","border":"1px solid #e5e5e5","borderRadius":"1rem","overflow":"hidden","textDecoration":"none"},"css":".gb-text-post001{display:flex;flex-direction:column;background-color:white;border:1px solid #e5e5e5;border-radius:1rem;overflow:hidden;text-decoration:none;transition:all 0.3s}.gb-text-post001:hover{transform:translateY(-6px);box-shadow:0 20px 60px rgba(0,0,0,0.15);border-color:#c0392b}"} -->
        <a class="gb-text gb-text-post001">
            <!-- wp:post-featured-image {"isLink":false,"aspectRatio":"12/5"} /-->

            <!-- wp:generateblocks/element {"uniqueId":"post002","tagName":"div","styles":{"padding":"1rem","display":"flex","flexDirection":"column","flex":"1"},"css":".gb-element-post002{padding:1rem;display:flex;flex-direction:column;flex:1}"} -->
            <div class="gb-element gb-element-post002">
                <!-- wp:post-title {"isLink":false,"style":{"typography":{"fontSize":"1.125rem","fontWeight":"700"}}} /-->
                <!-- wp:post-excerpt {"excerptLength":14} /-->
            </div>
            <!-- /wp:generateblocks/element -->
        </a>
        <!-- /wp:generateblocks/text -->

    <!-- /wp:post-template -->
</div>
<!-- /wp:query -->
```

## Unique ID Convention

- Format: `{section}{number}{letter}` (e.g., `hero001a`, `serv023`, `tool014`)
- Section prefix: 3-4 characters (hero, serv, tool, blog, feat)
- Number: Sequential 001-999
- Optional letter: For nested elements (a, b, c)

## Conversion Workflow

1. **Read original HTML/CSS** - Understand structure and styles
2. **Identify sections** - Break into logical components
3. **Map BEM classes to blocks** - Each `.block__element` becomes a GenerateBlocks element
4. **Extract base styles** - Put in `styles` attribute
5. **Extract complex styles** - Put in `css` attribute (hovers, pseudo, media queries)
6. **Create unique IDs** - Follow convention
7. **Test responsive behavior** - Ensure media queries work
8. **Handle dynamic content** - Use WordPress query blocks

## CSS Syntax Rules

### In `styles` attribute (JavaScript object):
```json
{
  "display": "flex",
  "flexDirection": "column",
  "backgroundColor": "#ffffff",
  "borderRadius": "1rem",
  "marginBottom": "2rem"
}
```

### In `css` attribute (CSS string):
```css
.gb-element-id{display:flex;flex-direction:column;background-color:#ffffff;border-radius:1rem;margin-bottom:2rem}.gb-element-id:hover{background-color:#f5f5f5}@media(max-width:768px){.gb-element-id{flex-direction:row}}
```

**Note**: CSS should be minified (no line breaks, minimal spaces).

## Responsive Patterns

### Mobile-First Grid
```css
.gb-element-grid{display:grid;grid-template-columns:1fr}@media(min-width:768px){.gb-element-grid{grid-template-columns:repeat(2, 1fr)}}@media(min-width:1024px){.gb-element-grid{grid-template-columns:repeat(4, 1fr)}}
```

### Desktop-First Grid (Match Original)
```css
.gb-element-grid{display:grid;grid-template-columns:repeat(4, 1fr);gap:1rem}@media(max-width:1024px){.gb-element-grid{grid-template-columns:repeat(2, 1fr)!important}}@media(max-width:768px){.gb-element-grid{grid-template-columns:1fr!important}}
```

### Sticky Sidebar
```css
.gb-element-sidebar{position:sticky;top:calc(var(--header-height, 80px) + 1rem)}@media(max-width:1024px){.gb-element-sidebar{position:static}}
```

## Common Gotchas

1. **Always escape quotes in CSS strings** - Use single quotes for content, attr values
2. **Duplicate properties** - Put in both `styles` and `css` for consistency
3. **Use !important sparingly** - Only for overriding at breakpoints
4. **Test hover states** - Parent hover affecting child (`.parent:hover .child`)
5. **Pseudo-elements need content** - `content:''` for ::before/::after
6. **Gradients only in CSS** - Can't use in `styles` attribute
7. **CSS variables work** - Use var(--custom-property) freely
8. **Transitions on all** - `transition:all 0.3s` for smooth interactions

## Performance Notes

- Inline styles are fast (no external CSS file)
- Each block's CSS is scoped to its unique ID
- GenerateBlocks automatically deduplicates common styles
- Media queries only load when needed
- Use `content-visibility: auto` for off-screen sections

## Example: Complete Hero Section

See `to-convert/home-hero-v2.html` for a complete real-world example with:
- Complex grid layout
- Multiple nested components
- Responsive breakpoints
- Hover effects
- Icon fonts
- Images with overlays
