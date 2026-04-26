# Golf Club Planner — Project Context

## What This Is
A single-file web app (`index.html`, ~821 lines) that helps golfers plan club selection for a round. Golf-green color scheme.

## Current Features (as of 2026-04-26)
1. **Step 1 — Course Info**: Search courses via Nominatim/OpenStreetMap. Auto-fetches GPS coordinates + per-hole distances (haversine) and bearings via Overpass API. Fetches elevation data per hole. Curated tee yardage scaling (Black/Blue/White/Gold/Red). Multiple Overpass endpoints with failover.
2. **Step 2 — Weather**: Auto-fetch via Open-Meteo (forecast or historical archive). Shows temp (°F), wind speed (mph), wind direction. Per-hole wind condition (headwind/tailwind/crosswind/calm) calculated from hole bearing + wind direction. Auto-fills and grays out section; "Edit manually" override button re-enables it.
3. **Step 3 — Bag**: User enters club name + max distance. Saved to localStorage. Defaults provided. Reset to defaults button.
4. **Step 4 — Scorecard**: Two input modes:
   - Paste text → regex extracts 18 yardages (80–749 yard range filter)
   - Upload photo → Tesseract.js OCR (lazy-loaded) → auto-parses
   - Manual entry in hole table as fallback
5. **Step 5 — Shot Physics**: Sliders/inputs for roll %, elevation effect %. Adjustments applied: temperature factor (`1 + (F-70)/1000`), wind factor (headwind max -50%, tailwind max +40%), elevation (shot 1 only), roll.
6. **Results**: Round summary stats grid, shot-by-shot table (sticky headers, per-hole weather header, club override dropdown), Club Usage Histogram (bar chart, pure CSS/DOM).
7. **Main Tab Bar**: 4 tabs — Planner, Holes, Weather, About.
8. **Hole Diagrams tab**: Leaflet.js mini-maps per hole (lazy-loaded), Google Maps satellite deep-links, elevation badge, par/distance info bar.
9. **Weather Charts tab**: Visual charts for per-hole temperature and wind conditions.
10. **About tab**: Formula breakdowns, data source table, physics explanation.
11. **Floating TOC**: Fixed right-side nav with scroll spy; hides on mobile (< 1100px).

## Architecture
- Pure vanilla HTML/CSS/JS, no build step, no dependencies except Tesseract.js (lazy-loaded on demand)
- External APIs: Nominatim (course search), Overpass (hole geometry), Open-Meteo (weather + elevation)
- State: `courseLat/Lon`, `holeResults[]`, `overrides{}` (club override per hole shot), localStorage for bag
- Key functions: `computeShots()`, `generateRound()`, `renderShotTable()`, `renderHistogram()`, `fetchWeather()`, `fetchElevations()`, `selectCourse()`

## What to Keep in Mind
- All in one file — keep it that way unless user asks to split
- No frameworks, no build tools — keep it simple
- The user wants the app to work well on mobile too (responsive CSS already in place)

## Potential Next Steps (unknown — ask user)
- Unknown what was being worked on when tokens ran out last session
- Check with user what feature/fix they want to continue
