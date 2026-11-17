# quick-tools

A small collection of browser tools I use often. Each one is a self-contained
HTML page: no build step, no framework and no server, so it works straight from
GitHub Pages or from a file saved to disk.

**Live site:** <https://ghaliomar.com/quick-tools/>

## Tools

| Tool | What it does | Network |
| --- | --- | --- |
| [LaTeX Scratchpad](./latex.html) | Type LaTeX and see it rendered live; copy or download the result as SVG or PNG. | Needs internet (renders through CodeCogs) |
| [Graph Paper](./graphpaper.html) | 78 printable graph-paper generators (grids, dots, hex, isometric, music staves, Cornell notes, polar, log, and more) exported as PDF, SVG or PNG. | Works fully offline |
| [QR Code Generator](./qr/) | QR codes for links, text, Wi-Fi, contacts, email, phone and SMS, exported as SVG, PNG, PDF, EPS or text. | Works fully offline |

### LaTeX Scratchpad — [`latex.html`](./latex.html)

A scratchpad for checking how an equation looks, or for getting an image of it
to paste into slides, docs or chat.

- **Live preview.** Renders about 0.7 s after you stop typing.
  <kbd>Ctrl/⌘</kbd>+<kbd>Enter</kbd> renders immediately; <kbd>Tab</kbd> inserts
  two spaces.
- **Options.** Size (`normal` through `\Huge`) and an *inline* toggle
  (`\inline`, i.e. text-style rather than display-style math).
- **Export.**
  - *Copy LaTeX*: the source.
  - *Copy image URL*: a CodeCogs SVG link you can embed anywhere that accepts
    an image URL.
  - *Share link*: a link to this page with the equation in the URL
    (`latex.html#tex=…`).
  - *SVG* / *PNG*: downloads; the PNG is 300 dpi.
- **Remembers your work.** The last equation, size and inline setting are saved
  in the browser's `localStorage` under `quick-tools:latex`. A `#tex=` share
  link overrides the saved equation.
- **Errors.** Render problems reported by the API show under the preview. If the
  API rate-limits (HTTP 429), the page waits for the `Retry-After` delay and
  retries on its own.
- **Dark mode.** Follows the system setting; rendered equations are inverted
  to stay readable.

Rendering is done by the
[CodeCogs FxTex API](https://fxeditor.codecogs.com/docs/1-fxtexserver_api.php),
so the equation text is sent to CodeCogs and the page doesn't work offline.

### Graph Paper — [`graphpaper.html`](./graphpaper.html)

A copy of Kevin MacLeod's
[Incompetech Graph Paper Generators](https://incompetech.com/graphpaper/)
(version 2.3), kept here so it's always one click away and available offline.

- **Pick a paper type** from the index, or narrow the list with the filter box.
  Each generator has its own settings (spacing, line weights, colors and so on)
  plus a live page preview and a zoomed detail view.
- **Page setup.** Letter, Tabloid, A4 and A3 presets, a *Rotate* button, custom
  width/height in inches or cm, and margins.
- **Export.** *Download PDF*, *Open PDF* (opens it in a tab to print from
  there), *SVG* (for cutting machines and design apps) and *PNG* (300 dpi, for
  tablets and virtual tabletops).
- **Links.** Each generator has its own URL hash (e.g. `graphpaper.html#…`), and
  *Copy Link* captures the exact settings.
- **Remembers your page.** Page size, units, margins and colors are saved in
  `localStorage` under `gpAppPrefs`.
- **Download the entire app** saves a standalone copy of the HTML file that
  works offline.

The file and the PDFs it produces are released under
[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain).
For newer versions, see the [upstream page](https://incompetech.com/graphpaper/).

### QR Code Generator — [`qr/`](./qr/)

A QR code generator without ads, sign-ups, tracking or expiring links. The code
is generated in the browser, so nothing you type leaves the page.

- **What to encode** (tabs):
  - *Text / URL*: anything, as is.
  - *Wi-Fi*: network name, password, security and hidden network. Phones offer
    to join when they scan it.
  - *Contact*: name, phone, email, organization, job title and website, as a
    vCard 3.0.
  - *Email*, *Phone*, *SMS*: build `mailto:`, `tel:` and `SMSTO:` codes.

  *Encoded text* under the form shows exactly what goes into the code.
- **Options.**
  - *Error correction* L/M/Q/H (default M). If a higher level fits without
    making the code bigger, the encoder uses it, and the info line says so.
  - *Quiet zone* in modules (default 4, the standard).
  - Foreground and background colors, with a transparent-background option.
- **Scan warnings** for low contrast, light-on-dark colors, a quiet zone under
  4 modules, and text too long to fit.
- **Exports.**

  | Export | Details |
  | --- | --- |
  | PNG | Size of 256 to 4096 px. Each module is a whole number of pixels so edges stay sharp, so the actual size is shown next to the setting. |
  | SVG | Vector, one compact path. |
  | Copy image | Puts the PNG on the clipboard. |
  | PDF, EPS | Vector, one page exactly the *Print size* (in, cm or mm, quiet zone included). |
  | Text | Unicode half-block characters for terminals and code blocks, with a version for dark backgrounds. |

  File names come from the content (e.g. `qr-wifi-homenet.png`).
- **Remembers your settings** in `localStorage` under `quick-tools:qr`, except
  the Wi-Fi password, which is never stored.
- **Share link** (Text tab only): `qr/#text=…` opens the page with the text
  filled in. Other tabs have no link, so passwords and contact details don't end
  up in browser history or chat logs.

Encoding uses [Project Nayuki's QR Code generator library](https://www.nayuki.io/page/qr-code-generator-library)
(MIT), stored as [`qr/qrcodegen.js`](./qr/qrcodegen.js). Upstream only ships it
as TypeScript, so this copy is v1.8.0 with the type annotations removed; the
header comment explains the conversion. To update it, convert the new
`qrcodegen.ts` the same way.

## Running locally

Open any `.html` file directly in a browser. Some browser features (clipboard,
`fetch`) behave better over `http://` than `file://`, so a tiny local server is
handy:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000/> for the tool index.

## Adding a tool

1. **Create the page.** Either a single `name.html` at the repo root, or, for
   something that outgrows one file, a directory `name/` with an `index.html`
   (it will be served at `…/quick-tools/name/`).
2. **Keep it self-contained.** CSS and JavaScript go inline. If a library is
   unavoidable, load a pinned version from a CDN. Prefer tools that run fully in
   the browser; if one calls an external API, say so in the page and in the
   table above.
3. **Follow the shared conventions** (see [`latex.html`](./latex.html) for a
   compact example):
   - `<meta name="viewport" …>` so it works on a phone.
   - A short `<title>` and an inline SVG data-URI favicon.
   - In a tool directory, load a library with a plain `<script src="…">`, not
     `type="module"`: browsers block modules on pages opened from disk.
   - Colors as CSS variables on `:root`, with a
     `@media (prefers-color-scheme: dark)` override.
   - Persist state in `localStorage` under a `quick-tools:<tool>` key, wrapped
     in `try/catch` so the page still works when storage is blocked.
   - Put shareable state in the URL hash (`#key=value`) so a link reproduces
     what's on screen.
4. **List it** in two places:
   - [`index.html`](./index.html): copy one of the `<li class="tool">` cards,
     then update its link, icon, text, tags and `data-keywords` (extra words
     the filter box matches on).
   - The table above, with a short section if it has non-obvious features.

## Deployment

GitHub Pages serves the `main` branch from the repository root at
<https://ghaliomar.com/quick-tools/>. Pushing to `main` publishes changes,
usually within a minute. [`index.html`](./index.html) is the landing page: a
card for each tool, with a filter box (<kbd>/</kbd> focuses it, <kbd>Enter</kbd>
opens the first match).

## License

The graph paper generator is CC0, and `qr/qrcodegen.js` is MIT (see above;
its license text is in the file header). The other tools are my own work;
add a `LICENSE` file if you want to set terms for them.
