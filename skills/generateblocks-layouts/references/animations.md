---
title: Animations & motion
description: Hover transitions, keyframe animations, entrance effects, staggered reveals, scroll-driven animation, and reduced-motion — all within GenerateBlocks recovery constraints.
---

# Animations & Motion

GenerateBlocks has no animation UI — motion is authored in the `styles`
object and the `css` string. Everything here works on the free plugin.

## 1. Where motion lives (the rules)

| Mechanism | Where | Notes |
|---|---|---|
| Hover/focus state changes | `styles` object, `&:hover` / `&:focus` keys | Source-verified nested-selector keys |
| Transitions | base styles (`transition` property) | Pair every `&:hover` with a base transition |
| `@keyframes` + `animation` | `css` string | One of the allowed `css` exceptions |
| Parent-hover → child motion | child's `css`: `.gb-element-card1:hover .gb-shape-icon1{...}` | Allowed exception (both classes generator-stable) |
| Pseudo-element motion | `css`: `.gb-element-x::after{...}` + `.gb-element-x:hover::after{...}` | Allowed exception |
| Scroll-driven (modern CSS) | `css` string (`animation-timeline`) | Progressive enhancement |
| Entrance-on-scroll (JS) | none in GB free/Pro | See §6 for options |

The `styles` object also accepts `@media`, `@supports`, and `@container`
at-rule keys (verified in `styles-builder.js` — only those three; write
`@media (` with a space, the editor normalizes to that form).

## 2. Hover + transition (the bread-and-butter pattern)

Declare base + hover in `styles` using `&:hover`; keep the transition in the
base so it animates both directions:

```json
"styles":{
    "backgroundColor":"#0a0a0a",
    "color":"#ffffff",
    "transform":"translateY(0)",
    "transition":"transform 0.25s ease, box-shadow 0.25s ease",
    "&:hover":{
        "transform":"translateY(-6px)",
        "boxShadow":"0 20px 60px rgba(0,0,0,0.15)"
    }
}
```

With matching `css` (base props sorted; hover rule appended):

```json
"css":".gb-element-card001{background-color:#0a0a0a;color:#ffffff;transform:translateY(0);transition:transform 0.25s ease, box-shadow 0.25s ease}.gb-element-card001:hover{box-shadow:0 20px 60px rgba(0,0,0,0.15);transform:translateY(-6px)}"
```

Only animate **transform, opacity, box-shadow, color/background-color,
border-color, filter** — cheap properties. Never animate layout properties
(width/height/margin/top) on cards and grids.

## 3. Keyframe entrance animations

`@keyframes` go in the `css` string (allowed exception). Self-contained
fade-up on a hero:

```json
"css":".gb-element-hero001{animation:gbFadeUp 0.8s ease 0.1s both}@keyframes gbFadeUp{from{opacity:0;transform:translateY(24px)}to{opacity:1;transform:translateY(0)}}"
```

- Name keyframes uniquely per section (`gbFadeUp`, `heroReveal`) — multiple
  blocks may each carry their own `@keyframes`; identical names with
  different bodies will fight.
- `both` fill-mode prevents a flash of the pre-animation state.
- These run **on page load**, not on scroll-into-view (see §6).

### Staggered reveal (cards in a grid)

Same keyframes, increasing `animation-delay` per card — each card's own `css`:

```json
"css":".gb-element-feat001{animation:gbFadeUp 0.6s ease 0s both}@keyframes gbFadeUp{from{opacity:0;transform:translateY(24px)}to{opacity:1;transform:translateY(0)}}"
"css":".gb-element-feat002{animation:gbFadeUp 0.6s ease 0.12s both}@keyframes gbFadeUp{...}"
"css":".gb-element-feat003{animation:gbFadeUp 0.6s ease 0.24s both}@keyframes gbFadeUp{...}"
```

Inside a query loop you can't vary per-item CSS (one template) — use
`animation-delay:calc(...)` with `sibling-index()` only if you accept limited
support, or skip staggering in loops.

## 4. Micro-interactions catalog

```css
/* Icon nudge on card hover — in the icon (child) block's css */
.gb-element-card001:hover .gb-shape-icon001{transform:translateX(4px)}
/* base on the icon */
.gb-shape-icon001{transition:transform 0.2s ease}

/* Animated underline — pseudo-element */
.gb-text-link001{position:relative}.gb-text-link001::after{background:#c0392b;bottom:-2px;content:'';height:2px;left:0;position:absolute;transform:scaleX(0);transform-origin:left;transition:transform 0.25s ease;width:100%}.gb-text-link001:hover::after{transform:scaleX(1)}

/* Image zoom-in-frame — parent has overflow:hidden, child img scales */
.gb-element-card001:hover .gb-media-img001{transform:scale(1.05)}
/* base on the media block */ .gb-media-img001{transition:transform 0.4s ease}

/* Gradient shift on button */
.gb-element-btn001{background:linear-gradient(135deg,#c0392b,#8e2418);background-size:150% 150%;transition:background-position 0.3s ease}.gb-element-btn001:hover{background-position:100% 100%}

/* Pulse badge */
.gb-text-badge001{animation:gbPulse 2s ease infinite}@keyframes gbPulse{0%,100%{opacity:1}50%{opacity:0.55}}
```

(When pasting these into a `css` attribute: single line, single quotes,
properties sorted within each rule, and every `--` written as `\u002d\u002d`
if custom properties appear.)

## 5. Scroll-driven animations (CSS-only, no JS)

`animation-timeline: view()` runs an animation as the element enters the
viewport — Chromium-shipped, progressive enhancement elsewhere. Gate it with
`@supports` so non-supporting browsers just show the element:

```json
"css":"@supports (animation-timeline:view()){.gb-element-sec001{animation:gbFadeUp linear both;animation-range:entry 0% entry 60%;animation-timeline:view()}}@keyframes gbFadeUp{from{opacity:0;transform:translateY(40px)}to{opacity:1;transform:translateY(0)}}"
```

This is the recommended "animate on scroll" answer for GB — no plugin, no JS,
safe fallback.

## 6. Entrance-on-scroll with JS — what to tell the user

GB ships no scroll-trigger JS for regular blocks. (The one Pro exception:
**Overlay Panels** have built-in fade/slide/scale entrance animations and
scroll-percentage/exit-intent/time triggers — for popups and slide-ins, use
those instead of custom code; see `pro-interactive.md` §5.) For in-page
sections, options in order:

1. **CSS scroll-driven animations** (§5) — default recommendation.
2. A tiny IntersectionObserver snippet enqueued by the theme (GP hook or
   child theme) that toggles an `is-visible` class; write the matching
   `.gb-element-x.is-visible{...}` rules in each block's `css`.
3. Third-party animation plugins — last resort; they add global JS.

## 7. Accessibility — always

Wrap attention-grabbing or large-movement animation in a reduced-motion
guard, in the same `css` string:

```json
"css":".gb-element-hero001{animation:gbFadeUp 0.8s ease both}@keyframes gbFadeUp{from{opacity:0;transform:translateY(24px)}to{opacity:1;transform:translateY(0)}}@media (prefers-reduced-motion:reduce){.gb-element-hero001{animation:none}}"
```

Rules of thumb: subtle hovers (≤4px translate, ≤1.05 scale) don't need the
guard; entrance animations, pulses, parallax, and anything auto-playing do.
Never animate `outline` away — keyboard focus must stay visible
(see `css-patterns.md` focus section).

## 8. What NOT to do

- No `transition`/`:hover` generated from thin air in markup the editor will
  re-derive — follow `recovery-rules.md` §2 for what belongs in `styles` vs `css`.
- No JavaScript in block markup — `<script>` is stripped; behavior belongs in
  the theme or a plugin.
- No infinite movement loops on content users read (pulse a dot, not a card).
- No `\xxxx` CSS escapes in keyframe content strings (recovery rule §2.5).
- Don't animate inside `looper` templates expecting per-item stagger — every
  item shares one template and one uniqueId class.
