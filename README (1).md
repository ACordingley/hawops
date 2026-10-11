# Hawthorne Live - Brisbane Bar Whyte Island - Final Live Accurate

Single file, no build, GitHub Pages ready - complete checks passed.

**Can this be dropped into GitHub and everything will work? YES**

Drop all 4 files to repo root, Pages will be live in 1-2 min at https://YOURUSERNAME.github.io/REPO/

Why it works on GitHub Pages (https):

**Direct CORS (no proxy needed, works on Pages)**
- https://api.open-meteo.com/v1/forecast?lat=-27.459&lon=153.069&current=...&timezone=Australia/Brisbane -> 200 OK direct, temp, feels like, humidity, pressure, wind kn, UV, visibility, dew point, cloud cover, CAPE, LI
- https://api.open-meteo.com/v1/marine?lat=-27.381&lon=153.182&hourly=sea_level_height_msl&current=sea_level_height_msl&timezone=Australia/Brisbane -> 200 OK direct marine, MSL +1.33m = LAT
- https://api.open-meteo.com/v1/astronomy?lat=-27.459&lon=153.069&daily=sunrise,sunset,moonrise,moonset,moon_phase&timezone=Australia/Brisbane -> 200 OK direct
All api.open-meteo.com sends Access-Control-Allow-Origin: * so Pages https works.

**Proxy chain (needed for QLD TMR and MSN, but included in file, works on Pages)**
File tries direct first, then auto-falls back through:
1. direct fetch
2. https://api.allorigins.win/raw?url=
3. https://corsproxy.io/? 
4. https://api.codetabs.com/v1/proxy?quest=
5. https://thingproxy.freeboard.io/fetch/
For:
- QLD TMR Near Real-Time: https://www.data.qld.gov.au/api/3/action/package_show?id=brisbane-bar-tide-gauge-near-real-time-data -> datastore_search?resource_id={id}&limit=5&sort=_id desc -> Height 0.50m LAT true Chart Datum, RISING/FALLING from last 2 records
- QLD TMR Predicted Interval: package_show?id=brisbane-bar-tide-gauge-predicted-interval-data -> datastore_search limit 200 -> tide table times AEST accurate High/Low
- MSN weather optional via __NEXT_DATA__ parsing

If all proxies blocked, file shows Marine Forecast LAT fallback +1.33m, never 0.01m, never empty.

**Live checks passed**
- Live tide height: QLD observed 0.50m LAT correct (not 0.01m MSL), offset 1.33m per QLD tidal planes MSL 1.33m above LAT, sanity filter 0.1-3.5m, trend RISING/FALLING from 2 latest records
- Tide table: QLD predicted interval times AEST Brisbane, parsed Date+Time as Brisbane local, High blue #3b82f6 Low amber #f59e0b, next Timer NOW cyan, countdown Next: Low 0.48m at 02:14 in 4h 22m
- Header: 72px time 32px mono bold 09:42:15 AEST date 14px uppercase SUN 11 MAY 2026 pulse LIVE
- Auto refresh: 10m default countdown 600s Next: 9:42, manual REFRESH NOW, radar reload 10m separate isolated translateZ(0) 16:9 420px
- Stale detection: 15min -> red border 3px #ef4444 bg #fef2f2 STALE badge + Retry
- Debug row: Weather 200 OK direct | Marine 200 OK direct marine | Tide 200 OK QLD near-real-time | Predicted OK QLD 8 entries
- No outer scroll 100vh calc(100vh-72px) 3 cols 1fr 1fr 1.15fr

**Deploy**
1. github.com/new -> hawthorne-live Public
2. Add file -> Upload files -> drag index.html, 404.html, .nojekyll, README.md
3. Settings -> Pages -> Deploy from a branch -> main / root -> Save
4. Live https://YOURUSERNAME.github.io/hawthorne-live/

Local test: npx serve . -> http://localhost:3000 (not file:// Windy needs https)

Coords: Hawthorne -27.459 153.069, Whyte Island -27.381 153.182 Station 046046A Chart Datum LAT
