---
title: "Geocoding and OSM"
teaching: 100
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the various geocoding services that can be used?
- What are some limitations of the geocoding API?
- What are some categories of places that can be geocoded using the Open Street Map (OSM) API?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn how to use the Census geocoding API
- Learn how to use the Nominatim and Overpass APIs to access Open Street Map (OSM) data

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

The geocoding API is a service that converts street addresses into geographic coordinates (e.g., 610 Purdue Mall, West Lafayette, IN 47907, into latitudes and longitudes). This can be done either way, converting geographic coordinates back to street addresses.
Features of geocoding APIs include:

1. Geocoding – addresses to geographic coordinates.
2. Reverse geocoding – geographic coordinates to address.
3. Region Biasing – constrain results to specific region, county, or postal code.
4. Place IDs – place ID to address and vice versa.

There are several practical applications of geocoding such as:

1. Location Based Services: Delivery companies like Uber and DoorDash convert user addresses into GPS coordinates for efficient dispatching.
2. Mapping and GIS: Real estate platforms use geocoding to display properties of interest on maps.
3. Emergency Services: Police, fire, and medical services use geocoding for precise identification and to reduce response times by finding the fastest route.

Geocoding APIs also have certain limitations that need to be taken into account when using them. They are designed to convert predefined, static addresses and can result in an error if the address is incorrect. Several APIs may also impose daily usage limits for free access, potentially leading to additional charges if exceeded. Finally, geographic data involving geocoding needs to be constantly updated to avoid inaccuracies over time.

### Open Street Map
This tutorial demonstrates how to use the U.S. Census Geocoding API and OpenStreetMap (OSM) via the `geopy` library to convert addresses into geographic coordinates (latitude and longitude) and plot them using the `folium` library.

#### Part 1: Census Geocoding API
This part explores a simple walkthrough of geocoding street addresses using the Census API. The initial setup includes importing essential libraries:
- `geopy.geocoders`: Interfaces with OSM's Nominatim or Census API to geocode.
- `geopandas`: Manages geospatial data for spatial operations.
- `pandas`: Handles tabular data manipulation.
- `folium`: Creates interactive maps to visualize geocoded locations.
- `requests`: Facilitates HTTP requests to the U.S. Census Geocoding API.
- `time.sleep`: Introduces delays to avoid overwhelming APIs with rapid requests, especially for batch processing.

A function `get_coordinates` is defined to geocode a single address:
- Constructs an API request URL with parameters such as address, benchmark (e.g., Public_AR_Current for up-to-date public addresses), and JSON format.
- Sends a request and checks for a successful response (200).
- Parses the JSON response to extract coordinates if the address matches.
- Returns latitude (y) and longitude (x) if successful, or `None` if not.

The function is applied to a pandas DataFrame (e.g., reading a `museums.txt` file with museum names and addresses) to fetch coordinates, which are then added as new columns (Latitude and Longitude) for plotting with `folium`. Note that inaccurate addresses in the input file may result in `None` values.

#### Part 2: Open Street Map User Input Query
This part explores querying OSM for specific location types (e.g., supermarkets like Walmart) in a given area using the Overpass API. The notebook includes the following functions:

- **`get_locations`**: Queries the Overpass API[](http://overpass-api.de/api/interpreter) for locations matching specific categories, queries, and cities, with an optional "Brand" filter (e.g., "Walmart").
  - Creates an empty list `all_locations` to append addresses.
  - Checks for valid input and stops if invalid.
  - Sends requests and stores results in `all_locations`.

- **`plot_locations` and `display_locations`**: Import results (latitude, longitude, and name) from `all_locations`, displaying them using `folium` and saving them in a pandas DataFrame for further analysis.

The final part of the code accepts user input queries, which are passed through these functions to get results. Refer to the OSM query keywords webpage for desired outcomes (e.g., supermarkets in Indianapolis). An example use case is fetching coordinates of fast-food chains in West Lafayette by inputting relevant query words. This is reverse geocoding, as it passes a query to grab locations.

Regular geocoding was demonstrated by fetching coordinates based on addresses (e.g., random U.S. museums with their street addresses), passed to the Census Geocoder, which requires precise U.S. addresses to avoid null returns and unplotted locations. Locations of Supermarkets in Indianapolis were plotted using this OSM query approach.

::::::::::::::::::::::::::::::::::::: keypoints 

- Geocoding APIs can be used to identify locations of specific categories such as grocery stores or restaurants

::::::::::::::::::::::::::::::::::::::::::::::::

## Module Overview