# 3I/ATLAS (C/2025 N1) Research Notebook

A data aggregation and analysis pipeline for **3I/ATLAS** — the third known interstellar object to visit our solar system, discovered by the ATLAS survey in 2025.

**ORCID**: [https://orcid.org/0009-0008-0069-693X](https://orcid.org/0009-0008-0069-693X)

## Overview

This project programmatically fetches, catalogs, and visualizes observational data for interstellar comet C/2025 N1 (ATLAS). It combines predicted ephemerides from NASA JPL with actual space telescope observations from HST and JWST into a unified research dataset.

### What makes 3I/ATLAS special?

- Only the **third interstellar object** ever detected (after 1I/'Oumuamua and 2I/Borisov)
- Observed by both **Hubble** (WFC3/UVIS imaging) and **JWST** (NIRSpec spectroscopy)
- Spectroscopy revealed H₂O, CO₂, and CO — key volatiles for understanding interstellar chemistry

## Data Pipeline

The Jupyter notebook (`3I_ATLAS_research_notebook.ipynb`) runs a complete pipeline:

1. **Ephemeris Fetch** — Queries JPL Horizons for predicted RA/Dec, heliocentric distance, observer distance, and apparent magnitude over a 7-day rolling window
2. **MAST Archive Search** — Queries the Mikulski Archive for Space Telescopes for HST and JWST observations (images + spectra)
3. **Catalog Building** — Normalizes both sources into CSVs and a SQLite database with `ephemerides` and `mast_observations` tables
4. **Visualization** — Generates sky trajectory (RA vs Dec) and brightness timeline (V magnitude vs time) plots

## Data Sources

| Source | What it provides |
|--------|-----------------|
| **JPL Horizons** | Predicted positions, distances, apparent magnitude |
| **MAST (STScI)** | HST WFC3/UVIS imaging, JWST NIRSpec spectroscopy metadata |
| **Minor Planet Center** | Orbital elements (Cartesian state vector), raw astrometric observations |

## Repository Structure

```
3I-Atlas-Research/
├── README.md                                    # This file
├── docs/
│   └── getting-started.md                       # Setup and usage guide
├── 3I_ATLAS_research_notebook.ipynb             # Main analysis notebook
├── 3I-Atlas-Research.code-workspace             # VS Code workspace config
│
├── Reference Data:
├── 3I_ATLAS_Public_Datasets.csv                 # Public dataset links (HST, JWST, NASA)
├── 3I_ATLAS_Public_Datasets_with_Metadata.csv   # Extended version with instrument details
├── ephemerides.csv                              # JPL Horizons position/brightness data
├── mast_catalog.csv                             # HST & JWST observation records
│
├── Mpc-metadata/                                # Minor Planet Center data
│   ├── id.json                                  # Object identification (IAU/MPC designations)
│   ├── 3I_mpc_orb.json                          # Orbital elements & covariance matrix
│   └── 3I.obs                                   # Raw astrometric observations
│
└── outputs_3I_ATLAS/                            # Generated outputs
    ├── ephemerides.csv                          # Processed ephemeris table
    ├── mast_catalog.csv                         # Processed MAST observations
    ├── dataset_catalog.csv                      # Master index of all datasets
    ├── catalog.sqlite                           # SQLite database (2 tables)
    ├── trajectory_ra_dec.png                    # Sky position plot
    └── brightness_timeline.png                  # Apparent magnitude vs time
```

## Key Observations

| Telescope | Date | Instrument | Details |
|-----------|------|------------|---------|
| **HST** | 2025-07-21 | WFC3/UVIS | F350LP filter, 0.04″/pix, Proposal GO 17830 |
| **JWST** | 2025-08-06 | NIRSpec | 0.6–5.3 µm spectroscopy, H₂O/CO₂/CO detected |

## MPC Object Identity

| Field | Value |
|-------|-------|
| IAU Designation | (3I) |
| Name | ATLAS |
| Provisional | C/2025 N1 |
| MPC Packed ID | 0003I |
| Classification | Interstellar object |

## Outputs

- **`trajectory_ra_dec.png`** — RA vs Dec sky plane trajectory (RA inverted per astronomical convention)
- **`brightness_timeline.png`** — V magnitude vs time (inverted axis: lower = brighter)
- **`catalog.sqlite`** — Queryable database with `ephemerides` and `mast_observations` tables
- **CSVs** — Flat files for import into any analysis tool

## Technical Requirements

- Python 3.10+
- `astropy` — Astronomical calculations
- `astroquery` — JPL Horizons and MAST programmatic access
- `matplotlib` — Plotting
- `pandas` — Data manipulation
- `numpy` — Numerical computing

## References

- **JPL Horizons**: https://ssd.jpl.nasa.gov/horizons/
- **MAST Archive**: https://mast.stsci.edu/
- **Minor Planet Center**: https://www.minorplanetcenter.net/
- **NASA 3I/ATLAS**: https://science.nasa.gov/solar-system/comets/3i-atlas/
- **ESA/JWST 3I/ATLAS**: https://www.esa.int/ESA_Multimedia/Images/2025/08/Webb_observations_of_interstellar_comet_3I_ATLAS

## License & Attribution

**Developer**: Prabhu Sadasivam
**ORCID**: https://orcid.org/0009-0008-0069-693X

Data courtesy of NASA JPL, STScI/MAST, and the Minor Planet Center.
