# Forecast

A weather app built as a Claude Design canvas (`Forecast.dc.html`). It runs entirely in the
browser — live conditions, 48-hour and 7-day forecasts from [Open-Meteo](https://open-meteo.com),
with city search and reverse geocoding. No API keys, no server, no build step.

## Files

| Path | What it is |
| --- | --- |
| `Forecast.dc.html` | The app — a single design-canvas artboard |
| `support.js` | Design-canvas runtime (generated; do not edit) |
| `_ds/` | The "Organic" design system: tokens, stylesheet, component guide |
| `render.yaml` | Render static-site blueprint |

## Responsive layout

One page, four breakpoints. The artboard's inline styles are the desktop baseline; a media-query
layer in `Forecast.dc.html` overrides them from there down.

| Width | What changes |
| --- | --- |
| ≤ 1080px | Tighter page padding, smaller hero temperature (large tablets, small laptops) |
| ≤ 900px | Grid collapses to a single column; search bar stretches to fill the header |
| ≤ 640px | Phone scale: 2-up stat tiles, shorter charts, condensed 7-day rows, safe-area padding, 16px search field so iOS does not zoom on focus |
| ≤ 380px | Small phones (iPhone SE, Galaxy S) — tighter day rows and hour tiles |

Landscape phones get a shorter top/bottom padding rule. Hour strips scroll horizontally with
snap points; verified from 320px to 1024px with no horizontal page overflow.

## Run locally

```sh
python3 -m http.server 8000
# then open http://localhost:8000/Forecast.dc.html
```

Any static file server works. Opening the file directly with `file://` will not — the runtime
fetches its dependencies over HTTP.

## Deploy on Render

The repo carries a [Blueprint](https://render.com/docs/infrastructure-as-code), so Render can
configure the service itself:

1. In the Render dashboard, choose **New → Blueprint** and pick this repository.
2. Render reads `render.yaml` and creates a free static site named `forecast`.
3. Click **Apply**. Every push to `main` redeploys automatically.

Or set it up by hand with **New → Static Site**:

- **Build command:** leave empty
- **Publish directory:** `.`
- **Redirects/Rewrites:** rewrite `/` → `/Forecast.dc.html`

That last rewrite is what serves the app at the site root; without it the root path 404s and the
app is only reachable at `/Forecast.dc.html`.
