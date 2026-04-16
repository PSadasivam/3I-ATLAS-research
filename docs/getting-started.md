# Getting Started — 3I/ATLAS Research Notebook

## Prerequisites

- **Python 3.10+** (tested with 3.13)
- **Jupyter Notebook** or **VS Code** with the Jupyter extension
- Internet connection (for live data queries to JPL Horizons and MAST)

## 1. Environment Setup

```bash
# Clone or navigate to the project
cd 3I-Atlas-Research

# Create virtual environment
python -m venv .venv

# Activate
# Linux/macOS:
source .venv/bin/activate
# Windows:
.venv\Scripts\activate

# Install dependencies
pip install astropy astroquery matplotlib pandas numpy
```

## 2. Running the Notebook

### Option A: VS Code (Recommended)

1. Open `3I-Atlas-Research.code-workspace` in VS Code
2. Open `3I_ATLAS_research_notebook.ipynb`
3. Select the `.venv` Python kernel when prompted
4. Click **Run All** or run cells individually

### Option B: Jupyter

```bash
pip install jupyter
jupyter notebook 3I_ATLAS_research_notebook.ipynb
```

## 3. Notebook Walkthrough

The notebook has 6 stages:

| Cell(s) | Stage | What it does |
|---------|-------|-------------|
| 1–2 | Setup | Imports libraries, sets date window and output directory |
| 3 | Dependencies | Loads `astroquery` for JPL Horizons and MAST access |
| 4–5 | Ephemerides | Queries JPL Horizons for C/2025 N1 positions, distances, magnitude |
| 6–7 | MAST Search | Queries HST and JWST observation archives |
| 8–9 | Catalog Build | Normalizes data → CSVs + SQLite database |
| 10–11 | Visualization | Generates trajectory and brightness plots |

## 4. Outputs

After running all cells, check `outputs_3I_ATLAS/`:

```
outputs_3I_ATLAS/
├── ephemerides.csv          # Daily positions and brightness
├── mast_catalog.csv         # HST/JWST observation records
├── dataset_catalog.csv      # Master index
├── catalog.sqlite           # Queryable SQLite database
├── trajectory_ra_dec.png    # RA vs Dec sky plot
└── brightness_timeline.png  # V magnitude over time
```

## 5. Querying the SQLite Database

```python
import sqlite3
import pandas as pd

conn = sqlite3.connect('outputs_3I_ATLAS/catalog.sqlite')

# Ephemerides
eph = pd.read_sql('SELECT * FROM ephemerides', conn)
print(eph[['datetime_str', 'RA', 'DEC', 'V']].head())

# MAST observations
obs = pd.read_sql('SELECT * FROM mast_observations', conn)
print(obs[['obsid', 'mission', 'instrument_name', 'filters']].head())

conn.close()
```

## 6. Reference Data (Already Included)

These files are pre-populated and don't require running the notebook:

| File | Description |
|------|-------------|
| `ephemerides.csv` | Cached JPL Horizons results |
| `mast_catalog.csv` | Cached MAST query results |
| `3I_ATLAS_Public_Datasets.csv` | Curated list of public data links |
| `3I_ATLAS_Public_Datasets_with_Metadata.csv` | Extended version with instrument/filter details |
| `Mpc-metadata/id.json` | MPC object identification |
| `Mpc-metadata/3I_mpc_orb.json` | Orbital elements (Cartesian state vector) |
| `Mpc-metadata/3I.obs` | Raw astrometric observations |

## 7. Offline Mode

If JPL Horizons or MAST is unreachable, the notebook falls back to the cached CSVs in the project root. The pre-existing `ephemerides.csv` and `mast_catalog.csv` will be used for catalog building and visualization.

## 8. Troubleshooting

| Issue | Solution |
|-------|----------|
| `ModuleNotFoundError: astroquery` | Run `pip install astroquery` in your venv |
| `ConnectionError` from Horizons | Check internet; the notebook has fallback to cached data |
| MAST query timeout | Default is 120s; retry or use cached `mast_catalog.csv` |
| Plots not displaying in VS Code | Ensure `matplotlib` is installed; restart the kernel |
| SQLite file locked | Close any other connections to `catalog.sqlite` |
