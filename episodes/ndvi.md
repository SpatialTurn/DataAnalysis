---
title: "NDVI Analysis"
teaching: 120
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is NDVI and why is it useful?
- How do we calculate NDVI from Landsat imagery?
- How do we load and visualize raster data in Python?
- How can we classify and map greenness using NDVI?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand NDVI and the spectral bands needed to compute it
- Learn to read geospatial raster files using rasterio
- Calculate NDVI using Red & NIR bands from Landsat
- Visualize NDVI as a map with color gradients
- Create a simple vegetation classification from NDVI values

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Normalized Difference Vegetation Index (NDVI) is a widely used metric to estimate vegetation health and greenness from satellite imagery. It uses the **Near-Infrared (NIR)** and **Red** spectral bands:

\[
NDVI = \frac{NIR - RED}{NIR + RED}
\]

Values range from **-1 to +1**, where:

| NDVI Value | Meaning |
|---|---|
| < 0 | Water / clouds / snow |
| 0–0.2 | Barren land / urban surfaces |
| 0.2–0.5 | Sparse vegetation |
| > 0.5 | Dense, healthy vegetation |

In this lesson, we will compute NDVI for Indiana using Landsat bands and generate maps with Python.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Pause after NDVI formula — ensure learners understand band meanings and data types.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Installing Required Libraries

```python
!pip install rasterio matplotlib numpy
```

## 2. Import Dependencies 
```python
import rasterio
import numpy as np
import matplotlib.pyplot as plt
```

## 3. Load Landsat RED and NIR Bands
Make sure your directory contains Landsat `.TIF` files (Band 4 = Red, Band 5 = NIR).
```python
red = rasterio.open("LC08_L1TP_red.tif")
nir = rasterio.open("LC08_L1TP_nir.tif")

red_band = red.read(1).astype('float32')
nir_band = nir.read(1).astype('float32')
```python

Plot a band to inspect:
```python
plt.imshow(red_band, cmap='Reds')
plt.title("Red Band")
plt.colorbar()
plt.show()
```

## 4. Calculate NDVI
```python
ndvi = (nir_band - red_band) / (nir_band + red_band)
```
Visualize NDVI:
```python
plt.figure(figsize=(7,6))
plt.imshow(ndvi, cmap='YlGn')
plt.colorbar(label="NDVI Value")
plt.title("NDVI Map of Indiana")
plt.show()
```

## 5. Classify NDVI into Vegetation Categories
```python
ndvi_class = np.digitize(ndvi, bins=[0, 0.2, 0.5])

# 0 = water/barren, 1 = low vegetation, 2 = dense vegetation
colors = ['blue', 'yellow', 'green']
plt.imshow(ndvi_class, cmap=plt.matplotlib.colors.ListedColormap(colors))
plt.title("NDVI Vegetation Classification")
plt.show()
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1 — Try It Yourself

* Change the NDVI color map (cmap)

* Classify NDVI into four categories instead of three

* Add labels or legends to your final map

:::::::::::::::::::::::: solution

```python
bins = [0, 0.2, 0.4, 0.6]
ndvi_class = np.digitize(ndvi, bins=bins)
```

:::::::::::::::::::::::::::::::::

## Challenge 2 — Mask Water Pixels

Use NDVI to mask water (<0):
```python
water_mask = ndvi < 0
ndvi_water_removed = np.where(water_mask, np.nan, ndvi)

plt.imshow(ndvi_water_removed, cmap='YlGn')
plt.title("NDVI with Water Masked")
plt.show()
```
:::::::::::::::::::::::: solution

Water regions become transparent/ignored in the plot.

:::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: callout

NDVI is affected by seasonality, cloud cover, and atmospheric effects.
Always check metadata to ensure you're comparing compatible scenes.

::::::::::::::::::::::::::::::::::::::::::::::::

## Math

NDVI uses reflectance difference between two bands:

`NDVI = (NIR - RED)/(NIR + RED)`

NIR increases with vegetation health — higher NDVI = greener land.

::::::::::::::::::::::::::::::::::::: keypoints

* NDVI uses Red & NIR reflectance from satellite imagery

* Landsat Band 4 = Red, Band 5 = NIR for NDVI

* NDVI ranges from -1 (water) to +1 (healthy vegetation)

* Python tools: rasterio, numpy, matplotlib

* NDVI maps reveal vegetation patterns visually and quantitatively

::::::::::::::::::::::::::::::::::::::::::::::::



# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/NDVI_Indiana.ipynb" target="_blank">Beginner</a> | Introduction to NDVI using LANDSAT Dataset. |
| [Advanced]()  |  (to be added) |
