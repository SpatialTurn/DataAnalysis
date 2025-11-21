---
title: "Introduction to Spatial Clustering in Python"
teaching: 100
exercises: 1
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is spatial clustering and why do we use it?
- How can we perform basic clustering on geographic point data?
- How do algorithms like K-Means, Hierarchical Clustering, and DBSCAN differ?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the concept of spatial clustering
- Learn how to prepare point data for clustering
- Apply K-Means, Hierarchical Clustering, and DBSCAN in Python
- Visualize clustering results on simple scatterplots and maps

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Spatial clustering is a core method used in geography, archaeology, ecology, and
urban studies. It helps identify *patterns* in the spatial distribution of points—such
as hotspots of crime, clusters of archaeological artifacts, or regions with similar
environmental characteristics.

This beginner tutorial walks you through the fundamentals of spatial clustering
using a simple dataset of geographic coordinates. The workflow is entirely in
Python, following the structure used in your uploaded notebook.

We will cover:

- Loading and exploring point data  
- Preparing coordinates for clustering  
- Running three clustering algorithms  
- Visualizing the results  

All examples use standard Python libraries:  
`pandas`, `geopandas`, `matplotlib`, `sklearn`, and `scipy`.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Learners may need extra time understanding the differences between clustering
algorithms. Consider pausing after each method and showing multiple visualizations.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Loading Spatial Point Data

Spatial clustering typically starts with a set of point locations. A minimal example:

```python
import pandas as pd

df = pd.read_csv("points.csv")     # contains columns: lon, lat
df.head()


Visualize the raw points:

import matplotlib.pyplot as plt

plt.scatter(df.lon, df.lat, s=10)
plt.title("Raw Spatial Points")
plt.xlabel("Longitude"); plt.ylabel("Latitude")
plt.show()

This simple scatterplot helps identify whether your data already looks clustered.




