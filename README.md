# World Bathymetry Explorer

**A browser-based bathymetry, nautical chart, contour and 3D globe explorer using public ocean data services.**

![Licence](https://img.shields.io/badge/licence-GPL--3.0--or--later-blue)
![Single File](https://img.shields.io/badge/build-none%20%E2%80%93%20single%20HTML%20file-green)
![No API Key](https://img.shields.io/badge/API%20keys-none%20required-brightgreen)

---

## Overview

World Bathymetry Explorer is a zero-install, single-file HTML application that lets you visualise ocean bathymetry, terrain elevation, nautical charts and contour data from public sources. It runs entirely in the browser with no build step, no server, and no API keys.

Open the file. Explore the ocean floor.

---

## Features

### Map Projections

| Mode | Projection | Notes |
|------|-----------|-------|
| **2D Map** | Web Mercator (EPSG:3857) | Full XYZ tile stack, deep zoom (z 0–20), standard web mapping |
| **2D Map** | Plate Carree (EPSG:4326) | WMS-only layers, poles visible, max zoom ~8 |
| **3D Globe** | Cesium 3D | CesiumJS loaded on demand, camera tilt/rotate, terrain draping |

The application **auto-transitions** between Mercator and Plate Carree based on zoom level:

- Zoom out to z ≤ 3 → switches to Plate Carree (global screening view)
- Zoom in to z ≥ 5 → switches to Mercator (detail view with full tile layers)
- Zoom 4 is a hysteresis band (no switch) to prevent flickering

### Bathymetry and Overlay Layers

- **GEBCO 2024** — global bathymetry colour-shaded raster (WMS)
- **GEBCO contour lines** — isobath contours at multiple intervals (XYZ tiles)
- **NOAA BAG hillshade** — high-resolution survey bathymetry (WMS)
- **NOAA/NCEI DEM hillshade** — coastal digital elevation models (WMS)
- **ETOPO 2022** — combined topo-bathy elevation model (ERDDAP)
- **EMODnet** — European marine bathymetry (WMS)
- **OpenSeaMap** — nautical seamark overlay (XYZ tiles)
- **Esri Ocean Reference** — labels, boundaries, place names (XYZ tiles)
- **Esri Ocean Basemap** — stylised ocean base layer
- **CARTO dark/light** — minimal cartographic basemaps
- **Esri Street, Satellite, Hybrid** — general-purpose basemaps
- **ArcGIS ENC** — Electronic Nautical Charts rendered via Esri (XYZ tiles)

### Point Depth / Elevation Probe

Click anywhere on the 2D map to query depth or elevation. The probe uses a multi-source fallback stack:

1. **NOAA BAG** (survey-grade, where available)
2. **NOAA/NCEI DEM** (coastal high-resolution)
3. **GEBCO WMS** GetFeatureInfo (global coverage)
4. **ETOPO 2022** via ERDDAP CSV (global fallback)

Results are displayed in a depth card showing the value, source, and coordinates.

### Elevation / Bathymetry Profile (Transect Tool)

1. Click **Profile** in the control bar
2. Click point **A** (start) on the map
3. Click point **B** (end) on the map
4. The tool samples ~81 elevation points along the transect line

The profile panel displays:

- **Canvas cross-section chart** with gradient terrain fill, sea-level reference line, grid, and axis labels
- **Distance** in kilometres, nautical miles, and metres (Haversine great-circle formula)
- **Min / max elevation** and total elevation range along the transect
- Sampling progress indicator and fallback interpolation for failed points

Elevation data is sampled from GEBCO WMS GetFeatureInfo (primary) with ETOPO ERDDAP CSV as fallback.

### Navigation Controls

A Google Maps-style control stack on the right side of the screen:

| Control | Function |
|---------|----------|
| **Compass** | Rotates the map 90 degrees per click (0 → 90 → 180 → 270 → 0). Needle always points to geographic north. Works in both 2D (CSS rotation with coordinate un-rotation) and 3D (Cesium camera heading). |
| **Geolocate** | Uses browser geolocation to fly to your position. Drops a blue marker with accuracy circle on the map. |
| **Zoom +/-** | Zoom in/out. Works in both Leaflet (2D) and Cesium (3D) modes. Replaces the default Leaflet zoom control. |

### Layer Opacity Controls

Dedicated sliders for:

- **Bathymetry** layer opacity (0–100%)
- **Contour** overlay opacity (0–100%)
- **Nautical** overlay opacity (0–100%)

### UI Presets

Quick-apply configurations for common use cases:

| Preset | Description |
|--------|-------------|
| **50/50 blend** | Balanced bathymetry + basemap |
| **Contours** | Chart-style contour emphasis |
| **Dive detail** | Maximum bathymetry + nautical overlay |

### Search

- **Place name search** via OpenStreetMap Nominatim geocoding
- **Direct coordinate entry** (e.g. `-32.05, 115.74` or `32°03'S 115°44'E`)
- Fly-to animation on selection

### Additional Features

- **Quick view bookmarks** — jump to predefined locations of interest
- **Minimap** — overview inset map (Mercator mode only)
- **Font scaling** — adjustable UI text size
- **Hide UI** — collapse all controls for a clean full-screen map
- **Responsive design** — mobile-optimised layout with 44px minimum touch targets and safe-area-inset support for notched devices

---

## Getting Started

No installation, no build, no server required.

```
1. Download  worldbathymetrymap_v2.html
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. Explore
```

The application loads all map tiles, WMS services, and elevation data from public endpoints on the internet. An active internet connection is required.

### Requirements

- A modern web browser with JavaScript enabled
- Internet connection (for tile and WMS data services)
- No API keys needed

---

## Data Sources and Attribution

| Source | Data | Licence / Terms | Link |
|--------|------|----------------|------|
| GEBCO | Global bathymetry grid, contours, WMS | GEBCO Terms of Use | [gebco.net](https://www.gebco.net/) |
| NOAA NCEI | BAG surveys, DEM hillshades | Public domain (US Gov) | [ncei.noaa.gov](https://www.ncei.noaa.gov/) |
| ETOPO 2022 | Global topo-bathymetry grid | Public domain (US Gov) | [ERDDAP](https://coastwatch.pfeg.noaa.gov/erddap/) |
| EMODnet | European marine bathymetry | EMODnet Terms | [emodnet.ec.europa.eu](https://emodnet.ec.europa.eu/) |
| OpenSeaMap | Nautical seamarks | ODbL | [openseamap.org](https://www.openseamap.org/) |
| Esri | Basemaps (street, satellite, ocean) | Esri Master Licence | [esri.com](https://www.esri.com/) |
| CARTO | Dark/light basemap tiles | CARTO Terms | [carto.com](https://carto.com/) |
| OpenStreetMap / Nominatim | Geocoding, place search | ODbL | [openstreetmap.org](https://www.openstreetmap.org/) |
| ArcGIS | Electronic Nautical Charts (ENC) | Esri Terms | [nauticalcharts.noaa.gov](https://nauticalcharts.noaa.gov/) |
| Leaflet | 2D map library (v1.9.4) | BSD-2-Clause | [leafletjs.com](https://leafletjs.com/) |
| CesiumJS | 3D globe library (v1.103) | Apache-2.0 | [cesium.com](https://cesium.com/) |

Third-party map data, chart data, bathymetry services, tiles, libraries, and basemaps remain under their own licences and terms. See individual sources for details.

---

## Technical Notes

### Architecture

- **Single-file HTML application** (~95 KB) — all CSS, HTML, and JavaScript in one file
- **Leaflet 1.9.4** for 2D map rendering (loaded from CDN)
- **CesiumJS 1.103** for 3D globe (loaded on demand from CDN, only when 3D mode is activated)
- **No build tools** — no npm, webpack, or transpilation step
- **No API keys** — all services used are publicly accessible without authentication

### Auto Projection Switching

The 2D map mode intelligently switches between two projections:

- **Web Mercator** (EPSG:3857) at zoom ≥ 5 — supports the full XYZ tile stack (GEBCO contour tiles, OpenSeaMap, Esri overlays) and deep zoom for coastal detail
- **Plate Carree** (EPSG:4326) at zoom ≤ 3 — WMS-only layers, but shows the full globe including poles

A hysteresis band at zoom 4 prevents rapid switching. The transition preserves the current map centre and bearing.

### Map Rotation

Leaflet does not natively support map bearing/rotation. The implementation uses:

1. **CSS `transform: rotate()`** applied to the `.leaflet-map-pane` element
2. **Coordinate un-rotation patch** on `L.Map.prototype.mouseEventToContainerPoint` to translate screen coordinates back to geographic coordinates on the rotated map

This ensures clicks, panning, coordinate display, and all overlays work correctly at any bearing. The rotation animates with a 350ms cubic-bezier transition.

In 3D Globe mode, compass rotation controls the Cesium camera heading directly.

### Profile Tool Sampling

The transect tool samples elevation at ~81 evenly spaced points along the A→B line:

1. **Primary source:** GEBCO WMS GetFeatureInfo — queries the GEBCO visual layer for each point
2. **Fallback source:** ETOPO 2022 via ERDDAP CSV — grid query for the nearest cell
3. **Response parsing:** Uses the same `parseGebcoText()` function as the depth probe, which extracts elevation from keyword lines (`value`, `elevation`, `height`) in the WMS plain-text response
4. **Interpolation:** Failed samples are linearly interpolated from their nearest valid neighbours

Distance is calculated using the **Haversine formula** for great-circle distance on a spherical Earth (R = 6,371 km).

### Depth Probe Fallback Stack

The point depth query cascades through up to four sources:

```
NOAA BAG (survey-grade) → NOAA/NCEI DEM (coastal) → GEBCO WMS → ETOPO ERDDAP
```

Each source is tried in order; the first to return a valid numeric value is used. The depth card displays the source alongside the result.

---

## Browser Compatibility

Tested in:

- Google Chrome 120+
- Mozilla Firefox 120+
- Microsoft Edge 120+
- Safari 17+

Mobile browsers (iOS Safari, Chrome for Android) are supported with responsive layout and touch-optimised controls.

---

## Licence

Application code is licensed under the **GNU General Public License v3.0 or later** (GPL-3.0-or-later).

Third-party map data, chart data, bathymetry services, tiles, libraries, and basemaps remain under their own licences and terms.

See [LICENCE](LICENCE) for the full licence text.

---

## Disclaimer

**Not for navigation.** This is a screening and exploration tool. Use official nautical charts and products for navigation and safety-of-life decisions. The application licence does not re-license third-party map, chart, or bathymetry data.

---

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes to `worldbathymetrymap_v2.html`
4. Test in at least two browsers (desktop + mobile)
5. Submit a pull request with a clear description of the change

### Guidelines

- Maintain the single-file architecture — all code stays in one HTML file
- Use `var` and function declarations (no `let`/`const`/arrow functions) to match the existing code style
- Ensure all new UI elements meet the 44px minimum touch target on mobile
- Test at multiple zoom levels and in all three view modes (Mercator, Plate Carree, 3D Globe)
- Do not add dependencies that require API keys

---

## Project Structure

```
.
├── worldbathymetrymap_v2.html   # The application (single file)
├── README.md                    # This file
├── LICENCE                      # GPL-3.0-or-later
└── ATTRIBUTION.md               # Third-party data source attribution
```
