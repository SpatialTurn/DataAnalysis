---
site: sandpaper::sandpaper_site
---

This module teaches the core methods of spatial analysis using **Python in Google Colab**.
It is written for social scientists and other beginners: no GIS software to install, and every
step comes with code you can run in your browser.

Each episode explains one method on this site, then links to three short Colab notebooks
(Beginner, Intermediate and Advanced). Each notebook takes about 20 to 30 minutes and covers a
single idea.

| Episode | Question it answers | Python tools |
|---|---|---|
| Network Analysis | How far is it by road, and how long does it take? | `osmnx`, `networkx` |
| Spatial Autocorrelation with PySAL | Are similar values clustered, and where? | `libpysal`, `esda`, `splot` |
| Spatial Clustering | Where do points concentrate into hotspots? | `scikit-learn`, `scipy` |
| NDVI Analysis | How green is the landscape in a satellite image? | `rasterio`, `numpy`, `scikit-learn` |

The episodes can be taken in any order, though Spatial Clustering builds lightly on ideas from PySAL.
Geocoding (turning addresses into points and downloading OpenStreetMap features) is taught in the
[Data Collection module](https://spatialturn.github.io/DataCollection/geocoding.html) and is good preparation for Network Analysis.

::::::::::::::::::::::::::::::::::::::::::: prereq

## Prerequisites

- A Google account, to open the notebooks in Google Colab
- Basic Python: running cells, variables, lists, and simple `pandas` DataFrames
- A free [Census API key](https://api.census.gov/data/key_signup.html) of your own (needed for the PySAL episode)

New to Python or Colab? Start with the **Geospatial Python** session of the 2026 workshop, then
read the [Setup](learners/setup.md) page.

::::::::::::::::::::::::::::::::::::::::::::::::::

This module is part of [Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
a set of training modules developed at Purdue University with support from the National Science Foundation.
