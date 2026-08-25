# Forecast

A weather app that runs entirely in the browser — current conditions, 48 hours ahead, and a
seven-day outlook, drawn from [Open-Meteo](https://open-meteo.com). No API keys, no server, no
build step. It is a single HTML file plus its runtime.

## What it does

- **Current conditions** — temperature, "feels like", humidity, wind, and precipitation for
  wherever you are or whatever you searched for.
- **Next 48 hours** — a horizontally scrolling hour strip, plus temperature and rain-chance charts.
- **Seven days** — daily highs and lows, condition icons, and a daylight band showing sunrise
  and sunset.
- **City search** — type a place name and pick from Open-Meteo's geocoder.
- **Use my location** — one tap for browser geolocation, reverse-geocoded to a place name.
- **Pinned places** — save the cities you check often; they persist in `localStorage`, as does
  the last place you viewed, so the app opens where you left it.
- **°C / °F** — a unit toggle that carries through every reading and both charts.

Everything is fetched client-side from Open-Meteo's free, key-less endpoints:
`api.open-meteo.com` for forecasts and `geocoding-api.open-meteo.com` for place lookup.

## Layout

| Path | What it is |
| --- | --- |
| `Forecast.dc.html` | The whole app — markup, styles, and logic in one design-canvas artboard |
| `support.js` | Design-canvas runtime (generated; do not edit by hand) |
| `_ds/` | The "Organic" design system — the token stylesheet the app draws all of its color, type, and spacing from, plus a written guide |
| `render.yaml` | Render static-site blueprint |

## Responsive behavior

One page, four breakpoints. The artboard's inline styles are the desktop baseline; a media-query
layer at the top of `Forecast.dc.html` overrides them from there down.

| Width | What changes |
| --- | --- |
| ≤ 1080px | Tighter page padding, smaller hero temperature (large tablets, small laptops) |
| ≤ 900px | Grid collapses to a single column; the search bar stretches to fill the header |
| ≤ 640px | Phone scale: 2-up stat tiles, shorter charts, condensed 7-day rows, safe-area padding, and a 16px search field so iOS does not zoom on focus |
| ≤ 380px | Small phones (iPhone SE, Galaxy S) — tighter day rows and hour tiles |

Landscape phones get a shorter top/bottom padding rule. Hour strips scroll horizontally with snap
points; verified from 320px to 1024px with no horizontal page overflow.

## Run locally

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000/Forecast.dc.html>.

Any static file server works. Opening the file directly over `file://` will **not** — the runtime
fetches its dependencies over HTTP, and browsers block that from a file URL.

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

That last rewrite is what serves the app at the site root. Without it the root path 404s and the
app is only reachable at `/Forecast.dc.html`. The same two settings work on Netlify, Vercel,
GitHub Pages, or any other static host.

## Credits

- Weather and geocoding data from [Open-Meteo](https://open-meteo.com), used under its free
  non-commercial terms.
- Icons from [Lucide](https://lucide.dev).
- Typefaces: [Caprasimo](https://fonts.google.com/specimen/Caprasimo) and
  [Figtree](https://fonts.google.com/specimen/Figtree), served by Google Fonts.
