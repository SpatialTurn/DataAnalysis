---
title: "Spatial Autocorrelation with PySAL"
teaching: 50
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- What does it mean for data to be spatially autocorrelated?
- How do we tell the computer which places are neighbors?
- How do Global and Local Moran's I measure and locate clustering?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Build Rook, Queen and K-nearest neighbor spatial weights with PySAL
- Compute and interpret Global Moran's I and its p-value
- Read a Moran scatterplot
- Map statistically significant hotspots and coldspots with Local Moran's I (LISA)

::::::::::::::::::::::::::::::::::::::::::::::::

## Why spatial autocorrelation?

> "Everything is related to everything else, but near things are more related than distant things."
> (Tobler's first law of geography)

Most statistics assume observations are independent. Places are not: a county's income or median
age is usually similar to its neighbors'. **Spatial autocorrelation** measures how strong that
similarity is, and **PySAL** (the Python Spatial Analysis Library) provides the standard tools.

| Question | PySAL method |
|---|---|
| Who counts as a neighbor? | Spatial weights (`libpysal.weights`) |
| Is there clustering across the whole map? | Global Moran's I (`esda.Moran`) |
| Where are the hotspots and coldspots? | Local Moran's I, LISA (`esda.Moran_Local`) |
| Could the pattern be chance? | Permutation tests (`p_sim`) |

In this episode we use 2023 American Community Survey data for Indiana counties, loaded with a
Census API key (see the Census data session for how to get one).

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Spatial weights are the hardest idea here. Use the chess analogy (Rook moves straight, Queen also
moves diagonally) and draw a 3 by 3 grid before showing code. Also stress that an unfiltered LISA map
(without the p-value cutoff) shows many false hotspots.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Build spatial weights

`counties` is a GeoDataFrame of Indiana counties with a `median_age` column, projected to meters.

```python
from libpysal.weights import Rook, Queen, KNN

w_rook = Rook.from_dataframe(counties, use_index=True)    # shared edge
w_queen = Queen.from_dataframe(counties, use_index=True)  # shared edge or corner
w_knn = KNN.from_dataframe(counties, k=4)                 # 4 closest centers

print(w_rook.mean_neighbors, w_queen.mean_neighbors)
w_queen.transform = "R"    # row standardize: each county's weights sum to 1
```

## 2. Global Moran's I

```python
from esda.moran import Moran

mi = Moran(counties["median_age"], w_queen, permutations=999)
print(f"I = {mi.I:.3f}, p = {mi.p_sim:.3f}")
```

- I clearly above 0 with p below 0.05: similar values cluster together
- I near 0: no spatial pattern
- I below 0: neighbors tend to be different

```python
from splot.esda import moran_scatterplot
moran_scatterplot(mi, aspect_equal=True)
```

## 3. Local Moran's I (LISA)

```python
from esda.moran import Moran_Local
from splot.esda import lisa_cluster

lisa = Moran_Local(counties["median_age"], w_queen, permutations=999, seed=42)
lisa_cluster(lisa, counties, p=0.05)     # only significant counties are colored
```

Labels: **High-High** (hotspot), **Low-Low** (coldspot), **High-Low** and **Low-High** (outliers),
and **Not significant**.

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Rook or Queen?

Print the Rook and Queen neighbors of the same county, for example
`w_rook.neighbors[0]` and `w_queen.neighbors[0]`. Why can Queen never have fewer neighbors than Rook?

:::::::::::::::::::::::: solution

Queen counts every county that shares an edge (like Rook) **plus** any that only touch at a corner.
So it always includes all Rook neighbors, and sometimes a few more.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: A second variable

Compute Global Moran's I for `pct_asian`. Is it clustered? Then make its LISA map.

:::::::::::::::::::::::: solution

```python
mi2 = Moran(counties["pct_asian"], w_queen, permutations=999)
print(mi2.I, mi2.p_sim)
lisa_cluster(Moran_Local(counties["pct_asian"], w_queen, seed=42), counties, p=0.05)
```

A positive I with p below 0.05 means clustering. High-High counties usually sit around large
cities and universities.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math

Global Moran's I is

$$
I = \frac{N}{W} \cdot \frac{\sum_i \sum_j w_{ij}(x_i - \bar{x})(x_j - \bar{x})}{\sum_i (x_i - \bar{x})^2}
$$

where $N$ is the number of places, $w_{ij}$ is the weight between places $i$ and $j$ (non zero only
for neighbors), $W$ is the sum of all weights, and $x$ is the variable. The top of the fraction is
large when neighbors are on the same side of the mean.

::::::::::::::::::::::::::::::::::::: keypoints

- Spatial weights define who is a neighbor: Rook (edges), Queen (edges and corners), KNN (closest)
- Global Moran's I gives one number for clustering across the whole map
- The permutation p-value tells you whether the pattern could be chance
- Local Moran's I (LISA) locates hotspots, coldspots and outliers; always filter by p-value

::::::::::::::::::::::::::::::::::::::::::::::::

## Notebooks

| Level | Notebook | What you will do |
|---|---|---|
| Beginner | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/03a_Spatial_Weights_Neighbors.ipynb" target="_blank">3a. What Is a Neighbor? Spatial Weights</a> | Load Indiana counties and compare Rook, Queen and KNN neighbors |
| Intermediate | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/03b_Global_Morans_I.ipynb" target="_blank">3b. Global Moran's I</a> | Test whether median age is clustered and read the Moran scatterplot |
| Advanced | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/03c_Local_Morans_I_Hotspots.ipynb" target="_blank">3c. Local Moran's I and Hotspots</a> | Map significant clusters for counties, then for Marion County tracts |
