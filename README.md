# overlapui.fscss

> CSS-first overlapping UI stacks for the FSCSS ecosystem.  
> Avatars, cards, and images **overlap at rest** and **spread on hover/focus** — no JS required for the interaction.

[github.com/fscss-ttr/overlapui.fscss](https://github.com/fscss-ttr/overlapui.fscss)

Requires **FSCSS v1.2.3+**. **Recommended: FSCSS 1.2.5 or later** (stable shorthands + array attribute maps).

<meta name="description" content="overlapui.fscss — overlapping avatar, card, and image stacks for FSCSS with --ou-* design tokens." />
<meta name="keywords" content="overlapui, overlapui.fscss, FSCSS, CSS avatar stack, card overlap, image stack, pure CSS UI" />


---

## Table of Contents

1. [What is overlapui?](#1-what-is-overlapui)
2. [Installation](#2-installation)
3. [Design tokens — `@ou-root`](#3-design-tokens--ou-root)
4. [Mixins](#4-mixins)
5. [Markup patterns](#5-markup-patterns)
6. [Full examples](#6-full-examples)
7. [Token reference](#7-token-reference)
8. [Accessibility & motion](#8-accessibility--motion)
9. [License](#9-license)

---

## 1. What is overlapui?

**overlapui.fscss** is an FSCSS module for stacked UI:

| Component | Behavior |
|-----------|----------|
| **Avatar overlap** | Circles pull together; hover/focus spreads them; `data-alph` sets letter + color |
| **Card overlap** | Cards peek under each other; expand vertically on hover/focus |
| **Image overlap** | Tilted photos overlap; straighten and gap on hover/focus |

Everything is driven by **`--ou-*`** custom properties. Compile with the CLI or run with the browser runtime — output is plain CSS.

---

## 2. Installation

### CLI for productions / CDN for prototyping 

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
```

```css
@import((*) from overlapui)

@overlapui()
```

Local path from this repo:

```css
@import((*) from "./overlapui.fscss")
```

### Selective import

```css
@import((ou-root, avatar-overlap, ou-reduced-motion) from overlapui)

@ou-root()
@avatar-overlap(.avatar-overlap)
@ou-reduced-motion()
```

### CLI

```bash
npm install -g fscss@1.2.5
fscss app.fscss app.css
```

**Version note:** Array-driven `data-alph` color maps work from **1.2.3+**. Prefer **1.2.5+** for property shorthands elsewhere in your app and current stable tooling.

---

## 3. Design tokens — `@ou-root`

```css
@ou-root()              /* :root */
@ou-root(.theme-dark)   /* scoped */
```

Helpers:

```css
:root {
  @ou-avatar-size(48px, -16px)
  @ou-card-size(40px, 80px)
  @ou-img-size(112px, -60px)
}
```

Override any token after `@ou-root()`:

```css
:root {
  --ou-ring: #0f172a;
  --ou-time: 0.4s;
  --ou-size: 40px;
}
```

---

## 4. Mixins

| Define | Purpose |
|--------|---------|
| `@ou-root(root)` | Inject `--ou-*` tokens |
| `@ou-avatar-size(size, overlap)` | Avatar diameter + pull-in |
| `@ou-card-size(peek, height)` | Card peek / min-height |
| `@ou-img-size(size, overlap)` | Image size + overlap |
| `@ou-avatar-colors()` | Internal A–Z / 0–9 color arrays (used by avatar mixin) |
| `@avatar-overlap(sel)` | Avatar stack + letter colors |
| `@card-overlap(sel)` | Vertical card stack |
| `@img-overlap(sel)` | Horizontal image stack |
| `@ou-reduced-motion()` | Respect `prefers-reduced-motion` |
| `@overlapui(avatar, card, img)` | Install all with default class names |

```css
@overlapui()
/* equals: ou-root + avatar + card + img + reduced-motion */
```

Custom selectors:

```css
@avatar-overlap(.team-faces)
@card-overlap(.notify-stack)
@img-overlap(.shot-row)
```

---

## 5. Markup patterns

### Avatars

Letter from `data-alph`. Optional `<img>` covers the letter (letter remains fallback). Visible name can stay in the DOM for screen readers (`font-size: 0` on the face).

```html
<ul class="avatar-overlap" tabindex="0">
  <li data-alph="g">Gina Park</li>
  <li data-alph="h"><img src="https://i.pravatar.cc/80?img=2" alt="">Hank Lee</li>
  <li data-alph="5">5001</li>
</ul>
```

### Cards

Direct child `div`s: image + `h3` + text.

```html
<div class="card-overlap" tabindex="0">
  <div>
    <img src="https://picsum.photos/96?random=1" alt="">
    <h3>Deploy succeeded</h3>
    production · 2m ago
  </div>
  <div>
    <img src="https://picsum.photos/96?random=2" alt="">
    <h3>New comment</h3>
    on overlapui README
  </div>
</div>
```

### Images

```html
<div class="img-overlap" tabindex="0">
  <img src="https://picsum.photos/96?random=4" alt="">
  <img src="https://picsum.photos/96?random=5" alt="">
  <img src="https://picsum.photos/96?random=6" alt="">
</div>
```

Use **`tabindex="0"`** on the stack so keyboard focus can trigger `:focus-within` spread.

---

## 6. Full examples

### Minimal avatars

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from overlapui)
  @overlapui()

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0b1220;
  }
</style>

<ul class="avatar-overlap" tabindex="0">
  <li data-alph="g">G</li>
  <li data-alph="h">H</li>
  <li data-alph="i">I</li>
  <li data-alph="j">J</li>
</ul>
```

### Dashboard members table

Member column uses **avatar-overlap**; other columns stay normal table layout.

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from overlapui)

  @ou-root()
  @avatar-overlap(.avatar-overlap)
  @ou-reduced-motion()

  :root {
    --ou-size: 36px;
    --ou-overlap: -12px;
    --ou-gap: 8px;
    --ou-ring: #0f172a;
  }

  body {
    margin: 0;
    min-height: 100vh;
    padding: 32px 16px;
    background: #0b1220;
    color: #e2e8f0;
    font-family: system-ui, sans-serif;
  }

  .panel {
    max-width: 720px;
    margin: 0 auto;
    background: #121a2e;
    border: 1px solid #1e293b;
    border-radius: 16px;
    overflow: hidden;
  }

  .panel header {
    padding: 16px 20px;
    border-bottom: 1px solid #1e293b;
    font-weight: 700;
    letter-spacing: 0.02em;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
  }

  th, td {
    padding: 14px 20px;
    text-align: left;
    border-bottom: 1px solid #1e293b;
  }

  th {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: #94a3b8;
  }

  td.role { color: #94a3b8; }
  td.amount {
    font-weight: 600;
    font-variant-numeric: tabular-nums;
  }
  td.amount.pos { color: #4ade80; }
  td.amount.neg { color: #f87171; }

  .member {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .member-meta {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .member-meta strong { font-size: 14px; color: #f8fafc; }
  .member-meta span { font-size: 12px; color: #64748b; }
</style>

<div class="panel">
  <header>Team · active members</header>
  <table>
    <thead>
      <tr>
        <th>Members</th>
        <th>Role</th>
        <th>Plan</th>
        <th>Balance</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>
          <div class="member">
            <ul class="avatar-overlap" tabindex="0">
              <li data-alph="a"><img src="https://i.pravatar.cc/72?img=11" alt="">Ada</li>
              <li data-alph="n">Nia</li>
              <li data-alph="k">Ken</li>
            </ul>
            <div class="member-meta">
              <strong>Design pod</strong>
              <span>3 people</span>
            </div>
          </div>
        </td>
        <td class="role">Product design</td>
        <td>Pro</td>
        <td class="amount pos">+$1,240</td>
      </tr>
      <tr>
        <td>
          <div class="member">
            <ul class="avatar-overlap" tabindex="0">
              <li data-alph="m"><img src="https://i.pravatar.cc/72?img=12" alt="">Mo</li>
              <li data-alph="s">Sam</li>
            </ul>
            <div class="member-meta">
              <strong>Platform</strong>
              <span>2 people</span>
            </div>
          </div>
        </td>
        <td class="role">Engineering</td>
        <td>Team</td>
        <td class="amount pos">+$890</td>
      </tr>
      <tr>
        <td>
          <div class="member">
            <ul class="avatar-overlap" tabindex="0">
              <li data-alph="r">Rae</li>
              <li data-alph="j"><img src="https://i.pravatar.cc/72?img=15" alt="">Jo</li>
              <li data-alph="t">Ty</li>
              <li data-alph="5">Ops</li>
            </ul>
            <div class="member-meta">
              <strong>Growth</strong>
              <span>4 people</span>
            </div>
          </div>
        </td>
        <td class="role">GTM</td>
        <td>Starter</td>
        <td class="amount neg">−$120</td>
      </tr>
    </tbody>
  </table>
</div>
```

### Notification card stack

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((ou-root, card-overlap, ou-reduced-motion) from overlapui)
  @ou-root()
  @card-overlap(.card-overlap)
  @ou-reduced-motion()

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0b1220;
    font-family: system-ui, sans-serif;
  }
</style>

<div class="card-overlap" tabindex="0">
  <div>
    <img src="https://picsum.photos/96?random=1" alt="">
    <h3>Invoice paid</h3>
    Acme Corp · $2,400
  </div>
  <div>
    <img src="https://picsum.photos/96?random=2" alt="">
    <h3>Seat added</h3>
    overlapui workspace
  </div>
  <div>
    <img src="https://picsum.photos/96?random=3" alt="">
    <h3>Comment</h3>
    “Ship the avatar stack”
  </div>
</div>
```

### Image strip

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from overlapui)
  @ou-root()
  @img-overlap(.img-overlap)
  @ou-reduced-motion()

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0b1220;
  }
</style>

<div class="img-overlap" tabindex="0">
  <img src="https://picsum.photos/120?random=4" alt="">
  <img src="https://picsum.photos/120?random=5" alt="">
  <img src="https://picsum.photos/120?random=6" alt="">
</div>
```

---

## 7. Token reference

| Variable | Default (approx.) | Usage |
|----------|-------------------|--------|
| `--ou-ring` | `#17203a` | Avatar/image ring color |
| `--ou-ease` | `cubic-bezier(.3, 1.3, .5, 1)` | Spread easing |
| `--ou-time` | `.35s` | Spread duration |
| `--ou-size` | `44px` | Avatar diameter |
| `--ou-overlap` | `-14px` | Avatar pull-in (`margin-left`) |
| `--ou-gap` | `6px` | Avatar spread gap |
| `--ou-c` | `#f59e0b` | Fallback face color |
| `--ou-letter` | `#fff` | Letter color |
| `--ou-ring-width` | `3px` | Ring thickness |
| `--ou-peek` | `34px` | Visible card peek |
| `--ou-card-h` | `76px` | Card min-height |
| `--ou-card-gap` | `10px` | Card spread gap |
| `--ou-card-bg` | `#1f2a48` | Card background |
| `--ou-card-color` | `#cbd5ee` | Card body text |
| `--ou-card-title` | `#fff` | Card `h3` |
| `--ou-img` | `96px` | Image size |
| `--ou-img-overlap` | `-52px` | Image pull-in |
| `--ou-img-gap` | `12px` | Image spread gap |
| `--ou-img-tilt` / `--ou-img-tilt-even` | `-5deg` / `4deg` | Rest rotation |

---

## 8. Accessibility & motion

- Keep **real names** (or labels) in the DOM for avatars; faces use `font-size: 0` for the decorative letter only.
- Provide meaningful **`alt`** on photos when the image carries meaning; decorative faces can use empty `alt` with the name in text beside the stack.
- **`tabindex="0"`** on the stack enables keyboard `:focus-within` spread.
- `@ou-reduced-motion()` disables transform/margin transitions when the user prefers reduced motion.

---

## 9. License

MIT · Built with [FSCSS](https://fscss.devtem.org) · [fscss-ttr](https://github.com/fscss-ttr)

```bash
npm install -g fscss@1.2.5   # recommended
```

[Issues](https://github.com/fscss-ttr/overlapui.fscss/issues) · [FSCSS docs](https://fscss.devtem.org/docs)

