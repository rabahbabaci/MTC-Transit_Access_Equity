# Transit Access Equity: Time-Aware Isochrones in Alameda County

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**  
**Track B (Advanced)**  
**Mentor:** Metropolitan Transportation Commission (MTC)  
**Team:** Rabah Babaci & Adithya Ayanam

---

## Quickstart

```bash
git clone https://github.com/<your-username>/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate          # Mac/Linux
# .venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

Create a `.env` file in the project root:

```
CENSUS_API_KEY=your_census_api_key
API_511_KEY=your_511_key_here       # required for GTFS downloads
```

Launch the analysis:

```bash
jupyter notebook notebooks/transit_access_equity_midterm.ipynb
```

All outputs are saved automatically into:

- `data/processed/`
- `visualizations/`
- `results/`

The notebook runs end-to-end (Restart & Run All) using **relative paths only**.

---

## Repository Structure

```
MTC-Transit_Access_Equity/
├── data/
│   ├── raw/                         # original EPC, TIGER, ACS, LODES, GTFS, OSM
│   └── processed/                   # cleaned tracts, merged datasets, R5 outputs
│
├── docs/
│   └── data_dictionary.txt          # definitions of all fields used in analyses
│
├── notebooks/
│   └── transit_access_equity_midterm.ipynb   # full analysis (midterm + final phase)
│
├── results/                         # markdown summaries, PDF reports, tables
├── visualizations/                  # PNG charts + HTML interactive maps
├── src/                             # (Track B) placeholders for modularization
│
├── .env                             # API keys (gitignored)
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## Project Overview

This project examines **spatial and temporal inequities in transit-based job accessibility** across **Alameda County**, with specific attention to **Equity Priority Communities (EPCs)** as defined in _MTC Plan Bay Area 2050_.

We evaluate two complementary metrics:

1. **Midterm metric (proxy model):**
   Jobs reachable within 30 minutes using straight-line distance + 40 km/h travel proxy.

2. **Final Phase (Track B): Time-aware R5 accessibility model**

   - Uses **GTFS transit schedules (AC Transit + BART)**
   - Uses **OSM street network (Alameda County extract)**
   - Computes **AM Peak vs Late Night travel-time matrices**
   - Generates **15 / 30 / 45-minute isochrones**
   - Quantifies accessibility differences between EPC vs non-EPC tracts

**Core Research Questions**

- How do job accessibility levels vary between EPC and non-EPC communities?
- How much do accessibility levels change from **AM Peak (7:30–8:30)** to **Late Night (22:00–23:00)**?
- Do EPC tracts experience **greater reductions** in late-night access?
- What insights support equitable transit and mobility planning?

---

## Data Sources

All required datasets are stored in `data/raw/` (large files excluded from GitHub per CYPLAN 101 policy).

- **EPC Layer** — MTC Plan Bay Area 2050
- **TIGER/Line Tracts (2022)** — U.S. Census Bureau
- **ACS 2018–2022 5-year estimates** — Population
- **LODES (WAC 2021)** — Workplace jobs
- **GTFS Feeds (511 API)** — AC Transit & BART (time-aware schedules)
- **OpenStreetMap (Geofabrik Extract)** — NorCal network

All merged and processed datasets are stored in `data/processed/`.

---

## Summary of Methods

### 1. Data Cleaning & Validation

- Standardized FIPS GEOID formatting (11 digits)
- Merged EPC, population, TIGER geometry, and LODES jobs
- Identified missing ACS and LODES values; classified missingness per _Dark Data_ typology
- Validated CRS (EPSG:4326 for storage; EPSG:26910 for distance)
- Ensured geometries were valid; fixed or removed invalid shapes

### 2. Proxy Accessibility (Midterm)

- Computed centroid-to-centroid distances
- Converted distance to travel time using a uniform 40 km/h assumption
- Estimated jobs reachable within 30 minutes
- Produced boxplots, choropleths, and bar charts

### 3. Time-Aware Accessibility (Final)

- Downloaded GTFS automatically via 511 API
- Clipped NorCal OSM to Alameda County using `osmium`
- Built an **R5py TransportNetwork** (OSM + GTFS)
- Selected **6 representative tracts** (3 EPC, 3 non-EPC)
- Computed full **travel-time matrices** for AM Peak and Late Night
- Generated **convex-hull isochrones (15/30/45 min)**
- Calculated job accessibility for each time window
- Computed **% change** from AM → Late Night
- Mapped findings using static and interactive visualizations

---

## Key Outputs

All outputs below are automatically generated during execution.

### Processed Data

| File                            | Description                            |
| ------------------------------- | -------------------------------------- |
| `alameda_final.geojson`         | Cleaned tracts with EPC and population |
| `travel_times_am_late.csv`      | R5 travel-time matrix (AM + Late)      |
| `isochrones_am.geojson`         | AM Peak 15/30/45-min isochrones        |
| `isochrones_late.geojson`       | Late Night 15/30/45-min isochrones     |
| `job_accessibility_summary.csv` | Jobs reachable + % change              |

### Visualizations

| Output                                             | Path                                                 |
| -------------------------------------------------- | ---------------------------------------------------- |
| Interactive Map (All thresholds)                   | `visualizations/explorer_all_thresholds.html`        |
| AM vs Late 30-min Comparison Map                   | `visualizations/main_am_vs_late_30min.html`          |
| Paired Static Map (AM Access vs Late-Night Change) | `visualizations/final_paired_map.png`                |
| Percent-Change Bar Chart (City Labels)             | `visualizations/job_access_change_bar_citynames.png` |
| Midterm Boxplot                                    | `visualizations/jobs30_boxplot.png`                  |
| Midterm Choropleth                                 | `visualizations/jobs30_map.png`                      |
| Midterm Bar Chart                                  | `visualizations/jobs30_bar_chart.png`                |

### Documentation

- `docs/data_dictionary.txt`
- `results/early_findings.md`
- `results/report.pdf` (midterm)

---

## Findings Summary

### Midterm (Static Proxy)

- EPC tracts reached **~75,000 more jobs** on average than non-EPC tracts.
- Accessibility highest along the **West County corridor** (Berkeley → Oakland → Hayward).
- Eastern Alameda County exhibited significantly lower accessibility.

### Final (Time-Aware Accessibility)

- Accessibility drops **substantially at night**, and **unevenly** across the county.
- Some EPC tracts maintain strong late-night access; others decline sharply.
- Several non-EPC suburban tracts see **large percentage declines**, reflecting limited evening service.
- The paired maps and bar charts illustrate which communities are most vulnerable to service reductions.

---

## Reproducibility Notes

- Notebook uses **relative file paths** only.
- Dependencies fully captured in `requirements.txt`.
- R5py requires a working Java runtime (automatically started on import).
- All figures export automatically to the `visualizations/` folder.
- The notebook fully supports “Restart & Run All.”

---

## License

MIT License — free for academic, instructional, and research use.

---

## Contact

**Rabah Babaci** — [https://linkedin.com/in/rabahbabaci](https://linkedin.com/in/rabahbabaci)
**Adithya Ayanam** — UC Berkeley
**Mentor:** Metropolitan Transportation Commission (MTC)
