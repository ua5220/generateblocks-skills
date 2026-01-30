---
title: Block Types Reference
description: Complete attribute specifications for GenerateBlocks V2 blocks
---

# Block Types Reference

GenerateBlocks V2 provides four block types. Each has specific attributes and behaviors.

## 1. Element Block (`generateblocks/element`)

Container block for layout structure. Can nest other blocks inside.

### Attributes

```json
{
  "uniqueId": "elem001",
  "tagName": "div",
  "styles": {},
  "css": "",
  "globalClasses": [],
  "htmlAttributes": [],
  "className": ""
}
```

### Available Tag Names

| Tag | Use For |
|-----|---------|
| `div` | Generic container (default) |
| `section` | Major page section |
| `article` | Self-contained content |
| `aside` | Sidebar content |
| `header` | Introductory content |
| `footer` | Footer content |
| `nav` | Navigation links |
| `main` | Main page content |
| `figure` | Image with caption |
| `a` | Clickable wrapper |
| `ul`, `ol` | Lists |
| `li` | List items |

### Class Pattern

```html
<div class="gb-element gb-element-{uniqueId}">
```

**Note:** Base `gb-element` class is only added when `className` is set. Otherwise just `gb-element-{uniqueId}`.

### Examples

**Basic Container:**
```html
<!-- wp:generateblocks/element {"uniqueId":"cont001","tagName":"div","styles":{"padding":"2rem","backgroundColor":"#f5f5f5"},"css":".gb-element-cont001{padding:2rem;background-color:#f5f5f5}"} -->
<div class="gb-element gb-element-cont001">
    <!-- Child blocks -->
</div>
<!-- /wp:generateblocks/element -->
```

**Full-Width Section with Container:**
```html
<!-- wp:generateblocks/element {"uniqueId":"sect001","tagName":"section","styles":{"backgroundColor":"#0a0a0a","paddingTop":"6rem","paddingBottom":"6rem","paddingLeft":"1.5rem","paddingRight":"1.5rem","position":"relative","overflow":"hidden"},"css":".gb-element-sect001{background-color:#0a0a0a;overflow:hidden;position:relative;padding:6rem 1.5rem}","align":"full","className":"gb-element alignfull"} -->
<section class="gb-element-sect001 gb-element alignfull">
    <!-- wp:generateblocks/element {"uniqueId":"sect002","tagName":"div","styles":{"maxWidth":"var(\u002d\u002dgb-container-width)","marginLeft":"auto","marginRight":"auto"},"css":".gb-element-sect002{margin-left:auto;margin-right:auto;max-width:var(\u002d\u002dgb-container-width)}","className":"gb-element"} -->
    <div class="gb-element-sect002 gb-element">
        <!-- Section content -->
    </div>
    <!-- /wp:generateblocks/element -->
</section>
<!-- /wp:generateblocks/element -->
```

**Grid with Responsive Styles:**
```html
<!-- wp:generateblocks/element {"uniqueId":"grid001","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"minmax(0, 1fr) minmax(0, 1fr)","gap":"4rem","alignItems":"center","@media (max-width:1024px)":{"gridTemplateColumns":"minmax(0, 1fr)"}},"css":".gb-element-grid001{align-items:center;display:grid;gap:4rem;grid-template-columns:minmax(0,1fr) minmax(0,1fr)}@media (max-width:1024px){.gb-element-grid001{grid-template-columns:minmax(0,1fr)}}","className":"gb-element"} -->
<div class="gb-element-grid001 gb-element">
    <!-- Grid items -->
</div>
<!-- /wp:generateblocks/element -->
```

**Link Wrapper:**
```html
<!-- wp:generateblocks/element {"uniqueId":"link001","tagName":"a","htmlAttributes":[{"attribute":"href","value":"/page/"},{"attribute":"target","value":"_blank"},{"attribute":"rel","value":"noopener"}],"styles":{"display":"block","textDecoration":"none"},"css":".gb-element-link001{display:block;text-decoration:none}"} -->
<a class="gb-element gb-element-link001" href="/page/" target="_blank" rel="noopener">
    <!-- Clickable content -->
</a>
<!-- /wp:generateblocks/element -->
```

---

## 2. Text Block (`generateblocks/text`)

Text content with any inline/block tag. Leaf block - cannot contain inner blocks.

### Attributes

```json
{
  "uniqueId": "text001",
  "tagName": "p",
  "styles": {},
  "css": "",
  "globalClasses": [],
  "htmlAttributes": [],
  "className": "",
  "icon": "",
  "iconLocation": "before",
  "iconOnly": false
}
```

### Available Tag Names

| Tag | Use For |
|-----|---------|
| `p` | Paragraphs (default) |
| `span` | Inline text |
| `div` | Block text container |
| `h1` - `h6` | Headings |
| `a` | Links (inline or block) |
| `button` | Buttons |
| `figcaption` | Image captions |
| `li` | List items |

### Class Pattern

```html
<p class="gb-text gb-text-{uniqueId}">
```

**Note:** `gb-text` base class is always present.

### Examples

**Heading:**
```html
<!-- wp:generateblocks/text {"uniqueId":"head001","tagName":"h1","styles":{"fontSize":"clamp(2rem, 5vw, 3.5rem)","fontWeight":"900","lineHeight":"1.1","letterSpacing":"-0.03em","color":"#0a0a0a"},"css":".gb-text-head001{font-size:clamp(2rem, 5vw, 3.5rem);font-weight:900;line-height:1.1;letter-spacing:-0.03em;color:#0a0a0a}"} -->
<h1 class="gb-text gb-text-head001">Page Title</h1>
<!-- /wp:generateblocks/text -->
```

**Paragraph:**
```html
<!-- wp:generateblocks/text {"uniqueId":"para001","tagName":"p","styles":{"fontSize":"1.125rem","lineHeight":"1.7","color":"#5c5c5c"},"css":".gb-text-para001{font-size:1.125rem;line-height:1.7;color:#5c5c5c}"} -->
<p class="gb-text gb-text-para001">Body text goes here.</p>
<!-- /wp:generateblocks/text -->
```

**Button Link:**
```html
<!-- wp:generateblocks/text {"uniqueId":"btn001","tagName":"a","styles":{"display":"inline-flex","alignItems":"center","gap":"0.5rem","padding":"0.875rem 1.75rem","backgroundColor":"#c0392b","color":"#ffffff","borderRadius":"2rem","fontSize":"1rem","fontWeight":"600","textDecoration":"none"},"css":".gb-text-btn001{align-items:center;background-color:#c0392b;border-radius:2rem;color:#ffffff;display:inline-flex;font-size:1rem;font-weight:600;gap:0.5rem;padding:0.875rem 1.75rem;text-decoration:none}"} -->
<a class="gb-text gb-text-btn001">Get Started</a>
<!-- /wp:generateblocks/text -->
```

**IMPORTANT: Text `<a>` blocks do NOT use `htmlAttributes` for `href`.** GenerateBlocks manages the link URL internally through the editor UI. Do not add `href` to the JSON attributes or the HTML tag. The `css` attribute should only contain base styles (alphabetically sorted), not hover states or transitions - those are managed by the `styles` object and generated by the plugin.

**Card as Link (Element Block):**

For clickable cards that contain inner blocks, use `generateblocks/element` with `tagName: "a"`:

```html
<!-- wp:generateblocks/element {"uniqueId":"card001","tagName":"a","htmlAttributes":[{"attribute":"href","value":"/services/"}],"styles":{"display":"flex","flexDirection":"column","padding":"2rem","backgroundColor":"white","borderRadius":"1rem","textDecoration":"none","border":"1px solid transparent"},"css":".gb-element-card001{background-color:white;border:1px solid transparent;border-radius:1rem;display:flex;flex-direction:column;padding:2rem;text-decoration:none}"} -->
<a class="gb-element gb-element-card001" href="/services/">
    <!-- Inner blocks go here -->
</a>
<!-- /wp:generateblocks/element -->
```

**Note:** Element blocks with `tagName: "a"` DO use `htmlAttributes` for `href` since they are container blocks that wrap inner blocks. Only text `<a>` blocks omit `htmlAttributes`.

---

## 3. Media Block (`generateblocks/media`)

Images only. Self-closing tag.

### Attributes

```json
{
  "uniqueId": "img001",
  "tagName": "img",
  "mediaType": "image",
  "mediaId": 0,
  "styles": {},
  "css": "",
  "globalClasses": [],
  "htmlAttributes": [
    {"attribute": "src", "value": ""},
    {"attribute": "alt", "value": ""},
    {"attribute": "width", "value": ""},
    {"attribute": "height", "value": ""},
    {"attribute": "loading", "value": "lazy"}
  ],
  "linkHtmlAttributes": []
}
```

### HTML Attributes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `src` | Yes | Image URL |
| `alt` | Yes | Alt text (accessibility) |
| `width` | Recommended | Image width |
| `height` | Recommended | Image height |
| `loading` | Optional | `lazy` or `eager` |

### Class Pattern

```html
<img class="gb-media gb-media-{uniqueId}" />
```

**Note:** No base `gb-media` class when no styles applied.

### Examples

**Basic Image:**
```html
<!-- wp:generateblocks/media {"uniqueId":"img001","mediaType":"image","htmlAttributes":[{"attribute":"src","value":"https://example.com/photo.jpg"},{"attribute":"alt","value":"Description"},{"attribute":"width","value":"800"},{"attribute":"height","value":"600"},{"attribute":"loading","value":"lazy"}]} -->
<img src="https://example.com/photo.jpg" alt="Description" width="800" height="600" loading="lazy" />
<!-- /wp:generateblocks/media -->
```

**Styled Image:**
```html
<!-- wp:generateblocks/media {"uniqueId":"img002","mediaType":"image","htmlAttributes":[{"attribute":"src","value":"https://example.com/photo.jpg"},{"attribute":"alt","value":"Hero image"},{"attribute":"loading","value":"eager"},{"attribute":"width","value":"1200"},{"attribute":"height","value":"800"}],"styles":{"width":"100%","height":"100%","objectFit":"cover","borderRadius":"1rem"},"css":".gb-media-img002{width:100%;height:100%;object-fit:cover;border-radius:1rem}"} -->
<img class="gb-media gb-media-img002" src="https://example.com/photo.jpg" alt="Hero image" loading="eager" width="1200" height="800" />
<!-- /wp:generateblocks/media -->
```

**Linked Image:**
```html
<!-- wp:generateblocks/media {"uniqueId":"img003","mediaType":"image","htmlAttributes":[{"attribute":"src","value":"https://example.com/product.jpg"},{"attribute":"alt","value":"Product"}],"linkHtmlAttributes":[{"attribute":"href","value":"/product/"},{"attribute":"target","value":"_blank"},{"attribute":"rel","value":"noopener"}],"styles":{"display":"block","borderRadius":"0.5rem"},"css":".gb-media-img003{display:block;border-radius:0.5rem;transition:transform 0.3s}.gb-media-img003:hover{transform:scale(1.02)}"} -->
<a href="/product/" target="_blank" rel="noopener">
    <img class="gb-media gb-media-img003" src="https://example.com/product.jpg" alt="Product" />
</a>
<!-- /wp:generateblocks/media -->
```

---

## 4. Shape Block (`generateblocks/shape`)

SVG icons and decorative shapes. Wrapped in `<span>`.

### Attributes

```json
{
  "uniqueId": "icon001",
  "styles": {
    "display": "inline-flex",
    "svg": {
      "fill": "currentColor",
      "width": "1.5rem",
      "height": "1.5rem"
    }
  },
  "css": "",
  "globalClasses": [],
  "htmlAttributes": []
}
```

**Two approaches for shape styling:**

1. **`styles.svg` object** - SVG-specific styles go in `styles.svg`. The plugin generates `.gb-shape-{id} svg{...}` CSS:
```json
{"styles": {"display": "inline-flex", "svg": {"fill": "none", "height": "1.5rem", "stroke": "currentColor", "width": "1.5rem"}}}
```

2. **Simple styles** - Width/height/color on the wrapper, SVG attributes inline:
```json
{"styles": {"width": "20px", "height": "20px", "color": "#10b981"}}
```
With SVG: `<svg stroke="currentColor" fill="none" viewBox="0 0 24 24">...</svg>`

Both patterns work. Use `styles.svg` for complex SVG styling, simple styles for small icons.

### Class Pattern

```html
<span class="gb-shape gb-shape-{uniqueId}">
    <svg>...</svg>
</span>
```

### CSS Targeting

```css
/* Wrapper span - layout and positioning */
.gb-shape-icon001 { display: inline-flex; }

/* SVG element inside (from styles.svg) - size, fill, color */
.gb-shape-icon001 svg { fill: currentColor; height: 1.5rem; width: 1.5rem; }
```

**Note:** CSS properties are sorted alphabetically in the `css` attribute.

### Examples

**Basic Icon:**
```html
<!-- wp:generateblocks/shape {"uniqueId":"icon001","styles":{"display":"inline-flex","svg":{"fill":"currentColor","height":"1.5rem","width":"1.5rem"}},"css":".gb-shape-icon001{display:inline-flex}.gb-shape-icon001 svg{fill:currentColor;height:1.5rem;width:1.5rem}"} -->
<span class="gb-shape gb-shape-icon001">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M5 12h14M12 5l7 7-7 7"/>
    </svg>
</span>
<!-- /wp:generateblocks/shape -->
```

**Colored Icon:**
```html
<!-- wp:generateblocks/shape {"uniqueId":"icon002","styles":{"display":"inline-flex","svg":{"fill":"currentColor","height":"2rem","width":"2rem","color":"#c0392b"}},"css":".gb-shape-icon002{display:inline-flex}.gb-shape-icon002 svg{color:#c0392b;fill:currentColor;height:2rem;width:2rem}"} -->
<span class="gb-shape gb-shape-icon002">
    <svg viewBox="0 0 24 24"><path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" fill="currentColor"/></svg>
</span>
<!-- /wp:generateblocks/shape -->
```

**Icon with CSS Variables:**
```html
<!-- wp:generateblocks/shape {"uniqueId":"icon003","styles":{"display":"inline-flex","svg":{"fill":"currentColor","height":"30px","width":"30px","color":"var(\u002d\u002dwp\u002d\u002dpreset\u002d\u002dcolor\u002d\u002drose, #F43F5E)"}},"css":".gb-shape-icon003{display:inline-flex}.gb-shape-icon003 svg{color:var(\u002d\u002dwp\u002d\u002dpreset\u002d\u002dcolor\u002d\u002drose,#F43F5E);fill:currentColor;height:30px;width:30px}"} -->
<span class="gb-shape gb-shape-icon003"><svg aria-hidden="true" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="..."/></svg></span>
<!-- /wp:generateblocks/shape -->
```

**Note:** CSS custom properties use unicode escapes in JSON: `\u002d\u002d` for `--`.

**Simple Icon (Live Site Pattern):**
```html
<!-- wp:generateblocks/shape {"uniqueId":"check001","styles":{"width":"20px","height":"20px","color":"#10b981"},"css":".gb-shape-check001{color:#10b981;height:20px;width:20px}"} -->
<span class="gb-shape gb-shape-check001"><svg stroke-linejoin="round" stroke-linecap="round" stroke-width="3" stroke="currentColor" fill="none" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg></span>
<!-- /wp:generateblocks/shape -->
```

See [SVG Icons Reference](svg-icons.md) for more patterns.

---

## Attribute Summary Table

| Block | Required Attrs | Optional Attrs | Can Nest |
|-------|---------------|----------------|----------|
| element | uniqueId, tagName | styles, css, htmlAttributes, globalClasses, className, align | Yes |
| text | uniqueId, tagName | styles, css, htmlAttributes, globalClasses, className, icon | No |
| media | uniqueId, tagName, htmlAttributes.src, htmlAttributes.alt | styles, css, linkHtmlAttributes, globalClasses | No |
| shape | uniqueId | styles (incl. styles.svg), css, htmlAttributes, globalClasses | No |

**Important notes:**
- Text `<a>` blocks do NOT use `htmlAttributes` for `href` - the link URL is managed by the editor
- Element `<a>` blocks DO use `htmlAttributes` for `href` since they wrap inner blocks
- `align: "full"` with `className: "gb-element alignfull"` for full-width sections
- `globalClasses` is an array of global CSS class slugs (e.g., `["lede"]`)
- `styles` supports responsive keys: `"@media (max-width:1024px)": {"gridTemplateColumns": "minmax(0, 1fr)"}`
- The `css` attribute should contain only base styles (alphabetically sorted), not hover/transition - those are generated by the plugin from the `styles` object
- Element blocks use `className: "gb-element"` to add the base class; class order in HTML is `gb-element-{id} gb-element`
- Shape blocks support `styles.svg` for SVG-specific properties (fill, stroke, width, height, color)

---

## Common HTML Attributes

**For element blocks** (containers, link wrappers):
```json
{
  "htmlAttributes": [
    {"attribute": "id", "value": "section-id"},
    {"attribute": "href", "value": "/page/"},
    {"attribute": "target", "value": "_blank"},
    {"attribute": "rel", "value": "noopener noreferrer"},
    {"attribute": "aria-label", "value": "Description"},
    {"attribute": "aria-hidden", "value": "true"},
    {"attribute": "data-custom", "value": "value"},
    {"attribute": "role", "value": "button"}
  ]
}
```

**For text `<a>` blocks:** Do NOT include `href` in `htmlAttributes`. The link URL is managed by the block editor UI, not in the serialized markup.
```

---

## 5. When to Use Core Blocks

GenerateBlocks covers layout and styling, but WordPress Core Blocks are better for specialized content types with built-in functionality.

### Media Blocks

| Content Type | Use Core Block | GenerateBlocks Alternative |
|--------------|----------------|---------------------------|
| Image with caption | `core/image` | None (no caption support) |
| Image gallery | `core/gallery` | Manual grid with `generateblocks/media` |
| Video (self-hosted) | `core/video` | None |
| Video (YouTube/Vimeo) | `core/embed` | None |
| Audio | `core/audio` | None |
| Cover/background image | `core/cover` | `generateblocks/element` with background CSS |

### Content Blocks

| Content Type | Use Core Block | GenerateBlocks Alternative |
|--------------|----------------|---------------------------|
| Data table | `core/table` | None (use Core for semantics) |
| Bulleted/numbered list | `core/list` | `generateblocks/element` with ul/ol tagName |
| Blockquote with citation | `core/quote` | `generateblocks/element` with blockquote tagName |
| Preformatted code | `core/code` | `generateblocks/text` with pre tagName |
| Horizontal rule | `core/separator` | `generateblocks/element` with border |
| File download | `core/file` | None |
| Details/accordion | `core/details` | None |

### Dynamic Blocks

| Content Type | Use Core Block | GenerateBlocks Alternative |
|--------------|----------------|---------------------------|
| Post query loop | `core/query` | GenerateBlocks Pro query loop |
| Post title | `core/post-title` | `generateblocks/text` with dynamic tag |
| Post excerpt | `core/post-excerpt` | `generateblocks/text` with dynamic tag |
| Post featured image | `core/post-featured-image` | `generateblocks/media` with dynamic tag |
| Post date | `core/post-date` | `generateblocks/text` with dynamic tag |
| Post terms | `core/post-terms` | `generateblocks/text` with dynamic tag |

### Decision Guide

**Use GenerateBlocks when:**
- You need custom layouts (flex, grid)
- You want inline styling with hover states
- You need precise design control
- Building reusable section patterns

**Use Core Blocks when:**
- Content has built-in functionality (video player, audio player)
- Semantic HTML matters (tables, quotes, lists)
- Using WordPress features (embeds, file downloads)
- Dynamic content from posts/queries

### Example: Mixing GenerateBlocks and Core Blocks

```html
<!-- wp:generateblocks/element {"uniqueId":"sect001","tagName":"section","styles":{"padding":"4rem 0"},"css":".gb-element-sect001{padding:4rem 0}"} -->
<section class="gb-element gb-element-sect001">
    <!-- wp:generateblocks/element {"uniqueId":"sect001a","tagName":"div","styles":{"maxWidth":"var(--gb-container-width)","margin":"0 auto","padding":"0 1rem"},"css":".gb-element-sect001a{max-width:var(--gb-container-width);margin:0 auto;padding:0 1rem}"} -->
    <div class="gb-element gb-element-sect001a">

        <!-- wp:generateblocks/text {"uniqueId":"sect001b","tagName":"h2","styles":{"fontSize":"2rem","marginBottom":"2rem"},"css":".gb-text-sect001b{font-size:2rem;margin-bottom:2rem}"} -->
        <h2 class="gb-text gb-text-sect001b">Watch Our Demo</h2>
        <!-- /wp:generateblocks/text -->

        <!-- Use Core embed for YouTube video -->
        <!-- wp:embed {"url":"https://www.youtube.com/watch?v=xyz","type":"video","providerNameSlug":"youtube","responsive":true} -->
        <figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube">
            <div class="wp-block-embed__wrapper">
                https://www.youtube.com/watch?v=xyz
            </div>
        </figure>
        <!-- /wp:embed -->

    </div>
    <!-- /wp:generateblocks/element -->
</section>
<!-- /wp:generateblocks/element -->
```
