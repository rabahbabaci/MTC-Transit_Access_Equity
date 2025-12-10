# Transit Access Equity: Time-Aware Isochrones in Alameda County

CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)
Track B (Advanced)
Mentor: Metropolitan Transportation Commission (MTC)
Team: Rabah Babaci & Adithya Ayanam

---

## Project Overview

This project evaluates **time-of-day transit accessibility** in Alameda County, with a focus on **Equity Priority Communities (EPCs)** identified by the Metropolitan Transportation Commission (MTC) in Plan Bay Area 2050.
Our central research question:

**How does transit-based job accessibility change from AM Peak to Late Night, and do EPC tracts experience disproportionately greater losses when transit service frequencies decline?**

The analysis combines:

- Census TIGER geometries
- ACS population
- LODES (WAC) job locations
- GTFS feeds from AC Transit and BART
- OpenStreetMap street networks
- r5py routing engine for time-aware multimodal travel times

We compute **AM Peak (7:30–8:30)** and **Late Night (22:00–23:00)** accessibility using R5, generate isochrones at 15 / 30 / 45 minutes, build travel-time matrices, compute job accessibility for six representative EPC and non-EPC tracts, and visualize results using static and interactive maps.

The final story examines **where, when, and for whom** transit access contracts after dark—and what this implies for equity-oriented transportation planning.

---

## Quickstart

### Clone & set up environment

```bash
git https://github.com/rabahbabaci/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate          # Mac/Linux
# .venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

### Add API keys

Create `.env` in the project root:

```
CENSUS_API_KEY=your_census_key_here
API_511_KEY=your_511_key_here
```

### Run the analysis

```bash
jupyter notebook notebooks/time_aware_transit_access_equity.ipynb
```

All outputs are automatically saved to:

- `data/processed/`
- `visualizations/`
- `results/`

The notebook runs fully end-to-end using **relative paths** and performs all data downloading, cleaning, routing, and visualization.

---

## Repository Structure

```
MTC-Transit_Access_Equity/
├── data/
│   ├── raw/                       # EPC, TIGER, ACS, LODES, GTFS, OSM (not committed)
│   └── processed/                 # cleaned datasets, matrices, isochrones
│
├── docs/
│   └── data_dictionary.txt        # definitions of major fields
│
├── notebooks/
│   └── time_aware_transit_access_equity.ipynb   # complete workflow
│
├── results/                       # summary tables, findings
├── visualizations/                # static + interactive HTML files
├── src/                           # optional modular Track B code
│
├── .gitignore
├── .env
├── LICENSE
├── requirements.txt
└── README.md
```

Large datasets (GTFS, OSM) follow course policy and should not be committed. Scripts for downloading them are included in the notebook.

---

## Data Sources

- MTC Plan Bay Area 2050 — Equity Priority Communities (EPC)
- US Census TIGER/Line 2022 — tract geometries
- ACS 2018–2022 — population
- LEHD LODES 2021 WAC — job counts
- 511.org GTFS — AC Transit and BART service schedules
- OpenStreetMap (Geofabrik NorCal extract)

All raw sources are cleaned, merged, validated, and saved to `data/processed/`.

---

## Methods

### 1. Data Cleaning and Preparation

Steps include:

- Standardizing all GEOIDs to 11-digit format

- Merging EPC, ACS, LODES, and TIGER geometries

- Validating tract geometries and CRS

- Documenting and classifying missing values:

  - **ACS zero-population tracts** → structural missingness
  - **LODES blind spots** → workplace suppression or post-processing artifacts
  - **GTFS limitations** → missing exceptions, schedule-only reliability

- Preparing centroids for routing (projected CRS 26910 → WGS84)

Cleaning steps follow lecture guidelines for transparency, reproducibility, and accountable handling of “dark data.”

---

### 2. Midterm Accessibility (Proxy Model)

Before time-aware routing, a 30-minute accessibility proxy was computed:

- Centroid-to-centroid Euclidean distance
- 40 km/h travel-speed assumption
- Cumulative jobs reachable

Purpose: establish baseline patterns to later compare against R5-derived results.

---

### 3. Time-Aware Routing (Final Phase)

Using **r5py**:

1. Download GTFS (AC Transit + BART)
2. Download and clip OSM street network
3. Build a unified multimodal routing graph
4. Select six representative tracts:

   - Three EPC
   - Three non-EPC
   - Spread across multiple cities

5. Compute **two full travel-time matrices**:

   - AM Peak (7:30–8:30)
   - Late Night (22:00–23:00)

6. Generate **15 / 30 / 45-minute isochrones** using convex hull over reachable centroids
7. Merge job data to compute:

   - Jobs reachable (AM)
   - Jobs reachable (Late Night)
   - % change

---

### 4. Visualization Strategy

All visualizations follow expressiveness/effectiveness guidelines:

- Meaningful use of color
- Clear legends and scales
- No pie charts, unnecessary gradients, or vertical labels
- No overplotting or spaghetti lines

Interactive visualizations use **Folium** and include:

- Layer toggles by time window and threshold
- EPC vs non-EPC comparison
- Spatial interpretation of shrinking isochrones

---

## Key Results

### Midterm (Proxy Metric)

- EPC tracts reach **~75,000 more jobs** within 30 minutes than non-EPC tracts.
- Access highest in **Berkeley → Oakland → San Leandro** corridor.
- Eastern county exhibits lower accessibility due to distance and network separation.

### Final (R5 Time-Aware Results)

Clear time-of-day disparities:

- Many tracts experience large drops in late-night job accessibility.
- EPC tracts do not uniformly lose more access—results vary by city, service frequency, and spatial context.
- Some EPC tracts remain resilient due to strong trunk-line service.
- Several non-EPC suburban tracts see severe contractions in late-night reach due to low service span and poor coverage.

The combined maps and bar charts provide a clear explanation of **where and why** accessibility gaps widen after dark.

---

## Limitations

### Missing and Dark Data

- LODES workplace data includes imputed/suppressed job counts.
- ACS data contains sampling variability—especially for low-population tracts.
- GTFS describes scheduled service, not real-world reliability.

### Interpretation Challenges

- Tract-level statistics → risk of **ecological fallacy**.
- Choropleths inherit **MAUP** due to tract shapes and aggregation.
- Convex-hull isochrones may over-generalize geographic reach.

### Technical Constraints

- R5 does not model fares or agency-specific transfer penalties unless encoded in GTFS.
- Late-night service variability makes travel times sensitive to GTFS calendar definitions.

---

## Interactive Visualizations (Required for Track B)

Two fully interactive visualizations are provided:

1. **Isochrone Explorer**
   `visualizations/explorer_all_thresholds.html`

   - Toggle 15/30/45 min
   - Toggle AM Peak vs Late Night
   - EPC vs non-EPC coloring

2. **AM vs Late Night 30-minute Comparison**
   `visualizations/main_am_vs_late_30min.html`

   - Side-by-side layer control
   - Highlights shrinkage of transit reach after dark

These HTML files can be embedded directly in the StoryMap / GitHub Pages site.

---

## Reproducibility

- Full workflow runs with **Restart & Run All**.
- Dependencies listed in `requirements.txt`.
- All outputs saved via relative paths.
- Raw datasets are fetched via scripted downloads (GTFS, OSM).
- No placeholder or non-explanatory comments.

---

## How This Repository Supports the Final Submission

This repository fulfills all Track B expectations:

- Clear narrative + technical documentation
- Reproducible, modular workflow
- Validated datasets and documented cleaning steps
- Multiple maps and charts aligned with course visualization principles
- Two required interactive visualizations
- Ready for StoryMap / GitHub Pages integration

---

## License

MIT License — available for research and educational use.

---

## Contact

Rabah Babaci
Adithya Ayanam
Mentor: Metropolitan Transportation Commission (MTC)
