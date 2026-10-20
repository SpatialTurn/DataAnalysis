# GIS Data Analysis for Social Scientists

Source for the **Data Analysis** module of
[Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
published at <https://spatialturn.github.io/DataAnalysis/>.

The lesson is built with [The Carpentries Workbench](https://carpentries.github.io/sandpaper-docs/).

## Repository layout

| Folder | Contents |
|---|---|
| `episodes/` | The six lesson pages shown on the website |
| `notebooks/` | The 15 Google Colab notebooks linked from the episodes (`01a` to `01c`, `03a` to `06c`) |
| `data/` | Data the notebooks read directly from GitHub into memory |
| `learners/`, `instructors/` | Setup, glossary and instructor notes |

Notebooks open in Colab from
`https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/<name>.ipynb`
and read data from `https://raw.githubusercontent.com/SpatialTurn/DataAnalysis/main/data/<file>`.

| Episode file | Topic | Notebooks | Data |
|---|---|---|---|
| `geoprocessing-python.md` | Geoprocessing in Python (intermediate) | `01a` to `01c` | Foundations and CaseStudies `data/` (Lafayette stores, city limits, Tippecanoe tracts with ACS 2024) |
| `geoprocessing-qgis.md` | Geoprocessing in QGIS (intermediate, same data and results) | none | as above |
| `networkanalysis.md` | Network analysis | `03a` to `03c` | OpenStreetMap |
| `pysal.md` | Spatial autocorrelation with PySAL | `04a` to `04c` | Census API and boundary files (learner's own API key) |
| `cluster.md` | Spatial clustering | `05a` to `05c` | `data/CrimesChicago20220225.csv` |
| `ndvi.md` | NDVI analysis | `06a` to `06c` | `data/landsat/band2.TIF` to `band5.TIF` |

Numbers `02a` and `02b` are reserved for the Raster Analysis episode (planned). In October 2026 (Step 8 of
the rework) the advanced notebooks were renumbered from `01a` to `04c` to `03a` to `06c` to make room for the
new intermediate episodes.

In October 2026 the geocoding episode (`introduction.md`, notebooks `01a` to `01c` and `data/museums.txt`)
moved to the [Data Collection module](https://github.com/SpatialTurn/DataCollection), and the remaining
notebooks were renumbered (`02a` became `01a`, and so on).

The Landsat bands are 85 to 95 MB each, under GitHub's 100 MB file limit.

The diagrams named `da_*.png` in `episodes/` (October 2026) were drawn with Python
(`SpatialTurn_build_scripts/figure_builders/da_figures.py`); the clustering points are synthetic and the
spectral curves are illustrative. Each has a `-dark` copy for the site's dark mode.
`da_geoprocessing_ops.png` comes from `figure_builders/da_geoprocessing_figures.py`. The `gp_qgis_*.png`
screenshots were taken in QGIS 4.0 (light and dark UI) through the QGIS MCP plugin; `gp_qgis_result.png` is a
QGIS render composed by `figure_builders/gp_result_fig.py`. Notebooks `01a` to `01c` are written by
`notebook_builders/da_geoprocessing.py`.

Contact: ilindsay@purdue.edu
