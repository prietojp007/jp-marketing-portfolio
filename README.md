# JP — Marketing Portfolio

A single-file marketing portfolio built on the Resend design system ("black velvet with violet
neon") — pure-black canvas, hairline graphite borders over drop shadows, ghost buttons (never
filled), and a single violet accent reserved for code-style strings like the LinkedIn URL.

## Files

- **`jp-portfolio.html`** — the entire site. Self-contained: all CSS and JS are inline, and every
  image is embedded as base64, so the page works by just opening the file in a browser (no build
  step, no server).
- **`index.html`** — a redirect stub for GitHub Pages; sends the root URL to
  `jp-portfolio.html#about`.
- **`jp-hero.jpg`**, **`about-1.jpg` … `about-8.jpg`** — original image sources, kept for future
  edits. Not loaded at runtime (the page uses the embedded base64 copies instead).

## Running it locally

Just open `jp-portfolio.html` in any browser. To serve it over HTTP (needed to see the animated
backgrounds exactly as they'll behave once hosted):

```bash
python3 -m http.server 8000
# then open http://localhost:8000/jp-portfolio.html
```

## What's on the page

- Tab-based nav — About, Work, Results, Photography, and Capabilities are switched by the pill
  nav instead of scrolled past. Opening the site with no hash lands on About by default.
- **About tab**: a static hero ("Marketing that gets measured, not just made.", serif display
  type) plus a short personal intro block (headline, 3-sentence bio, one CTA into Work).
- **Work tab**: the full story — bio copy and quick-facts panel, the Selected Work project grid
  (glass-icon tiles, all sharing one violet gradient per the design system), Credentials, and the
  three hover-to-expand accordion galleries (Campaigns & Projects, Involvement, Events) built from
  real campaign/event graphics.
- **Results tab**: an analytics dashboard (stat cards, delta chips, reach-by-platform bars and
  interactive breakdown charts) pulled from LinkedIn Analytics and Meta Business Suite, switchable
  per program plus an "All Programs" collective rollup.
- **Photography tab**: a depth-stacked, drag/scroll carousel (DriftWall) of real event and
  portrait photos.
- **Capabilities tab**: a WebGL pixel-dither shader background, tinted the brand violet.
- **Get in touch** (top-right button, not a nav tab): contact form that composes a pre-filled
  email — this is a static file with no backend, so there's nothing to wire it to yet — plus a
  linked LinkedIn URL styled as a violet monospace identifier.
