---
title: 'Instructor Notes'
---

## Overall approach

- Each episode page gives the concept and a short code walkthrough. The hands-on work happens in
  the three linked Colab notebooks (Beginner, Intermediate, Advanced), each about 20 to 30 minutes.
- In a 90 minute session, teach the episode page and the Beginner notebook together, then let
  learners continue with the Intermediate notebook. Treat Advanced notebooks as take home material.
- Each episode has a theory part (concepts, assumptions, interpretation and pitfalls) and a code part.
  The third challenge in each episode is conceptual and works well as a pair discussion while a slow
  cell runs.
- Ask everyone to **Save a copy in Drive** before running anything. Most "lost work" questions
  come from editing the read only original.
- Notebooks were last tested (October 2026) with OSMnx 2.1, scikit-learn 1.9, rasterio 1.5,
  libpysal 4.15 and esda 2.10.
  OSMnx 2.0 removed `geometries_from_place` and `utils_graph`; older tutorials online still use them.

## Episode by episode

### Network Analysis

- Draw a small graph (four intersections, five roads) before any code.
- The Case Studies module's grocery access case study does the same kind of analysis in QGIS, without
  code. It is a good follow-up, or an alternative for groups that prefer a GUI.
- `ox.nearest_nodes(G, X=lon, Y=lat)` takes **longitude first**. This is the most common bug.
- `add_edge_speeds(G, fallback=40)` fills in road types with no speed data. Mention that speeds are
  estimates from OSM tags, not real traffic.
- Downloading the network can take up to a minute. Start it, then explain nodes and edges.

### Spatial Autocorrelation with PySAL

- **Every learner needs their own free Census API key.** Ask them to request and activate it
  **before** the session (it arrives by email). Without it the data cell stops with a clear message.
- Spatial weights are the hardest idea. Use the chess analogy and a 3 by 3 grid on the board.
- Stress the p-value filter on LISA maps. Without it, many "hotspots" are noise.
- Results depend on the units used (counties vs tracts): a good discussion point on the modifiable
  areal unit problem.

### Spatial Clustering

- The key lesson is to **reproject before clustering**. Show that `eps=1000` means 1,000 meters only
  after `to_crs(epsg=32616)`.
- Crime data reflects reporting and policing as well as crime. Discuss this before showing maps.
- HDBSCAN now ships with scikit-learn (`sklearn.cluster.HDBSCAN`); the separate `hdbscan`
  package is not needed.

### NDVI Analysis

- Bands are read from GitHub into memory (about 90 MB each). Run the reading cell, then explain NDVI while it loads.
- The provided bands are Landsat Collection 2 **Level-2** surface reflectance. They must be scaled with
  `value * 0.0000275 - 0.2` before computing NDVI.
- Full resolution images have about 60 million pixels. The notebooks plot every 4th pixel
  (`array[::4, ::4]`) so Colab does not run out of memory.
- The Random Forest example uses stand in labels from NDVI rules, so its near perfect accuracy is not
  realistic. Point this out, and show how real training polygons would be drawn in QGIS.

## Common problems

| Symptom | Likely cause | Fix |
|---|---|---|
| `NameError: name 'x' is not defined` | A cell was skipped | **Runtime > Run before** |
| `HTTPError` or a timeout while reading data | Network hiccup reading from GitHub or the Census site | Rerun the cell |
| `ModuleNotFoundError: osmnx` (or `libpysal`, `rasterio`) | The `!pip install` cell did not run | Run it, then rerun the imports |
| OpenStreetMap download times out | Free server busy or rate limited | Wait a minute and rerun |
| `KeyError` or HTTP 400 from the Census API | Key missing, mistyped or not yet activated | Check the key and the activation email |
| Map shows "403 Access blocked" or "API key required" behind the points | OpenStreetMap and CARTO tiles do not work inside Colab | Add `tiles="Esri.WorldGrayCanvas"` to `.explore()` |
| Colab session crashed | Out of memory on a large raster | **Runtime > Restart session**, then rerun |
