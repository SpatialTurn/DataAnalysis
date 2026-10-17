---
title: "Spatial Clustering"
teaching: 50
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- What is spatial clustering and when is it useful?
- Why must coordinates be in meters before clustering?
- How do K-Means, hierarchical clustering, DBSCAN and HDBSCAN differ?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Prepare point data for clustering by reprojecting to a meter based CRS
- Find dense hotspots with DBSCAN and HDBSCAN and recognize noise points
- Divide points into zones with K-Means and choose k with the elbow method
- Choose a clustering method that fits the research question

::::::::::::::::::::::::::::::::::::::::::::::::

## Why spatial clustering?

Clustering finds groups of points that sit close together: crime hotspots, disease outbreaks,
clusters of archaeological finds, or concentrations of businesses. It turns thousands of points into
a few areas you can describe and compare.

| Method | You choose | Noise points? | Best for |
|---|---|---|---|
| K-Means | Number of clusters (k) | No, every point is assigned | Dividing an area into zones |
| Hierarchical | Number of clusters or a cut height | No | Seeing structure at several scales (small datasets) |
| DBSCAN | Search radius (`eps`) and `min_samples` | Yes | Hotspots of any shape at a known distance |
| HDBSCAN | `min_cluster_size` | Yes | Hotspots with varying density |

The notebooks use about 200,000 crime reports from the
[Chicago Data Portal](https://data.cityofchicago.org/) and focus on robbery.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

The most common mistake is clustering raw latitude and longitude. Show what `eps=1000` means in
meters versus degrees before running DBSCAN. Pause after each method and compare maps side by side.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Load points and reproject to meters

```python
import pandas as pd
import geopandas as gpd
import numpy as np

crimes = pd.read_csv("CrimesChicago20220225.csv")
crimes.columns = crimes.columns.str.strip()
robbery = crimes[crimes["PRIMARY DESCRIPTION"] == "ROBBERY"].dropna(subset=["LATITUDE", "LONGITUDE"])

robbery = gpd.GeoDataFrame(
    robbery,
    geometry=gpd.points_from_xy(robbery["LONGITUDE"], robbery["LATITUDE"]),
    crs="EPSG:4326",
).to_crs(epsg=32616)          # UTM zone 16N: x and y in meters

coords = np.column_stack([robbery.geometry.x, robbery.geometry.y])
```

Latitude and longitude are angles. One degree of longitude in Chicago is about 85 km while one
degree of latitude is about 111 km, so distances in degrees are distorted. Always reproject first.

## 2. DBSCAN: density based hotspots

```python
from sklearn.cluster import DBSCAN

db = DBSCAN(eps=1000, min_samples=50).fit(coords)   # eps in meters
robbery["dbscan"] = db.labels_                       # -1 means noise
```

A point with at least `min_samples` neighbors within `eps` meters is a core point; nearby core
points form a cluster. Choose `eps` with a k-distance plot (see notebook 4b).

## 3. HDBSCAN: no radius needed

```python
from sklearn.cluster import HDBSCAN

robbery["hdbscan"] = HDBSCAN(min_cluster_size=50, min_samples=10).fit_predict(coords)
```

## 4. K-Means: zones

```python
from sklearn.cluster import KMeans

robbery["kmeans"] = KMeans(n_clusters=6, n_init=10, random_state=42).fit_predict(coords)
```

## 5. Hierarchical clustering and the dendrogram

```python
from scipy.cluster.hierarchy import linkage, dendrogram, fcluster

sample = robbery.sample(2000, random_state=1)        # hierarchical is slow on large data
tree = linkage(np.column_stack([sample.geometry.x, sample.geometry.y]), method="ward")
dendrogram(tree, truncate_mode="lastp", p=20, no_labels=True)
sample["hier"] = fcluster(tree, t=6, criterion="maxclust")
```

Plot any result with `robbery.plot(column="dbscan", categorical=True, markersize=1)`.

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: K-Means or DBSCAN?

Map the K-Means and DBSCAN results side by side. Which one would you use to report
"robbery hotspots" to a city council, and why?

:::::::::::::::::::::::: solution

DBSCAN (or HDBSCAN). K-Means assigns every robbery to a zone, even isolated ones, so its "clusters"
cover the whole city. DBSCAN keeps only dense areas and labels scattered points as noise, which is
what a hotspot means.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: Adjusting DBSCAN

Run DBSCAN with `eps=500` and `eps=1500` (meters). How do the number of clusters and the share of
noise points change?

:::::::::::::::::::::::: solution

```python
for eps in [500, 1500]:
    labels = DBSCAN(eps=eps, min_samples=50).fit(coords).labels_
    print(eps, labels.max() + 1, "clusters", f"{(labels == -1).mean():.0%} noise")
```

A smaller `eps` gives more, smaller clusters and more noise. A larger `eps` merges clusters into a
few large ones.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math

DBSCAN looks at the **neighborhood** of each point $p$: all points within distance $\varepsilon$
(`eps`). If that neighborhood holds at least `min_samples` points, $p$ is a **core point**.
Core points within $\varepsilon$ of each other join the same cluster, along with the border points
near them. Every other point is **noise**.

K-Means instead chooses $k$ centers $\mu_1, \dots, \mu_k$ that minimize the total squared distance
$\sum_i \lVert x_i - \mu_{c(i)} \rVert^2$, where $c(i)$ is the center closest to point $x_i$.
This total is the **inertia** used in the elbow plot.

::::::::::::::::::::::::::::::::::::: keypoints

- Reproject to meters before clustering so distances are real
- DBSCAN and HDBSCAN find dense hotspots of any shape and label scattered points as noise
- K-Means splits all points into k round zones; choose k with the elbow method
- Hierarchical clustering shows how groups merge, but is slow on large datasets
- Pick the method that matches your question: hotspots or zones

::::::::::::::::::::::::::::::::::::::::::::::::

## Notebooks

| Level | Notebook | What you will do |
|---|---|---|
| Beginner | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/04a_Exploring_Crime_Points.ipynb" target="_blank">4a. Exploring Crime Points</a> | Summarize Chicago crimes and turn them into points in meters |
| Intermediate | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/04b_DBSCAN_and_HDBSCAN.ipynb" target="_blank">4b. DBSCAN and HDBSCAN</a> | Choose `eps`, find robbery hotspots and compare with HDBSCAN |
| Advanced | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/04c_KMeans_and_Comparing_Methods.ipynb" target="_blank">4c. K-Means and Comparing Methods</a> | Use the elbow method, a dendrogram and a side by side comparison |
