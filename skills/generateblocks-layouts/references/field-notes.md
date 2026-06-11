---
title: Field Notes — real conversion fixes & doc contradictions
description: Hard-won lessons from converting an existing HTML/CSS design to GenerateBlocks V2 by hand. Covers the safe escaping workflow, the documentation contradictions (and which source wins), and the pre-delivery validation pass. Read this when hand-authoring blocks or migrating a designed section.
---

# Field Notes

Practical rectifications from a real hand-conversion (a multi-card hero:
27 blocks — element/text/shape — styled with theme CSS variables). These
supplement `recovery-rules.md`; they don't replace it. Where a rule here
conflicts with `SKILL.md` or `block-types.md`, **this file and
`recovery-rules.md` win** — they reflect what actually round-trips.

---

## 1. The escaping workflow that actually works

Don't try to hand-type `\u002d\u002d` while authoring — you'll miss some and
mangle others. Author with **literal** `--`, `<`, `>`, `&`, then run a
post-processor that applies the substitutions to **the JSON inside block
delimiters only**, leaving the rendered HTML body untouched. (Five
substitutions exist — see `recovery-rules.md` §1; the script below covers the
four character ones. The fifth, `\"` → `\u0022`, only matters when a JSON
string value contains a double quote, e.g. inline HTML in `content` — add it
as the LAST replace step if needed.)

This mirrors what WordPress does: `serialize_block_attributes()` applies the
substitutions to the whole serialized JSON string, but the HTML body
between the delimiters is plain HTML and keeps literal characters.

```python
import re, json
lines = open('section.html').read().split('\n')
out = []
delim = re.compile(r'^(<!-- wp:[^\s]+ )(\{.*\})( -->)$')   # opening delimiters only
for ln in lines:
    m = delim.match(ln)
    if m:
        pre, j, post = m.groups()
        json.loads(j)                                       # validate structure BEFORE escaping
        j = (j.replace('--', '\\u002d\\u002d')              # order matters: -- first
               .replace('<', '\\u003c')
               .replace('>', '\\u003e')
               .replace('&', '\\u0026'))
        out.append(pre + j + post)
    else:
        out.append(ln)                                      # HTML body: leave literal
open('section.html','w').write('\n'.join(out))
```

Consequences you must keep straight — the same value appears in **two** forms:

| Where | `--color-primary` | `<em>` | `?a=1&b=2` |
|---|---|---|---|
| JSON (inside `<!-- wp: … -->`) | `var(--color-primary)` | `<em>` | `?a=1&b=2` |
| Rendered HTML body | `var(--color-primary)` | `<em>` | `?a=1&amp;b=2` |

So an inline `style="color:var(--color-primary)"` on an `<em>` in a text
block's **content attribute** is escaped (`var(--…)`), but the **same
markup in the rendered `<h1>` body** stays literal.

### 1.1 The no-op trap

`"-- → --"` is a no-op. In a `<<'PY'` heredoc the replacement target must be a
**raw/literal backslash sequence**:

```python
.replace('--', r'--')          # ❌ WRONG — replaces -- with -- (does nothing)
.replace('--', '\\u002d\\u002d')  # ✅ RIGHT — emits literal --
```

After running, grep to prove it: `grep '^<!-- wp:' f.html | grep -o '{.*}' | grep -c -- '--'` must be `0`.

---

## 2. Documentation contradictions — which source wins

These bit during the conversion. The resolutions below are what round-trips.

### 2.1 `htmlAttributes`: plain object, never array

Older docs showed the **array** form
`[{"attribute":"href","value":"…"}]`. That form triggers recovery and has
been purged from this repo (June 2026 rebuild). `recovery-rules.md` §3.1 is
authoritative:

```json
"htmlAttributes":{"href":"https://example.com/","rel":"noopener"}   // ✅
"htmlAttributes":[{"attribute":"href","value":"…"}]                  // ❌ recovery
```

Applies to every block, including `media` (use `{"src":"…","alt":"…"}`).

### 2.2 `className`: omit the id-class (Option A)

(An old `SKILL.md` rule said to include the uniqueId class in `className` —
fixed in the June 2026 rebuild.) **Follow Option A — omit the id-class**; the
plugin auto-injects `gb-{type}-{id}` when `styles` is non-empty:

```json
"className":"gb-element"            // element  → rendered: class="gb-element-{id} gb-element"
"className":"gb-element alignfull"  // full-width section
// text / shape / media: omit className entirely
```

### 2.3 Rendered base-class ORDER differs by block type

Not a contradiction to fight — just match the editor. What's really going on
(verified against live-site exports in `examples/from-gauravtiwari-org/`):
the order depends on whether the JSON carries `className`.

- **`className` present** (the editor adds `"className":"gb-element"` for
  element blocks): rendered = id-class **prepended** →
  `gb-element-{id} gb-element`.
- **`className` absent** (the editor's default for text/media/shape): the
  save function emits the base class itself and the id-class is **appended**
  → `gb-text custom-classes gb-text-{id}`.

Editor-default conventions per block:

| Block | JSON | Rendered class list |
|---|---|---|
| `element` | `"className":"gb-element"` | `gb-element-{id} gb-element` (id first) |
| `text` | no className | `gb-text gb-text-{id}` (base first, id appended) |
| `media` | no className | `gb-media gb-media-{id}` (base first) |
| `shape` | no className | `gb-shape gb-shape-{id}` (base first) |

Both authoring paths round-trip (recovery-rules §3.3). Pick ONE convention
per block and make JSON + body agree. When `styles` is empty, no id-class is
injected at all — production text blocks without styles render plain
`class="gb-text"`.

### 2.4 `styles` longhand vs `css` shorthand for padding/margin

The editor stores shorthand `padding` as **four longhand keys** in the
`styles` object, but the `css` string is taken as-authored (shorthand is fine).
Match the known-good pattern:

```json
"styles":{"paddingTop":"3rem","paddingBottom":"3rem","paddingLeft":"1rem","paddingRight":"1rem"},
"css":".gb-element-x{padding:3rem 1rem}"
```

### 2.5 `shape`: the `html` attribute is optional

`recovery-rules.md` §3.4 lists `html` 2nd for shape, but the working pattern
omits it and puts the SVG in the rendered `<span>` body, with sizing via
`styles.svg`. Keep SVG attribute order per SKILL.md rule #23
(`stroke-linejoin, stroke-linecap, stroke-width, stroke, fill, viewBox, height, width`).

---

## 3. Robustness decisions when converting a real design

- **External image in a custom layout → CSS `background-image` on an element
  block**, not `generateblocks/media`. It sidesteps the media-block
  `htmlAttributes` shape ambiguity (§2.1) and the missing attachment `mediaId`
  for CDN-hosted images. Add `role="img"` + `aria-label` for accessibility.
  Use `core/image` only when you need a real caption.
- **Style with the theme's CSS variables** (`var(--color-bg)`,
  `var(--color-headline)`, `var(--radius-l)`, `var(--font-head)`) + `color-mix`
  for surfaces. The section then **inherits the theme and flips with the
  theme's dark mode for free** — no `[data-theme="dark"]` overrides, which can't
  go in the `css` string cleanly anyway.
- **Drop decorative pseudo/SVG background layers** (animated grids, gradient
  orbs, circuit SVGs). They need multi-property `::before/::after` rules whose
  alphabetical-sort + vendor-prefix ordering (`-webkit-mask-image` vs
  `mask-image`) is fragile in the `css` string. Recreate them as a GB Pro
  "Effects" layer or a small global CSS snippet, not inline block CSS. The
  structural look (card system, mono labels, badges, accent color) survives
  without them.
- **Empty decorative elements** (a badge dot) are fine as an element block with
  `styles` and an empty body: `<span class="gb-element-x gb-element"></span>` —
  keep it on one line (compact nesting, §3.6).

---

## 4. Pre-delivery validation pass

Recovery is an **editor-side** re-serialization check — you cannot fully
reproduce it headlessly. But you can catch the structural/escaping failures
that cause most of it. Run this before handing off the file:

```python
import re, json
txt = open('section.html').read()
opens  = re.findall(r'<!-- wp:(generateblocks/\w+) (\{.*?\}) -->', txt)
closes = re.findall(r'<!-- /wp:(generateblocks/\w+) -->', txt)
assert len(opens) == len(closes), (len(opens), len(closes))      # balanced delimiters

for typ, j in opens:                                              # JSON valid after un-escaping
    raw = (j.replace('\\u002d\\u002d','--').replace('\\u003c','<')
             .replace('\\u003e','>').replace('\\u0026','&'))
    d = json.loads(raw)
    assert list(d)[0] == 'uniqueId'                              # canonical key order starts right
    if 'className' in d:                                          # className is last among declared
        assert list(d).index('className') > list(d).index('css')

stack = []                                                        # nesting balance
for m in re.finditer(r'<!-- (/?)wp:(generateblocks/\w+)', txt):
    if m.group(1) == '': stack.append(m.group(2))
    else: assert stack.pop() == m.group(2)
assert not stack
print("structurally valid:", len(opens), "blocks")
```

Then state the limit honestly: structure + escaping are verified; the only
real recovery proof is pasting into the editor. Offer to push it to a **draft**
page and hand back the edit URL so the user can confirm it loads clean.

---

## 5. Quick checklist (this file's deltas, on top of recovery-rules.md §7)

- [ ] Authored with literal chars, then escaped JSON-only via the §1 script
- [ ] `grep` proves zero literal `--` inside delimiter JSON
- [ ] `htmlAttributes` are objects (§2.1)
- [ ] `className` omits the id-class for element; omitted entirely for text/shape/media (§2.2)
- [ ] Rendered class order: element id-first, others base-first (§2.3)
- [ ] External images via `background-image`, not media block (§3)
- [ ] Decorative grid/orb/SVG layers dropped or moved to global CSS (§3)
- [ ] Ran the §4 validation script; reported the headless-verification limit
