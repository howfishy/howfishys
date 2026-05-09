# Known issues and considerations

## Planned — loading screen
Status: not yet implemented
A loading screen must sit above the scene while season background
images and pixel art assets download. Prevents flash of unstyled scene.
Use window.load not DOMContentLoaded — fires only when all assets ready.
Safety timeout of 8 seconds in case of very slow connections.
See decisions.md for full reasoning.

## Planned — season flash prevention
Status: not yet implemented
Scene must be hidden (visibility: hidden) until JS has read localStorage
and applied the correct season class. Otherwise visitor briefly sees
the default season before JS runs. Loading screen covers this gap.
All season background images preloaded in CSS so switching is instant.

## Planned — localStorage per-browser limitation
Status: accepted, will not fix
localStorage is scoped per browser, not per user.
Visiting on a different device gives a fresh season roll.
Acceptable for a purely aesthetic system — noted here for awareness.

## Planned — Astro build time vs localStorage
Status: known constraint, architecture accounts for it
Astro pre-renders at build time with no browser context.
localStorage unavailable during build — all season/weather/time logic
must live in client-side script tags, not Astro component frontmatter.

## Planned — mobile horizontal scroll
Status: not yet implemented
Scene wider than mobile viewport, overflow-x: scroll on container.
Scroll snap points to be tested on real devices — behaviour varies
between iOS Safari and Android Chrome.