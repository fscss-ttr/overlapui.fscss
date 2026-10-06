# overlapui.fscss

> CSS-first overlapping UI stacks for the FSCSS ecosystem —  
> **plus plain `overlapui.css` for pages that never touch FSCSS.**

Avatars, cards, images, and **radial circle menus** that overlap at rest and open on hover/focus. No JavaScript for the interaction.

**MIT** · **v1.0.0** · [github.com/fscss-ttr/overlapui.fscss](https://github.com/fscss-ttr/overlapui.fscss)

| Channel | Use |
|--------|-----|
| **npm** | `npm install overlapui` |
| **Plain CSS** | Link `overlapui.css` (no FSCSS) |
| **FSCSS module** | `@import((…) from overlapui)` — selective, token-driven, smaller output |

Requires **FSCSS v1.2.3+** only when you compile or run `.fscss` source. **Recommended: 1.2.5+**.  
CSS-only consumers need **no** FSCSS install.

---

## Table of Contents

1. [What is overlapui?](#1-what-is-overlapui)
2. [Choose a delivery mode](#2-choose-a-delivery-mode)
3. [Plain CSS (standalone)](#3-plain-css-standalone)
4. [FSCSS module (customizable)](#4-fscss-module-customizable)
5. [Default classes (standalone expand)](#5-default-classes-standalone-expand)
6. [Design tokens — `--ou-*`](#6-design-tokens----ou-)
7. [Mixins (FSCSS)](#7-mixins-fscss)
8. [Markup patterns](#8-markup-patterns)
9. [Full examples](#9-full-examples)
10. [Token reference](#10-token-reference)
11. [Accessibility & motion](#11-accessibility--motion)
12. [Repo layout & build](#12-repo-layout--build)
13. [License](#13-license)

---

## 1. What is overlapui?

| Component | Behavior |
|-----------|----------|
| **Avatar overlap** | Faces pull together; hover/focus spreads them; `data-alph` → letter + color |
| **Card overlap** | Cards peek under each other; expand on hover/focus |
| **Image overlap** | Tilted photos; straighten and gap on hover/focus |
| **Circle overlap** | Center control; satellites fan on a ring (`--n` = count) |

Everything is driven by **`--ou-*`** custom properties.

---

## 2. Choose a delivery mode

```text
Need only default class names (.avatar-overlap, …)?
  → Plain CSS: overlapui.css

Need custom selectors, fewer rules, or @define helpers?
  → FSCSS: import overlapui.fscss (selective)
```

| Mode | Install | Output size | Selectors |
|------|---------|-------------|-----------|
| **Standalone CSS** | `<link>` or npm `style` | Full default expand | Fixed classes below |
| **FSCSS selective** | CLI / runtime + `@import` | Only what you call | Any selector you pass |

---

## 3. Plain CSS (standalone)

Compiled from `overlapui.standalone.fscss` (CI keeps `overlapui.css` on `main`).

### npm

```bash
npm install overlapui
```

```html
<link rel="stylesheet" href="node_modules/overlapui/overlapui.css">
```

### jsDelivr CDN

```html
<link rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.css">
```

Minified:

```html
<link rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.min.css">
```

From GitHub `main`:

```html
<link rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/fscss-ttr/overlapui.fscss@main/overlapui.css">
```

### CSS `@import`

```css
@import url("https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.css");
```

### Theme without FSCSS

```css
:root {
  --ou-size: 40px;
  --ou-overlap: -12px;
  --ou-center-bg: #6366f1;
  --ou-ring: #0f172a;
}
```

Markup uses the **default classes** in [§5](#5-default-classes-standalone-expand).

---

## 4. FSCSS module (customizable)

### Runtime prototype

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
```

```css
@import((*) from overlapui)
@overlapui()
```

### npm package as source

```bash
npm install overlapui
```

```css
@import((*) from "overlapui/overlapui.fscss")
/* or, if your resolver maps the package name: */
@import((*) from overlapui)
```

### Selective import (recommended for production FSCSS)

```css
@import((
  ou-root,
  ou-avatar-colors,
  avatar-overlap,
  circle-overlap,
  ou-reduced-motion
) from overlapui)

@ou-root()
@avatar-overlap(.team-faces)
@circle-overlap(.fab-menu)
@ou-reduced-motion()
```

### Remote source (fork / pinned commit)

```css
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/overlapui.fscss@main/overlapui.fscss")
```

### CLI

```bash
npm install -g fscss@1.2.5
fscss app.fscss app.css
```

**Why FSCSS:** custom class names, only the components you call, size helpers (`@ou-avatar-size`, …), same tokens as the CSS build.

---

## 5. Default classes (standalone expand)

These selectors are what **`overlapui.css`** contains after compile. Use them as-is with the plain CSS file, or pass different names via FSCSS mixins.

### Roots & motion

| Class / target | Role |
|----------------|------|
| `:root` (tokens) | `--ou-*` design tokens from `@ou-root()` |
| *(media)* `prefers-reduced-motion` | Transitions off via `@ou-reduced-motion()` |

### Avatar

| Class | Role |
|-------|------|
| `.avatar-overlap` | Stack root (`display: flex`, …) |
| `.avatar-overlap > li` | Face cell |
| `.avatar-overlap > li:first-child` | No pull-in margin |
| `.avatar-overlap > li::before` | Letter from `data-alph` |
| `.avatar-overlap > li > img` | Optional photo cover |
| `.avatar-overlap:hover` / `:focus-within` | Spread siblings |

### Card

| Class | Role |
|-------|------|
| `.card-overlap` | Stack root |
| `.card-overlap > div` | Card panel |
| `.card-overlap > div img` / `h3` | Media + title |
| `.card-overlap:hover` / `:focus-within` | Expand stack |

### Image

| Class | Role |
|-------|------|
| `.img-overlap` | Stack root |
| `.img-overlap > img` | Tilted / overlapping images |
| `.img-overlap:hover` / `:focus-within` | Align + gap |

### Circle

| Class | Role |
|-------|------|
| `.circle-overlap` | Radial root; set `--n` on element |
| `.circle-overlap > :first-child` | Center control |
| `.circle-overlap > :not(:first-child)` | Satellites |
| `.circle-overlap:hover` / `:focus-within` | Fan-out |

FSCSS custom example (not in standalone CSS unless you compile it yourself):

```css
@avatar-overlap(.members)
@circle-overlap(.settings-orbit)
```

→ emits `.members`, `.settings-orbit`, … instead of the defaults.

---

## 6. Design tokens — `--ou-*`

Works the same for **CSS link** and **FSCSS**.

```css
:root {
  --ou-ring: #0f172a;
  --ou-time: 0.4s;
  --ou-size: 40px;
  --ou-center-bg: #6366f1;
}
```

FSCSS helpers:

```css
@ou-root()

:root {
  @ou-avatar-size(48px, -16px)
  @ou-card-size(40px, 80px)
  @ou-img-size(112px, -60px)
  @ou-circle-size(48px, 80px)
}
```

---

## 7. Mixins (FSCSS)

| Define | Purpose |
|--------|---------|
| `@ou-root(root)` | Inject `--ou-*` |
| `@ou-avatar-size(size, overlap)` | Avatar diameter + pull-in |
| `@ou-card-size(peek, height)` | Card peek / min-height |
| `@ou-img-size(size, overlap)` | Image size + overlap |
| `@ou-circle-size(item, radius)` | Circle size + orbit |
| `@ou-avatar-colors()` | Internal A–Z / 0–9 maps |
| `@avatar-overlap(sel)` | Avatar stack |
| `@card-overlap(sel)` | Card stack |
| `@img-overlap(sel)` | Image stack |
| `@circle-overlap(sel)` | Radial menu |
| `@ou-reduced-motion()` | Reduced motion |
| `@overlapui(…)` | Install all defaults |

```css
@overlapui()
/* ou-root + avatar + card + img + circle + reduced-motion */
```

---

## 8. Markup patterns

### Avatars

```html
<ul class="avatar-overlap" tabindex="0">
  <li data-alph="g">Gina Park</li>
  <li data-alph="h"><img src="https://i.pravatar.cc/80?img=2" alt="">Hank Lee</li>
  <li data-alph="5">5001</li>
</ul>
```

### Cards

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

### Circle

First child = center; `--n` = satellite count.

```html
<div class="circle-overlap" style="--n:5" tabindex="0">
  <button type="button" aria-label="Menu">+</button>
  <a href="#">H</a>
  <a href="#">S</a>
  <a href="#">C</a>
  <a href="#">G</a>
  <a href="#">P</a>
</div>
```

Use **`tabindex="0"`** so `:focus-within` matches hover.

---

## 9. Full examples

### CSS-only (no FSCSS)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>overlapui CSS-only</title>
  <link rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.min.css">
  <style>
    :root {
      --ou-center-bg: #6366f1;
      --ou-ring: #0f172a;
    }
    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      gap: 48px;
      background: #0b1220;
    }
  </style>
</head>
<body>
  <ul class="avatar-overlap" tabindex="0">
    <li data-alph="g">G</li>
    <li data-alph="h">H</li>
    <li data-alph="i">I</li>
    <li data-alph="j">J</li>
  </ul>

  <div class="circle-overlap" style="--n:5" tabindex="0">
    <button type="button" aria-label="Menu">+</button>
    <a href="#" aria-label="Home">H</a>
    <a href="#" aria-label="Search">S</a>
    <a href="#" aria-label="Chat">C</a>
    <a href="#" aria-label="Groups">G</a>
    <a href="#" aria-label="Profile">P</a>
  </div>
</body>
</html>
```

### FSCSS one-shot

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

### FSCSS selective + custom class

```css
@import((
ou-root,
ou-avatar-colors,
avatar-overlap,
ou-reduced-motion
) from overlapui)

@ou-root()
@avatar-overlap(.members)
@ou-reduced-motion()

:root {
  --ou-size: 36px;
  --ou-overlap: -12px;
}
```

```html
<ul class="members" tabindex="0">
  <li data-alph="a">Ada</li>
  <li data-alph="n">Nia</li>
</ul>
```

---

## 10. Token reference

| Variable | Default (approx.) | Usage |
|----------|-------------------|--------|
| `--ou-ring` | `#17203a` | Ring color |
| `--ou-ease` | `cubic-bezier(.3, 1.3, .5, 1)` | Easing |
| `--ou-time` | `.35s` | Duration |
| `--ou-size` | `44px` | Avatar diameter |
| `--ou-overlap` | `-14px` | Avatar pull-in |
| `--ou-gap` | `6px` | Avatar spread gap |
| `--ou-c` | `#f59e0b` | Fallback face color |
| `--ou-letter` | `#fff` | Letter color |
| `--ou-ring-width` | `3px` | Ring thickness |
| `--ou-peek` | `34px` | Card peek |
| `--ou-card-h` | `76px` | Card min-height |
| `--ou-card-gap` | `10px` | Card gap when open |
| `--ou-card-bg` | `#1f2a48` | Card background |
| `--ou-card-color` | `#cbd5ee` | Card body text |
| `--ou-card-title` | `#fff` | Card title |
| `--ou-img` | `96px` | Image size |
| `--ou-img-overlap` | `-52px` | Image pull-in |
| `--ou-img-gap` | `12px` | Image gap when open |
| `--ou-img-tilt` / `--ou-img-tilt-even` | `-5deg` / `4deg` | Rest tilt |
| `--ou-item` | `48px` | Circle control size |
| `--ou-r` | `86px` | Orbit radius |
| `--ou-center-bg` | `#ec4899` | Center button |
| `--n` | `6` | Satellite count (on element) |

---

## 11. Accessibility & motion

- Keep **real names** in the DOM for avatars; faces may use `font-size: 0` for the decorative letter.
- Meaningful **`alt`** when the image matters; otherwise name text beside the stack.
- **`tabindex="0"`** on stack roots for keyboard `:focus-within`.
- **`aria-label`** on circle center and each action.
- Standalone CSS includes reduced-motion rules; under FSCSS call `@ou-reduced-motion()`.

---

## 12. Repo layout & build

```text
overlapui.fscss              # FSCSS source (selective)
overlapui.standalone.fscss   # expands defaults → CSS
overlapui.css                # compiled (CI / npm)
package.json                 # "style": "overlapui.css"
templates/stacks/            # demo page
.github/workflows/           # compile standalone on change
```

Local compile:

```bash
npm install -g fscss@1.2.5
fscss overlapui.standalone.fscss overlapui.css
```

---

## 13. License

MIT · [FSCSS](https://fscss.devtem.org) · [fscss-ttr](https://github.com/fscss-ttr)

```bash
npm install overlapui
npm install -g fscss@1.2.5   # only if you use the .fscss source
```

[Issues](https://github.com/fscss-ttr/overlapui.fscss/issues) · [FSCSS](https://fscss.devtem.org/)
