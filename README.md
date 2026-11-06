# MOYSTER

An avant-garde New Orleans oyster finder (plus the SHUCK oyster-punk art journal). Goal: a tourist finds a spot in under 60 seconds.

Live site: GitHub Pages (`/moyster`), deployed automatically from `main`.

## How the finder works

- **Opens straight into results.** It asks for your location right away. Until you answer, it shows picks from the French Quarter (and says so). If location is blocked, an address box appears.
- **Four modes:** Near me · Happy hour (live now, then later today, then this week) · Cheapest (per oyster, deals still available today first) · Map.
- **Three instant picks** with walking time, price, happy-hour status and a "Walk there" button (Google Maps walking directions). Everything else is listed below.
- **Map and list are linked:** tap an oyster-eye marker to highlight its card; tap a card to highlight its marker. Overlapping markers merge into a numbered cluster.
- Share (phone share sheet), save (stays on the phone, no account), details sheet with the source link.
- Happy-hour status updates every minute in New Orleans time, wherever the visitor's phone is set.

## Updating restaurant info (no coding needed)

All restaurant data lives in **`src/deals.json`**. Edit it on GitHub (pencil icon) and commit.

- Change a price/deal: edit `price`, `unit` (`each`, `half-dozen`, `dozen`, or `variable` if unknown), `deal`, `days` (0 = Sunday … 6 = Saturday), `start`/`end` (24-hour, e.g. `"15:00"`).
- When you've re-checked prices, update **`"updated"`** at the top to today's date. After 45 days the app warns visitors that info may be stale.
- New restaurant: copy an existing entry, give it a unique `id`, and leave out `lat`/`lng`. The robot fills them in.
- Never guess. If a price isn't posted, use `"price": null, "unit": "variable"` and the app shows "Market price".

### The robot (automatic updates)

`.github/workflows/refresh-data.yml` runs every Monday and whenever `deals.json` changes:
1. Finds map coordinates for any address missing them (US Census geocoder, OpenStreetMap fallback).
2. Checks every source link still loads, and lists broken ones in the run summary.
3. Commits the result and redeploys the site.

It never touches prices, deals or times. Those stay human-verified.

## For developers

```bash
pnpm install
pnpm dev            # local server
pnpm test           # finder logic + data validation (Node test runner)
pnpm typecheck
pnpm build && pnpm preview --port 4173
python3 tests/e2e.py   # browser tests at phone + desktop sizes (needs Python Playwright)
pnpm refresh-data   # run the data robot locally (needs internet)
```

Finder code: `src/finder/` — `logic.ts` (all rules, tested), `location.ts`, `OysterMap.tsx` (Leaflet + custom markers), `OysterFinder.tsx` (UI), `finder.css`.
External services, all free and keyless: CARTO map tiles, Photon address search (OpenStreetMap), Google Maps for directions.
