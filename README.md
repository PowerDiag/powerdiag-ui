# PowerDiag UI

The shared visual language for the PowerDiag browser tools — one stylesheet, no build step,
no dependencies. Drop it in, add an app-specific stylesheet on top, and every tool looks like it
came from the same company.

Open [`index.html`](index.html) for the component reference: every class in the library rendered on
one page. It doubles as the check when changing anything here — if something looks wrong there, it
looks wrong in every tool.

## Using it

Vendored (recommended — the tool stays self-contained and works offline):

```html
<link rel="stylesheet" href="./vendor/powerdiag.css">
<link rel="stylesheet" href="./styles.css">   <!-- app-specific, loaded after -->
```

Or from the shared deployment, when a tool would rather follow the design system automatically:

```html
<link rel="stylesheet" href="https://powerdiag.jp/ui/v1/powerdiag.css">
```

Vendoring is the safer default: a mistake here cannot take every tool down at once, and a tool with
its own copy still works on `localhost` and offline. Copy the file in and update it deliberately.

## What is in it

| Area | Classes |
|---|---|
| App shell | `.app-shell` on `<body>`, `.app-main`, `.app-content` |
| App bar | `.appbar`, `.appbar-brand`, `.appbar-product`, `.appbar-left/right`, `.port` |
| Status bar | `.statusbar`, `.statusbar-right`, `.build`, `.copyright` |
| Status | `.status` + `.ok/.warn/.error`, `.dot` + `.on/.warn/.error`, `.conn-state`, `.badge` + `.ok/.warn/.danger` |
| Buttons | `.btn` + `.primary/.danger/.ghost/.small/.big`, `.iconbtn`, `.actions` |
| Forms | `select`, `input[type=text\|number]` (element selectors; the select caret is drawn by the library) |
| Cards | `.grid`, `.card` |
| Readout | `.readout`, `.readout-label/value/side/aux` |
| Lists | `.fields`, `.fields .row`, `.fields.compact`, `.block-field` |
| Meters | `.meters`, `.meter`, `.meter-name/track/fill/value`, `.meter-fill.warn/.crit` |
| Notices | `.hint` + `.warn`, `.footnote`, `.banner` + `.warn` |
| Log | `.log`, `.log-line` + `.tx/.rx/.err` |
| Utilities | `.hidden`, `.mono`, `.wrap` |

## The app shell

`.app-shell` on `<body>` gives the layout these tools want: a bar pinned to the top, a status strip
pinned to the bottom, content stretching between them, and no page scrolling — a technician should
see the whole reading without hunting for a scrollbar. Cards scroll internally when their contents
overflow.

Below 860 x 620 the columns would be unusably narrow, so the shell falls back to ordinary document
scrolling rather than squeezing.

```html
<body class="app-shell">
  <header class="appbar">…</header>
  <main class="app-main">
    <div class="app-content">…</div>
  </main>
  <footer class="statusbar">…</footer>
</body>
```

## Theming

Everything is driven by custom properties on `:root`, prefixed `--pd-`. Override them after the
library loads; do not edit the library to change one colour:

```css
:root { --pd-accent: #35d0a5; }
```

Light mode is handled through `prefers-color-scheme` — only the surface and text tokens change, the
layout does not. Corners are near-square by design (`--pd-radius: 3px`): these are instruments, not
consumer apps.

## Conventions

* State goes on a modifier class (`.ok`, `.warn`, `.danger`), never a separate component.
* Numbers use `--pd-mono` with `tabular-nums`, so figures do not jitter as readings update.
* Nothing here depends on JavaScript, and nothing here is loaded from another origin.

## Icons

`.iconbtn` expects an inline SVG child, sized and coloured by the library. The close glyph used in
the tools is Material Symbols `close` (Apache-2.0), inlined rather than linked: nothing here loads
from another origin, so the tools keep working offline.

## Licence

MIT — see [LICENSE.md](LICENSE.md).
