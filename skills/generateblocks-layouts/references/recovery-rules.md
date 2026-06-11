---
title: Recovery Error Rules (read FIRST)
description: Every known cause of "Attempt Recovery" errors in the WordPress block editor when emitting GenerateBlocks markup, with the exact fix.
---

# Recovery Error Rules

This file is the authoritative checklist. Read it before generating any GB markup.
Every rule below comes from a real failure observed in production.

## The meta-rule (read this first)

> **The WordPress block editor validates blocks by re-serializing the
> attributes and string-comparing against the markup you pasted. Any
> deviation — even semantically-equivalent JSON or HTML — is treated as
> corruption and triggers "Attempt Recovery".**

This is the unifying principle behind every rule in this file. The goal isn't
"valid JSON and valid HTML." The goal is **byte-identical to what the editor
itself would emit if it round-tripped the block**.

That means:
- The same JSON key order
- The same string-escape form for special characters
- The same minification of CSS
- The same class list (including any auto-injected duplicates)
- The same alphabetization of CSS properties
- The same spacing and quoting

If you remember nothing else, remember this: **emit what the editor emits, not
what you think is correct**.

---

## 1. JSON encoding rules (the silent killers)

WordPress's `serialize_block_attributes()` runs five substitutions on the JSON
string after `wp_json_encode()` to make it safe for the HTML comment context
the block delimiter lives in:

| Literal | Canonical form | Why |
|---|---|---|
| `--` | `\u002d\u002d` | Avoids HTML comment terminator collision |
| `<` | `\u003c` | Defends against `</script>` injection |
| `>` | `\u003e` | Same |
| `&` | `\u0026` | Defends against entity injection |
| `\"` (escaped quote) | `\u0022` | Quote inside a JSON string value |

If you emit the literal form, the editor re-serializes to the escaped form,
the strings differ, recovery fires. **Apply all five substitutions to every
JSON string value** inside block delimiter attributes (`styles` values, `css`
strings, `htmlAttributes` values, content strings — everywhere).

The fifth matters whenever a string value contains a double quote — e.g.
inline HTML in a text block's `content` or an SVG string in a text block's
`icon` attribute. Never emit `\"` — the canonical form is `\u0022`:

```json
"content":"Read the \u003ca href=\u0022https://example.com/\u0022\u003eguide\u003c/a\u003e now"
```

### 1.1 Escape `--` as `\u002d\u002d`

```json
// WRONG
"styles":{"maxWidth":"var(--gb-container-width)"}
"css":".gb-element-x{color:var(--accent)}"

// RIGHT
"styles":{"maxWidth":"var(\u002d\u002dgb-container-width)"}
"css":".gb-element-x{color:var(\u002d\u002daccent)}"
```

Applies to: every CSS custom property reference, every vendor prefix
(`--webkit-...`), every double-dash in any string.

### 1.2 Escape `&` as `\u0026`

```json
// WRONG
"htmlAttributes":{"href":"https://example.com/?a=1&b=2"}

// RIGHT
"htmlAttributes":{"href":"https://example.com/?a=1\u0026b=2"}
```

Applies to: query strings with multiple params (`?a=1&b=2`), URL fragments,
any string anywhere with a literal `&`.

In the **rendered HTML body** (the part outside the block delimiter), the
same `&` is written as `&amp;`:

```html
<a href="https://example.com/?a=1&amp;b=2">link</a>
```

So a single URL with two query params appears in three different forms across
one block:

| Place | Form |
|---|---|
| JSON `htmlAttributes.href` | `https://example.com/?a=1\u0026b=2` |
| Rendered HTML `<a href="...">` | `https://example.com/?a=1&amp;b=2` |
| What it actually means | `https://example.com/?a=1&b=2` |

All three are correct in their respective places.

### 1.3 Escape `<` and `>` as `\u003c` and `\u003e`

Rare in normal use but bites if a string value contains literal angle
brackets:

```json
// WRONG
"content":"Use the <button> tag"

// RIGHT
"content":"Use the \u003cbutton\u003e tag"
```

### 1.4 Use single quotes inside `css` strings

Anything inside `content:''`, `font-family:'...'`, `url('...')` must use single
quotes. Double quotes would terminate the surrounding JSON string.

```json
"css":".x::after{content:'→'}"
"css":".y{font-family:'Inter',sans-serif}"
```

### 1.5 No trailing commas, no unescaped newlines

The `css` attribute is a single-line minified string. If you need a literal
newline inside content, encode `\n`.

### 1.6 Inline HTML `style=""` is NOT JSON

Inside the rendered HTML body (e.g. `style="..."` on a real `<span>`), use
literal characters: `var(--foo)`, `&amp;`, `<`, `>`. The HTML body is not
JSON-parsed, so the five substitutions above do not apply there.

This is why the canonical link with a multi-param URL has `&amp;` in the
HTML body and `\u0026` in the JSON — same character, different escape rules
for different contexts.

---

## 2. CSS attribute restrictions

The `css` string has tighter rules than normal CSS. Anything that violates
them will be silently stripped or trigger recovery.

### 2.1 No nested child selectors in `css`

The editor strips selectors that descend into the rendered DOM, e.g.:

```css
/* ALL OF THESE GET STRIPPED */
.gb-text-x small{...}
.gb-text-x code{...}
.gb-text-x strong{...}
.gb-text-x .u{...}
.gb-text-x a{...}
```

Stripping changes the serialized output → hash mismatch → recovery.

**Fix:** put the styles as `style="..."` attributes directly on the inner HTML
element (`<small>`, `<code>`, `<strong>`, `<span>`) inside the text block's
content. Inline `style=""` lives inside the rendered HTML body, not JSON, so
it can use literal `var(--font-size-sm)` without escaping.

```html
<!-- wp:generateblocks/text {"uniqueId":"t1","tagName":"p","css":".gb-text-t1{...}"} -->
<p class="gb-text gb-text-t1">
    Some text with <small style="font-size:var(--font-size-sm);color:#666">a footnote</small>
    and <strong style="font-weight:700">emphasis</strong>.
</p>
<!-- /wp:generateblocks/text -->
```

**The two allowed exceptions** (these are NOT child descent, they target the
block's own DOM extras):

- Pseudo-elements on the block's own selector: `.gb-element-x::before`,
  `.gb-element-x::after`
- Parent-hover targeting a child block by **its own** generated class:
  `.gb-element-card1:hover .gb-text-title1{...}` — this works because both
  classes are generator-stable (each block writes its own class), not arbitrary
  descendants.

### 2.2 No `transition` property in `css`

The plugin generates transitions from the `styles` object, not the `css`
string. If you put a `transition:` declaration in `css`, the plugin overwrites
or de-duplicates it on save → mismatch → recovery.

```css
/* WRONG */
.gb-element-btn1{background:#000;color:#fff;transition:all .2s ease}

/* RIGHT — declare hover/transition via styles object only */
"styles":{"backgroundColor":"#000","color":"#fff",":hover":{"backgroundColor":"#222"}}
```

### 2.3 No hover states in `css` either

Same reason as transitions — generated by the plugin from the `styles` object.

Exception: pseudo-element hover (`.gb-element-x:hover::after`) and parent
hover targeting a child class are allowed because the styles object can't
express them.

### 2.4 No spaces inside CSS function arguments

The editor minifies CSS. `clamp(2rem, 5vw, 3rem)` becomes `clamp(2rem,5vw,3rem)`
in the saved version → mismatch with the literal you wrote → recovery.

Write minified from the start, every time:

```css
/* RIGHT */
clamp(1rem,4vw,2.5rem)
repeat(4,1fr)
minmax(0,1fr)
calc(100%-2rem)
rgba(0,0,0,0.1)

/* WRONG */
clamp(1rem, 4vw, 2.5rem)
repeat(4, 1fr)
```

### 2.5 No CSS `\xxxx` escape sequences for special characters

`content:'\2192'` (right arrow) breaks because `\2` is interpreted by JSON
string escaping rules and the round-trip mangles it.

**Fix:** put the literal character in the rendered HTML text or in the
`content` value:

```css
/* WRONG */
.gb-text-link::after{content:'\2192'}

/* RIGHT — literal character in CSS */
.gb-text-link::after{content:'→'}
```

Or, even better, put the arrow in the rendered text content itself rather
than in a pseudo-element. Pseudo-element arrows are fragile; literal arrows
are bulletproof.

### 2.6 CSS properties must be alphabetically sorted in `css`

The plugin alphabetizes properties on save. If your literal isn't sorted, you
get a hash mismatch.

```css
/* WRONG */
.x{padding:2rem;background:#fff;display:flex}

/* RIGHT */
.x{background:#fff;display:flex;padding:2rem}
```

### 2.7 The `css` string must be a single minified line

No line breaks. No indentation. Concatenate every rule.

---

## 3. Block-level HTML rules

### 3.1 `htmlAttributes` MUST be a plain object, never an array

```json
// RIGHT
"htmlAttributes":{"href":"https://example.com/","target":"_blank","rel":"noopener"}

// WRONG — guaranteed recovery
"htmlAttributes":[{"attribute":"href","value":"https://example.com/"}]
```

### 3.2 Use full absolute URLs in `htmlAttributes.href`

The editor canonicalizes relative URLs to absolute on save → mismatch →
recovery.

```json
// RIGHT
"htmlAttributes":{"href":"https://gauravtiwari.org/contact/"}

// WRONG
"htmlAttributes":{"href":"/contact/"}
```

### 3.3 `className` and the auto-injected id-class

GenerateBlocks **automatically injects** `gb-{type}-{uniqueId}` into the
rendered HTML class list whenever `styles` is non-empty. This is in addition
to whatever you put in the `className` attribute.

The editor's canonical serialization is **the union, with duplicates kept as-is**.
So if `className` already contains the id-class, the rendered HTML has it
**twice**:

```json
// className includes id-class:
"className":"gb-element-top001 gb-element alignfull"
```
```html
<!-- canonical rendered HTML — id-class appears TWICE -->
<header class="gb-element-top001 gb-element-top001 gb-element alignfull">
```

There are two ways to stay drift-free. Pick one and stick to it:

**Option A (recommended): omit the id-class from `className`.** Let the
plugin auto-inject it. The rendered HTML has it once.

```json
"className":"gb-element alignfull"
```
```html
<header class="gb-element-top001 gb-element alignfull">
```

This is cleaner output. Use it for new work.

**Option B: include the id-class in `className` AND duplicate it in the HTML.**

```json
"className":"gb-element-top001 gb-element alignfull"
```
```html
<header class="gb-element-top001 gb-element-top001 gb-element alignfull">
```

Ugly but valid. Use this only when matching an existing pattern that already
emits the duplicate.

**The rule applies to every block type**, not just element. `gb-text-{id}`,
`gb-media-{id}`, `gb-shape-{id}`, `gb-query-{id}`, `gb-looper-{id}`,
`gb-loop-item-{id}`, `gb-query-page-numbers-{id}` all auto-inject the same
way.

**When `styles` is empty** (e.g. an empty `"styles":{}`), the plugin does NOT
auto-inject the id-class. In that case the rendered HTML class list is exactly
what you put in `className`. This is rare — most blocks have non-empty styles.

**Legacy form (no `className` at all):** older validated markup in this repo
omits `className` entirely and renders `class="gb-element gb-element-{id}"`
(base class first — the plugin's backup class generation). This round-trips
too, but the order differs from the Option A form. Never mix forms within one
block: either `"className":"gb-element"` + id-first body (new work), or no
className + base-first body (when matching existing legacy markup).

### 3.4 JSON attribute key order matters

The editor serializes block attributes in a fixed key order (declaration order
from each block's `block.json`). Even though `{"a":1,"b":2}` and `{"b":2,"a":1}`
are semantically identical JSON, the editor's diff is **string-based**, so a
different key order in your output → mismatch → recovery.

The canonical order **per block** (verified against `block.json` declarations):

```
generateblocks/element
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes, align

generateblocks/text
  uniqueId, tagName, content, styles, css, globalClasses, htmlAttributes,
  icon, iconLocation, iconOnly

generateblocks/media
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes,
  mediaId, linkHtmlAttributes

generateblocks/shape
  uniqueId, html, styles, css, globalClasses, htmlAttributes

generateblocks/query
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes,
  queryType, paginationType, query, inheritQuery, showTemplateSelector

generateblocks/looper
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes

generateblocks/loop-item
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes

generateblocks/query-page-numbers
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes, midSize
```

**`className` is a WordPress core attribute** (provided by `supports.className`).
It is NOT in `block.json`. Empirically, `className` serializes **after** the
block's declared attributes — i.e. last in the JSON object.

Things that bite most often:

- **Text block: `content` comes 3rd**, right after `tagName`, BEFORE `styles`.
  Most other blocks put `styles` 3rd, but text is the exception.
- **Shape block: no `tagName`**, `html` is 2nd.
- **`htmlAttributes` BEFORE `className`** — `className` is always last among
  the common attributes.
- **`styles` BEFORE `css`** — both are styling, styles comes first.
- **Query block: `queryType, paginationType, query, inheritQuery`** in that
  order. `query` is NOT last.

When in doubt, omit the attribute rather than guess. An unset attribute is
safer than a misordered one.

### 3.6 Compact nesting — closing comments adjacent to closing tags

```html
<!-- WRONG -->
<!-- wp:generateblocks/element {"uniqueId":"x"} -->
<div class="gb-element-x gb-element">

</div>

<!-- /wp:generateblocks/element -->

<!-- RIGHT -->
<!-- wp:generateblocks/element {"uniqueId":"x"} -->
<div class="gb-element-x gb-element"></div>
<!-- /wp:generateblocks/element -->
```

Blank lines inside an empty block trigger recovery on some block types.

### 3.5 Never add stray HTML comments

The only allowed comments are `<!-- wp:... -->` block delimiters. No section
labels, no explanations, no `<!-- TODO -->`. Every other comment breaks
parsing.

---

## 4. The `<a>` tag rules (read carefully)

There are TWO competing failure modes for links. The correct pattern threads
between them.

### Failure mode A: text `<a>` strips its href on save

A `generateblocks/text` block with `tagName: "a"` does **not** reliably
preserve `htmlAttributes.href` through save. The href gets stripped.

### Failure mode B: element `<a>` with raw text content triggers recovery

A `generateblocks/element` with `tagName: "a"` is a container — it expects
inner blocks. If you put raw text inside (no `<!-- wp: -->` child), the
editor tries to recover.

### The correct pattern: element `<a>` wrapping a text child

For action links, buttons, and any link with visible text:

```html
<!-- wp:generateblocks/element {"uniqueId":"link1","tagName":"a","htmlAttributes":{"href":"https://example.com/page/"},"styles":{...},"css":"...","className":"gb-element-link1 gb-element"} -->
<a class="gb-element-link1 gb-element" href="https://example.com/page/">
    <!-- wp:generateblocks/text {"uniqueId":"link2","tagName":"span","styles":{...},"css":"..."} -->
    <span class="gb-text gb-text-link2">Read the full guide →</span>
    <!-- /wp:generateblocks/text -->
</a>
<!-- /wp:generateblocks/element -->
```

Why this works:
- Element `<a>` has an inner block child → no "raw text content" recovery
- `htmlAttributes.href` on the element block survives save → href preserved
- The text child is a `span`, not an `a` → no href stripping
- Arrow is a literal `→` in the text content → no `\2192` escape problems

For text-only inline links inside paragraphs (e.g. an `<a>` inside a sentence),
write the `<a>` directly into the rich text content of a `generateblocks/text`
paragraph block — don't wrap each one in its own block.

---

## 5. Block-type selection rules

### 5.1 Use `core/image` for static images with captions

`generateblocks/media` is fragile when combined with figcaptions and inline
border styles. For any static image that needs a caption, use `core/image`:

```html
<!-- wp:image {"id":123,"sizeSlug":"large","linkDestination":"none","style":{"border":{"radius":"0.75rem"}}} -->
<figure class="wp-block-image size-large has-custom-border" style="border-radius:0.75rem">
    <img src="https://example.com/image.jpg" alt="..." class="wp-image-123"/>
    <figcaption class="wp-element-caption">Caption text here</figcaption>
</figure>
<!-- /wp:image -->
```

Use `generateblocks/media` only for: hero/background-style images that participate
in a custom GB layout and don't need a caption, AND images inside dynamic
contexts (loop items, query loops) where you need GB's dynamic source binding.

### 5.2 Element vs Text vs Media vs Shape

| Need | Block |
|---|---|
| Container, layout wrapper | `generateblocks/element` |
| Visible text (heading, paragraph, span) | `generateblocks/text` |
| Linked container wrapping inner blocks | `generateblocks/element` with `tagName:"a"` |
| Static image with caption | `core/image` |
| Dynamic / loop image | `generateblocks/media` |
| SVG icon | `generateblocks/shape` |
| List | `core/list` with `className:"list"` |
| Emoji | `core/paragraph` (GB renders emoji glyphs incorrectly) |

### 5.3 No mixed content inside an element block

An element block holds inner blocks OR is empty. It cannot hold raw text
between its tags.

---

## 6. Responsive rules

### 6.1 Avoid responsive rules that require nested selectors

If a responsive rule needs a nested child selector inside `css`, drop it. The
selector will be stripped and the layout won't behave as you expect.

```css
/* WRONG — child selector inside media query gets stripped */
@media(max-width:640px){.gb-element-stats .stat-item{border:none}}

/* RIGHT — collapse the layout via base rules on the child block itself */
/* (give .stat-item its own GB block uniqueId and style it directly) */
```

### 6.2 Prefer `styles` object responsive keys when possible

```json
"styles":{
    "display":"grid",
    "gridTemplateColumns":"repeat(3,1fr)",
    "@media (max-width:768px)":{
        "gridTemplateColumns":"1fr"
    }
}
```

The plugin generates the matching CSS, sorted, minified, escaped — no manual
work, no recovery risk.

### 6.3 Skip cosmetic responsive details that need nested selectors

A 1-pixel border-width adjustment on small screens is not worth a recovery
risk. Drop it.

---

## 7. Pre-flight checklist

Before saving any output to a file, verify:

**JSON string escapes (the five substitutions)**
- [ ] Every `--` inside JSON strings is `\u002d\u002d`
- [ ] Every `&` inside JSON strings is `\u0026`
- [ ] Every `<` inside JSON strings is `\u003c`
- [ ] Every `>` inside JSON strings is `\u003e`
- [ ] Every `"` inside JSON string values is `\u0022` (never `\"`)

**JSON attribute order**
- [ ] Attributes appear in canonical key order (see §3.4): `uniqueId`, `tagName`, `styles`, `css`, `globalClasses`, `htmlAttributes`, `className`, `align`, ...
- [ ] `htmlAttributes` always comes BEFORE `className`
- [ ] `styles` always comes BEFORE `css`

**className and rendered class list**
- [ ] `className` does NOT include the auto-injected `gb-{type}-{uniqueId}` class (Option A — recommended)
- [ ] OR if it does, the rendered HTML duplicates the id-class (Option B)
- [ ] The rendered HTML class list matches what the editor would emit byte-for-byte

**CSS string rules**
- [ ] No `transition:` declarations in any `css` string
- [ ] No `:hover` rules in `css` (except pseudo-element hover or parent-hover-child)
- [ ] No descendant selectors in `css` other than the two allowed exceptions
- [ ] All CSS function args have no spaces (`repeat(3,1fr)`, not `repeat(3, 1fr)`)
- [ ] CSS properties alphabetically sorted inside each rule block
- [ ] `css` string is single-line and minified
- [ ] No `\xxxx` CSS escapes for special characters — use literal characters

**HTML body**
- [ ] `htmlAttributes` is `{}`, never `[]`
- [ ] All `href` values in JSON are absolute `https://...` URLs with `&` encoded as `\u0026`
- [ ] All `href` values in rendered `<a>` HTML are the same URL with `&` encoded as `&amp;`
- [ ] Element `<a>` blocks contain at least one inner block child
- [ ] No HTML comments other than WP block delimiters
- [ ] No `core/image` figure rendered as `generateblocks/media` if it has a caption (and vice versa for dynamic loop images)
