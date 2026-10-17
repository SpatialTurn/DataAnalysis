---
title: "Geocoding and Adding Spatial Context"
teaching: 50
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions

- What is geocoding, and why do social scientists need it?
- How do we turn a list of addresses into points on a map?
- How can OpenStreetMap add context such as stores, schools or parks?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Geocode U.S. addresses with the free U.S. Census Geocoder
- Check match rates and find addresses that failed
- Turn coordinates into a GeoDataFrame and map it
- Download features from OpenStreetMap by tag
- Handle rate limits and ambiguous place names when geocoding many places

::::::::::::::::::::::::::::::::::::::::::::::::

## Why geocoding matters

Most social science data has a location written as text: a survey respondent's address, a list of
clinics, a business directory. **Geocoding** turns that text into latitude and longitude so the data
can be mapped and analyzed spatially.

| Input (text) | Output (coordinates) |
|---|---|
| `1600 Pennsylvania Ave NW, Washington, DC 20500` | 38.8977, -77.0365 |

Once your data are points, you can combine them with **OpenStreetMap (OSM)** layers. Census data
tells you *who* lives in an area; OSM tells you *what is there* (stores, schools, transit).
Together they support access studies, public health work and planning research.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Stress responsible use of free services: one request per second for Nominatim, and no bulk
geocoding of personal addresses without considering privacy. Remind learners that match rates
depend heavily on address quality.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## 1. Geocode one address with the Census Geocoder

```python
import requests

def get_coordinates(address):
    reply = requests.get(
        "https://geocoding.geo.census.gov/geocoder/locations/onelineaddress",
        params={"address": address, "benchmark": "Public_AR_Current", "format": "json"},
        timeout=30,
    )
    matches = reply.json()["result"]["addressMatches"]
    if matches:
        point = matches[0]["coordinates"]
        return point["y"], point["x"]   # latitude, longitude
    return None, None

get_coordinates("610 Purdue Mall, West Lafayette, IN 47907")
```

## 2. Geocode a list and turn it into map points

```python
import pandas as pd
import geopandas as gpd

museums = pd.read_csv("museums.txt", sep="\t", header=None, names=["Name", "Address"])
results = museums["Address"].apply(get_coordinates)
museums["Latitude"] = [lat for lat, lon in results]
museums["Longitude"] = [lon for lat, lon in results]

matched = museums.dropna(subset=["Latitude"])
points = gpd.GeoDataFrame(
    matched,
    geometry=gpd.points_from_xy(matched["Longitude"], matched["Latitude"]),
    crs="EPSG:4326",     # latitude and longitude
)
points.explore(tooltip="Name")
```

Always check the rows that did not match (`museums[museums["Latitude"].isna()]`). Incomplete or
landmark style addresses are the usual cause.

## 3. Add context from OpenStreetMap

OSM features are described by **tags** such as `shop=supermarket` or `amenity=school`
(see the [Map Features list](https://wiki.openstreetmap.org/wiki/Map_features)).

```python
import osmnx as ox

stores = ox.features_from_place("West Lafayette, Indiana, USA", tags={"shop": "supermarket"})
stores = stores.reset_index()[["name", "geometry"]]
stores["geometry"] = stores.geometry.representative_point()   # polygons to points
stores.explore(tooltip="name", color="green")
```

## 4. Measure what is nearby

To count features within a distance, first **reproject to meters**. Buffers on latitude and
longitude would be measured in degrees.

```python
lat, lon = get_coordinates("610 Purdue Mall, West Lafayette, IN 47907")
campus = gpd.GeoDataFrame(geometry=gpd.points_from_xy([lon], [lat]), crs="EPSG:4326")

campus_m = campus.to_crs(epsg=5070)      # USA Contiguous Albers, units are meters
stores_m = stores.to_crs(epsg=5070)

one_mile = campus_m.buffer(1600).iloc[0]          # circle with a 1 mile (1,600 m) radius
stores_m[stores_m.within(one_mile)]               # supermarkets within 1 mile of campus
```

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Query a different OSM feature

Download schools (`amenity=school`) or parks (`leisure=park`) for West Lafayette and map them.

:::::::::::::::::::::::: solution

```python
parks = ox.features_from_place("West Lafayette, Indiana, USA", tags={"leisure": "park"})
parks.explore(color="darkgreen")
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: Why did it fail?

One museum in the list is written as `10th St. & Constitution Ave. NW, Washington, DC 20560`.
Why might the Census Geocoder fail on it, and how would you fix it?

:::::::::::::::::::::::: solution

The geocoder expects a numbered street address, not an intersection. Rewriting it as
`1000 Madison Dr NW, Washington, DC 20560` gives a match.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Geocoding converts text addresses into coordinates
- The U.S. Census Geocoder is free and works best with complete U.S. street addresses
- Always check match rates and inspect failed addresses
- OpenStreetMap tags (`key=value`) let you download context layers such as stores or schools
- Reproject to a meter based CRS before buffering or measuring distance

::::::::::::::::::::::::::::::::::::::::::::::::

## Notebooks

Work through these in order. Each one opens in Google Colab.

| Level | Notebook | What you will do |
|---|---|---|
| Beginner | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/01a_Geocoding_Addresses.ipynb" target="_blank">1a. Geocoding Addresses</a> | Geocode museum addresses with the Census Geocoder and map them |
| Intermediate | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/01b_Finding_Places_with_OSM.ipynb" target="_blank">1b. Finding Places with OpenStreetMap</a> | Download supermarkets by tag and reverse geocode a point |
| Advanced | <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/DataAnalysis/01c_Batch_Geocoding_and_Ambiguous_Names.ipynb" target="_blank">1c. Batch Geocoding and Ambiguous Names</a> | Geocode many places politely and resolve names like Springfield |
