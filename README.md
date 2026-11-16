# Haze Watch

Single-page app showing:

- **Singapore haze by area** (North, South, East, West, Central): 24-hour PSI, PM2.5, PM10 and main pollutant from the National Environment Agency (NEA) via data.gov.sg. Singapore only.
- **Weather for Singapore and 12 ASEAN cities** from Open-Meteo.

It is a static site: one `index.html`, no build step, no dependencies, no API keys.

## Data sources

| Data | Source | Endpoint |
| --- | --- | --- |
| PSI, PM2.5, PM10 | NEA via data.gov.sg | `https://api-open.data.gov.sg/v2/real-time/api/psi` |
| Weather | Open-Meteo | `https://api.open-meteo.com/v1/forecast` |

The browser calls both APIs directly. If a request fails, the haze card falls back to a saved NEA reading (16 Nov 2026, 10:00 SGT) and the weather panels show an unavailable message.

data.gov.sg allows 6 calls per 10 seconds per IP without an API key. This app makes one call per load or refresh, so that is plenty for normal use. If you expect heavy traffic, request a free key at data.gov.sg and proxy the call through a Vercel function so the key stays server-side.

Open-Meteo’s free API is for non-commercial use. Check their terms before using this commercially.

NEA data is used under the Singapore Open Data Licence v1.0. Keep the attribution in the page footer.

## Deploy to Vercel

1. Create a GitHub repository and upload these files to the repo root: `index.html`, `vercel.json`, `README.md`, `.gitignore`.
2. In Vercel, choose **Add New → Project** and import the repository.
3. Set **Framework Preset** to **Other**. Leave **Build Command** and **Output Directory** empty. Select **Deploy**.

Or from a terminal:

```bash
npm i -g vercel
vercel --prod
```

## Run locally

```bash
npx serve .
```

Opening `index.html` directly in a browser also works.

## Notes

- `vercel.json` adds basic security headers and a Content-Security-Policy that allows only Google Fonts, data.gov.sg and Open-Meteo. If you add another API or script host, add it to the policy or the browser will block it.
- Fonts (Bricolage Grotesque, Figtree, Inter, JetBrains Mono, Newsreader) load from Google Fonts, with system fallbacks.
