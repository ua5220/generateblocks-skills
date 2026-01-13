# GenerateBlocks V2 Layout Builder Skill

Use this skill when creating layouts using GenerateBlocks V2 elements. GenerateBlocks is a WordPress block plugin that provides generic, flexible blocks for building any layout.

## When to Use This Skill

- Building landing pages or sections with GenerateBlocks
- Converting HTML/designs into GenerateBlocks markup
- Creating custom layouts using element, text, media, and shape blocks
- Structuring content with semantic HTML via GenerateBlocks

## GenerateBlocks V2 Block System

GenerateBlocks V2 uses four primary blocks:

1. **element** - Container blocks (div, section, header, etc.)
2. **text** - Text content (p, h1-h6, span, button, a, etc.)
3. **media** - Images
4. **shape** - SVG icons and shapes

## Block Format

All GenerateBlocks follow WordPress block comment syntax:

```html
<!-- wp:generateblocks/{type} {attributes_json} -->
<{tagName} class="gb-{type} [gb-{type}-{uniqueId}] [custom-classes]">
    [content or child blocks]
</{tagName}>
<!-- /wp:generateblocks/{type} -->
```

## 1. Element Block (generateblocks/element)

**Purpose:** Container block for layout structure. Can contain child blocks.

**Available tagNames:**
- `div` (default)
- `section`, `article`, `aside`, `header`, `footer`, `nav`, `main`
- `figure`, `a`, `ul`, `ol`, `li`, `dl`, `dt`, `dd`

**Core Attributes:**

```json
{
  "uniqueId": "abc123",           // Required: unique 8-character ID
  "tagName": "div",               // HTML element type
  "styles": {},                   // CSS properties object (camelCase)
  "css": "",                      // Generated CSS string
  "globalClasses": [],            // Array of predefined global classes
  "htmlAttributes": {},           // HTML attributes object
  "align": ""                     // Block alignment
}
```

**Example - Simple Container:**

```html
<!-- wp:generateblocks/element {"uniqueId":"a1b2c3d4","tagName":"div"} -->
<div class="gb-element">
    <!-- Child blocks here -->
</div>
<!-- /wp:generateblocks/element -->
```

**Example - Styled Section with Flexbox:**

```html
<!-- wp:generateblocks/element {"uniqueId":"x7y8z9w0","tagName":"section","styles":{"display":"flex","flexDirection":"column","gap":"2rem","padding":"3rem 1.5rem","backgroundColor":"#f8f9fa"},"css":".gb-element-x7y8z9w0{display:flex;flex-direction:column;gap:2rem;padding:3rem 1.5rem;background-color:#f8f9fa}","className":"hero-section"} -->
<section class="hero-section gb-element-x7y8z9w0">
    <!-- Child blocks -->
</section>
<!-- /wp:generateblocks/element -->
```

**Example - Link Wrapper:**

```html
<!-- wp:generateblocks/element {"uniqueId":"m4n5o6p7","tagName":"a","htmlAttributes":{"href":"https://example.com","target":"_blank","rel":"noopener noreferrer"}} -->
<a class="gb-element" href="https://example.com" target="_blank" rel="noopener noreferrer">
    <!-- Child blocks -->
</a>
<!-- /wp:generateblocks/element -->
```

## 2. Text Block (generateblocks/text)

**Purpose:** Text content with customizable tag names. Self-closing (no child blocks).

**Available tagNames:**
- `p` (default)
- `span`, `div`
- `h1`, `h2`, `h3`, `h4`, `h5`, `h6`
- `a`, `button`
- `figcaption`, `li`

**Core Attributes:**

```json
{
  "uniqueId": "def456",           // Required: unique 8-character ID
  "tagName": "p",                 // HTML element type
  "content": "",                  // Not in JSON - content is in HTML
  "styles": {},                   // CSS properties object
  "css": "",                      // Generated CSS string
  "globalClasses": [],            // Array of global classes
  "htmlAttributes": {},           // HTML attributes
  "icon": "",                     // SVG icon HTML
  "iconLocation": "before",       // "before" or "after"
  "iconOnly": false               // Show only icon, hide text
}
```

**Class Pattern for Text:**
- Base class: `gb-text` (always present)
- Custom classes: from `className` attribute (if provided)
- Unique class: `gb-text-{uniqueId}` (only when styles exist)

**Example - Heading:**

```html
<!-- wp:generateblocks/text {"uniqueId":"h1a2b3c4","tagName":"h1","styles":{"fontSize":"2.5rem","fontWeight":"700","color":"#1a1a1a","marginBottom":"1rem"},"css":".gb-text-h1a2b3c4{font-size:2.5rem;font-weight:700;color:#1a1a1a;margin-bottom:1rem}"} -->
<h1 class="gb-text gb-text-h1a2b3c4">Welcome to Our Site</h1>
<!-- /wp:generateblocks/text -->
```

**Example - Paragraph:**

```html
<!-- wp:generateblocks/text {"uniqueId":"p5d6e7f8","tagName":"p"} -->
<p class="gb-text">This is a simple paragraph with no custom styles.</p>
<!-- /wp:generateblocks/text -->
```

**Example - Link:**

```html
<!-- wp:generateblocks/text {"uniqueId":"l9g0h1i2","tagName":"a","htmlAttributes":{"href":"https://example.com"},"styles":{"color":"#0073aa","textDecoration":"none"},"css":".gb-text-l9g0h1i2{color:#0073aa;text-decoration:none}"} -->
<a class="gb-text gb-text-l9g0h1i2" href="https://example.com">Learn More</a>
<!-- /wp:generateblocks/text -->
```

**Example - Button:**

```html
<!-- wp:generateblocks/text {"uniqueId":"b3j4k5l6","tagName":"button","styles":{"padding":"0.75rem 1.5rem","backgroundColor":"#0073aa","color":"#ffffff","border":"none","borderRadius":"4px","fontSize":"1rem","fontWeight":"600","cursor":"pointer"},"css":".gb-text-b3j4k5l6{padding:0.75rem 1.5rem;background-color:#0073aa;color:#ffffff;border:none;border-radius:4px;font-size:1rem;font-weight:600;cursor:pointer}","htmlAttributes":{"type":"submit"}} -->
<button class="gb-text gb-text-b3j4k5l6" type="submit">Submit Form</button>
<!-- /wp:generateblocks/text -->
```

## 3. Media Block (generateblocks/media)

**Purpose:** Image elements. Self-closing.

**Available tagNames:**
- `img` (only option)

**Core Attributes:**

```json
{
  "uniqueId": "ghi789",           // Required: unique ID
  "tagName": "img",               // Always "img"
  "mediaId": 0,                   // WordPress media library ID
  "styles": {},                   // CSS properties
  "css": "",                      // Generated CSS string
  "globalClasses": [],            // Array of global classes
  "htmlAttributes": {},           // src, alt, width, height, etc.
  "linkHtmlAttributes": {}        // If wrapped in <a>, link attributes
}
```

**Class Pattern for Media:**
- Custom classes: from `className` attribute (if provided)
- Unique class: `gb-media-{uniqueId}` (only when styles exist)
- NO base `gb-media` class (unlike text which has gb-text)

**Example - Simple Image:**

```html
<!-- wp:generateblocks/media {"uniqueId":"i7m8n9o0","tagName":"img","htmlAttributes":{"src":"https://example.com/image.jpg","alt":"Description"}} -->
<img src="https://example.com/image.jpg" alt="Description"/>
<!-- /wp:generateblocks/media -->
```

**Example - Styled Image:**

```html
<!-- wp:generateblocks/media {"uniqueId":"m1p2q3r4","tagName":"img","styles":{"width":"100%","maxWidth":"600px","height":"auto","borderRadius":"8px","objectFit":"cover"},"css":".gb-media-m1p2q3r4{width:100%;max-width:600px;height:auto;border-radius:8px;object-fit:cover}","htmlAttributes":{"src":"https://example.com/photo.jpg","alt":"Featured photo"}} -->
<img class="gb-media-m1p2q3r4" src="https://example.com/photo.jpg" alt="Featured photo"/>
<!-- /wp:generateblocks/media -->
```

**Example - Linked Image:**

```html
<!-- wp:generateblocks/media {"uniqueId":"l5s6t7u8","tagName":"img","htmlAttributes":{"src":"https://example.com/product.jpg","alt":"Product"},"linkHtmlAttributes":{"href":"https://example.com/shop","target":"_blank","rel":"noopener noreferrer"}} -->
<a href="https://example.com/shop" target="_blank" rel="noopener noreferrer">
    <img src="https://example.com/product.jpg" alt="Product"/>
</a>
<!-- /wp:generateblocks/media -->
```

## 4. Shape Block (generateblocks/shape)

**Purpose:** SVG icons and shapes. Self-closing, wrapped in `<span>`.

**Core Attributes:**

```json
{
  "uniqueId": "jkl012",           // Required: unique ID
  "html": "<svg>...</svg>",       // SVG markup (stored here, not in JSON when viewing)
  "styles": {},                   // CSS properties
  "css": "",                      // Generated CSS string
  "globalClasses": [],            // Array of global classes
  "htmlAttributes": {}            // HTML attributes for wrapper span
}
```

**CSS for Shapes:**
- Wrapper: `.gb-shape-{uniqueId}{...}`
- SVG element: `.gb-shape-{uniqueId} svg{...}`

**Example - Icon:**

```html
<!-- wp:generateblocks/shape {"uniqueId":"s9v0w1x2","styles":{"width":"24px","height":"24px","svg":{"fill":"#0073aa"}},"css":".gb-shape-s9v0w1x2{width:24px;height:24px}.gb-shape-s9v0w1x2 svg{fill:#0073aa}"} -->
<span class="gb-shape gb-shape-s9v0w1x2">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
        <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
    </svg>
</span>
<!-- /wp:generateblocks/shape -->
```

## CSS Styles System

**How Styles Work:**

1. Define styles as JavaScript object in `styles` attribute (camelCase properties)
2. Generate CSS string in `css` attribute (kebab-case, targeting unique class)
3. CSS is output inline or enqueued separately
4. Supports nested objects for pseudo-classes and pseudo-elements
5. Responsive styles are handled through the @edge22/block-styles system

**Common CSS Properties (camelCase in JSON):**

Layout:
- `display`, `position`, `top`, `right`, `bottom`, `left`
- `width`, `height`, `minWidth`, `maxWidth`, `minHeight`, `maxHeight`
- `margin`, `marginTop`, `marginRight`, `marginBottom`, `marginLeft`
- `padding`, `paddingTop`, `paddingRight`, `paddingBottom`, `paddingLeft`

Flexbox:
- `flexDirection`, `flexWrap`, `justifyContent`, `alignItems`, `alignContent`
- `gap`, `rowGap`, `columnGap`
- `flex`, `flexGrow`, `flexShrink`, `flexBasis`
- `order`, `alignSelf`

Grid:
- `gridTemplateColumns`, `gridTemplateRows`, `gridAutoFlow`
- `gridColumn`, `gridRow`, `gridArea`
- `justifyItems`, `alignItems`

Typography:
- `fontSize`, `fontFamily`, `fontWeight`, `fontStyle`, `lineHeight`
- `color`, `textAlign`, `textDecoration`, `textTransform`
- `letterSpacing`, `wordSpacing`

Visual:
- `backgroundColor`, `opacity`
- `border`, `borderTop`, `borderRight`, `borderBottom`, `borderLeft`
- `borderRadius`, `borderTopLeftRadius`, etc.
- `boxShadow`, `textShadow`
- `transform`, `transformOrigin`, `transition`

Other:
- `overflow`, `overflowX`, `overflowY`
- `cursor`, `pointerEvents`, `userSelect`
- `zIndex`, `objectFit`, `objectPosition`

**Example CSS Generation:**

```javascript
// Input styles object
{
  "display": "flex",
  "gap": "1rem",
  "padding": "2rem",
  "backgroundColor": "#ffffff",
  "borderRadius": "8px"
}

// Generated CSS string
".gb-element-abc123{display:flex;gap:1rem;padding:2rem;background-color:#ffffff;border-radius:8px}"
```

## Responsive Layouts

GenerateBlocks V2 uses breakpoints to handle responsive designs. The system generates `@media` queries automatically.

### Breakpoints

| Device | Media Query | Use Case |
|--------|-------------|----------|
| Desktop | `(min-width: 1025px)` | Large screens only |
| Desktop/Tablet | `(min-width: 768px)` | Tablet and larger |
| Tablet | `(max-width: 1024px)` | Tablet and smaller |
| Tablet Only | `(max-width: 1024px) and (min-width: 768px)` | Only tablets |
| Mobile | `(max-width: 767px)` | Mobile devices |

### Responsive Approach in V2

While GenerateBlocks V2 uses the @edge22/block-styles system that manages responsive styles internally, when creating layouts manually, follow these patterns:

**Desktop-First Approach:**
- Define base styles for desktop
- Override with tablet and mobile styles as needed
- Use max-width media queries to target smaller devices

**Example - Responsive Padding:**

```json
{
  "uniqueId": "resp001",
  "tagName": "section",
  "styles": {
    "padding": "4rem 2rem",
    "fontSize": "1.125rem"
  },
  "css": ".gb-element-resp001{padding:4rem 2rem;font-size:1.125rem}@media (max-width:1024px){.gb-element-resp001{padding:3rem 1.5rem;font-size:1rem}}@media (max-width:767px){.gb-element-resp001{padding:2rem 1rem;font-size:0.875rem}}"
}
```

**Note:** The @edge22/block-styles system handles responsive style management in the WordPress editor UI. When building layouts, the responsive styles are compiled into the `css` attribute with appropriate `@media` queries.

### Common Responsive Patterns

**1. Responsive Grid Columns:**

```html
<!-- wp:generateblocks/element {"uniqueId":"grid001","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"repeat(3, 1fr)","gap":"2rem"},"css":".gb-element-grid001{display:grid;grid-template-columns:repeat(3, 1fr);gap:2rem}@media (max-width:1024px){.gb-element-grid001{grid-template-columns:repeat(2, 1fr);gap:1.5rem}}@media (max-width:767px){.gb-element-grid001{grid-template-columns:1fr;gap:1rem}}"} -->
<div class="gb-element-grid001">
    <!-- Content -->
</div>
<!-- /wp:generateblocks/element -->
```

**2. Responsive Flexbox Direction:**

```html
<!-- wp:generateblocks/element {"uniqueId":"flex001","tagName":"div","styles":{"display":"flex","flexDirection":"row","gap":"2rem"},"css":".gb-element-flex001{display:flex;flex-direction:row;gap:2rem}@media (max-width:767px){.gb-element-flex001{flex-direction:column;gap:1rem}}"} -->
<div class="gb-element-flex001">
    <!-- Content -->
</div>
<!-- /wp:generateblocks/element -->
```

**3. Responsive Typography:**

```html
<!-- wp:generateblocks/text {"uniqueId":"head001","tagName":"h1","styles":{"fontSize":"3rem","lineHeight":"1.2"},"css":".gb-text-head001{font-size:3rem;line-height:1.2}@media (max-width:1024px){.gb-text-head001{font-size:2.5rem}}@media (max-width:767px){.gb-text-head001{font-size:2rem}}"} -->
<h1 class="gb-text gb-text-head001">Responsive Heading</h1>
<!-- /wp:generateblocks/text -->
```

**4. Show/Hide Elements by Device:**

```html
<!-- Desktop only -->
<!-- wp:generateblocks/element {"uniqueId":"desk001","tagName":"div","styles":{"display":"block"},"css":".gb-element-desk001{display:block}@media (max-width:767px){.gb-element-desk001{display:none}}"} -->
<div class="gb-element-desk001">Desktop content</div>
<!-- /wp:generateblocks/element -->

<!-- Mobile only -->
<!-- wp:generateblocks/element {"uniqueId":"mob001","tagName":"div","styles":{"display":"none"},"css":".gb-element-mob001{display:none}@media (max-width:767px){.gb-element-mob001{display:block}}"} -->
<div class="gb-element-mob001">Mobile content</div>
<!-- /wp:generateblocks/element -->
```

## Pseudo-Classes and Interactive States

GenerateBlocks V2 supports pseudo-classes and pseudo-elements through nested style objects.

### Supported Pseudo-Classes

Common pseudo-classes that can be used:
- `:hover` - Element is hovered
- `:focus` - Element has focus
- `:active` - Element is being activated
- `:visited` - Link has been visited
- `:focus-visible` - Element has keyboard focus
- `:focus-within` - Element or child has focus

### Pseudo-Elements

- `::before` - Insert content before element
- `::after` - Insert content after element

### Syntax

Use nested objects with `&:` prefix for pseudo-classes:

```javascript
{
  "styles": {
    "property": "value",           // Base styles
    "&:hover": {                   // Hover state
      "property": "value"
    },
    "&:is(:hover, :focus)": {      // Combined states
      "property": "value"
    },
    "&::before": {                 // Pseudo-element
      "content": "''",
      "property": "value"
    }
  }
}
```

### CSS Generation for Pseudo-Classes

The nested pseudo-class objects generate additional CSS selectors:

```javascript
// Input
{
  "backgroundColor": "#0073aa",
  "color": "#ffffff",
  "&:hover": {
    "backgroundColor": "#005a87",
    "color": "#ffffff"
  }
}

// Generated CSS
.gb-element-abc123{background-color:#0073aa;color:#ffffff}
.gb-element-abc123:hover{background-color:#005a87;color:#ffffff}
```

### Examples

**1. Button with Hover State:**

```html
<!-- wp:generateblocks/text {"uniqueId":"btn001","tagName":"button","styles":{"padding":"1rem 2rem","backgroundColor":"#0073aa","color":"#ffffff","border":"none","borderRadius":"4px","cursor":"pointer","transition":"all 0.3s ease","&:is(:hover, :focus)":{"backgroundColor":"#005a87","transform":"translateY(-2px)","boxShadow":"0 4px 12px rgba(0,115,170,0.3)"}},"css":".gb-text-btn001{padding:1rem 2rem;background-color:#0073aa;color:#ffffff;border:none;border-radius:4px;cursor:pointer;transition:all 0.3s ease}.gb-text-btn001:is(:hover, :focus){background-color:#005a87;transform:translateY(-2px);box-shadow:0 4px 12px rgba(0,115,170,0.3)}"} -->
<button class="gb-text gb-text-btn001">Hover Me</button>
<!-- /wp:generateblocks/text -->
```

**2. Link with Multiple States:**

```html
<!-- wp:generateblocks/text {"uniqueId":"link001","tagName":"a","htmlAttributes":{"href":"#"},"styles":{"color":"#0073aa","textDecoration":"none","borderBottom":"2px solid transparent","transition":"all 0.2s","&:hover":{"color":"#005a87","borderBottomColor":"#005a87"},"&:focus":{"outline":"2px solid #0073aa","outlineOffset":"2px"},"&:active":{"color":"#004a6e"}},"css":".gb-text-link001{color:#0073aa;text-decoration:none;border-bottom:2px solid transparent;transition:all 0.2s}.gb-text-link001:hover{color:#005a87;border-bottom-color:#005a87}.gb-text-link001:focus{outline:2px solid #0073aa;outline-offset:2px}.gb-text-link001:active{color:#004a6e}"} -->
<a class="gb-text gb-text-link001" href="#">Interactive Link</a>
<!-- /wp:generateblocks/text -->
```

**3. Card with Hover Effect:**

```html
<!-- wp:generateblocks/element {"uniqueId":"card001","tagName":"article","styles":{"padding":"1.5rem","backgroundColor":"#ffffff","borderRadius":"8px","boxShadow":"0 2px 4px rgba(0,0,0,0.1)","transition":"all 0.3s ease","&:hover":{"boxShadow":"0 8px 16px rgba(0,0,0,0.15)","transform":"translateY(-4px)"}},"css":".gb-element-card001{padding:1.5rem;background-color:#ffffff;border-radius:8px;box-shadow:0 2px 4px rgba(0,0,0,0.1);transition:all 0.3s ease}.gb-element-card001:hover{box-shadow:0 8px 16px rgba(0,0,0,0.15);transform:translateY(-4px)}"} -->
<article class="gb-element-card001">
    <!-- Card content -->
</article>
<!-- /wp:generateblocks/element -->
```

**4. Icon with Hover Color Change:**

```html
<!-- wp:generateblocks/shape {"uniqueId":"icon001","styles":{"width":"48px","height":"48px","transition":"all 0.3s","svg":{"fill":"#666666","transition":"fill 0.3s"},"&:hover":{"transform":"scale(1.1)"},"&:hover svg":{"fill":"#0073aa"}},"css":".gb-shape-icon001{width:48px;height:48px;transition:all 0.3s}.gb-shape-icon001 svg{fill:#666666;transition:fill 0.3s}.gb-shape-icon001:hover{transform:scale(1.1)}.gb-shape-icon001:hover svg{fill:#0073aa}"} -->
<span class="gb-shape gb-shape-icon001">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 2l10 5-10 5-10-5 10-5z"/></svg>
</span>
<!-- /wp:generateblocks/shape -->
```

**5. Element with ::before Pseudo-Element:**

```html
<!-- wp:generateblocks/element {"uniqueId":"badge001","tagName":"div","styles":{"position":"relative","padding":"1rem","backgroundColor":"#f0f0f0","&::before":{"content":"''","position":"absolute","top":"0","left":"0","width":"4px","height":"100%","backgroundColor":"#0073aa"}},"css":".gb-element-badge001{position:relative;padding:1rem;background-color:#f0f0f0}.gb-element-badge001::before{content:'';position:absolute;top:0;left:0;width:4px;height:100%;background-color:#0073aa}"} -->
<div class="gb-element-badge001">
    <!-- Content with left border via ::before -->
</div>
<!-- /wp:generateblocks/element -->
```

### Best Practices for Interactive States

1. **Always include transitions** for smooth hover effects
2. **Use `&:is(:hover, :focus)`** for both mouse and keyboard users
3. **Maintain sufficient contrast** in all states for accessibility
4. **Don't rely solely on hover** - ensure functionality works on touch devices
5. **Use `cursor: pointer`** on interactive elements
6. **Provide visual feedback** for active/pressed states
7. **Test with keyboard navigation** (Tab, Enter, Space keys)

### Combining Responsive and Pseudo-Classes

You can combine responsive breakpoints with pseudo-classes:

```javascript
{
  "styles": {
    "padding": "1rem",
    "&:hover": {
      "backgroundColor": "#0073aa"
    }
  },
  "css": ".gb-element-abc123{padding:1rem}.gb-element-abc123:hover{background-color:#0073aa}@media (max-width:767px){.gb-element-abc123{padding:0.5rem}.gb-element-abc123:hover{background-color:#005a87}}"
}
```

## Building Layouts - Common Patterns

### Pattern 1: Hero Section

```html
<!-- wp:generateblocks/element {"uniqueId":"hero001","tagName":"section","styles":{"display":"flex","flexDirection":"column","alignItems":"center","justifyContent":"center","minHeight":"500px","padding":"3rem 1.5rem","backgroundColor":"#1a1a1a","color":"#ffffff"},"css":".gb-element-hero001{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:500px;padding:3rem 1.5rem;background-color:#1a1a1a;color:#ffffff}"} -->
<section class="gb-element-hero001">

    <!-- wp:generateblocks/text {"uniqueId":"hero002","tagName":"h1","styles":{"fontSize":"3rem","fontWeight":"700","marginBottom":"1rem","textAlign":"center"},"css":".gb-text-hero002{font-size:3rem;font-weight:700;margin-bottom:1rem;text-align:center}"} -->
    <h1 class="gb-text gb-text-hero002">Build Anything with GenerateBlocks</h1>
    <!-- /wp:generateblocks/text -->

    <!-- wp:generateblocks/text {"uniqueId":"hero003","tagName":"p","styles":{"fontSize":"1.25rem","marginBottom":"2rem","textAlign":"center","maxWidth":"600px"},"css":".gb-text-hero003{font-size:1.25rem;margin-bottom:2rem;text-align:center;max-width:600px}"} -->
    <p class="gb-text gb-text-hero003">Flexible, lightweight blocks that work the way you want.</p>
    <!-- /wp:generateblocks/text -->

    <!-- wp:generateblocks/text {"uniqueId":"hero004","tagName":"a","htmlAttributes":{"href":"#features"},"styles":{"padding":"1rem 2rem","backgroundColor":"#0073aa","color":"#ffffff","borderRadius":"4px","textDecoration":"none","fontSize":"1.125rem","fontWeight":"600"},"css":".gb-text-hero004{padding:1rem 2rem;background-color:#0073aa;color:#ffffff;border-radius:4px;text-decoration:none;font-size:1.125rem;font-weight:600}"} -->
    <a class="gb-text gb-text-hero004" href="#features">Get Started</a>
    <!-- /wp:generateblocks/text -->

</section>
<!-- /wp:generateblocks/element -->
```

### Pattern 2: Two-Column Layout

```html
<!-- wp:generateblocks/element {"uniqueId":"layout01","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"1fr 1fr","gap":"2rem","padding":"2rem"},"css":".gb-element-layout01{display:grid;grid-template-columns:1fr 1fr;gap:2rem;padding:2rem}"} -->
<div class="gb-element-layout01">

    <!-- wp:generateblocks/element {"uniqueId":"col01","tagName":"div"} -->
    <div class="gb-element">
        <!-- wp:generateblocks/text {"uniqueId":"col01t1","tagName":"h2"} -->
        <h2 class="gb-text">Left Column</h2>
        <!-- /wp:generateblocks/text -->
    </div>
    <!-- /wp:generateblocks/element -->

    <!-- wp:generateblocks/element {"uniqueId":"col02","tagName":"div"} -->
    <div class="gb-element">
        <!-- wp:generateblocks/text {"uniqueId":"col02t1","tagName":"h2"} -->
        <h2 class="gb-text">Right Column</h2>
        <!-- /wp:generateblocks/text -->
    </div>
    <!-- /wp:generateblocks/element -->

</div>
<!-- /wp:generateblocks/element -->
```

### Pattern 3: Card Component

```html
<!-- wp:generateblocks/element {"uniqueId":"card001","tagName":"article","styles":{"backgroundColor":"#ffffff","borderRadius":"8px","boxShadow":"0 2px 8px rgba(0,0,0,0.1)","padding":"1.5rem","display":"flex","flexDirection":"column","gap":"1rem"},"css":".gb-element-card001{background-color:#ffffff;border-radius:8px;box-shadow:0 2px 8px rgba(0,0,0,0.1);padding:1.5rem;display:flex;flex-direction:column;gap:1rem}"} -->
<article class="gb-element-card001">

    <!-- wp:generateblocks/media {"uniqueId":"card002","tagName":"img","styles":{"width":"100%","height":"200px","objectFit":"cover","borderRadius":"4px"},"css":".gb-media-card002{width:100%;height:200px;object-fit:cover;border-radius:4px}","htmlAttributes":{"src":"https://example.com/image.jpg","alt":"Card image"}} -->
    <img class="gb-media-card002" src="https://example.com/image.jpg" alt="Card image"/>
    <!-- /wp:generateblocks/media -->

    <!-- wp:generateblocks/text {"uniqueId":"card003","tagName":"h3","styles":{"fontSize":"1.5rem","fontWeight":"600","marginBottom":"0.5rem"},"css":".gb-text-card003{font-size:1.5rem;font-weight:600;margin-bottom:0.5rem}"} -->
    <h3 class="gb-text gb-text-card003">Card Title</h3>
    <!-- /wp:generateblocks/text -->

    <!-- wp:generateblocks/text {"uniqueId":"card004","tagName":"p"} -->
    <p class="gb-text">Card description text goes here.</p>
    <!-- /wp:generateblocks/text -->

    <!-- wp:generateblocks/text {"uniqueId":"card005","tagName":"a","htmlAttributes":{"href":"#"},"styles":{"color":"#0073aa","textDecoration":"none","fontWeight":"600"},"css":".gb-text-card005{color:#0073aa;text-decoration:none;font-weight:600}"} -->
    <a class="gb-text gb-text-card005" href="#">Read More →</a>
    <!-- /wp:generateblocks/text -->

</article>
<!-- /wp:generateblocks/element -->
```

### Pattern 4: Flexbox Row with Gap

```html
<!-- wp:generateblocks/element {"uniqueId":"flex001","tagName":"div","styles":{"display":"flex","gap":"1rem","alignItems":"center"},"css":".gb-element-flex001{display:flex;gap:1rem;align-items:center}"} -->
<div class="gb-element-flex001">

    <!-- wp:generateblocks/shape {"uniqueId":"icon001","styles":{"width":"48px","height":"48px","svg":{"fill":"#0073aa"}},"css":".gb-shape-icon001{width:48px;height:48px}.gb-shape-icon001 svg{fill:#0073aa}"} -->
    <span class="gb-shape gb-shape-icon001">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 2l10 5-10 5-10-5 10-5z"/></svg>
    </span>
    <!-- /wp:generateblocks/shape -->

    <!-- wp:generateblocks/element {"uniqueId":"flex002","tagName":"div","styles":{"display":"flex","flexDirection":"column","gap":"0.25rem"},"css":".gb-element-flex002{display:flex;flex-direction:column;gap:0.25rem}"} -->
    <div class="gb-element-flex002">
        <!-- wp:generateblocks/text {"uniqueId":"flex003","tagName":"h4","styles":{"fontSize":"1.25rem","fontWeight":"600","marginBottom":"0"},"css":".gb-text-flex003{font-size:1.25rem;font-weight:600;margin-bottom:0}"} -->
        <h4 class="gb-text gb-text-flex003">Feature Title</h4>
        <!-- /wp:generateblocks/text -->

        <!-- wp:generateblocks/text {"uniqueId":"flex004","tagName":"p"} -->
        <p class="gb-text">Feature description</p>
        <!-- /wp:generateblocks/text -->
    </div>
    <!-- /wp:generateblocks/element -->

</div>
<!-- /wp:generateblocks/element -->
```

## Best Practices

### 1. Unique IDs
- Always generate unique 8-character alphanumeric IDs
- Use pattern: lowercase letters and numbers only
- Example: `a1b2c3d4`, `x7y8z9w0`

### 2. Class Naming Order
**Element blocks:**
- Custom className first (if provided)
- `gb-element-{uniqueId}` only when styles exist
- NO base `gb-element` class

**Text blocks:**
- `gb-text` base class (always)
- Custom className (if provided)
- `gb-text-{uniqueId}` only when styles exist

**Media blocks:**
- Custom className (if provided)
- `gb-media-{uniqueId}` only when styles exist
- NO base `gb-media` class

**Shape blocks:**
- `gb-shape` base class (always)
- Custom className (if provided)
- `gb-shape-{uniqueId}` only when styles exist

### 3. CSS Generation
- Only include `css` attribute when `styles` object has properties
- Generate CSS in format: `.gb-{type}-{uniqueId}{property:value;...}`
- For shapes with SVG styles: `.gb-shape-{uniqueId}{wrapper-props}.gb-shape-{uniqueId} svg{svg-props}`
- Use kebab-case for CSS properties
- No spaces in CSS string

### 4. HTML Attributes
- Use `htmlAttributes` object for standard HTML attributes
- Common attributes: `href`, `target`, `rel`, `src`, `alt`, `width`, `height`, `id`, `data-*`, `aria-*`
- Don't include `class` in htmlAttributes (use className or computed classes)

### 5. Semantic HTML
- Use appropriate semantic tagNames:
  - `section` for major content sections
  - `article` for self-contained content
  - `header`, `footer`, `nav`, `aside`, `main` for landmark regions
  - `figure` for images with captions
  - `h1`-`h6` for hierarchical headings
  - `p` for paragraphs
  - `a` for links
  - `button` for buttons

### 6. Layout Techniques
- Prefer CSS Grid for two-dimensional layouts
- Use Flexbox for one-dimensional layouts (rows/columns)
- Use `gap` property instead of margins between flex/grid items
- Apply responsive styles using CSS custom properties or inline responsive styles

### 7. Accessibility
- Always include `alt` text for images
- Use proper heading hierarchy (h1 → h2 → h3...)
- Include `aria-*` attributes in htmlAttributes when needed
- Ensure sufficient color contrast
- Add `rel="noopener noreferrer"` for external links with `target="_blank"`

## Generating Unique IDs

Use this JavaScript function pattern:

```javascript
function generateUniqueId(length = 8) {
    const chars = 'abcdefghijklmnopqrstuvwxyz0123456789';
    let result = '';
    for (let i = 0; i < length; i++) {
        result += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return result;
}
```

## Converting CSS to Styles Object

**From CSS:**
```css
display: flex;
gap: 1rem;
padding-top: 2rem;
background-color: #ffffff;
border-radius: 8px;
```

**To Styles Object:**
```json
{
  "display": "flex",
  "gap": "1rem",
  "paddingTop": "2rem",
  "backgroundColor": "#ffffff",
  "borderRadius": "8px"
}
```

**Rules:**
- Convert kebab-case to camelCase
- Keep values as strings
- Preserve units (px, rem, %, vh, etc.)
- Keep color values as-is (#hex, rgb(), hsl())

## Common Layout Structures

### Full-Width Section with Max-Width Inner
```html
<!-- wp:generateblocks/element {"uniqueId":"sect001","tagName":"section","styles":{"width":"100%","padding":"4rem 1.5rem"},"css":".gb-element-sect001{width:100%;padding:4rem 1.5rem}"} -->
<section class="gb-element-sect001">
    <!-- wp:generateblocks/element {"uniqueId":"inner001","tagName":"div","styles":{"maxWidth":"1200px","margin":"0 auto"},"css":".gb-element-inner001{max-width:1200px;margin:0 auto}"} -->
    <div class="gb-element-inner001">
        <!-- Content -->
    </div>
    <!-- /wp:generateblocks/element -->
</section>
<!-- /wp:generateblocks/element -->
```

### Three-Column Grid
```html
<!-- wp:generateblocks/element {"uniqueId":"grid001","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"repeat(3, 1fr)","gap":"2rem"},"css":".gb-element-grid001{display:grid;grid-template-columns:repeat(3, 1fr);gap:2rem}"} -->
<div class="gb-element-grid001">
    <!-- 3 child elements -->
</div>
<!-- /wp:generateblocks/element -->
```

### Centered Content
```html
<!-- wp:generateblocks/element {"uniqueId":"center01","tagName":"div","styles":{"display":"flex","flexDirection":"column","alignItems":"center","justifyContent":"center","textAlign":"center"},"css":".gb-element-center01{display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center}"} -->
<div class="gb-element-center01">
    <!-- Content -->
</div>
<!-- /wp:generateblocks/element -->
```

## Notes

- GenerateBlocks V2 blocks are dynamic (server-rendered in PHP)
- The HTML shown in block comments is just a preview - actual output is generated server-side
- Styles are processed and optimized on the backend
- GlobalClasses reference predefined style sets (not covered here)
- All blocks support dynamic content via WordPress context (postId, loopItem, etc.)

## Quick Reference

| Block | Base Class | Unique Class | tagName Options |
|-------|-----------|--------------|-----------------|
| element | none | gb-element-{id} | div, section, header, footer, nav, main, article, aside, figure, a, ul, ol, li, dl, dt, dd |
| text | gb-text | gb-text-{id} | p, span, div, h1-h6, a, button, figcaption, li |
| media | none | gb-media-{id} | img |
| shape | gb-shape | gb-shape-{id} | (wrapped in span) |
