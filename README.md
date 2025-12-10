# Transit Access Equity: Time-Aware Isochrones in the Bay Area

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**  
**Track B (Advanced)** | **Mentor:** Metropolitan Transportation Commission (MTC)  
**Team:** Rabah Babaci & Adithya Ayanam

---

## Quickstart

```bash
git clone https://github.com/<your-username>/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate          # Mac/Linux
# .venv\Scripts\activate         # Windows

pip install -r requirements.txt
```

Create a `.env` file in the root folder:

```
CENSUS_API_KEY=your_census_key_here
API_511_KEY=your_511_key_here        # optional
```

Launch the analysis:

```bash
jupyter notebook notebooks/transit_access_equity_final.ipynb
```

All results are automatically saved to `data/processed/`, `visualizations/`, and `results/`.

---

## Repository Structure

```
MTC-Transit_Access_Equity/
├── .venv/
├── data/
│   ├── raw/
│   └── processed/
├── docs/
├── notebooks/
│   └── transit_access_equity_final.ipynb
├── results/
├── visualizations/
├── .env
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## Project Overview

This project analyzes transit-based job accessibility equity in Alameda County, focusing on **Equity Priority Communities (EPCs)** defined by MTC’s Plan Bay Area 2050.

### Midterm Phase

Static 30-minute accessibility (proxy method)

### Final (Track B) Phase

Time-aware accessibility using:

- R5 routing engine (r5py)
- GTFS schedules
- OpenStreetMap street network
- LODES 2021 employment data
- 15 / 30 / 45-minute isochrones
- AM Peak vs Late Night comparison

### Key Questions

- How does job accessibility change from AM Peak to Late Night?
- Do EPC tracts lose disproportionately more access after dark?
- Which locations experience the largest accessibility shifts?
- How can time-aware metrics guide equitable transit planning?

---

## Key Outputs (auto-generated)

| Output                    | Location                                             | Description                |
| ------------------------- | ---------------------------------------------------- | -------------------------- |
| Cleaned tracts + EPC flag | `data/processed/alameda_final.geojson`               | Final dataset              |
| Travel-time matrices      | `data/processed/travel_times_am_late.csv`            | AM & Late Night R5 results |
| Isochrones                | `data/processed/isochrones_*.geojson`                | 15 / 30 / 45 min polygons  |
| Accessibility summary     | `data/processed/job_accessibility_summary.csv`       | Jobs reachable + % change  |
| Interactive explorer      | `visualizations/explorer_all_thresholds.html`        | Full time × threshold tool |
| AM vs Late comparison     | `visualizations/final_paired_map.png`                | Side-by-side analysis map  |
| Percent-change bar chart  | `visualizations/job_access_change_bar_citynames.png` | EPC vs non-EPC change      |
| Final report              | `results/report.pdf`                                 | Submission-ready PDF       |

---

## License

MIT License — free for academic and research use.

---

## Contact

Rabah Babaci — https://linkedin.com/in/rabahbabaci  
Adithya Ayanam — UC Berkeley CYPLAN 101  
Mentor: Metropolitan Transportation Commission (MTC)
