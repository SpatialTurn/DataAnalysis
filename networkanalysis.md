---
title: "Network Analysis"
teaching: 100
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- How do we download and visualize road networks with OSM data?
- What is a graph network and how is it represented in Python?
- How can we compute shortest paths and network distances?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn how to retrieve OpenStreetMap road data using OSMnx
- Convert road networks into graphs for routing and analysis
- Visualize networks and shortest paths on a map
- Compute route distances and travel time across a network

::::::::::::::::::::::::::::::::::::::::::::::::

## Overview
This tutorial provides a practical introduction to performing road network analysis using Python, focusing on analyzing road networks in a specified area (e.g., West Lafayette, Indiana) to study food deserts. It uses libraries such as `networkx`, `osmnx`, `folium`, `pandas`, `geopandas`, and `matplotlib` to fetch, visualize, and analyze road networks, compute centroid nodes, and calculate the shortest path based on travel time. The tutorial also applies this analysis to food accessibility data in Indiana.

The coordinates of grocery stores in Indiana were fetched using OpenStreetMap (OSM). Network analysis is used to calculate distances and times from grocery store locations to the center of areas of interest, considering factors such as the number of supermarkets, income, and vehicle accessibility. Distances are classified as good (around 1 mile) or low-accessible (over 5 miles), depending on rural or urban settings. Low Income and Low Access maps were created for each census tract in Indiana and compared to the USDA food desert dataset.

#### Why Network Analysis for Food Deserts?
1. **Mapping Accessibility**: Models connections between grocery stores and transportation systems to identify areas with limited healthy food access due to distance or lack of transportation.
2. **Area Development**: Helps improve accessibility and quality of life in underserved regions.
3. **Promotes Equity**: Highlights disparities to create solutions for equitable access to nutritious food.
4. **Optimization of Resources**: Ensures equal distribution of resources for all individuals.

#### Environment Setup
Libraries imported for this tutorial:
- `osmnx`: Fetches and processes OpenStreetMap road network data.
- `networkx`: Performs graph-based computations, such as shortest path calculations.
- `folium`: Enables interactive map visualizations.
- `geopandas` and `shapely`: Handle geospatial data and geometry operations.
- `matplotlib`: Generates static plots, including network visualization.
- `geopy`: Calculates geodesic distances for spatial analysis.

#### Data Acquisition
The road network for West Lafayette, Indiana, is fetched using `ox.graph.from_place("West Lafayette, Indiana", network_type="drive")`, retrieving the drivable road network from OpenStreetMap as a graph (nodes as intersections, edges as road segments). The graph can be saved as a GraphML file (e.g., `westlafayette_indiana_network.graphml`) using `ox.save_graphml` to avoid redundant downloads. This can be adapted for any U.S. area with a single line of code.

#### Applications
- **Urban Planning**: Analyzing road connectivity and accessibility in cities.
- **Transportation Studies**: Optimizing routes based on travel time or distance.
- **Geospatial Analysis**: Studying spatial relationships in infrastructure networks.
- **Emergency Response**: Identifying the fastest routes for first responders.

#### Visualization
A folium map example shows a blue star indicating the centroid of West Lafayette to a random point (green flag), with the shortest path marked by a red line/polygon.

#### Limitations
- **Data Dependency**: Relies on OpenStreetMap data, which may vary in quality or availability by region.
- **Performance**: Large networks may require significant computational resources for fetching and processing.


## Introduction

Network analysis allows us to study movement, connectivity, and accessibility across
geographic space. Roads, sidewalks, rivers, power lines, and transit systems can be
modeled as **graphs**, where intersections are *nodes* and paths are *edges*.

This lesson demonstrates how to:

1. Download a road network using **OSMnx**
2. Convert it into a **graph using NetworkX**
3. Visualize the network
4. Run **shortest path routing** between two locations

---

## 1. Install Required Libraries

```python
!pip install osmnx networkx matplotlib
```

## 2. Import Libraries 
```python
import osmnx as ox
import networkx as nx
import matplotlib.pyplot as plt
```

## 3. Download a Road Network from OpenStreetMap
```python
place = "West Lafayette, Indiana, USA"

G = ox.graph_from_place(place, network_type="drive")
```
Visualize network:
```python
fig, ax = ox.plot_graph(G, node_size=5, edge_color="gray")
```

## 4. Convert the Graph to Nodes and Edges GeoDataFrames
```python
nodes, edges = ox.graph_to_gdfs(G)
nodes.head(), edges.head()
```
Plot edges alone:
```python
edges.plot(figsize=(8,6), linewidth=0.8)
plt.title("Road Network")
plt.show()
```

## 5. Find Shortest Route Between Two Points
Choose two coordinates manually or by clicking on a map.
```python
orig = ox.distance.nearest_nodes(G, -86.9145, 40.4253)  # lon, lat
dest = ox.distance.nearest_nodes(G, -86.9079, 40.4268)
```
Calculate shortest path:
```python
route = nx.shortest_path(G, orig, dest, weight="length")
```
Plot route:
```python
ox.plot_graph_route(G, route, route_color="red")
```

::::::::::::::::::::::::::::::::::::: challenge

Challenge 1 — Try Your Own Route

* Pick any two points in a city of your choice.
* Compute and visualize the shortest path between them.

:::::::::::::::::::::::: solution
```python
orig = ox.distance.nearest_nodes(G, lon1, lat1)
dest = ox.distance.nearest_nodes(G, lon2, lat2)
route = nx.shortest_path(G, orig, dest, weight="length")
ox.plot_graph_route(G, route)
```
:::::::::::::::::::::::::::::::::

Challenge 2 — Estimate Travel Distance

Using your computed route, calculate the total path length:
```python
total_length = sum(
    ox.utils_graph.get_route_edge_attributes(G, route, "length")
)
print("Route length (meters):", total_length)
```
:::::::::::::::::::::::: solution
Convert meters → km:
```python
print(total_length/1000, "km")
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math

A network is represented as a graph:

G = (V,E)

Where:

* V = set of nodes (intersections)
* E = edges (roads)

Shortest path = minimum weighted path across `E`.

::::::::::::::::::::::::::::::::::::: keypoints

* OSMnx simplifies downloading and converting OSM road networks

* Graphs model movement and connectivity in space

* NetworkX allows shortest path and routing analysis

* Visualization helps interpret accessibility patterns

::::::::::::::::::::::::::::::::::::::::::::::::

# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/Network_Analysis_Tutorial.ipynb" target="_blank">Beginner</a> | Introduction to Network Analysis and color coding distances. |
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/OSM_Network_Tutorial.ipynb" target="_blank">Advanced</a>  |  Obtains coordinates from OSM and uses centroid analysis to calculate distances and travel times for multiple points of interests. |
