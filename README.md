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

- Tab-based nav — Work, About, Results, Photography, Capabilities, and Contact
  are switched by the pill nav instead of scrolled past; the hero is scoped to
  the Work tab rather than staying pinned above every tab
- Hero with a live cursor-reactive particle mesh and a bordered-block layout
  ending in a "product preview" panel of real Results-tab numbers
- Work section with glass-icon tiles per project
- Photography section with a depth-stacked, drag/scroll carousel of real event
  and portrait photos
- Results tab: an analytics dashboard (stat cards, delta chips, reach-by-
  platform bars) pulled from LinkedIn Analytics and Meta Business Suite,
  switchable per program plus an "All Programs" collective rollup
- Capabilities section with a WebGL pixel-dither shader background
- About tab: bio, credentials, and three hover-to-expand accordion galleries
  (Campaigns & Projects, Involvement, Events) built from real campaign/event
  graphics
- Contact form (name/email/reason/message) that composes a pre-filled email —
  this is a static file with no backend, so there's nothing to wire it to yet
