# Decisions
Mainly for myself

## 2025-05-09 — Chose Astro over plain HTML
Plain HTML requires manually maintaining every page by hand.
Astro gives content collections, markdown handling, typed schemas,
and compiles to identical static files for GitHub Pages.
Rejected: Next.js (too heavy for a static blog), plain HTML (doesn't scale).

## 2025-05-09 — No page-flip animation as primary reading mode
Rejected: StPageFlip turn.js entirely.
Turn.js and StPageFlip require fixed page dimensions which breaks
Tufte sidenotes and constrains Plotly chart embeds.
Decision: scroll with excellent typography as primary reading mode.
Subtle page-turn transition between articles only, not within them.

## 2025-05-09 — PNG sprites over canvas palette rendering
Rejected: canvas pixel rendering, SVG pixel art.
TabbyCat extension itself uses PNG sprites — validated by source inspection.
Canvas palette approach (Game Boy style) requires building a mini render engine
before drawing a single element. Too much infrastructure for V1.
Revisit in V2 if palette-swapping for seasons becomes painful.

## 2025-05-09 — Time-based lighting, randomised weather and season
Time of day via getHours() — morning, afternoon, sunset, evening/night.
Weather randomised per visit from: sunny, light rain, thunderstorm etc.
Season randomised but changes slowly, less frequent randomized (implementation to be discussed)
No external API needed. No location data. No privacy considerations.
Rejected: Open-Meteo live weather (adds API dependency and location tracking).
Earlier Rejected: OpenWeatherMap (requires API key), WeatherAPI (rate limits).

## 2025-05-09 — Option B for mobile (horizontal scroll)
One set of pixel art assets serves both desktop and mobile.
Keep the landscape room on mobile, let user pan horizontally.
overflow-x: scroll on scene container, room wider than the mobile viewport.
Deferred: portrait room variant (doubles art workload, revisit in V2).

## 2025-05-09 — Season applied client-side via localStorage
Astro pre-renders HTML at build time with no browser context.
Season cannot be known at build time — must be applied by JS after load.
Solution: preload all season backgrounds in CSS, hide scene until JS runs,
apply correct season class, then reveal. Eliminates flash of wrong season.
localStorage stores current season and timestamp it was set.
Re-randomises after 21 days. Strings only — numbers stored via String()
and retrieved via Number().
Caveat: localStorage is per-browser, not per-user. Different devices
get independent season rolls — acceptable for a purely aesthetic system.

## 2025-05-09 — Loading screen over scene while assets download
Loading screen is pure HTML and inline CSS — loads instantly, no assets needed.
Covers scene while season backgrounds and pixel art download in background.
Uses window.addEventListener('load') not DOMContentLoaded —
'load' fires only when ALL assets are fully downloaded, not just HTML parsed.
Safety timeout of 8 seconds: if assets not loaded, reveal scene anyway.
Prevents punishing visitors on very slow connections with infinite wait.
Loading screen aesthetic: atmospheric text cycling on dark warm background.
"Lighting the lamp...", "The cat finds a spot..." etc.
No spinner — fits the world, works with zero assets, pleasant on slow connections.