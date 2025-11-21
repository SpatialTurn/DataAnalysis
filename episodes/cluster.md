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

df = pd.read_csv("points.csv")   # contains lon, lat
df.head()
```

Visualize the raw points:

```python
import matplotlib.pyplot as plt

plt.scatter(df.lon, df.lat, s=10)
plt.title("Raw Spatial Points")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.show()
```

This simple scatterplot helps identify whether your data already looks clustered.


## 2. K-Means Clustering

K-Means is the simplest clustering algorithm.
It works best when:

- You know the number of clusters you want

- Clusters are roughly circular

- Points are evenly distributed

```python
from sklearn.cluster import KMeans

coords = df[['lon', 'lat']]
kmeans = KMeans(n_clusters=4, random_state=42)
df['kmeans_label'] = kmeans.fit_predict(coords)
```

Visualize results:

```python
plt.scatter(df.lon, df.lat, c=df.kmeans_label, cmap='tab10')
plt.title("K-Means Clustering")
plt.show()
```

## 3. Hierarchical Clustering


Hierarchical clustering builds clusters step-by-step.
It is useful when:

- You want a dendrogram

- You don’t know the number of clusters beforehand

- Clusters may have irregular shapes

Example:


```python
from sklearn.cluster import AgglomerativeClustering

agg = AgglomerativeClustering(n_clusters=4)
df['hier_label'] = agg.fit_predict(coords)
```

Plot:

```python
plt.scatter(df.lon, df.lat, c=df.hier_label, cmap='viridis')
plt.title("Hierarchical Clustering")
plt.show()
```


## 4. DBSCAN: Density-Based Clustering

DBSCAN is ideal for spatial datasets because:

- It finds clusters of any shape

- It identifies noise points

- It does not require the number of clusters in advance

Example:


```python
from sklearn.cluster import DBSCAN
import numpy as np

epsilon = 0.01   # distance threshold
db = DBSCAN(eps=epsilon, min_samples=5).fit(coords)

df['dbscan_label'] = db.labels_

```

Points labeled `-1` are noise (outliers).

Plot:
```python
plt.scatter(df.lon, df.lat, c=df.dbscan_label, cmap='Accent')
plt.title("DBSCAN Spatial Clusters")
plt.show()
```

::::::::::::::::::::::::::::::::::::: challenge

Challenge 1: Exploring Your Own Dataset

Using the examples above:

1. Load your own set of spatial coordinates

2. Apply K-Means and DBSCAN

3. Compare the results

Which method performs better, and why?

:::::::::::::::::::::::: solution

Example Interpretation

* K-Means finds evenly divided clusters

* DBSCAN finds natural geographic groups and labels outliers

* For irregular spatial patterns, DBSCAN usually performs better

:::::::::::::::::::::::::::::::::


Challenge 2: Adjusting DBSCAN Sensitivity

Try changing the `eps` parameter:

```python
DBSCAN(eps=500, min_samples=5) # eps is in meters
DBSCAN(eps=1000,  min_samples=5)
```

:::::::::::::::::::::::: solution

Larger `eps` creates larger clusters.
Smaller `eps` creates more clusters and more noise points.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math

DBSCAN uses density to define clusters. Its key idea:

A point belongs to a cluster if it has at least `min_samples` neighbors within a
distance `ε`:

`$\text{density} = \frac{\text{neighbors}}{\pi \varepsilon^2}$`

Higher density regions form clusters; low density points become noise.

::::::::::::::::::::::::::::::::::::: keypoints

* Spatial clustering groups geographic points into meaningful patterns

* K-Means is simple but assumes circular clusters

* Hierarchical clustering builds clusters step-wise

* DBSCAN is best for irregular shapes and detecting noise

* Always visualize your clusters to interpret them correctly

::::::::::::::::::::::::::::::::::::::::::::::::


# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/spatialclustering.ipynb" target="_blank">Beginner</a> | Introduction to Spatial Clustering using Crime Datasets. |
| [Advanced]()  |  (to be added) |
