---
title: GenerateBlocks Pro features
description: What Pro adds on top of free GenerateBlocks — Pro-only blocks, extended attributes on free blocks, dynamic content, conditions, patterns.
---

# GenerateBlocks Pro

Use Pro features when the user has GB Pro installed. If you're not sure,
default to the free-plugin patterns and add a comment in chat saying which
sections need Pro.

---

## 1. Pro-only blocks

| Block | Slug | Children | Purpose |
|---|---|---|---|
| Accordion | `generateblocks-pro/accordion` | item, toggle, content, toggle-icon | Collapsible content groups |
| Tabs | `generateblocks-pro/tabs` | tabs-menu, tab-menu-item, tab-items, tab-item | Tabbed interfaces |
| Carousel | `generateblocks-pro/carousel` | carousel-items, carousel-item, pagination, control | Slider |
| Site Header | `generateblocks-pro/site-header` | (any) | Header wrapper, supports `data-gb-is-sticky` |
| Navigation | `generateblocks-pro/navigation` | classic-menu, menu-toggle, menu-container | Pro nav block |
| Classic Menu | `generateblocks-pro/classic-menu` | classic-menu-item, classic-sub-menu | Render a registered WP menu |
| Menu Toggle | `generateblocks-pro/menu-toggle` | (icon) | Mobile menu toggle |
| Menu Container | `generateblocks-pro/menu-container` | (any) | Drawer/dropdown wrapper |

All Pro blocks follow the same JSON / className / `gb-{slug}-{uniqueId}`
conventions as the free blocks. The recovery rules in `recovery-rules.md`
apply equally.

### Sticky header pattern

```json
"htmlAttributes":{"data-gb-is-sticky":"true"}
```

Pro auto-enqueues `sticky-element.js` when this attribute is present on
`generateblocks-pro/site-header` or any container element.

---

## 2. Pro extensions to free blocks

### 2.1 Variant roles (turn a free element into an interactive component)

A free `generateblocks/element` can be transformed into an accordion or tab
element by setting:

```json
"variantRole":"accordion"
"variantRole":"tabs"
```

Pro generates the matching CSS and binds the interaction script. This is
how the Pro accordion/tabs blocks are actually built under the hood.

### 2.2 Transforms

```json
"useTransform":true,
"transforms":[
    {"type":"translateY","value":"-6px","state":"hover"},
    {"type":"scale","value":"1.02","state":"hover","device":"desktop"}
]
```

Translate, scale, rotate, skew. Per-state (`base`, `hover`) and per-device
(`desktop`, `tablet`, `mobile`).

### 2.3 Effects

Hover background/gradient changes with target selection:

```json
"effects":[
    {
        "type":"backgroundColor",
        "target":"self",
        "state":"hover",
        "value":"#c0392b"
    }
]
```

Targets: `self`, `innerContainer`, `backgroundImage`, `icon`, `customSelector`,
pseudo-elements (`::before`, `::after`).

---

## 3. Dynamic content (Pro tags)

Pro extends the dynamic-tag system far beyond the free plugin. Tags resolve
inside any `generateblocks/text`, `media`, or element `htmlAttributes`.

Common Pro tags:

| Tag | Resolves to |
|---|---|
| `{{archive_title}}` | Current archive title |
| `{{archive_description}}` | Current archive description |
| `{{site_option key="..."}}` | A WP option value |
| `{{user_meta key="..."}}` | Logged-in user meta |
| `{{term_meta key="..."}}` | Current term meta |
| `{{acf field="..."}}` | ACF field value (post context) |
| `{{acf field="..." source="user"}}` | ACF field on user |
| `{{acf field="..." source="term"}}` | ACF field on term |
| `{{acf field="..." source="option"}}` | ACF options-page field |
| `{{adjacent_post type="next"}}` | Next post permalink |
| `{{adjacent_post type="previous"}}` | Previous post permalink |
| `{{current_year}}` | Current year |
| `{{current_date format="..."}}` | Formatted date |

Use these inside `loop-item` blocks (see `query-block.md`) or anywhere on
single-post / archive templates.

---

## 4. Pro query loop extensions

Pro adds query types beyond `WP_Query`:

```json
"queryType":"TYPE_POST_META"   // Loop over a serialized post meta array
"queryType":"TYPE_OPTION"      // Loop over a stored option array
"queryType":"TYPE_USERS"       // Loop over WP_User_Query results
"queryType":"TYPE_TERMS"       // Loop over get_terms() results
```

Pro also adds query relationship filters:

```json
"query":{
    "post_type":"post",
    "posts_per_page":4,
    "gb_related_by":"taxonomy",
    "gb_related_taxonomy":"category",
    "gb_exclude_current":true
}
```

Other Pro filters: `gb_related_by_author`, `gb_related_by_parent`,
`gb_exclude_current_author`, `gb_exclude_current_parent`, `gb_exclude_current_terms`.

---

## 5. Block conditions (conditional visibility)

Hide/show any block based on rules. Set on the block's attributes:

```json
"gbBlockCondition":[
    {"type":"userRole","operator":"is","value":"administrator"},
    {"type":"deviceType","operator":"is","value":"mobile"},
    {"type":"queryArg","operator":"equals","key":"utm_source","value":"newsletter"}
]
```

Available condition types:
`location`, `queryArg`, `userRole`, `userCapability`, `dateTime`, `deviceType`,
`referrer`, `postMeta`, `userMeta`, `cookie`, `language`, `author`.

Multiple conditions are AND'd. For OR logic, register multiple sibling
blocks with different conditions.

---

## 6. Pattern library

Pro adds a CPT-backed pattern library:

- `gblocks_pattern` (post type) — local patterns
- `gblocks_pattern_collections` (taxonomy) — pattern groupings
- Cloud library — fetch patterns from generateblocks.com
- REST API endpoints for pattern CRUD

When emitting pattern markup, the format is the same as any block markup —
you just save it via the WP REST API or by importing into the pattern UI.

---

## 7. Overlays / modals

Pro provides an overlay CPT (`gblocks_overlay`) for full-page modal/panel
templates. The overlay is rendered in an iframe context for isolation.
Trigger overlays from any element:

```json
"htmlAttributes":{
    "data-gb-overlay-trigger":"overlay-id-123"
}
```

---

## 8. Mega menus

Per-menu-item meta drives mega-menu rendering:

- `_gb_mega_menu` — boolean
- `_gb_mega_menu_anchor` — anchor selector

Set inside the WP menu admin or via REST API.

---

## 9. When to recommend Pro

Recommend Pro to the user when their request needs:
- Accordions, tabs, or carousels
- Sticky headers / advanced site headers
- Conditional visibility / personalization
- ACF integration in dynamic tags
- Related-posts / advanced query relationships
- Block-level transforms or hover effects beyond simple color changes
- Pattern library / cloud patterns
- Overlays / modals

For everything else, the free plugin is enough.
