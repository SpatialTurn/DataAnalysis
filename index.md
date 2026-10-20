---
site: sandpaper::sandpaper_site
---

This module teaches advanced methods of spatial analysis in Python. It is written for social scientists
who already know the everyday operations (buffers, overlays, map algebra and zonal statistics), which are taught
in the [Data Processing](https://spatialturn.github.io/DataProcessing/) module.

Each episode explains one method on this site, then links to three short Colab notebooks (Beginner,
Intermediate and Advanced), each about 20 to 30 minutes.

| Episode | Question it answers | Tools |
|---|---|---|
| Network Analysis | How far is it by road, and how long does it take? | `osmnx`, `networkx` |
| Spatial Autocorrelation with PySAL | Are similar values clustered, and where? | `libpysal`, `esda`, `splot` |
| Spatial Clustering | Where do points concentrate into hotspots? | `scikit-learn`, `scipy` |
| NDVI Analysis | How green is the landscape in a satellite image? | `rasterio`, `numpy`, `scikit-learn` |

The episodes can be taken in any order, though Spatial Clustering builds lightly on ideas from PySAL.
Geocoding (turning addresses into points and downloading OpenStreetMap features) is taught in the
[Data Collection module](https://spatialturn.github.io/DataCollection/geocoding.html) and is good preparation for Network Analysis.
QGIS users will find raster analysis of elevation and satellite imagery in Workshop 2026 Track A
([DEM analysis](https://spatialturn.github.io/Workshop2026-Session2A/dem-analysis.html),
[NDVI](https://spatialturn.github.io/Workshop2026-Session3A/ndvi-analysis.html) and
[flood mapping](https://spatialturn.github.io/Workshop2026-Session3A/flood-analysis.html)), and network analysis in
the Case Studies module's [Grocery Access Part 2](https://spatialturn.github.io/CaseStudies/grocery-network.html).

::::::::::::::::::::::::::::::::::::::::::: prereq

## Prerequisites

- A Google account, to open the notebooks in Google Colab
- Basic Python, `pandas` and GeoPandas (Foundations notebooks 2a to 4c); the Data Processing module's
  Geoprocessing in Python episode is recommended
- A free [Census API key](https://api.census.gov/data/key_signup.html) of your own (needed for the PySAL episode)

New to Python or Colab? Start with the [Foundations](https://spatialturn.github.io/Foundations/) module,
then read the [Setup](learners/setup.md) page.

::::::::::::::::::::::::::::::::::::::::::::::::::

This module is part of [Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
a set of training modules developed at Purdue University with support from the National Science Foundation.
