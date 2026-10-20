---
title: Setup
---

The Python episodes run in **Google Colab**, a free online notebook service, so you do not need to install
Python. The **Geoprocessing in QGIS** and **Raster Analysis in QGIS** episodes need QGIS on your computer; follow the
[Foundations setup](https://spatialturn.github.io/Foundations/index.html#part-2-install-qgis) to install it.

## What you need

- A Google account
- A web browser (Chrome, Firefox, Edge or Safari)
- **A free Census API key.** Every learner needs their own. Request one at
  <https://api.census.gov/data/key_signup.html>; it arrives by email within a few minutes, and you must
  click the activation link in that email before it works. The PySAL episode cannot run without it.

## Opening a notebook

1. Click a notebook link in the **Notebooks** table at the end of any episode. It opens in Colab.
2. Click **File > Save a copy in Drive**. Your own copy keeps your changes; the original stays read only.
3. Run cells from top to bottom. Click a cell and press **Shift + Enter**, or click the play button
   to its left.
4. The first time you run a cell, Colab may warn that the notebook was not written by Google.
   Click **Run anyway**.

::::::::::::::::::::::::::::::::::::::::::: callout

### Colab tips

- **Run cells in order.** Most errors come from skipping a cell, for example the one that reads the data.
- **Start over cleanly** with **Runtime > Restart session and run all**.
- Data lives in memory, so it disappears when the session ends. Rerun the notebook to get it back.
  To keep an optional output (such as `ndvi.tif`), open the Files panel on the left, right click the
  file and choose **Download**.
- Cells that start with `!pip install` add libraries Colab does not include by default.
  They take a few seconds and only need to run once per session.

:::::::::::::::::::::::::::::::::::::::::::::::::::

## Data

You never need to download data. Every notebook reads its data straight from the web into memory:
`pd.read_csv()` and `gpd.read_file()` accept a web address (including zipped shapefiles), and
`rasterio.open()` reads satellite images the same way.

| Episode | Data | Source |
|---|---|---|
| Raster Analysis (Python and QGIS) | Tippecanoe County elevation (USGS 3DEP, 30 m) and land cover (NLCD 2021), with tracts and 2024 ACS estimates | USGS; Multi-Resolution Land Characteristics Consortium; U.S. Census Bureau (pre-fetched) |
| Geoprocessing (Python and QGIS) | Lafayette food stores and city limits; Tippecanoe County tracts with 2024 ACS estimates | OpenStreetMap; U.S. Census Bureau (pre-fetched, no key needed) |
| Network Analysis | Road network of West Lafayette, Indiana | OpenStreetMap, through OSMnx |
| PySAL | Indiana counties and Marion County tracts, ACS 2023 5-year estimates | U.S. Census Bureau API and cartographic boundary files |
| Spatial Clustering | One year of Chicago crime reports (`CrimesChicago20220225.csv`) | Chicago Data Portal |
| NDVI | Landsat 8/9 Collection 2 Level-2 bands over Indiana (`band2.TIF` to `band5.TIF`) | USGS |

The Landsat bands are about 90 MB each, so reading them takes a few seconds.

## Working offline or on your own computer (optional)

If you prefer to run the notebooks locally, install [Anaconda](https://www.anaconda.com/download)
and create an environment with the libraries used in this module:

```bash
conda create -n spatialturn -c conda-forge python=3.11 geopandas osmnx rasterio libpysal esda splot mapclassify scikit-learn folium geopy jupyterlab
conda activate spatialturn
jupyter lab
```

Then download a notebook from GitHub (**File > Download > Download .ipynb** in Colab) and open it in JupyterLab.
The data is still read from the web, so you need an internet connection.
