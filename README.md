# Workbench

**Your files. Your browser. No detours.**

A collection of browser-based image processing tools that run completely in your browser. Drop a file, tweak the controls, download the result. No file ever leaves your device.

![status](https://img.shields.io/badge/status-active-brightgreen)
![license](https://img.shields.io/badge/license-MIT-blue)
![vanilla](https://img.shields.io/badge/stack-vanilla%20JS-yellow)

> Built by **Rudra Thorat**

---

## Features

| | | |
|---|---|---|
| **Zero uploads** | No server, no cloud, no analytics. Your file never leaves the browser tab. | **Offline-ready** | Works from `localhost`, a USB drive, or any static host. No internet needed after load. |
| **No dependencies** | Zero npm packages, no frameworks, no build step. Pure HTML, CSS, and JavaScript. | **Desktop + mobile** | Mouse drag on desktop, touch drag on mobile. Responsive down to 320 px. |
| **Batch processing** | Compress and convert multiple files at once with per-file download. | **Live preview** | Canvas-based preview updates in real time as you adjust controls. |
| **Hero file forwarding** | Drop an image on the landing page, then jump straight to any tool with the file loaded. | **Start Over** | Every tool has a Start Over button to reset and load a new file without refreshing. |

---

## Tools

| Tool | Route | Description |
|------|-------|-------------|
| **Resize** | `/resize/` | Scale images to exact dimensions with aspect-ratio lock, quick presets (Full HD, HD, Square, Portrait, Icon), and live canvas preview |
| **Compress** | `/compress/` | Reduce file size via quality slider (1–100), batch process multiple files, force output format, see savings percentage in real time |
| **Convert** | `/convert/` | Convert between PNG, JPEG, and WebP with quality controls for lossy formats and live preview |
| **Crop** | `/crop/` | Draggable, resizable crop overlay with rule-of-thirds grid guides. Aspect ratio presets: Free, 1:1, 4:3, 16:9, 9:16. Full touch support. |
| **Rotate / Flip** | `/rotate/` | Rotate 90° left/right, flip horizontally or vertically. Actions stack; reset returns to original orientation |
| **Image to Base64** | `/base64/` | Encode any image as a `data:` URI or raw base64 string. Decode a pasted base64 string back into an image with live preview and auto MIME detection |

---

## How It Works

Every tool follows the same pipeline — all running client-side via the Canvas 2D API:

```
Drop/select file
  → loadImage() creates an offscreen <img>
    → drawToCanvas() renders it to a <canvas>
      → User adjusts controls (resize, compress, crop, etc.)
        → canvas.toBlob() exports the result
          → downloadBlob() triggers a file download
```

No server round-trip. No data leaves the tab. The entire process takes milliseconds.

---

## Getting Started

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

## Project Structure

```
workbench/
├── index.html              # Landing page — hero dropzone + tool grid
├── resize/index.html       # Resize tool
├── compress/index.html     # Compress tool (batch support)
├── convert/index.html      # Convert tool
├── crop/index.html         # Crop tool (drag + resize overlay)
├── rotate/index.html       # Rotate / Flip tool
├── base64/index.html       # Base64 encode / decode tool
├── shared/
│   ├── app.js              # Shared utilities (loadImage, formatBytes, initDropzone, canvas helpers)
│   ├── style.css           # Global design system (CSS custom properties, layout, components)
│   └── fonts/fonts.css     # Self-hosted font declarations (offline fallback)
└── LICENSE
```

---

## Design System

All styling uses CSS custom properties defined in `shared/style.css`:

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#16171a` | Page background (graphite) |
| `--surface` | `#1e1f23` | Card / panel backgrounds |
| `--accent` | `#ff6a3d` | Primary action color (signal orange) |
| `--accent-hover` | `#e85a2c` | Hover state for primary buttons |
| `--success` | `#6bcb77` | Savings indicator, positive feedback |
| `--success-hover` | `#5ab866` | Hover state for download buttons |
| `--text` | `#e8e6e1` | Primary text (warm off-white) |
| `--dim` | `#9b9a96` | Secondary / muted text |
| `--border` | `#34353a` | Subtle dividers and card borders |
| `--font-display` | `'Space Grotesk'` | Headings and logo |
| `--font-body` | `'Inter'` | Body text and controls |
| `--font-mono` | `'JetBrains Mono'` | Data readouts and base64 output |

---

## Roadmap

Planned tools for future releases:

| Tool | Status |
|------|--------|
| Images to PDF | Planned |
| PDF to Images | Planned |
| PDF Merge / Split | Planned |
| QR Code Generator | Planned |
| Watermark | Planned |
| Image Collage | Planned |
| MP4 to MP3 | Planned |
| Video Compress | Planned |
| GIF Maker | Planned |
| Background Remover | Planned |

---

## Browser Support

Modern browsers only (Chrome, Firefox, Safari, Edge — latest 2 major versions). Requires:

- `CanvasRenderingContext2D`
- `HTMLCanvasElement.toBlob()`
- `URL.createObjectURL`
- `FileReader.readAsDataURL` (base64 encode)
- `navigator.clipboard.writeText` (base64 copy)
- CSS custom properties, Grid, and Flexbox

---

## Accessibility

- **Skip-to-content link** — Keyboard users can skip the nav bar on every page
- **`<main>` landmark** — All tool content is wrapped in a `<main>` element for screen reader navigation
- **`prefers-reduced-motion`** — All animations and transitions are disabled when the OS reduce-motion setting is active
- **`focus-visible`** — Orange focus ring appears only for keyboard navigation, not mouse clicks
- **`.sr-only`** — Utility class for screen-reader-only content
- **ARIA attributes** — `aria-current="page"` on active nav links, `role="radiogroup"` on crop aspect ratio presets, `aria-label` on file inputs
- **Touch support** — Crop tool handles both mouse and pointer events with `touch-action: none`
- **Semantic HTML** — Proper headings, labels, and ARIA attributes throughout

---

## Development

No build step. Edit the HTML, CSS, or JS files and reload the browser.

### Shared Utilities (`shared/app.js`)

| Function | Purpose |
|----------|---------|
| `formatBytes(bytes)` | Human-readable file sizes (e.g. "1.2 MB"). Handles NaN, negative, and non-numeric input. |
| `splitFilename(name)` | Split "photo.webp" → `{ base, ext }` |
| `getMimeFromExt(ext)` | Extension → MIME type |
| `loadImage(file)` | Promise-based image loading from a File. Revokes the Object URL after load to prevent memory leaks. |
| `drawToCanvas(img, w, h)` | Render an image to an offscreen canvas |
| `canvasToBlob(canvas, mime, quality)` | Promise-based `canvas.toBlob()` with null/error handling |
| `downloadBlob(blob, filename)` | Trigger a browser file download with try/catch |
| `initDropzone(el, onFile)` | Wire up drag-and-drop + file input + paste events. Validates drag types. |

### Adding a New Tool

1. Create a folder like `my-tool/` with an `index.html`
2. Follow the standard page structure: `top-bar` → `page-header` → `dropSection` → `workbench` → `privacy-note`
3. Import `shared/style.css` and `shared/app.js`
4. Wrap all JS in an IIFE to avoid global pollution
5. Register the tool in `index.html` by adding it to the `tools` array and nav links

---

## FAQ

**Does my file get uploaded anywhere?**
No. All processing happens in your browser via the Canvas API. No data is sent to any server.

**Is there a file size limit?**
There's no hard limit, but very large images (50+ MP) may cause slow rendering or memory warnings depending on your device. The canvas has a maximum dimension limit that varies by browser.

**Can I use this offline?**
Yes. Once loaded, the page works without an internet connection. Serve it from `localhost`, a USB drive, or any local file server.

**Why don't the planned tools work yet?**
They're displayed in the grid as a roadmap. Each tool will be added as a self-contained `index.html` page following the same pattern as the existing tools.

---

## License

[MIT](LICENSE) — Rudra Thorat
