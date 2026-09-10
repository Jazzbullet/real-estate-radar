# Real Estate Investment Radar v2.0

Static GitHub Pages dashboard for Istanbul investment screening.

## What changed
- Objects / Map / Investment Profile tabs
- Editable Investment Profile 1.0, saved in localStorage
- Live re-ranking of the current catalog with Profile Fit
- New-market search brief generator
- Original-listing photo galleries where the source exposes usable image URLs
- Google Maps JavaScript API integration with secure client-side key entry
- Leaflet/Esri satellite fallback when no Google key is configured
- Privacy and Terms pages

## Google Maps
Do NOT commit an unrestricted key. In Google Cloud:
1. Enable Maps JavaScript API.
2. Enable billing for the project.
3. Create a browser API key.
4. Application restriction: Websites / HTTP referrers.
5. Allow the GitHub Pages origin/domain for this site.
6. API restriction: Maps JavaScript API only.
7. Enter the key in the dashboard's Investment Profile tab. It is stored only in that browser's localStorage.

For a key intended to work automatically for every visitor, a website-restricted browser key can be embedded in site config, but it will be visible client-side by design and must be tightly restricted.

## Search behavior
"Apply and recalculate" re-scores the 18 stored listings client-side.
"New market search" generates a precise research brief. A static GitHub Pages site cannot securely crawl the live market or call private AI/search APIs without a backend.
