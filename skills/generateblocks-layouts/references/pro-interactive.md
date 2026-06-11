---
title: GB Pro interactive blocks
description: Accordion, Tabs, Carousel, Navigation, Site Header, Overlays, Mega Menus — block hierarchies, verified attributes, and hand-authoring guidance.
---

# GB Pro Interactive Blocks

All of these require GB Pro. They follow the same serialization rules as free
blocks (`recovery-rules.md` applies in full), with their own `gb-{slug}-{uniqueId}`
class patterns and frontend JS that the plugin enqueues automatically when the
block is present.

## ⚠ Attribute declaration order differs per block

Recovery rule §3.4 (JSON keys serialize in block.json declaration order) still
applies, but several Pro blocks declare attributes in a different order than
the free blocks. Verified orders:

```
accordion / accordion-item / accordion-toggle / accordion-content / tabs /
tabs-menu / tab-menu-item / tab-items / tab-item
  uniqueId, tagName, styles, css, globalClasses, htmlAttributes [, extras]

carousel / carousel-items / carousel-pagination / navigation / menu-container /
site-header
  uniqueId, styles, css, globalClasses, tagName, htmlAttributes [, extras]

carousel-control
  uniqueId, controlType, openIcon, closeIcon, iconLocation, iconOnly, content,
  carouselId, tagName, htmlAttributes, styles, globalClasses, css

menu-toggle
  uniqueId, openIcon, closeIcon, iconLocation, styles, css, globalClasses,
  htmlAttributes, tagName, content, iconOnly

classic-menu
  menu, uniqueId, styles, css, globalClasses
```

Extras position: `openByDefault` (accordion-item) and `tabItemOpen`
(tab-menu-item, tab-item) come after `htmlAttributes`; `openIcon`/`closeIcon`
on accordion-toggle-icon likewise.

## 1. Accordion

```
generateblocks-pro/accordion                tagName: div|section|aside|nav|ul|ol|li
└── generateblocks-pro/accordion-item       tagName: div|section|aside|li
                                            + openByDefault (bool)
    ├── generateblocks-pro/accordion-toggle tagName: div|button (use button)
    │   └── [text blocks for the label]
    │   └── generateblocks-pro/accordion-toggle-icon
    │       tagName: span, + openIcon/closeIcon (SVG strings)
    └── generateblocks-pro/accordion-content tagName: div|section|aside|ul|ol
        └── [any blocks]
```

- ARIA wiring (`aria-expanded`, `aria-controls`, generated IDs) is added by
  the save/render functions — don't hand-write it.
- `openByDefault:true` on an item renders it expanded.
- Nested accordions are supported (2.1+).
- FAQ schema is available as an accordion option in the editor UI.
- Frontend: `dist/accordion.js` + `accordion-style.css`, auto-enqueued.

**Hand-authoring guidance:** the toggle/content wiring (IDs, aria, state
classes) is generated. Build one accordion in the editor, copy its serialized
markup as your template, then replicate items. Do not invent the rendered
HTML from the block comments alone.

## 2. Tabs

```
generateblocks-pro/tabs
├── generateblocks-pro/tabs-menu
│   └── generateblocks-pro/tab-menu-item    tagName: div|button|ul|ol|li, + tabItemOpen
└── generateblocks-pro/tab-items
    └── generateblocks-pro/tab-item         tagName: div|ul|ol|li, + tabItemOpen
```

- The Nth `tab-menu-item` controls the Nth `tab-item` — order is the link.
- Set `tabItemOpen:true` on exactly one menu item AND its matching item.
- ARIA roles (`tablist`/`tab`/`tabpanel`, `aria-selected`, `aria-controls`)
  are generated. Same guidance as accordion: copy from editor output.
- Frontend: `dist/tabs.js`.

## 3. Carousel (Pro 2.5+)

```
generateblocks-pro/carousel
├── generateblocks-pro/carousel-items
│   └── generateblocks-pro/carousel-item        (one per slide)
├── generateblocks-pro/carousel-control         tagName: button|a
│   controlType (prev|next), openIcon, iconOnly, carouselId
└── generateblocks-pro/carousel-pagination      (dots; server/JS-rendered)
```

- Swiper-based frontend (`dist/carousel.js`); options (autoplay, loop,
  slides-per-view, breakpoints) are set in the editor UI panel.
- Carousel settings live in editor-managed attributes/data — build the
  carousel shell in the editor, then hand-author only the `carousel-item`
  contents (those are ordinary blocks).

## 4. Navigation + Site Header (Pro 2.2+)

```
generateblocks-pro/site-header              tagName: div|section|aside|nav|header
└── [logo blocks, etc.]
└── generateblocks-pro/navigation           tagName: div|section|aside|nav
    + subMenuType ("hover" default)
    ├── generateblocks-pro/menu-toggle      tagName: button (mobile hamburger)
    │   openIcon/closeIcon (SVG), content, iconOnly
    └── generateblocks-pro/menu-container
        └── generateblocks-pro/classic-menu  menu: WP menu ID/slug
            (classic-menu-item / classic-sub-menu generated from the WP menu)
```

- `classic-menu` renders a **WordPress menu** (Appearance → Menus) — content
  comes from the menu, not from inner blocks.
- `subMenuType` controls dropdown behavior; mobile off-canvas/modal behavior
  is configured on the navigation/menu-container in the UI.
- Sticky header: set via the site-header UI (don't hand-write
  `data-gb-is-sticky` — verify the current attribute in the editor if needed).

## 5. Overlays (Pro 2.3+) — modals, off-canvas, mega menu panels

- Overlay templates are **`gblocks_overlay` posts** (Dashboard → GenerateBlocks
  → Overlays), built with normal blocks.
- An overlay is attached to a trigger element/menu item via the editor UI;
  placement options include modal, off-canvas panel, and anchored dropdown.
- **Mega menus** (2.3+): an overlay anchored to a WP menu item
  (`includes/mega-menus/class-mega-menus.php`). Configure on the menu item.
- Triggers: click, hover, exit intent, percentage scrolled, time delay, or a
  custom JS event (e.g. `wc-blocks_added_to_cart`), with cookie-based
  frequency capping. Entrance animations built in: fade / slide / scale from
  any direction with speed control.
- Hand-authoring: author the overlay's *content* as normal block markup
  inside the overlay post; leave trigger wiring and animation settings to
  the UI.

## 6. Decision guide

| Need | Use |
|---|---|
| FAQ with schema | Accordion (+ FAQ schema option) |
| Content switcher / pricing toggle | Tabs |
| Testimonial/logo slider | Carousel |
| Site header + menu (FSE-free) | Site Header + Navigation + Classic Menu |
| Modal / slide-in panel / mega menu | Overlay |
| One-off collapsible without Pro | `<details>`/`<summary>` is NOT in GB tag enums — use a Pro accordion, or core blocks with a custom HTML block |

## 7. Free-plugin fallbacks

Without Pro, tell the user the section needs Pro, and offer:
- Accordion → stacked sections (always-open), or core `details` via Custom HTML
- Tabs → anchor-linked sections
- Carousel → CSS scroll-snap row (overflow-x scroll on an element block —
  works with free GB, no JS; see `css-patterns.md`)
- Site header → theme header (GeneratePress + Elements)
