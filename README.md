# Real Estate Investment Radar v2.1

Static GitHub Pages dashboard for preliminary Istanbul investment screening.

## Reliability fixes
- Fixed startup crash: Array.map previously passed its index as the profile argument.
- PASS overrides archival decisions for excessive purchase budget, failed title/structure, and recorded completion, transaction cost, management fee or USD drawdown violations.
- Missing legal, structural, economic, developer and scenario checks prevent BUY and NEGOTIATE. Current unverified catalog entries are WATCH or PASS.
- Decisions and decision filters now recalculate with the applied profile.
- Removed district-name bonuses for growth, location and personal use. Unknown evidence earns no positive score.
- Age is informational, not a building safety proxy.
- Invalid form ranges and zero total weight are rejected.
- Empty results, unavailable photos, blocked map dependencies and stale asynchronous map requests have explicit handling.

## Evidence and scoring
The embedded catalog remains an archival snapshot; changing the profile does not fetch market prices, rental comparables or FX rates. Price, rent, yields and Fair/Good/Excellent values are archived estimates, not newly verified valuations. Property assertions are not independently confirmed by this application.

Structured fields used by the screening logic:
- legal_status, structural_status, economics_status: verified / failed / unknown (absent means unknown)
- developer_status: verified / unknown-small / unknown
- completion_months, transaction_cost_pct, management_pct, drawdown_usd_pct
- liquidity_months, growth_usd_pct, growth_horizon_years
- infrastructure_verified, gym, pool, green, water, asian: explicit booleans
- data_checked: date; BUY/NEGOTIATE require a check within seven days

Verified flags must only be supplied after external review; they are not substitutes for evidence. No current catalog item has these verifications. Preference fit is a conservative completeness-sensitive heuristic, not probability of investment success. Area and floor are preferences; purchase budget is a hard cap. No structural or title failure can be overridden by preference switches.

## Maps
The optional Google Maps browser API key is stored in localStorage. It is publicly visible to browser scripts and is not a secret or user login.
1. Enable Maps JavaScript API and billing in Google Cloud.
2. Restrict the key to the website's HTTP referrers and Maps JavaScript API.
3. Save the key in the profile tab. Saving/replacing/removing reloads the page so an old SDK cannot impersonate a newly validated key.
4. Google authentication failure, load failure or timeout falls back to Leaflet/Esri.
5. If Leaflet is unavailable, a clear error is shown. Property details also provide an external map link.

Removing a local key does not revoke it in Google Cloud. Google Cloud restrictions and billing cannot be verified from this repository.

## Search, profile and privacy
The research button generates a copyable research brief. It does not perform live internet search. Automated market ingestion needs an external service/backend and licensed or otherwise permitted data access; it is not implemented here.

There are no accounts, sessions or private routes. The catalog and repository are public. Profile edits are device-local and do not synchronize with the source Google Doc. No custom server or private API token is present.

## Validation
Run `node tests/regression.cjs`. The dependency-free tests exercise JavaScript boot, catalog rendering logic, decisions, invalid ranges, empty results and absent/deleted/rejected map keys in a simulated DOM. They do not replace real browser rendering or live Google Cloud checks.

## v2.2: distances, surroundings and scheduled updates
- Cards show approximate straight-line distances to the closest known promenade, metro and transport point in the available POI dataset. Missing points are explicitly unknown; the dataset is not exhaustive.
- The bootstrap waterfront reference is the named Bostancı İDO terminal at the coast. Its distance is NOT the shortest distance to the sea or the nearest promenade. Additional named coastal walking paths depend on Overpass availability.
- Property details add park/leisure distances and Google Maps walking directions. Listing pins are generally approximate; computed distance is not an entrance-to-entrance route.
- Nine referenced bootstrap POIs load without a data API. OpenStreetMap Overpass adds named parks, stations, terminals, malls, theatres/cinemas and named coastal pedestrian paths. Public Overpass may fail or throttle: keep bootstrap points and show the failure. Local POI cache expires after seven days. No fabricated coordinates.
- Leaflet supports per-category layers; Google has corresponding colored markers and global category checkboxes. Detail maps include POIs within 5 km and initially fit those within 3 km.
- POI sources are linked from map popups. OpenStreetMap data is © OpenStreetMap contributors under ODbL; see https://www.openstreetmap.org/copyright .
- projects.json is now the primary catalog. The embedded P array is an offline fallback. Keep both synchronized after validated research; preserve complete record fields and photos. The page refetches projects.json without cache on load and every five minutes.
- External ChatGPT task Real Estate Radar Daily (6aa2b3cea6848191b53311e4d71c74c4) is enabled for 10:00 Asia/Tashkent, starting September 12, 2026. It researches listings and publishes evidence-backed repository updates. This is the start of research, not a guaranteed publication completion time.
- update-meta.json records run state. Never advance last_success_at or per-listing data_checked solely because a scheduled run occurred. On unavailable sources retain the prior catalog with an explicit failed/partial state.
- For future updates read the actual profile and current schema; do not delete galleries, map layers or risk guards. Never equate a failed HTTP request with a withdrawn listing.
