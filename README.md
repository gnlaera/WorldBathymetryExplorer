# World Bathymetry Explorer

**A browser-based bathymetry, contour, nautical overlay, point-probe and 3D globe explorer using public ocean data services.**

![Licence](https://img.shields.io/badge/licence-GPL--3.0--or--later-blue)
![No Build](https://img.shields.io/badge/build-none-green)
![No API Key](https://img.shields.io/badge/API%20keys-none%20required-brightgreen)
![Architecture](https://img.shields.io/badge/app-two%20files-blue)

---

Live version: https://gnlaera.github.io/WorldBathymetryExplorer/

## Overview

World Bathymetry Explorer is a zero-install web application for exploring global bathymetry, coastal elevation, marine contours, nautical overlays and 3D globe context.

It runs entirely in the browser. There is no backend, no build step, no package manager and no private API key.

The release application is intentionally simple:

```text
index.html
app.js
```

Open `index.html` in a modern browser, or deploy the two files to a static host such as GitHub Pages.

**Not for navigation.** This is a screening, exploration and context tool. Use official nautical charts and approved products for navigation, marine operations and safety-of-life decisions.

---

## Key Features

### 2D Map and 3D Globe

| Mode | Technology | Purpose |
|---|---|---|
| **2D map** | Leaflet, Web Mercator | Main working view for bathymetry, contours, overlays, search, point probes and profiles |
| **3D globe** | CesiumJS, loaded on demand | Global context, terrain-aware visualisation and rotated globe exploration |

Plate Carree mode has been removed. The product now keeps the normal user experience focused on the 2D Web Mercator map and the optional 3D globe.

---

## Data Layers

World Bathymetry Explorer combines public map, bathymetry and marine data services.

### Bathymetry and elevation

| Layer | Description |
|---|---|
| **GEBCO bathymetry and elevation colour relief** | Global bathymetry and land elevation visual layer |
| **NOAA BAG** | High-resolution surveyed bathymetry where coverage exists |
| **NOAA/NCEI DEM** | Coastal digital elevation model coverage where available |
| **ETOPO 2022** | NOAA global topo-bathymetry fallback grid for context |

### Contours and marine overlays

| Layer | Description |
|---|---|
| **GEBCO / NOAA/NCEI depth contours** | Indicative depth contours derived from GEBCO 2019 contours |
| **OpenSeaMap seamarks** | Nautical seamark overlay |
| **NOAA ENC / Esri nautical chart layers** | Electronic nautical chart-style reference layers where available |
| **EMODnet bathymetry** | European marine bathymetry context |
| **USGS / Esri tectonics** | Plate boundary and tectonic reference overlays |

### Basemaps

| Basemap | Description |
|---|---|
| **Map** | General-purpose reference map |
| **Satellite hybrid** | Satellite imagery with labels |
| **Ocean chart** | Ocean-focused reference basemap |
| **CARTO / OpenStreetMap-derived basemaps** | Light and dark contextual mapping where enabled |

All third-party data remains subject to the relevant provider terms, licences and service availability.

---

## Default View

On first load, the app starts in 2D map mode with:

```text
Bathymetry: GEBCO colour relief
Contours: GEBCO / NOAA/NCEI indicative depth contours
Nautical overlays: off
```

The default internal active state is:

```javascript
aB: ["gebcoFlat"]
aC: ["gebcoContours"]
aN: []
```

This keeps the first-load experience focused on global bathymetry plus indicative contours without immediately increasing nautical overlay request volume.

---

## Contours

The default 2D contour layer uses the NOAA/NCEI GEBCO 2019 contour MapServer export service where available. This avoids relying only on overzoomed cached raster contour tiles at close zoom levels.

A cached GEBCO contour service may be used as a safety fallback if the dynamic export layer fails or returns blank tiles.

In the 3D globe, contours continue to use the cached contour provider with limited native detail. This keeps the globe stable without adding a large Cesium contour rewrite.

Contour caveats:

- Contours are indicative.
- They are not official chart contours.
- They may differ from the current GEBCO bathymetry colour-relief visual layer.
- They are not suitable for navigation or operational clearance decisions.

---

## Point Depth and Elevation Probe

Click the 2D map to query a point depth or elevation.

The probe uses a conservative fallback stack:

```text
NOAA BAG → NOAA/NCEI DEM → GEBCO → ETOPO 2022
```

The first valid numeric result is displayed in the depth card.

Where available, the result includes:

| Field | Meaning |
|---|---|
| **Value** | Depth or elevation at the clicked point |
| **Source** | Data service that returned the value |
| **Product** | Survey, DEM, grid or product name where returned |
| **Datum** | Vertical datum where available |
| **Resolution** | Cell size or approximate product resolution where available |
| **Confidence** | Practical confidence level based on source type |
| **Caveat** | Source-specific limitation and navigation warning |

Important interpretation points:

- NOAA BAG is high-resolution surveyed bathymetry where coverage exists.
- BAG and chart-derived products may use chart datums such as Mean Lower Low Water depending on source.
- NOAA DEMs may expose attributes such as DEM name, vertical datum, cell size and metadata URL.
- GEBCO is a global gridded product, not a surveyed spot depth.
- ETOPO 2022 is a 15 arc-second NOAA global grid using EGM2008 height and should be treated as context only.
- Values from different products and datums should not be treated as directly comparable.

---

## Profile Tool

The profile tool samples a transect between two points.

Workflow:

1. Select **Profile**.
2. Click point **A** on the map.
3. Click point **B** on the map.
4. Review the sampled bathymetry / elevation profile.

The profile panel shows:

- Cross-section chart
- Distance in kilometres and nautical miles
- Deepest valid sample
- Shallowest valid sample
- Highest valid sample where above sea level
- Source contribution summary
- Confidence warnings where fallback or missing samples are material

Profile results are based on valid numeric samples only. If too many samples rely on fallback data or return no numeric value, the app surfaces a warning rather than implying false precision.

---

## Search

Search supports two modes.

| Search type | Behaviour |
|---|---|
| **Coordinates** | Direct coordinate entry, handled immediately in-browser |
| **Named places** | OpenStreetMap Nominatim geocoding, throttled and cached in memory |

Examples:

```text
-31.9505, 115.8605
32°03'S 115°44'E
Perth, Western Australia
Mariana Trench
```

Named-place search is intentionally simple. There is no autocomplete and no background search loop. This keeps request volume low and respects public geocoding service limits.

Once named-place search is used, the attribution badge includes OpenStreetMap Nominatim.

---

## Controls

### Main controls

| Control | Function |
|---|---|
| **2D map** | Return to the main Leaflet map |
| **3D globe** | Switch to Cesium globe mode |
| **Hide UI** | Collapse interface controls for a cleaner view |
| **Search** | Fly to coordinates or named places |
| **Layers** | Toggle bathymetry, contour, nautical and reference overlays |
| **Quick view** | Jump to predefined areas of interest |
| **About data** | Review source and data caveats |

### Map controls

| Control | Function |
|---|---|
| **Compass** | Rotates the 2D map or 3D camera heading |
| **Geolocate** | Uses browser geolocation where permitted |
| **Zoom +/-** | Zooms the active view |
| **Profile** | Starts transect sampling |
| **Clear** | Clears active drawings, markers or overlays where applicable |

### Sliders

| Slider | Function |
|---|---|
| **Bathymetry** | Adjusts bathymetry layer opacity |
| **Contours** | Adjusts contour layer opacity |
| **Nautical** | Adjusts nautical overlay opacity |
| **Font size** | Adjusts interface text scaling |

---

## Presets

| Preset | Purpose |
|---|---|
| **50/50 blend** | Balanced bathymetry and basemap visibility |
| **Chart-style contours** | Emphasises contour interpretation |
| **Dive-detail preset** | Prioritises bathymetry and nautical overlays |
| **Tectonics on** | Adds tectonic context |
| **Plate boundary view** | Shows tectonic plate boundaries, not a map projection mode |
| **Clear overlays** | Resets optional overlays |

---

## Attribution

The app includes an active attribution badge that updates based on visible and recently used services.

Attribution may include, depending on active layers and actions:

- GEBCO
- NOAA / NCEI
- NOAA BAG
- NOAA ENC
- ETOPO 2022
- Esri
- OpenSeaMap
- EMODnet
- CARTO
- OpenStreetMap
- OpenStreetMap Nominatim
- USGS / Esri tectonics
- Leaflet
- CesiumJS

The application code is licensed separately from the data. Third-party map data, bathymetry grids, chart services, tiles, basemaps and JavaScript libraries remain under their own licences and terms.

---

## Getting Started

### Option 1: Open locally

```text
1. Download the release ZIP.
2. Extract index.html and app.js into the same folder.
3. Open index.html in a modern browser.
```

An active internet connection is required because the app streams public map, tile, Web Map Service and elevation data from external services.

### Option 2: Deploy with GitHub Pages

```text
1. Put index.html and app.js in the repository root.
2. Commit and push to GitHub.
3. Open repository Settings.
4. Go to Pages.
5. Select the main branch and root folder.
6. Save.
```

GitHub Pages will serve `index.html` as the application entry point.

---

## Requirements

- Modern browser with JavaScript enabled
- Internet connection
- WebGL-capable browser for 3D globe mode
- No API keys
- No build tools
- No package manager
- No backend server

Test target browsers:

- Chrome
- Edge
- Firefox
- Safari
- iOS Safari
- Chrome for Android

---

## Technical Notes

### Architecture

The release application is deliberately lightweight:

```text
index.html    # HTML shell, controls, layout and styles
app.js        # Application logic, map state, layers, search, probes and profiles
```

There is no bundler, transpiler, package manager or server runtime.

External libraries are loaded from public content delivery networks or public service endpoints. CesiumJS is loaded only when the 3D globe is activated.

### 2D map

The 2D map uses Leaflet in Web Mercator.

Leaflet panes are used to keep layer ordering predictable:

```text
Basemap
Bathymetry
Contours
Nautical overlays
Reference overlays
Labels and UI
```

The GEBCO contour layer remains in the contours pane above bathymetry.

### 3D globe

The 3D globe uses CesiumJS and is loaded lazily to avoid increasing first-load weight.

The 3D globe is intended for context and inspection. It should not be treated as a precise marine operational view.

### Request discipline

The app is designed to avoid unnecessary public service load.

Key behaviours:

- Named-place search is throttled.
- Repeated named-place searches are cached in memory.
- Coordinate search does not call Nominatim.
- ETOPO fallback requests are bounded.
- ETOPO profile fallback is used only for missing samples.
- Repeated ETOPO failures trigger backoff rather than hammering the service.
- Large sample data is not stored in localStorage.

---

## Limitations

World Bathymetry Explorer is useful for screening and visual context. It is not an authority of record.

Known limitations:

- Public services may be slow, unavailable, rate-limited or changed by providers.
- Different bathymetry and elevation products use different grids, resolutions and vertical datums.
- Global grids can smooth or miss local seabed features.
- Contours are indicative and may not align perfectly with the active bathymetry colour relief.
- NOAA BAG and DEM coverage is uneven globally.
- ETOPO is suitable as a broad fallback, not as a high-resolution local source.
- 3D globe contours are limited by the cached contour source.
- Browser geolocation depends on device, browser and permission quality.

---

## Licence

Application code is licensed under the **GNU General Public License v3.0 or later**.

Third-party map data, chart data, bathymetry services, tiles, libraries and basemaps remain under their own licences and terms.

See `LICENCE` for the full application licence text.

---

## Disclaimer

**Not for navigation.**

This application is a screening, exploration and educational tool. It must not be used for navigation, passage planning, safe clearance assessment, marine operations, engineering design, emergency response or safety-of-life decisions.

Use official nautical charts, notices to mariners, hydrographic office products and approved operational systems for navigation and marine safety.

---

## Contributing

Contributions are welcome, but changes should preserve the product's current operating discipline.

Guidelines:

1. Keep the release app to `index.html` and `app.js`.
2. Do not add a build step, package manager, backend or framework.
3. Do not add private tokens, paid services or authenticated-only services.
4. Preserve the default active state:

   ```javascript
   aB: ["gebcoFlat"]
   aC: ["gebcoContours"]
   aN: []
   ```

5. Keep the not-for-navigation stance visible.
6. Test both 2D map and 3D globe mode.
7. Test point probe, profile, search and layer opacity controls.
8. Avoid increasing request volume to public services.
9. Keep attribution and source caveats current.
10. Run a JavaScript syntax check before submitting:

   ```bash
   node --check app.js
   ```

---

## Project Structure

Typical repository structure:

```text
.
├── index.html
├── app.js
├── README.md
├── LICENCE
└── ATTRIBUTION.md
```

The deployable application itself requires only:

```text
index.html
app.js
```
