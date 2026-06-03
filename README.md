# A nutrient effect on the TEX<sub>86</sub> paleotemperature proxy
This is a repository for "A nutrient effect on the TEX<sub>86</sub> paleotemperature proxy" in _Geophysical Research Letters_.

<p align="center">
<img src="https://github.com/PaleoLipidRR/nutrient-effect-on-TEX/blob/main/figures/gridded_coretops_regression_nitrate_effect_global.png" width="600">
</p>

## Overview

The TEX<sub>86</sub> proxy is a widely used organic paleothermometer based on the ring distributions of isoprenoid glycerol dialkyl glycerol tetraethers (GDGTs) produced by marine ammonia-oxidizing archaea (AOA) (*Nitrosospharales*; formerly Thaumarchaeota). Because these archaea are also sensitive to nutrient availability — as shown in culture studies investigating growth rate/growth phase — TEX<sub>86</sub>-based temperature reconstructions may carry a non-thermal nutrient bias.

This study demonstrates, using a global coretop compilation combined with World Ocean Atlas (WOA23) and Copernicus Marine (CMEMS) ocean products, that **thermocline nitrate concentrations introduce a systematic, regionally variable offset in TEX<sub>86</sub>** and can be used as a practical proxy to evaluate *nutrient effect* on lipid biosynthesis on marine AOA. Two paleoceanographic case studies (Arabian Sea and Tasman Sea) illustrate how this nutrient effect can affect downcore temperature reconstructions.

## Citation

If you use this code or data, please cite:

> Rattanasriampaipong, R., Tierney, J. E., Abell, J. T., & Gilmore, L. D. (2025). A nutrient effect on the TEX<sub>86</sub> paleotemperature proxy. *Geophysical Research Letters*.

### Supplementary dataset (Zenodo)

> Rattanasriampaipong, R., Tierney, J., Abell, J., & Gilmore, L. D. (2025). *Supplementary Data for 'A nutrient effect on the TEX86 paleotemperature proxy'* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.14806962

## Repository structure

```
nutrient-effect-on-TEX/
├── notebooks/
│   ├── SI_code1_PreProcessing_rev1.ipynb   # Stage 1: data assembly & gridding
│   └── SI_code2_DataAnalysis_rev1.ipynb    # Stage 2: analysis & all paper figures
├── spreadsheets/
│   ├── ds01_updated_global_coretop_tex.csv     # Global coretop TEX86 compilation
│   ├── ds02_manual_regionName_assignment.xlsx   # Regional assignments
│   ├── ds03_processed_coretop_tex.csv           # Processed coretop data (output of Stage 1)
│   ├── ds07_TasmanSea_paleorecords.xlsx         # Tasman Sea downcore records
│   ├── fitted_models/                           # Saved OLS model objects (.pkl)
│   └── published_data/                          # External datasets used in analysis
├── ncfiles/
│   ├── ds04_gridded_coretop_tex.nc              # Gridded TEX86 coretop data
│   ├── ds05_gridded_AOM_ds.nc                   # Gridded ammonia oxidation rate data
│   └── ds06_calculated_ocean_properties.nc      # Thermocline T and nitrate fields
├── figures/                                     # All paper figures (PDF/PNG/SVG)
├── bibtex/
│   └── references.bib                           # BibTeX references
├── environment.yml                              # Conda environment specification
└── conda-lock.yml                               # Locked dependency versions
```

## Workflow

The analysis is split into two sequential Jupyter notebooks:

### Stage 1 — `SI_code1_PreProcessing_rev1.ipynb`
Assembles and preprocesses all input data:
- Loads the global coretop TEX<sub>86</sub> compilation
- Extracts thermocline depth, thermocline-integrated temperature, and thermocline-integrated nitrate from WOA23 and CMEMS at each coretop site
- Regrids ocean properties to a common spatial grid
- Exports processed datasets (`ds03`, `ds04`, `ds05`, `ds06`) for use in Stage 2

### Stage 2 — `SI_code2_DataAnalysis_rev1.ipynb`
Performs all statistical analyses and generates every paper figure:
- **Fig. 1** — Global map of TEX<sub>86</sub> residuals relative to thermocline temperature
- **Fig. 2** — Regional quantification of the nitrate effect
- **Fig. 3** — Arabian Sea paleoclimate case study (δ¹⁵N proxy for denitrification)
- **Fig. 4** — Tasman Sea paleoclimate case study (alkenone concentration proxy for productivity)
- **Figs. S1–S11** — Supporting figures (ammonia oxidation rates, calibration comparisons, additional paleo records, age model)

## Installation

### Prerequisites
- [Anaconda](https://www.anaconda.com/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

### Create the environment

```bash
# Using the locked environment (recommended for exact reproducibility)
conda-lock install --name texas-env conda-lock.yml

# Or using the environment.yml (may resolve to slightly different package versions)
conda env create -f environment.yml
conda activate texas-env
```

### Launch JupyterLab

```bash
conda activate texas-env
jupyter lab
```

## External data requirements

Some large data files are not stored in this repository and must be downloaded separately before running Stage 1:

| Dataset | Source | Variable |
|---|---|---|
| WOA23 temperature (0.25°) | [NCEI](https://www.ncei.noaa.gov/products/world-ocean-atlas) | Annual mean, 1991–2020 climatology |
| WOA23 nitrate (1°) | [NCEI](https://www.ncei.noaa.gov/products/world-ocean-atlas) | Annual mean |
| CMEMS nitrate (0.25°) | [Copernicus Marine](https://data.marine.copernicus.eu/product/GLOBAL_MULTIYEAR_BGC_001_029/description) | 30-year monthly climatology (1993–2022) |
| NICOPP δ¹⁵N database | [NOAA Paleoclimatology](https://www.ncei.noaa.gov/access/paleo-search/study/14114) | Sedimentary bulk δ¹⁵N |
| Tang et al. (2023) nitrification database | [ESSD](https://doi.org/10.5194/essd-15-5039-2023) | Global ocean nitrification rates |

Stage 2 relies on the pre-processed `.nc` and `.csv` outputs from Stage 1, which are already included in this repository under `ncfiles/` and `spreadsheets/`.

## Key dependencies

| Package | Purpose |
|---|---|
| `xarray` + `dask` | N-D array handling and lazy I/O for NetCDF ocean products |
| `xesmf` | Conservative regridding between ocean grids |
| `cartopy` / `proplot` | Map projections and publication-quality figures |
| `statsmodels` / `scikit-learn` | Ordinary least-squares regression and sliding-window analysis |
| `baysplinepy` / `baysparpy` | Bayesian alkenone (BAYSPLINE) and TEX<sub>86</sub> (BAYSPAR) calibrations |
| `cmdstanpy` | Stan-based Bayesian modelling |

## License

Code is released under the MIT License. Please see the [LICENSE](LICENSE) file for details.
