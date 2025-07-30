---
title: "Network Analysis"
teaching: 100
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can network analysis be used to assess food desert accessibility in different regions?
- What factors should be considered when calculating the shortest path between grocery stores and census tracts?
- How does the quality of OpenStreetMap data impact the accuracy of network analysis for urban planning?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Demonstrate how to use Python libraries like osmnx and networkx to perform road network analysis for food desert studies.
- Analyze the relationship between distance, transportation networks, and food accessibility using geospatial data.
- Evaluate the limitations of network analysis tools and suggest improvements for real-world applications.

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

::::::::::::::::::::::::::::::::::::: keypoints

- OSM is a great alternative to map real time locations using Folium feature of Python. 

::::::::::::::::::::::::::::::::::::::::::::::::

# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| [Basic](https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/Network_Analysis_Tutorial.ipynb) | Introduction to Network Analysis and color coding distances. |
| [Advanced](https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/OSM_Network_Tutorial.ipynb)  |  Obtains coordinates from OSM and uses centroid analysis to calculate distances and travel times for multiple points of interests. |