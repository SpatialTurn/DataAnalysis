---
site: sandpaper::sandpaper_site
---

This module teaches the core methods of spatial analysis. It is written for social scientists and
other beginners, and it climbs from everyday operations to advanced methods.

- The first episodes (**intermediate**) teach the everyday operations, buffers, overlays, distances,
  map algebra and zonal statistics,
  twice: once in **QGIS** (point and click) and once in **Python** (Google Colab), with the same data and
  the same results. Follow either one, or both.
- The later episodes (**advanced**) are Python only. Each explains one method on this site, then links to
  three short Colab notebooks (Beginner, Intermediate and Advanced), each about 20 to 30 minutes.

| Episode | Question it answers | Tools |
|---|---|---|
| Geoprocessing in Python / in QGIS | What lies within a distance, inside a boundary, or nearest? | `geopandas` / QGIS Vector menu |
| Raster Analysis in Python / in QGIS | How do rasters combine, and what do they say about each tract? | `rasterio`, `rasterstats` / Raster Calculator, Zonal statistics |
| Network Analysis | How far is it by road, and how long does it take? | `osmnx`, `networkx` |
| Spatial Autocorrelation with PySAL | Are similar values clustered, and where? | `libpysal`, `esda`, `splot` |
| Spatial Clustering | Where do points concentrate into hotspots? | `scikit-learn`, `scipy` |
| NDVI Analysis | How green is the landscape in a satellite image? | `rasterio`, `numpy`, `scikit-learn` |

Start with Geoprocessing, then Raster Analysis. The advanced episodes can then be taken in any order, though Spatial Clustering
builds lightly on ideas from PySAL.
Geocoding (turning addresses into points and downloading OpenStreetMap features) is taught in the
[Data Collection module](https://spatialturn.github.io/DataCollection/geocoding.html) and is good preparation for Network Analysis.

::::::::::::::::::::::::::::::::::::::::::: prereq

## Prerequisites

- A Google account, to open the notebooks in Google Colab
- For the Python episodes: basic Python, `pandas` and GeoPandas (Foundations notebooks 2a to 4c)
- For the QGIS episode: QGIS installed and the Foundations QGIS episode
- A free [Census API key](https://api.census.gov/data/key_signup.html) of your own (needed for the PySAL episode)

New to Python or Colab? Start with the [Foundations](https://spatialturn.github.io/Foundations/) module,
then read the [Setup](learners/setup.md) page.

::::::::::::::::::::::::::::::::::::::::::::::::::

This module is part of [Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
a set of training modules developed at Purdue University with support from the National Science Foundation.
