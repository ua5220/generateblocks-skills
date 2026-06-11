---
title: GenerateBlocks Pro overview (2.6)
description: What Pro adds on top of free GenerateBlocks — block catalog, global classes, query extensions, conditions, forms, overlays — with pointers to the deep-dive references.
---

# GenerateBlocks Pro (2.6)

Use Pro features only when the user has GB Pro installed. If unsure, default
to free-plugin patterns and say in chat which parts would need Pro.

Status note (June 2026): public stable is free 2.2.1 / Pro 2.5.0; free 2.3
and Pro 2.6 (Forms, CSS Mode) are in beta/RC — this repo carries the RC
source. If the user's site runs stable, Forms and CSS Mode aren't available
yet. Note 2.3 also disables the legacy "Additional CSS" block option by
default in favor of CSS Mode.

## What Pro adds — map

| Capability | Detail file |
|---|---|
| Accordion, Tabs, Carousel, Navigation, Site Header, Overlays, Mega Menus | `pro-interactive.md` |
| Forms (fields, validation, email/webhook/ESP integrations, Turnstile) — **2.6** | `pro-forms.md` |
| Block/menu conditions (`gbBlockCondition` → Conditions CPT) | `conditions.md` |
| Pro dynamic tags (`archive_title`, `site_*`, `option`, `term_meta`, `user_meta`, `loop_index`, `loop_item`, adjacent-post `source:`) | `dynamic-tags.md` §6 |
| ACF / custom-field deep integration | `acf-and-custom-fields.md` |
| Query extensions (`"current"` magic values, `stickyPosts`, `post_meta`/`option` loop types) | `query-block.md` §3, §6 |
| Global classes & Styles dashboard | below |
| Pattern library (local CPT + remote pro library) | below |

## Pro block catalog (28 blocks, verified)

```
Forms (2.6):     form, form-field, form-field-label, form-field-control, form-render
Accordion:       accordion, accordion-item, accordion-toggle, accordion-toggle-icon, accordion-content
Tabs:            tabs, tabs-menu, tab-menu-item, tab-items, tab-item
Carousel (2.5):  carousel, carousel-items, carousel-item, carousel-control, carousel-pagination
Navigation (2.2):navigation, menu-toggle, menu-container, classic-menu, classic-menu-item, classic-sub-menu
Header (2.2):    site-header
```

All namespaced `generateblocks-pro/{slug}`, class pattern
`gb-{slug}-{uniqueId}`, same recovery rules — but **attribute declaration
order differs per block**: see `pro-interactive.md` top section before
emitting any Pro block JSON.

## Global classes

- Created/managed in the **Styles dashboard** (GenerateBlocks → Styles);
  stored server-side (option + `gblocks_global_style` CPT), CSS compiled and
  enqueued site-wide.
- A block opts in via its `globalClasses` array attribute; the class names
  are also written into the rendered HTML class list:

```json
"globalClasses":["button-primary"]
```
```html
<a class="gb-element-cta001 gb-element button-primary" href="...">
```

- Use global classes when the same component style repeats across the site
  (buttons, cards, badges). The per-block `styles`/`css` then carries only
  instance-specific overrides.
- 2.6 adds **CSS Mode** (write raw CSS on blocks and global styles) and a CSS
  Properties panel in the Styles builder.
- Hand-authoring: reference existing global classes freely. Creating them
  programmatically goes through the Styles REST API — otherwise tell the user
  to create the class in the dashboard first.

## Pattern library

- Local patterns: `wp_block` posts + `gblocks_pattern_collections` taxonomy.
- Remote pro pattern library fetched from generatepress.com (2.0+),
  "instant patterns" since 2.5.
- Pattern markup is just block markup — anything this skill emits can be
  saved as a pattern. See `patterns.md` for registration options.

## Version timeline (what exists at which Pro version)

| Version | Features |
|---|---|
| 2.0 | v2 rewrite: accordion/tabs v2, ACF dynamic tags, query extensions |
| 2.1 | Device visibility, nested accordions, FAQ schema, a11y options |
| 2.2 | Navigation + Site Header blocks |
| 2.3 | Overlays (modals, off-canvas, anchored), mega menus |
| 2.4 | Conditions system (blocks + menu items) |
| 2.5 | Carousel, site logo/URL tags, grid-template controls |
| 2.6 | Forms system + integrations, CSS Mode |

## When to recommend Pro

- Accordions, tabs, carousels, mega menus, modals/off-canvas
- Site header/navigation built in blocks rather than the theme
- Native forms with ESP integrations
- Conditional rendering (roles, scheduling, devices, meta)
- Related-posts queries (`"current"` magic values) and ACF repeater loops
- Archive/site/option/term/user dynamic tags, prev/next post navigation
- Global classes / design-token management in the Styles dashboard

Everything else — layout, styling, static sections, standard query loops,
free dynamic tags — works on the free plugin.
