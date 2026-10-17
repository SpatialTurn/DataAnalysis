---
title: 'Reference'
---

## Glossary

API (Application Programming Interface)
:   A web address that returns data to a program instead of a web page. The Census data API and the
    Overpass API are examples.

Census tract
:   A small statistical area defined by the U.S. Census Bureau, usually with 1,200 to 8,000 people.

Coordinate Reference System (CRS)
:   The system that links coordinates to places on Earth. `EPSG:4326` is latitude and longitude;
    UTM zones (such as `EPSG:32616`) use meters.

DBSCAN
:   A density based clustering method that groups points with many neighbors within a radius (`eps`)
    and labels isolated points as noise.

Edge
:   In a network, a connection between two nodes. In a road network, a road segment.

GeoDataFrame
:   A `pandas` DataFrame with a `geometry` column, provided by GeoPandas.

HDBSCAN
:   A version of DBSCAN that tests many distances and keeps stable clusters, so you do not choose `eps`.

K-Means
:   A clustering method that splits points into a chosen number (k) of groups around central points.

LISA (Local Indicators of Spatial Association)
:   Local Moran's I values for each place, used to map hotspots (High-High), coldspots (Low-Low)
    and outliers.

Moran's I
:   A statistic that measures spatial autocorrelation: positive when similar values are near each
    other, near zero when there is no pattern.

NDVI (Normalized Difference Vegetation Index)
:   (NIR - Red) / (NIR + Red), a measure of vegetation greenness from satellite bands.

Near infrared (NIR)
:   Light just beyond what the eye can see. Healthy vegetation reflects it strongly.

Node
:   In a network, a point where edges meet. In a road network, an intersection or dead end.

OpenStreetMap (OSM)
:   A free, volunteer built map of the world. Features are described by `key=value` tags such as
    `shop=supermarket`.

Overpass API
:   The query service used to search and download OpenStreetMap data.

Permutation test
:   A significance test that shuffles values many times to see how often a result as strong as the
    real one appears by chance. Its result is a pseudo p-value (`p_sim`).

Projection (reprojecting)
:   Converting data from one CRS to another, for example from degrees to meters with `to_crs()`.

Raster
:   A grid of cells (pixels), each holding a value. Satellite images and elevation models are rasters.

Rate limit
:   The maximum number of requests a web service accepts in a period, for example one per second.

Reflectance
:   The share of incoming light a surface reflects, from 0 to 1.

Spatial autocorrelation
:   The tendency of nearby places to have similar (positive) or different (negative) values.

Spatial weights (W)
:   A record of which places are neighbors. Common types are Rook, Queen and K-nearest neighbors.

Vector data
:   Data stored as points, lines or polygons, such as addresses, roads or county boundaries.

## Library documentation

- [GeoPandas](https://geopandas.org/)
- [OSMnx](https://osmnx.readthedocs.io/)
- [NetworkX](https://networkx.org/documentation/stable/)
- [PySAL](https://pysal.org/) (libpysal, esda, splot)
- [scikit-learn clustering](https://scikit-learn.org/stable/modules/clustering.html)
- [rasterio](https://rasterio.readthedocs.io/)
- [OpenStreetMap Map Features](https://wiki.openstreetmap.org/wiki/Map_features)
