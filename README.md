# GIS Data Analysis for Social Scientists

Source for the **Data Analysis** module of
[Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
published at <https://spatialturn.github.io/DataAnalysis/>.

The lesson is built with [The Carpentries Workbench](https://carpentries.github.io/sandpaper-docs/).

## Repository layout

| Folder | Contents |
|---|---|
| `episodes/` | The four lesson pages shown on the website |
| `notebooks/` | The 12 Google Colab notebooks linked from the episodes (`01a` to `04c`) |
| `data/` | Data the notebooks read directly from GitHub into memory |
| `learners/`, `instructors/` | Setup, glossary and instructor notes |

Notebooks open in Colab from
`https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/<name>.ipynb`
and read data from `https://raw.githubusercontent.com/SpatialTurn/DataAnalysis/main/data/<file>`.

| Episode file | Topic | Notebooks | Data |
|---|---|---|---|
| `networkanalysis.md` | Network analysis | `01a` to `01c` | OpenStreetMap |
| `pysal.md` | Spatial autocorrelation with PySAL | `02a` to `02c` | Census API and boundary files (learner's own API key) |
| `cluster.md` | Spatial clustering | `03a` to `03c` | `data/CrimesChicago20220225.csv` |
| `ndvi.md` | NDVI analysis | `04a` to `04c` | `data/landsat/band2.TIF` to `band5.TIF` |

In October 2026 (Step 8) intermediate Geoprocessing and Raster Analysis episodes were added here and the
notebooks below were renumbered `03a` to `06c`. A few days later (Step 9) those intermediate episodes moved to the
new [Data Processing module](https://github.com/SpatialTurn/DataProcessing), and the notebooks went back to
`01a` to `04c`.

In October 2026 the geocoding episode (`introduction.md`, notebooks `01a` to `01c` and `data/museums.txt`)
moved to the [Data Collection module](https://github.com/SpatialTurn/DataCollection), and the remaining
notebooks were renumbered (`02a` became `01a`, and so on).

The Landsat bands are 85 to 95 MB each, under GitHub's 100 MB file limit.

The diagrams named `da_*.png` in `episodes/` (October 2026) were drawn with Python
(`SpatialTurn_build_scripts/figure_builders/da_figures.py`); the clustering points are synthetic and the
spectral curves are illustrative. Each has a `-dark` copy for the site's dark mode.

Contact: ilindsay@purdue.edu
