# Kahai Dashboard — IT101 Dashboard

A personal Chiang Mai dashboard, submitted as the teacher-approved alternative to the CV lab.

**Website:** https://kahai25.github.io/kahai-dashboard/

## Features
- Current weather, tomorrow and seven-day forecasts.
- Chiang Mai and Mobile clocks, daylight, sunrise and sunset.
- USD/THB/MMK reference-rate conversion and modeled PM2.5/US AQI.
- External Google Maps traffic view.
- Around Chiang Mai: USGS earthquake reports in the past 24 hours, filtered to 250/500/1,000 km and by magnitude. Relative-distance plot plus up to eight newest report links.

All HTML, CSS, JavaScript, and AI-created decorative artwork are embedded in index.html. No installation or build step is required. GitHub Pages publishes main / (root).

## Data sources and limits
- Weather: [Open-Meteo](https://open-meteo.com/).
- Air quality: CAMS through Open-Meteo; modeled values, not a local sensor.
- Daily reference rates: [ExchangeRate-API](https://www.exchangerate-api.com/); MMK local cash rates may differ.
- Earthquakes: [USGS GeoJSON feed](https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php). Reports can be delayed or revised. Coverage varies by region; no matching reports is not a guarantee of no earthquakes. Not an emergency alert service.
- Traffic opens Google Maps; vehicle locations and congestion are not simulated.
- Hero scenery is AI-created, not a live view.

## Reliability
The page distinguishes updated, saved, delayed, loading and unavailable readings. Provider observation timestamps and download timestamps are validated separately. All dependent values clear when no usable data remains. Forecasts keep seven dated slots when coverage is incomplete.

| Source | Normal polling | Delayed after | Removed after |
|---|---|---|---|
| Weather | 10 minutes | 2 hours | 6 hours |
| Air quality | 15 minutes | 2 hours | 6 hours |
| Daily exchange rates | 1 hour | 36 hours | 72 hours |
| USGS feed snapshot | 5 minutes | 15 minutes | 1 hour |

These are dashboard display policies, not promises about source publication frequency. Earthquake event times are filtered independently to the trailing 24 hours. Saved readings show their source timestamp and age; failed refreshes are labeled on every affected panel.

Each source has its own in-memory schedule, so blocked localStorage does not cause a polling loop. Failures use capped exponential backoff with jitter and honor browser-accessible Retry-After headers up to one hour. Repeated manual requests are throttled. Hidden pages pause periodic work and recheck on return. The original v1 cache is ignored to avoid reusing poorly validated readings.

## Tests
With Node.js, Playwright, and Microsoft Edge available, run `node tests/regression.cjs`.
Tests use intercepted synthetic feeds, not real earthquake reports: expired/future data, failed refreshes, seven-day gaps, regional and magnitude filtering, unsafe provider text/links, browser storage failure, retries/rate limits, currency validation, responsive widths, and local midnight rollover.

## Scope
The regional section is an original implementation against the same USGS source used by God's Eye View. No God's Eye View code, credentials or bundled data is copied. This is not the full 3D globe. Aircraft, live fire feeds and Windows tray/lock-screen integration are not included.
