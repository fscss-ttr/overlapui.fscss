# templates/stacks

Focused demo for **overlapui.fscss** — avatar, card, image, and circle stacks, plus a members table.

## Path in this repo

```text
overlapui.fscss/
├── overlapui.fscss
├── README.md
└── templates/
    └── stacks/
        ├── index.html
        ├── stacks.fscss
        └── README.md
```

## Run

From `templates/stacks/`:

```bash
npx serve .
```

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<link type="text/fscss" href="stacks.fscss">
```

`stacks.fscss` imports the module via relative path:

```fscss
@import((*) from "../../overlapui.fscss")
```

Or 

```fscss
@import((*) from overlapui)
```

## Compile

```bash
npx fscss stacks.fscss stacks.css
```

## Requires

- FSCSS **1.2.5+** recommended
- Parent module: `overlapui.fscss` at repo root

## License

MIT — same as the main module.
