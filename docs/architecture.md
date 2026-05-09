# Howfishy's Bookshelf — Architecture

## What this is
A personal blog built as an interactive 2D illustrated room.
Visitors click books on shelves to open collections of writing.

## Tech stack
- Astro (static site generator, content collections, markdown)
- Vanilla JS (no browser frameworks)
- GitHub Pages + GitHub Actions (hosting and deployment)
- Tufte CSS (typography and margin sidenotes)


## Scene layer system (back to front, z-index order) 
(not in order yet)
1. Window view — sky/outside, swapped per weather via CSS class
2. Room background — static walls, shelves, furniture
3. Book hotspots — clickable divs, map to Astro content collections
4. Weather FX — rain, snow, lightning, pointer-events: none
5. Cat sprite — PNG sprite sheet, 16x16 logical pixels
6. UI overlays — tooltips, hotspot labels

## Canvas
320x180 logical pixels, scaled 4x to 1280x720 for display.
image-rendering: pixelated on all assets.
All layers are identical dimensions with transparent backgrounds —
they share the same bounding box and stack via position: absolute; inset: 0.

## Mobile
Landscape room preserved on mobile. Visitor pans horizontally.
scene container: overflow-x: scroll, room wider than mobile viewport.
Single set of pixel art assets serves both desktop and mobile.
Portrait room variant deferred to V2.

## Scene / atmosphere system
Time of day: getHours() → morning, afternoon, sunset, evening, night.
Weather: randomised per visit — sunny, light rain, thunderstorm.
Season: randomised but slow (re-randomised only after several visits or several weeks?)
Feels like real seasonal drift.
No external APIs, no location data, fully client-side.

## Loading and reveal sequence
1. Loading screen renders immediately — pure HTML and inline CSS, no assets
2. Browser fetches all season backgrounds and pixel art in background
3. window.load fires when ALL assets are ready
4. JS reads localStorage → determines season, time of day, weather
5. Correct classes applied to hidden scene
6. Loading screen fades out, scene fades in (300ms delay feels intentional)
7. Safety timeout: scene revealed after 8 seconds regardless of load state

## localStorage usage
Key: 'season' → string ('spring', 'summer', 'autumn', 'winter')
Key: 'season-set-at' → string (Date.now() timestamp as string)
Re-randomised after 21 days (1000 * 60 * 60 * 24 * 21 milliseconds)
All localStorage access must be inside browser-side script tags —
never in Astro component frontmatter (no browser context at build time).

## Content collections
Each folder in src/content/ = one physical book on the shelf.
Each .md file inside = one entry within that book.
Schema defined in src/content/config.ts using Zod.

## Planned collections
- weekly-reads
- ideas
(other areas)
    - plasmonics
    - minecraft
    - bloomberg-jealousy-list
    - drone
    - simulation
    - music
    - video
    - economics

## Reading experience
Paul Graham-style prose. Margin sidenotes via Tufte CSS.
Plotly charts exported as self-contained HTML, embedded via iframe.
No page-flip animation as primary mode — scroll with good typography.
Subtle page-turn transition between articles only.

## Cat
PNG sprite sheet. Click-reactive animation.
Inspired by TabbyCat extension — credits given on site.
Frames: idle (4), blink (3), react (6), walk (8 for later). (not sure  yet)