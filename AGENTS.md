# AGENTS.md

Universal instructions for all LLM assistants working in this repo.
**The deep knowledge lives in `skills/generateblocks-layouts/`** — read its
`SKILL.md`, then `references/_index.md` (router), then
`references/recovery-rules.md` before emitting any GenerateBlocks markup.
This file is only the orientation summary.

Plugin source in this repo: GenerateBlocks free **2.3** (`generateblocks/`),
GB Pro **2.6** (`generateblocks-pro/`). All skill claims are verified against
this source. Canonical online docs for v2: learn.generatepress.com
(docs.generateblocks.com covers v1 only).

## V2 block names (never use V1 names)

| Need | Correct block | NOT these |
|---|---|---|
| Containers | `generateblocks/element` | ❌ `/container`, `/grid` |
| Text / buttons | `generateblocks/text` | ❌ `/headline`, `/button` |
| Images | `generateblocks/media` | ❌ `/image` |
| SVG icons | `generateblocks/shape` | — |
| Dynamic lists | `generateblocks/query` + `looper` + `loop-item` | ❌ `/query-loop` (legacy) |

Class pattern everywhere: `gb-{type}-{uniqueId} gb-{type}` (id-class
auto-injected first; `className` holds only the base class — Option A).

## The five serialization rules that break everything

1. **Five JSON substitutions** in block-comment strings:
   `--`→`\u002d\u002d`, `<`→`\u003c`, `>`→`\u003e`, `&`→`\u0026`, `\"`→`\u0022`.
2. **Attribute key order = block.json declaration order**, `className` last.
   Text block: `content` is 3rd. (Per-block orders:
   `references/recovery-rules.md` §3.4 and `references/pro-interactive.md`.)
3. **`htmlAttributes` is a plain object** — `{"href":"https://..."}` — never
   an array. Absolute URLs only.
4. **`css` string**: one line, minified, properties alphabetized, no
   descendant selectors (two exceptions: own-selector pseudo-elements,
   parent-hover targeting another GB block's class), no hover/transition
   (those go in `styles` via `&:hover` keys).
5. **Links**: element `<a>` wrapping a text `span` child. Text `<a>` strips
   href on save; element `<a>` with raw text triggers recovery.

## Dynamic tags — exact syntax (silently fails if wrong)

```
{{tag_name option:value|option2:value}}     ← space after tag name, pipes, NO quotes
```

Real tags: `{{post_title}}`, `{{post_permalink}}`, `{{post_excerpt length:20}}`,
`{{featured_image size:large}}`, `{{post_meta key:field_name}}`,
`{{term_list tax:category}}`, `{{post_date dateFormat:M j, Y}}`.

These do NOT exist: `{{post_url}}`, `{{featured_image_url}}`, `{{post_terms}}`,
`{{acf}}`, any `key="quoted"` form. Full catalog:
`skills/generateblocks-layouts/references/dynamic-tags.md`.

ACF: `{{post_meta key:acf_field}}`, nested via dot notation
(`key:repeater.0.subfield`); repeater loops via Pro `queryType:"post_meta"`.

## Other hard rules

- **No HTML comments** except `<!-- wp:... -->` delimiters.
- **Compact nesting** — closing comment adjacent to closing tag.
- **No spaces inside CSS function args**: `clamp(3rem,8vw,5rem)`.
- **Unique IDs**: `{section}{number}{letter?}` — `hero001`, `card012b`.
  Never reuse a uniqueId across blocks or posts (styles are coupled to it).
- **Output to files**, never inline in chat.
- Static captioned images → `core/image`; loop images →
  `generateblocks/media` with `{{featured_image size:large}}` src.
- Lists → `core/list` (`className:"list"`); emoji → `core/paragraph`.
- Responsive: `@media (max-width:1024px)` / `(max-width:768px)` keys in
  `styles`, mirrored in `css`.

## Skills in this repo

| Skill | Purpose |
|---|---|
| `skills/generateblocks-layouts/` | Build any GB layout — the knowledge base |
| `skills/html-to-generateblocks/` | Convert HTML/CSS to GB markup |
| `skills/elementor-to-generateblocks/` | Migrate Elementor layouts |
| `skills/figma-to-generateblocks/` | Convert Figma designs |

Converters delegate all markup rules to `generateblocks-layouts/references/`
— never duplicate or contradict them.

## Reference routing (inside generateblocks-layouts/references/)

- Recovery errors → `recovery-rules.md` (read EVERY task)
- Block specs → `block-types.md` · Dynamic data → `dynamic-tags.md`
- Query loops → `query-block.md` · ACF → `acf-and-custom-fields.md`
- Animations → `animations.md` · Conditions → `conditions.md`
- Forms → `pro-forms.md` · Accordion/tabs/nav → `pro-interactive.md`
- Full-site templates → `template-authoring.md` · Pro map → `gb-pro.md`
