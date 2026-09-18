# JP — Marketing Portfolio

A single-file marketing portfolio built on the [Linear](https://linear.app) design system —
dark-first, hairline borders over drop shadows, acid-lime reserved for one primary call to action.

## Files

- **`jp-portfolio.html`** — the entire site. Self-contained: all CSS and JS are inline, and every
  image is embedded as base64, so the page works by just opening the file in a browser (no build
  step, no server).
- **`jp-hero.jpg`**, **`about-1.jpg` … `about-8.jpg`** — the original image sources, kept for
  future edits. They're not loaded by the page at runtime (it uses the embedded base64 copies).

## Running it locally

Just open `jp-portfolio.html` in any browser. To serve it over HTTP (needed to see the animated
backgrounds exactly as they'll behave once hosted):

```bash
python3 -m http.server 8000
# then open http://localhost:8000/jp-portfolio.html
```

## What's on the page

- Hero with a live cursor-reactive particle mesh and a feathered portrait
- Metrics strip with a glassmorphism panel and hover light-sweep
- Work section with glass-icon tiles per project
- Capabilities section with a WebGL pixel-dither shader background
- About section with a mouse-parallax mosaic of real project pieces
- Pill-style nav with scroll-spy active state
