---
title: Setup
---

Everything in this module runs in **Google Colab**, a free online notebook service. You do not need
to install Python or any GIS software on your computer.

## What you need

- A Google account
- A web browser (Chrome, Firefox, Edge or Safari)
- For the **Spatial Autocorrelation with PySAL** episode only: a free Census API key.
  Request one at <https://api.census.gov/data/key_signup.html>; it arrives by email within a few minutes.

## Opening a notebook

1. Click a notebook link in the **Notebooks** table at the end of any episode. It opens in Colab.
2. Click **File > Save a copy in Drive**. Your own copy keeps your changes; the original stays read only.
3. Run cells from top to bottom. Click a cell and press **Shift + Enter**, or click the play button
   to its left.
4. The first time you run a cell, Colab may warn that the notebook was not written by Google.
   Click **Run anyway**.

::::::::::::::::::::::::::::::::::::::::::: callout

### Colab tips

- **Run cells in order.** Most errors come from skipping a cell, for example the one that downloads data.
- **Start over cleanly** with **Runtime > Restart session and run all**.
- **Files disappear** when the session ends. To keep an output (such as `ndvi.tif`), open the Files panel
  on the left, right click the file and choose **Download**.
- Cells that start with `!pip install` add libraries Colab does not include by default.
  They take a few seconds and only need to run once per session.

:::::::::::::::::::::::::::::::::::::::::::::::::::

## Data

You do not need to download anything in advance. Each notebook downloads its own data:

| Episode | Data | Source |
|---|---|---|
| Geocoding | 15 museum addresses (`museums.txt`); live OpenStreetMap features | This project; OpenStreetMap |
| Network Analysis | Road network of West Lafayette, Indiana | OpenStreetMap, through OSMnx |
| PySAL | Indiana counties and Marion County tracts, ACS 2023 5-year estimates | U.S. Census Bureau API and cartographic boundary files |
| Spatial Clustering | One year of Chicago crime reports (`CrimesChicago20220225.csv`) | Chicago Data Portal |
| NDVI | Landsat 8/9 Collection 2 Level-2 bands over Indiana (`band2.TIF` to `band5.TIF`) | USGS |

The Landsat bands are about 90 MB each, so the NDVI downloads take a minute or two.

## Working offline or on your own computer (optional)

If you prefer to run the notebooks locally, install [Anaconda](https://www.anaconda.com/download)
and create an environment with the libraries used in this module:

```bash
conda create -n spatialturn -c conda-forge python=3.11 geopandas osmnx rasterio libpysal esda splot mapclassify scikit-learn folium geopy jupyterlab
conda activate spatialturn
jupyter lab
```

Then download a notebook from GitHub (**File > Download > Download .ipynb** in Colab) and open it in JupyterLab.
Lines starting with `!wget` need the `wget` program; on Windows, download those files by hand from the
[DataCollection-Notebooks repository](https://github.com/SpatialTurn/DataCollection-Notebooks).
