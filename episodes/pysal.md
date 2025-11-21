---
title: "Introduction to Spatial Analysis with PySAL"
teaching: 100
exercises: 1
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is PySAL and what can it do for spatial analysis?
- How do we compute spatial weights and perform spatial autocorrelation?
- How do we interpret results like Moran’s I?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the purpose of PySAL in spatial data science
- Learn how to load spatial data using GeoPandas
- Construct spatial weight matrices
- Compute Global Moran’s I using PySAL
- Visualize spatial clustering and spatial autocorrelation

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

[PySAL](https://pysal.org/) is the Python Spatial Analysis Library — a powerful,
open-source toolkit for working with spatial data. It provides tools for:

- spatial weights  
- spatial autocorrelation  
- clustering  
- spatial regression  
- neighborhood analysis  

This tutorial introduces the **core PySAL workflow**, closely following the
structure used in your uploaded notebook.  

We will cover:

1. Loading polygon or point data  
2. Building spatial weights  
3. Running Global Moran’s I  
4. Visualizing results  

This tutorial assumes basic familiarity with `pandas`, `geopandas`, and Python.

What you need to know for Carpentries lessons:

 1. `questions` prime the learner for the lesson.
 2. `objectives` state what skills will be gained.
 3. `keypoints` summarize what was learned.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Learners may struggle initially with spatial weights (rook, queen, k-nearest).
Spend extra time walking through simple diagrams before showing code.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::


## 1. Loading Spatial Data

PySAL works seamlessly with **GeoPandas**.  
Here’s a simple example using a polygon shapefile:

```python
import geopandas as gpd

gdf = gpd.read_file("data/shapes.shp")
gdf.head()
```

Plot the boundaries:
```python
gdf.plot(edgecolor="black", figsize=(6,6))
```
This ensures the geometry is valid and loads correctly.

## 2. Building Spatial Weights

Spatial weights define who is a neighbor of whom.

PySAL includes:

- Rook contiguity

- Queen contiguity

- K-nearest neighbors

- Distance-based neighbors

### Example: Queen Contiguity
```python
from libpysal.weights import Queen

w = Queen.from_dataframe(gdf)
w.transform = "R"  # row-standardization
```
Check neighbors of the first polygon:
```python
w.neighbors[0]
```

## 3. Global Moran's I

Moran’s I measures global spatial autocorrelation:

- Positive values → clustering

- Negative values → dispersion

- Near zero → random pattern

Assume the dataset has a numeric column `value`:
```python
import esda
import numpy as np

y = gdf['value']
mi = esda.Moran(y, w)
```
View the results:
```python
mi.I, mi.p_sim
```

Plot the Moran scatterplot:
```python
import splot.esda as esdaplot

esdaplot.moran_scatterplot(mi)
```

## 4. Local Moran's I (Outlier Analysis)

Local Moran's I finds hotspots and coldspots.
```python
lisa = esda.Moran_Local(y, w)
```

Add LISA quadrant labels to the GeoDataFrame:
```python
gdf["lisa_cluster"] = lisa.q
```
Map the clusters:
```python
gdf.plot(column="lisa_cluster", cmap="Set1", figsize=(8,6), legend=True)
```
This creates a basic LISA cluster map.

::::::::::::::::::::::::::::::::::::: challenge

Challenge 1: Create Your Own Spatial Weights

Using the GeoDataFrame loaded above:

- Create rook contiguity weights

- Print the neighbor list for observation 10

- Compare how rook vs queen differ

```python
from libpysal.weights import Rook
w_rook = Rook.from_dataframe(gdf)
w_rook.neighbors[10]
```

:::::::::::::::::::::::: solution

Queen neighbors may include diagonal touches.
Rook neighbors require shared edges only.
You should see fewer rook neighbors than queen neighbors.

:::::::::::::::::::::::::::::::::

Challenge 2: Compute Moran’s I on a New Variable

Choose any numeric variable in your dataset:

Extract the variable

Compute Moran’s I

Interpret whether clustering exists

:::::::::::::::::::::::: solution

A positive Moran’s I with low p-value → strong clustering.
Near zero → randomness.
Negative → spatial dispersion.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::


## Math

Global Moran’s I is defined as:

`$ I = \frac{N}{W} \frac{\sum_i \sum_j w_{ij}(x_i - \bar{x})(x_j - \bar{x})} {\sum_i (x_i - \bar{x})^2} $`

Where:

`N` = number of observations

`W` = sum of all spatial weights

`w_ij` = weight between units i and j

`x` = variable of interest

::::::::::::::::::::::::::::::::::::: keypoints

PySAL provides tools for weights, autocorrelation, clustering, and modeling

Queen and rook weights define spatial neighbors differently

Moran’s I measures global autocorrelation

Local Moran (LISA) identifies hotspots and coldspots

GeoPandas and PySAL together form a powerful spatial analysis workflow

::::::::::::::::::::::::::::::::::::::::::::::::

# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/pysal_basic.ipynb" target="_blank">Beginner</a> | Introduction to Spatial Analysis using PySAL package. |
| [Advanced]()  |  (to be added) |