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
