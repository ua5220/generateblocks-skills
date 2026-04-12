---
title: Query / Looper / Loop-Item (V2 dynamic content)
description: How to build dynamic post lists, archives, related posts, and pagination with the GenerateBlocks V2 Query block family.
---

# Query Block (V2)

GenerateBlocks V2 ships its own query primitives. These are different from
WordPress core's `core/query` + `core/post-template` and from the legacy
`generateblocks/query-loop` block. Use these V2 blocks for any new dynamic
content.

The five blocks form a strict hierarchy:

```
generateblocks/query                        ← runs the WP_Query, holds query args
├── generateblocks/looper                   ← iterates results, ONE per query
│   └── generateblocks/loop-item            ← per-post template, ONE per looper
│       └── [any blocks: text, media, element, shape...]
├── generateblocks/query-no-results         ← optional empty state
└── generateblocks/query-page-numbers       ← optional pagination UI
```

`looper`, `query-no-results`, and `query-page-numbers` MUST be inside a `query`
ancestor. `loop-item` MUST be the only child type inside a `looper`.

---

## 1. The four blocks

### 1.1 `generateblocks/query`

Holds the query args and the WP_Query instance. Renders as a wrapper element.

| Attribute | Required | Notes |
|---|---|---|
| `uniqueId` | yes | id-class generated as `gb-query-{uniqueId}` if `styles` non-empty |
| `tagName` | yes | One of: `div`, `section`, `article`, `aside`, `header`, `footer`, `nav`, `main` |
| `queryType` | yes | `WP_Query` (default). GB Pro can add other types. |
| `paginationType` | yes | `standard` or `instant` (instant = AJAX/instant-page replacement) |
| `query` | yes | WP_Query args object — see §2 |
| `inheritQuery` | no | If `true`, inherits the global `$wp_query` (used inside archive templates). When true, `query` is ignored. |

The query block provides context to all descendants:
`generateblocks/queryData`, `generateblocks/queryId`, `generateblocks/queryType`,
`generateblocks/inheritQuery`, `generateblocks/paginationType`.

### 1.2 `generateblocks/looper`

Iterates the query results. Holds layout styles for the list (grid, flex, gap).

| Attribute | Required | Notes |
|---|---|---|
| `uniqueId` | yes | |
| `tagName` | yes | One of: `div`, `section`, `article`, `aside`, `header`, `footer`, `nav`, `main`, `ul`, `ol` |

**Constraint:** `allowedBlocks: ["generateblocks/loop-item"]`. The looper can
only contain a single `loop-item` child — that child is the template that
gets rendered once per post.

### 1.3 `generateblocks/loop-item`

Per-post template. Rendered once per iteration. WordPress post classes are
auto-injected via `WP_HTML_Tag_Processor` when the query type is `WP_Query`.

| Attribute | Required | Notes |
|---|---|---|
| `uniqueId` | yes | |
| `tagName` | yes | One of: `div`, `li`, `a`, `article`, `section`, `aside` |

Reads loop context: `generateblocks/loopItem` (current post), `generateblocks/loopIndex`
(1-based counter), `postId`, `postType`.

### 1.4 `generateblocks/query-no-results`

Conditional empty-state. Renders only when the parent query returns zero
results. No required attributes. Place inside the `query` block, after
the `looper`.

### 1.5 `generateblocks/query-page-numbers`

Pagination UI. Calls `paginate_links()` server-side.

| Attribute | Required | Notes |
|---|---|---|
| `uniqueId` | yes | |
| `tagName` | yes | One of: `div`, `section`, `nav` |
| `midSize` | no | Number of page links shown around the current page (default `3`) |

---

## 2. The `query` args object

The `query.query` JSON object maps directly to `WP_Query` arguments. Common
keys:

```json
"query": {
    "post_type": "post",
    "posts_per_page": 6,
    "order": "DESC",
    "orderby": "date",
    "paged": 1,
    "ignore_sticky_posts": true,
    "post_status": "publish",
    "tax_query": [
        {
            "taxonomy": "category",
            "field": "slug",
            "terms": ["wordpress","seo"]
        }
    ],
    "meta_query": [
        {
            "key": "_featured",
            "value": "1",
            "compare": "="
        }
    ]
}
```

For related posts (current post excluded):
```json
"query": {
    "post_type": "post",
    "posts_per_page": 3,
    "post__not_in": ["{{currentPostId}}"]
}
```

To inherit the archive query (use inside category/tag/archive templates):
```json
"inheritQuery": true,
"query": {}
```

---

## 3. Dynamic content inside loop-item

Inside a `loop-item`, child blocks resolve dynamic tokens from the current
post. The two common ways:

### 3.1 `generateblocks/text` with a dynamic source

`generateblocks/text` supports binding its content to a post field via
`htmlAttributes` and the GB dynamic-source registration. The simplest pattern
is to set the content via dynamic tags:

```html
<!-- wp:generateblocks/text {"uniqueId":"loop-title","tagName":"h3","content":"{{post_title}}","styles":{...},"css":"..."} -->
<h3 class="gb-text gb-text-loop-title">{{post_title}}</h3>
<!-- /wp:generateblocks/text -->
```

Available tags inside loop context:
- `{{post_title}}`, `{{post_excerpt}}`, `{{post_date}}`, `{{post_author}}`
- `{{post_url}}` (permalink)
- `{{post_meta key="..."}}` (custom field)
- `{{featured_image_url size="..."}}`
- `{{post_terms taxonomy="..."}}`

(GB Pro adds ACF, term meta, user meta, archive title, and more — see
`gb-pro.md`.)

### 3.2 `generateblocks/media` with dynamic source

For featured images inside a loop:

```html
<!-- wp:generateblocks/media {"uniqueId":"loop-img","tagName":"img","mediaId":"{{featured_image_id}}","htmlAttributes":{"src":"{{featured_image_url size=\"large\"}}","alt":"{{post_title}}"},"styles":{...},"css":"..."} -->
<img class="gb-media gb-media-loop-img" src="{{featured_image_url size=&quot;large&quot;}}" alt="{{post_title}}"/>
<!-- /wp:generateblocks/media -->
```

For featured images, **`generateblocks/media` is the right block** (not
`core/image`) because it can resolve the dynamic source from loop context.
This is the one case where you do NOT use `core/image`.

### 3.3 Element `<a>` linking to the current post

The card itself usually links to the post. Use `generateblocks/element` with
`tagName:"a"` and a dynamic href:

```html
<!-- wp:generateblocks/element {"uniqueId":"loop-link","tagName":"a","htmlAttributes":{"href":"{{post_url}}"},"styles":{...},"css":"..."} -->
<a class="gb-element-loop-link gb-element" href="{{post_url}}">
    <!-- nested text/media/shape children -->
</a>
<!-- /wp:generateblocks/element -->
```

---

## 4. Complete worked example: 3-column blog grid

The canonical, fully-validated, copy-pasteable example lives at:

**[`examples/layouts/query-blog-grid.html`](../examples/layouts/query-blog-grid.html)**

Six recent posts in a responsive grid, with dynamic featured images, post
terms, title, excerpt, "Read more" link, an empty state, and pagination.
Every recovery rule from `recovery-rules.md` is enforced.

Key things to copy from that file:
- Outer `query` block runs the WP_Query and provides the layout wrapper
- `looper` is a `<div>` with the responsive grid styles
- `loop-item` is `<article>` — WP post classes auto-injected by the plugin
- The card image link uses element `<a>` wrapping `generateblocks/media`
  (dynamic, so we use `media` not `core/image`)
- The "Read more" link uses element `<a>` wrapping a text `span` child —
  the canonical link pattern (see `recovery-rules.md` §4)
- Pagination uses `query-page-numbers` with empty `<nav>` body — the plugin
  fills it server-side
- Every JSON attribute is in canonical block.json declaration order
- Every `--` is `\u002d\u002d`, every `&` (in dynamic-tag attributes) is
  `\u0022` for quotes, etc.
- `className` omits the auto-injected id-class (Option A from
  `recovery-rules.md` §3.3)

Read the example file directly when you need to build a query loop. Do not
recreate it from memory — copy the structure.

---

## 5. Common patterns

### 5.1 Related posts (current post excluded)

```json
"queryType":"WP_Query",
"query":{
    "post_type":"post",
    "posts_per_page":3,
    "post__not_in":["{{currentPostId}}"],
    "orderby":"rand"
}
```

### 5.2 Inherit archive query

Used inside `archive.html`, `category.html`, `tag.html`, `home.html` templates:

```json
"inheritQuery":true,
"query":{}
```

### 5.3 Custom post type

```json
"query":{"post_type":"product","posts_per_page":12}
```

### 5.4 Featured posts only

```json
"query":{
    "post_type":"post",
    "posts_per_page":4,
    "meta_query":[{"key":"_featured","value":"1","compare":"="}]
}
```

### 5.5 Children of current page

```json
"query":{
    "post_type":"page",
    "post_parent":"{{currentPostId}}",
    "posts_per_page":-1,
    "orderby":"menu_order",
    "order":"ASC"
}
```

---

## 6. Recovery-error rules specific to query blocks

These are in addition to the global rules in `recovery-rules.md`.

1. **`looper` may only contain `loop-item`.** If you put any other block type
   directly inside a `looper`, the editor rejects it.

2. **`loop-item` must be inside a `looper`.** It cannot be used standalone
   anywhere else. Same for `query-no-results` and `query-page-numbers` — both
   require an ancestor `query` block.

3. **Only one `loop-item` per `looper`.** It is a template, not a list. Don't
   try to add multiple loop-items to vary the layout — use conditional dynamic
   tags inside a single loop-item instead.

4. **`query.query` is required.** Even when `inheritQuery: true`, emit at
   least an empty `"query":{}` object — missing the key has caused recovery
   in older builds.

5. **`paginationType` must be set on the `query` block.** Default to
   `"standard"`. `"instant"` requires GB Pro and the looper.js asset.

6. **Don't put `query-page-numbers` inside `looper`.** It's a sibling of
   `looper`, not a child. Pagination is per-query, not per-iteration.

7. **Double-quote escaping inside dynamic tags.** When a dynamic tag has a
   parameter with quoted value (like `{{featured_image_url size="large"}}`)
   AND that tag lives inside an HTML attribute, escape the inner quotes as
   `&quot;` in the rendered HTML body. The JSON `htmlAttributes` value uses
   `\"` escaping. See the example above for the exact form.

8. **Dynamic image src must use `generateblocks/media`, not `core/image`.**
   `core/image` cannot resolve loop context. The `recovery-rules.md` rule
   that says "use `core/image` for static images with captions" does NOT
   apply inside loop-items — there, `generateblocks/media` is correct.
