---
title: "Census Geocoding"
teaching: 100
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What is geocoding and why is it essential for census analysis?
- How can we convert addresses into spatial coordinates?
- How do we combine census data with OpenStreetMap features?
- How can spatial context improve demographic analysis?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand what geocoding is and how it works
- Convert address-based census data into geographic coordinates
- Query OpenStreetMap (OSM) features using Python
- Combine census points with OSM layers for spatial analysis
- Visualize geocoded census data alongside urban infrastructure

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Census and demographic datasets are often **non-spatial** — they exist as tables
containing addresses, place names, or administrative units. To analyze these data
geographically, we must first **geocode** them: converting text-based locations
into latitude and longitude coordinates.

Once census data are geocoded, they can be enriched with contextual information
from **OpenStreetMap (OSM)**, such as roads, buildings, parks, schools, or hospitals.
This enables deeper spatial insights into population distribution, accessibility,
and urban structure.

In this lesson, you will learn how to:

1. Geocode address-based census data
2. Convert results into spatial objects
3. Query OpenStreetMap features
4. Visualize census data in its geographic context

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Emphasize ethical use of geocoding services and rate limits. Remind learners that
geocoding accuracy varies by location and data quality.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Why Census Geocoding Matters ?

Census data becomes far more powerful when location is explicitly included.
Geocoding allows researchers to move from spreadsheets to spatial insight.

### What Census Geocoding Helps Us Understand

* Population distribution and density patterns  
* Access to services (schools, hospitals, transit, parks)  
* Spatial inequality and environmental justice  
* Urban growth and land-use change  
* Neighborhood-level demographic trends  
* Relationships between people and infrastructure  

### Why Researchers Combine Census Data with OSM

- Census data provides **who and what**
- OpenStreetMap provides **where and how**
- Together, they enable:
  - Accessibility studies
  - Urban planning analysis
  - Public health assessments
  - Infrastructure equity evaluations
  - Place-based policy analysis

Geocoding transforms census data from static tables into **spatial evidence**.

## Accessing the Census Data 

Before we can map or analyze census data spatially, we must first **obtain the data**.
The U.S. Census Bureau provides multiple ways to download data, including:

- Manual downloads from data.census.gov  
- Bulk file downloads  
- Programmatic access through official Census APIs  

In this lesson, we introduce **API-based census data access**, which allows users to
retrieve demographic and socioeconomic data directly into Python workflows.

### What Does “API-Based Census Access” Mean?

An API (Application Programming Interface) allows computers to request data directly
from a server using structured URLs. Instead of clicking through a web interface,
API calls return machine-readable data (e.g., JSON or CSV) that can be processed
automatically.

Using Census APIs, we can:

- Specify exactly **which variables** we want  
- Choose the **geographic scale** (state, county, tract, block group)  
- Automate downloads for reproducibility  
- Integrate census data directly into scripts and analyses  

This approach is especially useful for research, teaching, and large-scale analysis.

### How This Differs from Geocoding

| Census API Data Access | Geocoding |
|---|---|
| Downloads official census tables | Converts addresses → coordinates |
| Uses predefined census geographies | Uses external location services |
| Returns demographic attributes | Returns spatial point locations |
| Designed for aggregation | Designed for individual locations |
| Emphasizes consistency | Emphasizes positional accuracy |

In this notebook, we **do not geocode addresses**.  
Instead, we retrieve **attribute data that is already organized by census geography**.

### Why Use Census APIs Instead of Manual Downloads?

Researchers prefer APIs because they:

- Reduce manual steps and human error  
- Ensure **reproducibility** of results  
- Make workflows scalable and automatable  
- Allow direct integration with Python and GIS tools  
- Support consistent data retrieval across years  

For example, the same API query can be reused to:
- Update data annually
- Change geographic extent
- Swap variables without re-downloading files

### Typical Census API Workflow

1. Construct a request URL specifying variables and geography  
2. Send the request to the Census API endpoint  
3. Receive structured data (JSON or CSV)  
4. Convert results into a DataFrame  
5. Join data with geographic boundaries later (optional)  

At this stage, the data is **tabular**, not spatial — but ready to become spatial
through joins with census boundary files or mapping tools.

::::::::::::::::::::::::::::::::::::: callout

**Important**

Census APIs return **aggregate data only**.  
Individual-level records are never provided, ensuring privacy protection.

::::::::::::::::::::::::::::::::::::::::::::::::

### When to Use Census APIs

Use Census APIs when:

- You need demographic or socioeconomic variables
- You want official, authoritative data
- Reproducibility is important
- You plan to analyze multiple regions or years
- Data will later be joined to spatial boundaries

This API-based approach forms the **foundation** for spatial census analysis,
which we will build on in the remainder of this lesson.


## 1. Installing Required Libraries

```python
!pip install geopandas geopy osmnx matplotlib
```

## 2. Load Census Data or Address Data

```python 
import pandas as pd

df = pd.read_csv("census_addresses.csv")
df.head()
```
This dataset should contain an address column (e.g., street, city, state).


## 3. Geocode Addresses Using `Nominatim`

```python
from geopy.geocoders import Nominatim

geolocator = Nominatim(user_agent="census_geocoding_tutorial")

def geocode_address(address):
    try:
        location = geolocator.geocode(address)
        return location.latitude, location.longitude
    except:
        return None, None

df["lat"], df["lon"] = zip(*df["address"].apply(geocode_address))
```
*Note: Geocoding services may return None for incomplete or ambiguous addresses.*

## 4. Convert to a GeoDataFrame

```python
import geopandas as gpd

gdf = gpd.GeoDataFrame(
    df,
    geometry=gpd.points_from_xy(df.lon, df.lat),
    crs="EPSG:4326"
)

gdf.head()
```
Plot the geocoded points:
```python
gdf.plot(figsize=(6,6), color="red")
```

## 5. Query OpenStreetMap Features
OpenStreetMap provides free, global geographic data.

Example: download buildings in a city.
```python
import osmnx as ox

place = "Lafayette, Indiana, USA"

buildings = ox.geometries_from_place(
    place,
    tags={"building": True}
)
```
Plot buildings with census points:
```python
ax = buildings.plot(color="lightgray", figsize=(8,6))
gdf.plot(ax=ax, color="red", markersize=10)
```

## 6. Adding Spatial Context to Census Data
You can buffer census points to analyze nearby features.
```python
gdf_buffer = gdf.copy()
gdf_buffer["geometry"] = gdf_buffer.geometry.buffer(200)  # meters (after projection)
```
Spatial join example:
```python
join = gpd.sjoin(buildings, gdf_buffer, predicate="within")
join.head()
```
This links buildings to nearby census locations.


:::::::::::::::::::::::::::::::::::: challenge

Challenge 1 — Query a Different OSM Feature

Choose one:

* Roads → `{"highway": True}`

* Schools → `{"amenity": "school"}`

* Parks → `{"leisure": "park"}`

Plot the feature with census points.

:::::::::::::::::::::::: solution
```python
parks = ox.geometries_from_place(place, tags={"leisure": "park"})
parks.plot()
```
:::::::::::::::::::::::::::::::::

Challenge 2 — Accessibility Analysis

For each census point:

Create a buffer

Count how many buildings fall inside

Interpret spatial differences

:::::::::::::::::::::::: solution

Higher counts suggest higher accessibility or density.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Math
Geocoding transforms a text location `L` into coordinates (x,y)

Spatial joins evaluate relationships between geometries:

* within
* intersects
* contains
These operations allow census attributes to be analyzed spatially. 

::::::::::::::::::::::::::::::::::::: keypoints

* Geocoding converts census addresses into spatial coordinates

* GeoPandas enables spatial operations on tabular data

* OpenStreetMap provides rich contextual geographic layers

* Combining census + OSM reveals spatial patterns and inequality

* Spatial context transforms demographic data into actionable insight

::::::::::::::::::::::::::::::::::::::::::::::::



# Module Overview

| Lesson            | Overview                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------|
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/Address_based_geocode.ipynb" target="_blank">Beginner</a>  | Introduction to Address Geocoding |
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/OSM-Query_intermediate.ipynb" target="_blank">Intermediate</a>  | Introduction to OSM Overpass API |
| <a href="https://colab.research.google.com/github/SpatialTurn/DataCollection-Notebooks/blob/main/Census/OSM-Query_intermediate.ipynb" target="_blank">Advanced</a>  | Introduction to Advanced Batch-Geoding |