# overlapui.fscss
> CSS-first overlapping UI stack: avatars, cards, and images that overlap at rest and spread on hover/focus in FSCSS.

Stack avatars, cards, and images that **overlap at rest** and **spread on hover/focus**.

All visuals read from **`--ou-*`** tokens (set via `@ou-root()` or overrides on a parent).

## Install

```css
@import((*) from overlapui)
/* or path /

@overlapui()
```

Selective:

```css
@import((ou-root, avatar-overlap, ou-reduced-motion) from overlapui)

@ou-root()
@avatar-overlap(.avatar-overlap)
@ou-reduced-motion()
```

## Tokens (`--ou-*`)

| Token | Role | Default idea |
|-------|------|----------------|
| `--ou-ring` | Ring / border around avatars & images | `#17203a` |
| `--ou-ease` / `--ou-time` | Spread animation | spring-ish / `.35s` |
| `--ou-size` / `--ou-overlap` / `--ou-gap` | Avatar diameter, pull-in, spread gap | |
| `--ou-c` / `--ou-letter` | Fallback face color / letter color | |
| `--ou-peek` / `--ou-card-h` / `--ou-card-gap` | Card stack | |
| `--ou-card-bg` / `--ou-card-color` / `--ou-card-title` | Card chrome | |
| `--ou-img` / `--ou-img-overlap` / `--ou-img-gap` | Image stack | |
| `--ou-img-tilt` / `--ou-img-tilt-even` | Rest tilt | |

Helpers:

```css
:root {
  @ou-avatar-size(48px, -16px)
  @ou-img-size(112px, -60px)
}
```

## Markup

**Avatars** — letter from `data-alph`; optional `<img>` covers letter.

```html
<ul class="avatar-overlap" tabindex="0">
  <li data-alph="g">Gina</li>
  <li data-alph="h"><img src="..." alt="">Hank</li>
  <li data-alph="5">5001</li>
</ul>
```

**Cards** — direct child `div`s (thumb + `h3` + text).

```html
<div class="card-overlap" tabindex="0">
  <div>
    <img src="..." alt="">
    <h3>Title</h3>
    subtitle
  </div>
  <div>...</div>
</div>
```

**Images**

```html
<div class="img-overlap" tabindex="0">
  <img src="..." alt="">
  <img src="..." alt="">
</div>
```

`tabindex="0"` so keyboard `:focus-within` can spread the stack.

## Mixins

| Define | Purpose |
|--------|---------|
| `@ou-root()` | Inject `--ou-*` on `:root` (or custom selector) |
| `@avatar-overlap(sel)` | Avatar stack + A–Z / 0–9 color map |
| `@card-overlap(sel)` | Vertical card stack |
| `@img-overlap(sel)` | Horizontal image stack |
| `@ou-reduced-motion()` | Disable transitions when preferred |
| `@overlapui()` | All of the above with default class names |

## License

MIT — same spirit as other FSCSS UI modules.
