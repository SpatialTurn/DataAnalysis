---
title: "Network Analysis"
teaching: 50
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- How can a road map be represented as a graph?
- How do we download and visualize road networks from OpenStreetMap?
- How do we find the shortest route and measure travel time?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain nodes and edges in a road network
- Download a drivable network with OSMnx and inspect it as GeoDataFrames
- Compute shortest paths by distance and by travel time
- Measure travel time from many locations to one point, the basis of an access study

::::::::::::::::::::::::::::::::::::::::::::::::

## Why network analysis?

Straight line distance is misleading: a store 1 km away across a river may be a 6 km drive.
Network analysis measures distance **along roads**, which is how people actually travel.

Road networks are stored as **graphs**:

- **Nodes** are intersections and dead ends
- **Edges** are road segments, each with a length (and, once added, a speed and travel time)

Common uses include food and health care access, emergency response times, and transportation
planning.

::::::::::::::::::::::::::::::::::::: callout

## The same analysis in QGIS

The Case Studies module's [grocery access case study](https://spatialturn.github.io/CaseStudyFoodDesert/grocery-network.html) measures
shortest paths and service areas to supermarkets in Lafayette and West Lafayette with QGIS's network
tools, without writing code. Doing both is a good way to see what OSMnx does behind the scenes.

If you have not used OpenStreetMap tags before, the Data Collection module's
[Geocoding episode](https://spatialturn.github.io/DataCollection/geocoding.html) introduces them.

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Draw a small graph on the board (four intersections, five roads) before showing any code.
Learners grasp `nearest_nodes` and `shortest_path` much faster once they can picture nodes and edges.
Note that `nearest_nodes` takes longitude first.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Download a road network

```python
import osmnx as ox
import networkx as nx

G = ox.graph_from_place("West Lafayette, Indiana, USA", network_type="drive")
print(G.number_of_nodes(), "nodes,", G.number_of_edges(), "edges")
ox.plot_graph(G, node_size=5, edge_color="gray", bgcolor="white")
```

`network_type` can also be `"walk"` or `"bike"`. The graph is held in memory as `G`.

## 2. Look at nodes and edges as tables

```python
nodes, edges = ox.graph_to_gdfs(G)
edges[["name", "highway", "length"]].head()    # length is in meters
```

## 3. Add speeds and travel times

```python
G = ox.add_edge_speeds(G, fallback=40)   # km/h used where a road type has no speed data
G = ox.add_edge_travel_times(G)          # adds travel_time in seconds to every edge
```

## 4. Find the shortest route between two points

```python
start = ox.nearest_nodes(G, X=-86.9112, Y=40.4247)   # longitude, latitude
end = ox.nearest_nodes(G, X=-86.9290, Y=40.4580)

route = ox.shortest_path(G, start, end, weight="travel_time")
ox.plot_graph_route(G, route, route_color="red", node_size=0)
```

## 5. Measure the route

```python
route_edges = ox.routing.route_to_gdf(G, route)
print(round(route_edges["length"].sum() / 1000, 2), "km")
print(round(route_edges["travel_time"].sum() / 60, 1), "minutes")
```

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Shortest or fastest?

Compute the route between the same two points with `weight="length"` and with
`weight="travel_time"`. Are they the same route? Which is longer in kilometers?

:::::::::::::::::::::::: solution

```python
for w in ["length", "travel_time"]:
    e = ox.routing.route_to_gdf(G, ox.shortest_path(G, start, end, weight=w))
    print(w, round(e["length"].sum() / 1000, 2), "km",
          round(e["travel_time"].sum() / 60, 1), "min")
```

The fastest route often uses faster main roads and can be a little longer in distance.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: Travel time to everywhere

Use `nx.single_source_dijkstra_path_length(G, start, weight="travel_time")` to get the travel time
from `start` to every node. What is the longest drive time (in minutes) within the city?

:::::::::::::::::::::::: solution

```python
times = nx.single_source_dijkstra_path_length(G, start, weight="travel_time")
print(round(max(times.values()) / 60, 1), "minutes")
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math

A network is a graph $G = (V, E)$, where $V$ is the set of nodes (intersections) and $E$ the set of
edges (road segments). Each edge $e$ has a weight $w_e$, such as length or travel time.
The shortest path between two nodes is the route whose total weight $\sum_{e \in \text{route}} w_e$
is smallest. OSMnx finds it with Dijkstra's algorithm.

::::::::::::::::::::::::::::::::::::: keypoints

- Road networks are graphs of nodes (intersections) and edges (road segments)
- `ox.graph_from_place` downloads OpenStreetMap roads for any named place
- `add_edge_speeds` and `add_edge_travel_times` let you route by time instead of distance
- `nearest_nodes` (longitude first), `shortest_path` and `route_to_gdf` cover most routing tasks
- Network distance is more realistic than straight line distance for access studies

::::::::::::::::::::::::::::::::::::::::::::::::

## Notebooks

| Level | Notebook | What you will do |
|---|---|---|
| Beginner | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/01a_Road_Networks_Basics.ipynb" target="_blank">1a. Road Networks: Nodes and Edges</a> | Download, plot, inspect and save a road network |
| Intermediate | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/01b_Shortest_Paths_and_Travel_Time.ipynb" target="_blank">1b. Shortest Paths and Travel Time</a> | Route between two points and map travel time across the city |
| Advanced | <a href="https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/01c_Travel_Time_to_Stores.ipynb" target="_blank">1c. Travel Time to Grocery Stores</a> | Route from every OSM grocery store to the town center |
