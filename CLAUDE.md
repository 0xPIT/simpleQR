# simpleQR

## What this is

A fully self-contained, serverless, stateless QR code generator in a single `index.html` file. Deployed as a GitHub Page at https://0xpit.github.io/simpleQR/

## Architecture decisions

- **Single file**: Everything (HTML, CSS, JS, QR encoder) lives in `index.html`. No build step, no dependencies, no frameworks.
- **From-scratch QR encoder**: Rather than embedding a library, the QR encoder is written from scratch (~400 lines). Supports byte-mode encoding, versions 1–40, Reed-Solomon error correction over GF(2^8), all 8 mask patterns with penalty scoring.
- **URL hash state**: All parameters (`text`, `ec`, `size`, `margin`, `fg`, `bg`) are stored in `location.hash` using `URLSearchParams`. Uses `replaceState` to avoid polluting browser history. Supports bookmarking and sharing.
- **Canvas rendering**: QR codes render to `<canvas>` with `image-rendering: pixelated`. PNG export via `canvas.toBlob()`.
- **Modern CSS**: Uses `light-dark()` for automatic dark/light mode, `color-scheme: light dark`, `dvh` units, system font stack. No CSS framework.
- **Vanilla JS**: No frameworks, no transpilation. Native `<input type="color">`, `<select>`, `<textarea>`.

## Deployment

- Repo: `git@github.com-0xpit:0xPIT/simpleQR.git` (uses SSH host alias `github.com-0xpit` mapped to `~/.ssh/id_0xpit_github` key)
- GitHub Pages deployed via `.github/workflows/pages.yml` using `actions/deploy-pages` with `enablement: true`
- Git user for this project: `0xPIT` / `0xPIT@users.noreply.github.com`

## Features

- Live preview with 120ms debounce
- Error correction level toggle (L/M/Q/H)
- Foreground/background color pickers
- Size selector (128–1024px)
- Quiet zone margin control (narrow/normal/wide)
- Copy URL to clipboard
- Download as PNG
- `hashchange` listener for manual URL edits

## Known limitations

- Byte-mode encoding only (no alphanumeric/numeric/kanji optimizations — these would reduce size for certain inputs)
- No SVG export (canvas only)
- No logo/image overlay support
