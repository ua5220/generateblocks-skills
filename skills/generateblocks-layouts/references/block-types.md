---
title: Block Types Reference
description: Source-verified attribute specifications for the four GenerateBlocks V2 core blocks (element, text, media, shape) with canonical markup examples.
---

# Block Types Reference

The four core V2 blocks, verified against `dist/blocks/*/block.json` in
GB 2.4.0. All examples follow `recovery-rules.md` (Option A className, plain
object `htmlAttributes`, escaped `--`, sorted minified css).

2.4 note: `text.content` and `media.mediaId` are flagged `"role":"content"`
in block.json (powers Pro 2.7 content-only editing). Editor schema only — it
changes neither attribute order nor saved markup.

Common attributes on every block: `uniqueId`, `styles` (object), `css`
(string), `globalClasses` (array), `htmlAttributes` (**plain object**).
`className` is the WP core attribute, serialized last; per Option A it holds
the base class (`gb-element`, `gb-text`, ...) plus any extra classes, and the
plugin auto-injects `gb-{type}-{uniqueId}` at the front of the rendered class
list.

The `styles` object accepts camelCase CSS properties, `&`-prefixed nested
selectors (`&:hover`, `&::before`, `& > .child`), and `@media` / `@supports`
/ `@container` at-rule keys (those three only).

---

## 1. Element Block (`generateblocks/element`)

Container for layout structure. Holds inner blocks — never raw text.

**Attribute order:** `uniqueId, tagName, styles, css, globalClasses, htmlAttributes, align` (+ `className` last)

**tagName enum (verified):** `div`, `section`, `article`, `aside`, `header`,
`footer`, `nav`, `main`, `figure`, `a`, `ul`, `ol`, `li`, `dl`, `dt`, `dd`

No `blockquote`, no `form`, no `button` — those need core blocks or a text
block (`button`).

### Basic container

```html
<!-- wp:generateblocks/element {"uniqueId":"cont001","tagName":"div","styles":{"backgroundColor":"#f5f5f5","padding":"2rem"},"css":".gb-element-cont001{background-color:#f5f5f5;padding:2rem}","className":"gb-element"} -->
<div class="gb-element-cont001 gb-element">
    <!-- child blocks -->
</div>
<!-- /wp:generateblocks/element -->
```

### Full-width section + inner container

```html
<!-- wp:generateblocks/element {"uniqueId":"sect001","tagName":"section","styles":{"backgroundColor":"#0a0a0a","paddingTop":"6rem","paddingBottom":"6rem","paddingLeft":"1.5rem","paddingRight":"1.5rem"},"css":".gb-element-sect001{background-color:#0a0a0a;padding:6rem 1.5rem}","align":"full","className":"gb-element alignfull"} -->
<section class="gb-element-sect001 gb-element alignfull">
    <!-- wp:generateblocks/element {"uniqueId":"sect002","tagName":"div","styles":{"maxWidth":"var(\u002d\u002dgb-container-width)","marginLeft":"auto","marginRight":"auto"},"css":".gb-element-sect002{margin-left:auto;margin-right:auto;max-width:var(\u002d\u002dgb-container-width)}","className":"gb-element"} -->
    <div class="gb-element-sect002 gb-element">
        <!-- section content -->
    </div>
    <!-- /wp:generateblocks/element -->
</section>
<!-- /wp:generateblocks/element -->
```

### Responsive grid

```html
<!-- wp:generateblocks/element {"uniqueId":"grid001","tagName":"div","styles":{"display":"grid","gridTemplateColumns":"repeat(3,minmax(0,1fr))","gap":"2rem","@media (max-width:1024px)":{"gridTemplateColumns":"repeat(2,minmax(0,1fr))"},"@media (max-width:768px)":{"gridTemplateColumns":"1fr"}},"css":".gb-element-grid001{display:grid;gap:2rem;grid-template-columns:repeat(3,minmax(0,1fr))}@media (max-width:1024px){.gb-element-grid001{grid-template-columns:repeat(2,minmax(0,1fr))}}@media (max-width:768px){.gb-element-grid001{grid-template-columns:1fr}}","className":"gb-element"} -->
<div class="gb-element-grid001 gb-element">
    <!-- grid items -->
</div>
<!-- /wp:generateblocks/element -->
```

### Link wrapper (the canonical action-link pattern)

Element `<a>` wraps inner blocks; href lives in `htmlAttributes` (plain
object, absolute URL):

```html
<!-- wp:generateblocks/element {"uniqueId":"link001","tagName":"a","styles":{"display":"block","textDecoration":"none"},"css":".gb-element-link001{display:block;text-decoration:none}","htmlAttributes":{"href":"https://example.com/page/","target":"_blank","rel":"noopener"},"className":"gb-element"} -->
<a class="gb-element-link001 gb-element" href="https://example.com/page/" target="_blank" rel="noopener">
    <!-- wp:generateblocks/text {"uniqueId":"link002","tagName":"span","content":"Read the guide →","styles":{},"css":"","className":"gb-text"} -->
    <span class="gb-text-link002 gb-text">Read the guide →</span>
    <!-- /wp:generateblocks/text -->
</a>
<!-- /wp:generateblocks/element -->
```

See `recovery-rules.md` §4 for why links must be element-`<a>`-wrapping-text,
never a bare text `<a>` with href, never element `<a>` with raw text.

---

## 2. Text Block (`generateblocks/text`)

Leaf block for visible text. Cannot contain inner blocks; CAN contain inline
HTML (`<strong>`, `<em>`, `<a>`, `<span style="...">`) in its rich-text content.

**Attribute order:** `uniqueId, tagName, content, styles, css, globalClasses, htmlAttributes, icon, iconLocation, iconOnly` (+ `className` last) —
**`content` is 3rd**, the only block where styles isn't 3rd.

**tagName enum (verified):** `p`, `span`, `div`, `h1`–`h6`, `a`, `button`,
`figcaption`, `li`

### Heading

```html
<!-- wp:generateblocks/text {"uniqueId":"head001","tagName":"h1","content":"Page Title","styles":{"fontSize":"clamp(2rem,5vw,3.5rem)","fontWeight":"900","lineHeight":"1.1","letterSpacing":"-0.03em","color":"#0a0a0a"},"css":".gb-text-head001{color:#0a0a0a;font-size:clamp(2rem,5vw,3.5rem);font-weight:900;letter-spacing:-0.03em;line-height:1.1}","className":"gb-text"} -->
<h1 class="gb-text-head001 gb-text">Page Title</h1>
<!-- /wp:generateblocks/text -->
```

### Paragraph with inline link

Inline links go in the rich-text content, not separate blocks. In the JSON
`content` value, inline HTML is escaped with the five substitutions
(`recovery-rules.md` §1): `\u003c` for `<`, `\u003e` for `>`, `\u0022` for
`"`, `\u0026` for `&`, `\u002d\u002d` for `--`. The HTML body carries the
literal markup:

```html
<!-- wp:generateblocks/text {"uniqueId":"para001","tagName":"p","content":"Read our \u003ca href=\u0022https://example.com/guide/\u0022\u003efull guide\u003c/a\u003e for details.","styles":{"fontSize":"1.125rem","lineHeight":"1.7","color":"#5c5c5c"},"css":".gb-text-para001{color:#5c5c5c;font-size:1.125rem;line-height:1.7}","className":"gb-text"} -->
<p class="gb-text-para001 gb-text">Read our <a href="https://example.com/guide/">full guide</a> for details.</p>
<!-- /wp:generateblocks/text -->
```

### Icon support (`icon`, `iconLocation`, `iconOnly`)

A text block can carry an inline SVG icon. The icon HTML is stored in the
`icon` attribute (sourced from the `.gb-shape` span in the body):

```html
<!-- wp:generateblocks/text {"uniqueId":"feat001","tagName":"p","content":"Fast delivery","styles":{"display":"flex","alignItems":"center","columnGap":"0.5rem"},"css":".gb-text-feat001{align-items:center;column-gap:0.5rem;display:flex}","icon":"\u003csvg viewBox=\u00220 0 24 24\u0022 fill=\u0022none\u0022 stroke=\u0022currentColor\u0022 stroke-width=\u00222\u0022\u003e\u003cpath d=\u0022M5 13l4 4L19 7\u0022/\u003e\u003c/svg\u003e","iconLocation":"before","className":"gb-text"} -->
<p class="gb-text-feat001 gb-text"><span class="gb-shape"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 13l4 4L19 7"/></svg></span>Fast delivery</p>
<!-- /wp:generateblocks/text -->
```

`iconLocation`: `"before"` (default) or `"after"`. `iconOnly:true` hides the
text visually. The icon SVG appears twice — escaped in the JSON `icon`
attribute AND literally inside the `.gb-shape` span in the body; they must
match. If this dual-encoding feels risky, use a separate
`generateblocks/shape` block instead (simpler, same visual result).

### Button-tag text (for JS-triggered actions, not links)

```html
<!-- wp:generateblocks/text {"uniqueId":"btn001","tagName":"button","content":"Subscribe","styles":{"backgroundColor":"#c0392b","border":"0","borderRadius":"2rem","color":"#ffffff","cursor":"pointer","padding":"0.875rem 1.75rem"},"css":".gb-text-btn001{background-color:#c0392b;border:0;border-radius:2rem;color:#ffffff;cursor:pointer;padding:0.875rem 1.75rem}","className":"gb-text"} -->
<button class="gb-text-btn001 gb-text">Subscribe</button>
<!-- /wp:generateblocks/text -->
```

For links styled as buttons, use the element-`<a>` + text-span pattern, not
text `<a>` (its href doesn't survive saves).

---

## 3. Media Block (`generateblocks/media`)

Images only. **tagName enum: `img` — nothing else.** Self-closing body.

**Attribute order:** `uniqueId, tagName, styles, css, globalClasses, htmlAttributes, mediaId, linkHtmlAttributes` (+ `className` last)

| Attribute | Notes |
|---|---|
| `mediaId` | WP attachment ID (number). When > 0, the server adds `width`/`height`/`srcset`/`sizes` automatically at render. Use `0`/omit for external or dynamic images |
| `htmlAttributes` | `src` (required), `alt` (required — empty string for decorative), `loading`, `width`, `height` |
| `linkHtmlAttributes` | Plain object; when set with `href`, the save wraps the img in an `<a>` |

There is **no `mediaType` attribute** — older docs that show it are wrong.

### Static image (no caption)

```html
<!-- wp:generateblocks/media {"uniqueId":"img001","tagName":"img","styles":{"width":"100%","height":"auto","borderRadius":"1rem"},"css":".gb-media-img001{border-radius:1rem;height:auto;width:100%}","htmlAttributes":{"src":"https://example.com/photo.jpg","alt":"Team at work","loading":"lazy"},"className":"gb-media"} -->
<img class="gb-media-img001 gb-media" src="https://example.com/photo.jpg" alt="Team at work" loading="lazy"/>
<!-- /wp:generateblocks/media -->
```

### Dynamic image in a loop

```html
<!-- wp:generateblocks/media {"uniqueId":"img002","tagName":"img","styles":{"aspectRatio":"16/9","objectFit":"cover","width":"100%"},"css":".gb-media-img002{aspect-ratio:16/9;object-fit:cover;width:100%}","htmlAttributes":{"src":"{{featured_image size:large}}","alt":"{{featured_image key:alt}}","loading":"lazy"},"className":"gb-media"} -->
<img class="gb-media-img002 gb-media" src="{{featured_image size:large}}" alt="{{featured_image key:alt}}" loading="lazy"/>
<!-- /wp:generateblocks/media -->
```

(Tag syntax: `dynamic-tags.md`. `mediaId` omitted — there's no fixed attachment.)

### When NOT to use media

| Case | Use instead |
|---|---|
| Image with caption | `core/image` (figcaption support) |
| Gallery | `core/gallery` |
| Video / audio / embeds | `core/video`, `core/audio`, `core/embed` |

---

## 4. Shape Block (`generateblocks/shape`)

Inline SVG wrapped in a `<span class="gb-shape-{id} gb-shape">`. No `tagName`.

**Attribute order:** `uniqueId, html, styles, css, globalClasses, htmlAttributes` (+ `className` last)

The `html` attribute is HTML-sourced from the `.gb-shape` selector — the SVG
appears ONLY in the body (not duplicated in JSON), which makes shape the
safest icon carrier.

### Styling

Two working approaches:

1. **`styles.svg` object** — generates `.gb-shape-{id} svg{...}`:

```html
<!-- wp:generateblocks/shape {"uniqueId":"icon001","styles":{"display":"inline-flex","svg":{"fill":"currentColor","height":"1.5rem","width":"1.5rem"}},"css":".gb-shape-icon001{display:inline-flex}.gb-shape-icon001 svg{fill:currentColor;height:1.5rem;width:1.5rem}","className":"gb-shape"} -->
<span class="gb-shape-icon001 gb-shape"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 12h14M12 5l7 7-7 7"/></svg></span>
<!-- /wp:generateblocks/shape -->
```

2. **Wrapper styles + inline SVG attributes** (live-site pattern):

```html
<!-- wp:generateblocks/shape {"uniqueId":"check001","styles":{"width":"20px","height":"20px","color":"#10b981"},"css":".gb-shape-check001{color:#10b981;height:20px;width:20px}","className":"gb-shape"} -->
<span class="gb-shape-check001 gb-shape"><svg stroke-linejoin="round" stroke-linecap="round" stroke-width="3" stroke="currentColor" fill="none" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg></span>
<!-- /wp:generateblocks/shape -->
```

SVG attribute order in the body matters for round-trip stability — write:
`stroke-linejoin`, `stroke-linecap`, `stroke-width`, `stroke`, `fill`,
`viewBox`, `height`, `width` (the editor's observed reorder). More patterns:
`svg-icons.md`.

---

## Attribute summary

| Block | Order (block.json) | tagName enum | Inner blocks |
|---|---|---|---|
| element | uniqueId, tagName, styles, css, globalClasses, htmlAttributes, align | div section article aside header footer nav main figure a ul ol li dl dt dd | Yes |
| text | uniqueId, tagName, **content**, styles, css, globalClasses, htmlAttributes, icon, iconLocation, iconOnly | p span div h1–h6 a button figcaption li | No (inline HTML in content) |
| media | uniqueId, tagName, styles, css, globalClasses, htmlAttributes, mediaId, linkHtmlAttributes | img | No |
| shape | uniqueId, **html**, styles, css, globalClasses, htmlAttributes | — | No |

All four support `align: false`, `className: false` in block.json — meaning
no core alignment toolbar / additional-classes panel in the UI. `align` and
`className` still serialize (GB element declares `align` itself; `className`
comes from the serializer) — keep using `"align":"full"` +
`"className":"gb-element alignfull"` for full-width, exactly as production
markup does.

---

## 5. When to use core blocks instead

| Content | Block | Why |
|---|---|---|
| Captioned image | `core/image` | figcaption |
| Gallery / video / audio / embeds | `core/gallery` `core/video` `core/audio` `core/embed` | players & lightbox |
| Data table | `core/table` | semantics |
| List content | `core/list` (`className:"list"`) | rich-text list items |
| Quote with citation | `core/quote` | cite support |
| Code | `core/code` | escaping handled |
| Emoji-heavy text | `core/paragraph` | GB renders emoji glyphs incorrectly |
| Collapsible without Pro | `core/details` | free `<details>/<summary>` |

**Rule of thumb:** GB for structure + styling + dynamic data; core for
specialized content with built-in behavior. For dynamic post lists, prefer
GB query family over `core/query` (see `query-block.md`).
