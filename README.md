# Transit Access Equity: Time-Aware Isochrones in Alameda County

CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)  
Track B (Advanced)  
Mentor: Metropolitan Transportation Commission (MTC)  
Team: Rabah Babaci & Adithya Ayanam

---

## Project Overview

This project evaluates disparities in transit-based job accessibility across Alameda County, with an emphasis on Equity Priority Communities (EPCs) identified in Plan Bay Area 2050. The central question is how accessibility changes by time of day and whether EPC communities experience greater losses in access during periods of reduced transit service.

The final analysis uses time-aware routing with GTFS schedules, OpenStreetMap networks, and the r5py routing engine. Isochrones, travel-time matrices, and job-count accessibility measures are computed for AM Peak (7:30–8:30) and Late Night (22:00–23:00). Results are communicated through both static and interactive visualizations, as required for Track B.

The core story is whether transit access contracts unevenly after dark, and what that implies for transit equity and service planning.

---

## Quickstart

Clone the repository:

```bash
git clone https://github.com/<your-username>/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity
```

Set up the environment:

```bash
python3 -m venv .venv
source .venv/bin/activate        # Mac/Linux
# .venv\Scripts\activate         # Windows
pip install -r requirements.txt
```

Create a `.env` file in the project root with the required API keys:

```
CENSUS_API_KEY=your_key_here
API_511_KEY=your_key_here
```

Run the full analysis:

```bash
jupyter notebook notebooks/transit_access_equity_midterm.ipynb
```

All outputs are saved automatically to:

- data/processed/
- visualizations/
- results/

The notebook works end-to-end using relative paths.

---

## Repository Structure

```
MTC-Transit_Access_Equity/
├── data/
│   ├── raw/                         # EPC, TIGER, ACS, LODES, GTFS, OSM
│   └── processed/                   # cleaned tracts, merged tables, R5 matrices and isochrones
│
├── docs/
│   └── data_dictionary.txt          # definitions of all main fields
│
├── notebooks/
│   └── transit_access_equity_midterm.ipynb   # complete reproducible workflow
│
├── results/                         # early findings and summary tables
├── visualizations/                  # static and interactive figures
├── src/                             # optional modular code (Track B structure)
│
├── .gitignore
├── .env
├── LICENSE
├── requirements.txt
└── README.md
```

Large raw datasets (GTFS and OSM) are not committed to GitHub, following course policy. Instructions and code for downloading and processing them are provided in the notebook.

---

## Data Sources

- Equity Priority Communities (MTC Plan Bay Area 2050)
- TIGER/Line Census Tracts (2022)
- ACS 2018–2022 5-year population estimates
- LODES WAC (2021) workplace jobs
- 511.org GTFS feeds for AC Transit and BART
- OpenStreetMap NorCal extract (Geofabrik)

All raw inputs flow into standardized, validated datasets stored in data/processed/.

---

## Methods

### 1. Data Cleaning

- Standardized Census GEOID to 11-digit FIPS format
- Merged EPC, ACS population, TIGER geometries, and LODES job data
- Performed missing data classification using concepts from Dark Data:

  - ACS and LODES contain zero-population tracts (structural missingness)
  - LODES blind spots (employment suppressed or assigned to block groups)

- Ensured CRS consistency (EPSG:4326 for mapping; EPSG:26910 for distance metrics)
- Validated geometries and removed or corrected invalid shapes

All cleaning steps are included in the notebook and documented in comments.

### 2. Accessibility Measures (Midterm, Proxy Model)

- Distance between tract centroids
- Travel time estimated using constant 40 km/h proxy
- Jobs reachable within 30 minutes using merged LODES WAC data

This stage provided baseline insights prior to final routing.

### 3. Time-Aware Routing with R5 (Final Phase)

- Downloaded GTFS feeds using the 511 API
- Downloaded and clipped OSM NorCal extract to Alameda County boundary
- Constructed r5py TransportNetwork (OSM + GTFS)
- Selected six representative tracts (three EPC, three non-EPC)
- Computed AM Peak and Late Night travel-time matrices
- Generated 15, 30, and 45-minute isochrones using convex-hull method
- Aggregated LODES jobs accessible at each time threshold
- Calculated percent change in accessibility from AM to Late Night

### 4. Visualization

All required visualizations follow design guidance from course lectures:

- No unnecessary color variation
- Consistent scales
- Clear interpretability
- No pie charts, spaghetti plots, or visual clutter

Interactive outputs use Folium/Leaflet and include layer toggles (time × threshold).

---

## Key Results

### Midterm Findings (Proxy Model)

- EPC tracts could reach approximately 75,000 more jobs within 30 minutes than non-EPC tracts.
- Highest accessibility appeared along the Berkeley–Oakland corridor.
- Eastern parts of the county showed substantially lower proxy access.

### Final Findings (Time-Aware)

- Accessibility declines significantly at night for most tracts.
- The magnitude of decrease varies: some EPC tracts maintain competitive access, while others lose a large share of reachable jobs.
- Several non-EPC suburban tracts experience steep declines due to limited late-night service.
- The paired map and percent-change bar chart highlight which cities experience the largest reductions.

### Interpretive Notes

- Accessibility varies not only by geography but by time of day, which affects workers traveling during late-night or early-morning hours.
- EPC status alone does not predict late-night vulnerability; the interaction between location and service frequency matters.

---

## Limitations

This analysis includes several limitations, addressed explicitly per final submission requirements:

### Missing and Dark Data

- LODES jobs rely on synthetic workplace assignments with known blind spots.
- ACS population estimates include sampling variability that may affect high-resolution comparisons.
- GTFS reflects scheduled service, not real-world reliability or delays.

### Statistical Interpretation Risks

- Summary statistics can obscure extremes; median changes do not reveal outliers.
- Choropleths can introduce MAUP effects due to tract boundaries.
- Isochrones derived from convex hulls may over-generalize reachable areas.

### Ecological Fallacy

- Accessibility is measured at tract level but does not reflect individual-level travel behavior or mode preferences.

### Model Constraints

- R5 travel-time calculations do not include fare constraints or agency-specific transfer penalties beyond GTFS metadata.

---

## Interactive Visualizations

At least two interactive visualizations are required for Track B.

This project provides:

1. **Interactive Isochrone Explorer**
   `visualizations/explorer_all_thresholds.html`
   Allows toggling 15/30/45-minute thresholds for AM Peak and Late Night.

2. **AM vs Late Night Comparison Map**
   `visualizations/main_am_vs_late_30min.html`
   Provides immediate comparison of 30-minute isochrones across time periods.

Both are referenced in the StoryMap and support the final narrative.

The notebook that generates these is:
`notebooks/transit_access_equity_midterm.ipynb`
This notebook runs end-to-end and contains all processing steps.

---

## Reproducibility

- The entire workflow runs using relative paths.
- All dependencies are listed in `requirements.txt`.
- The notebook includes comments documenting algorithmic steps, not template placeholders.
- Raw data is not committed; the notebook provides automated scripts for fetching GTFS and OSM inputs.

---

## How This Repository Supports the Final Submission

This repository provides:

- A complete, reproducible analysis pipeline
- A clear narrative for StoryMap integration
- Interactive visualizations required for Track B
- Documentation suitable for the final evaluation rubric
- Data dictionary and structured folders following best practices from the Final Project Guide

---

## License

MIT License — free for academic and research use.

---

## Contact

Rabah Babaci
Adithya Ayanam
Mentor: Metropolitan Transportation Commission (MTC)
