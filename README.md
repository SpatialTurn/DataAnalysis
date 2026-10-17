# GIS Data Analysis for Social Scientists

Source for the **Data Analysis** module of
[Enabling the Geospatial Turn in the Social Sciences](https://spatialturn.github.io/),
published at <https://spatialturn.github.io/DataAnalysis/>.

The lesson is built with [The Carpentries Workbench](https://carpentries.github.io/sandpaper-docs/).

## Repository layout

| Folder | Contents |
|---|---|
| `episodes/` | The five lesson pages shown on the website |
| `notebooks/` | The 15 Google Colab notebooks linked from the episodes (`01a` to `05c`) |
| `data/` | Data the notebooks read directly from GitHub into memory |
| `learners/`, `instructors/` | Setup, glossary and instructor notes |

Notebooks open in Colab from
`https://colab.research.google.com/github/SpatialTurn/DataAnalysis/blob/main/notebooks/<name>.ipynb`
and read data from `https://raw.githubusercontent.com/SpatialTurn/DataAnalysis/main/data/<file>`.

| Episode file | Topic | Notebooks | Data |
|---|---|---|---|
| `introduction.md` | Geocoding and adding spatial context | `01a` to `01c` | `data/museums.txt`, OpenStreetMap |
| `networkanalysis.md` | Network analysis | `02a` to `02c` | OpenStreetMap |
| `pysal.md` | Spatial autocorrelation with PySAL | `03a` to `03c` | Census API and boundary files (learner's own API key) |
| `cluster.md` | Spatial clustering | `04a` to `04c` | `data/CrimesChicago20220225.csv` |
| `ndvi.md` | NDVI analysis | `05a` to `05c` | `data/landsat/band2.TIF` to `band5.TIF` |

The Landsat bands are 85 to 95 MB each, under GitHub's 100 MB file limit.

Contact: ilindsay@purdue.edu
