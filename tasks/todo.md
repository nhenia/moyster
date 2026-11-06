# Moyster — finder v2 (branch `feature/finder-v2`)

Goal: a tourist finds an oyster spot in under 60 seconds.

## Milestones
- [x] Audit repo (Vite + React PWA on GitHub Pages, art journal + finder, 22 venues, 17 missing coordinates)
- [x] Pure logic module + tests (NOLA time, happy hour now/next, price per oyster, walking minutes, ranking)
- [x] Location: auto-request on open, results shown immediately from a labelled default, address search fallback
- [x] Four modes: Near Me / Happy Hour / Cheapest / Map + three instant picks
- [x] Real map with custom oyster markers, clustering, map/list sync
- [x] Share sheet, saved spots (kept), walking directions, data freshness flags
- [x] Data refresh script + scheduled GitHub Action (geocode missing addresses, check source links)
- [x] Error recovery (error boundary, geolocation/geocoder/tile failures, offline cache)
- [x] Verify: type-check, unit tests, build, mobile + desktop screenshots

## Notes / lessons
- Sandbox can't reach map tiles or address search; browser tests mock those. Real geocoding of the 17 unmapped venues happens in the GitHub Action on push.
- Leaflet only requests ~4 tiles in the small phone map, so "tiles failed" must trigger at 3 errors, not 7.
- Pre-existing: 15px sideways scroll from marquee/posters (fixed with `overflow-x: clip` on html), type error in App tilt handlers (fixed).
- Intentionally dropped from v1 finder: free-text search and raw/chargrilled/today filters (brief: no complicated filters). Saved spots kept (same storage key).
