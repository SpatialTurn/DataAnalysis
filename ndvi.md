---
title: "NDVI Analysis"
teaching: 50
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- What is NDVI and why is it useful?
- How do we read Landsat bands and compute NDVI in Python?
- How can we classify NDVI into land cover groups?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain why vegetation is bright in near infrared and dark in red
- Read raster bands with `rasterio` and convert them to surface reflectance
- Compute, map and save NDVI as a GeoTIFF
- Classify NDVI with thresholds and summarize area by class

::::::::::::::::::::::::::::::::::::::::::::::::

## Why NDVI?

Healthy leaves absorb red light for photosynthesis and strongly reflect near infrared (NIR) light.
The **Normalized Difference Vegetation Index (NDVI)** uses that contrast to measure greenness
from satellite images. It is easy to compute, works with most sensors (Landsat, Sentinel-2, MODIS),
and is used for crop monitoring, drought, urban green space and land cover change.

| NDVI | Typical surface |
|---|---|
| below 0 | Water, snow, clouds |
| 0 to 0.2 | Bare soil, buildings, roads |
| 0.2 to 0.4 | Sparse vegetation, grass, early crops |
| above 0.4 | Dense, healthy vegetation |

The notebooks use one Landsat scene over Indiana (Collection 2 Level-2 surface reflectance).
For Landsat 8 and 9, **Band 4 is red** and **Band 5 is NIR**.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Pause after the formula and make sure learners know which band is which. Each band is about 90 MB and
is read from GitHub into memory, so the reading cell takes a few seconds.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Read the bands and convert to reflectance

```python
import numpy as np
import matplotlib.pyplot as plt
import rasterio

# rasterio reads the bands straight from GitHub into memory; nothing is downloaded
LANDSAT = "https://raw.githubusercontent.com/SpatialTurn/DataCollection-Notebooks/main/Census/landsat"

with rasterio.open(f"{LANDSAT}/band4.TIF") as src:     # red
    red = src.read(1).astype("float32")
    profile = src.profile
with rasterio.open(f"{LANDSAT}/band5.TIF") as src:     # near infrared
    nir = src.read(1).astype("float32")

red[red == 0] = np.nan          # 0 = outside the scene
nir[nir == 0] = np.nan

# Level-2 values are stored as integers; this converts them to reflectance (0 to 1)
red = np.clip(red * 0.0000275 - 0.2, 0, 1)
nir = np.clip(nir * 0.0000275 - 0.2, 0, 1)
```

## 2. Compute and map NDVI

```python
ndvi = (nir - red) / (nir + red)

plt.imshow(ndvi, cmap="RdYlGn", vmin=-0.2, vmax=0.8)
plt.colorbar(label="NDVI")
plt.title("NDVI, Landsat scene over Indiana")
plt.show()
```

## 3. Save NDVI as a GeoTIFF (optional)

```python
profile.update(dtype="float32", count=1, nodata=np.nan)
with rasterio.open("ndvi.tif", "w", **profile) as dst:
    dst.write(ndvi.astype("float32"), 1)
```

Everything above stays in memory. Save a file only if you want to open the result in QGIS;
it keeps the scene's location, so it lines up exactly.

## 4. Classify NDVI

`np.digitize` gives each pixel a class number. **Three** break values make **four** classes,
so we need four colors.

```python
from matplotlib.colors import ListedColormap

classes = np.digitize(ndvi, [0, 0.2, 0.4]).astype("float32")   # 0, 1, 2, 3
classes[np.isnan(ndvi)] = np.nan

colors = ["#2b83ba", "#d7a26b", "#a6d96a", "#1a9641"]   # water, bare, sparse, dense
plt.imshow(classes[::4, ::4], cmap=ListedColormap(colors), vmin=0, vmax=3,
           interpolation="nearest")    # every 4th pixel keeps the plot fast
plt.title("NDVI classes")
plt.show()
```

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Five classes

Classify NDVI into five classes with breaks `[0, 0.1, 0.2, 0.3]`. How many colors do you need?

:::::::::::::::::::::::: solution

Four breaks make five classes, so you need five colors and `vmax=4`.

```python
classes = np.digitize(ndvi, [0, 0.1, 0.2, 0.3]).astype("float32")
classes[np.isnan(ndvi)] = np.nan
colors = ["#2b83ba", "#d7a26b", "#fdae61", "#a6d96a", "#1a9641"]
plt.imshow(classes[::4, ::4], cmap=ListedColormap(colors), vmin=0, vmax=4,
           interpolation="nearest")
plt.show()
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: Mask water

Replace NDVI values below 0 with `np.nan` and map the result. What is the mean NDVI of land only?

:::::::::::::::::::::::: solution

```python
ndvi_land = np.where(ndvi < 0, np.nan, ndvi)
plt.imshow(ndvi_land[::4, ::4], cmap="YlGn", vmin=0, vmax=0.8)
plt.show()
print(np.nanmean(ndvi_land))
```

Masked pixels are left blank on the map and ignored by `np.nanmean`.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

NDVI changes with the seasons, clouds and the atmosphere. Compare scenes from the same time of
year, and use surface reflectance (Level-2) products rather than raw Level-1 values when possible.

::::::::::::::::::::::::::::::::::::::::::::::::

## Math

$$
NDVI = \frac{NIR - Red}{NIR + Red}
$$

NDVI ranges from -1 to +1. Healthy vegetation has high NIR and low red reflectance, which pushes NDVI
toward +1. Water reflects more red than NIR, so its NDVI is negative.

::::::::::::::::::::::::::::::::::::: keypoints

- NDVI compares NIR and red reflectance; for Landsat 8 and 9 these are bands 5 and 4
- Convert Level-2 values to reflectance (`value * 0.0000275 - 0.2`) before computing NDVI
- `rasterio` reads and writes rasters; reuse the input `profile` when saving
- `np.digitize` turns NDVI into classes; n breaks give n + 1 classes

::::::::::::::::::::::::::::::::::::::::::::::::

## Notebooks

| Level | Notebook | What you will do |
|---|---|---|
| Beginner | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/05a_Computing_NDVI.ipynb" target="_blank">5a. Computing NDVI from Landsat</a> | Read bands, convert to reflectance, map NDVI and save a GeoTIFF |
| Intermediate | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/05b_Classifying_NDVI.ipynb" target="_blank">5b. Classifying NDVI</a> | Group NDVI into classes, map them and measure area per class |
| Advanced | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/05c_Image_Classification_Advanced.ipynb" target="_blank">5c. Image Classification with Machine Learning</a> | True color image, K-Means and Random Forest classification |
