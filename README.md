# Workbench

**Client-side file utility tools — zero uploads, zero servers, zero bloat.**

Workbench is a collection of browser-based image processing tools that run completely in your browser. Drop a file, tweak the controls, download the result. No file ever leaves your device.

![screenshot](https://img.shields.io/badge/status-active-brightgreen)
![license](https://img.shields.io/badge/license-MIT-blue)

---

## Tools

| Tool | Route | What it does |
|------|-------|-------------|
| **Resize** | `/resize/` | Scale images to exact dimensions with aspect-ratio lock, quick presets, and live preview |
| **Compress** | `/compress/` | Reduce file size via quality slider (1–100), batch process multiple files, force output format |
| **Convert** | `/convert/` | Convert between PNG, JPEG, and WebP with quality controls for lossy formats |
| **Crop** | `/crop/` | Select any region of an image via a draggable, resizable overlay with rule-of-thirds grid guides. Aspect ratio presets: Free, 1:1, 4:3, 16:9, 9:16 |
| **Rotate / Flip** | `/rotate/` | Rotate 90° left/right, flip horizontally or vertically. Actions stack; reset returns to original orientation |
| **Image to Base64** | `/base64/` | Encode any image as a `data:` URI (or raw base64). Decode a pasted base64 string back into an image file with live preview |

Planned tools (inactive in the grid): Images to PDF, PDF to Images, PDF Merge / Split, QR Code Generator, Watermark, Image Collage, MP4 to MP3, Video Compress, GIF Maker, Background Remover.

---

## Why Workbench?

- **100% private.** No upload, no server round-trip, no analytics. Your file never leaves your browser tab.
- **Works offline.** Serve from `localhost` or a USB drive; no internet required after the initial page load.
- **No dependencies.** Zero npm packages, no frameworks, no build step. Vanilla HTML, CSS, and JavaScript.
- **Desktop + mobile.** Mouse drag works on desktop; touch drag works on mobile. Responsive layout down to 320 px.
- **Fast.** No JavaScript churn. Pages are static HTML that hydrate in a single event loop tick.

---

## Getting started

Workbench is a collection of static files. Serve them with any HTTP server:

```bash
# Python 3
python -m http.server 8080

# Node (with npx)
npx serve .

# Rust
cargo install simple-http-server && simple-http-server --port 8080
```

Then open `http://localhost:8080` in your browser.

---

## Project structure

```
workbench/
├── index.html              # Landing page with hero dropzone + tool grid
├── resize/index.html       # Resize tool
├── compress/index.html     # Compress tool
├── convert/index.html      # Convert tool
├── crop/index.html         # Crop tool
├── rotate/index.html       # Rotate / Flip tool
├── base64/index.html       # Base64 encode / decode tool
├── shared/
│   ├── app.js              # Shared utilities (loadImage, formatBytes, initDropzone, canvas helpers)
│   ├── style.css           # Global design system (CSS custom properties, layout, components)
│   └── fonts/fonts.css     # Self-hosted font declarations (optional)
└── README.md
```

---

## Design system

All styling uses CSS custom properties defined in `shared/style.css`:

| Token | Value |
|-------|-------|
| `--bg` | `#16171a` (graphite) |
| `--surface` | `#1e1f23` |
| `--accent` | `#ff6a3d` (signal orange) |
| `--success` | `#6bcb77` |
| `--text` | `#e8e6e1` |
| `--font-display` | `'Space Grotesk', system-ui, sans-serif` |
| `--font-body` | `'Inter', system-ui, sans-serif` |
| `--font-mono` | `'JetBrains Mono', 'Consolas', monospace` |

Fonts are loaded via Google Fonts. Each tool page is self-contained and reuses the shared stylesheet — no tool page introduces new colors, font families, or design tokens.

---

## Browser support

Modern browsers only (Chrome, Firefox, Safari, Edge — latest 2 major versions). Requires:

- `CanvasRenderingContext2D` (all tools)
- `HTMLCanvasElement.toBlob()` (all tools)
- `URL.createObjectURL` (all tools)
- `FileReader.readAsDataURL` (base64 encode)
- `navigator.clipboard.writeText` (base64 copy)
- `CSS custom properties`
- `CSS Grid` / `CSS Flexbox`

---

## Development

No build step. Edit the HTML, CSS, or JS files and reload the browser.

**Adding a new tool:**

1. Create a folder like `my-tool/` with an `index.html`.
2. Use the same page structure as the existing tools: topbar, page-header, dropzone, workbench (or layout-single), privacy-note.
3. Import `shared/style.css` and `shared/app.js`.
4. Register the tool in `index.html` by adding it to the `tools` array and the nav links.

---

## License

[MIT](LICENSE)
