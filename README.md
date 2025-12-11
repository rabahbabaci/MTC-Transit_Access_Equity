# **Transit Access Equity: Time-Aware Isochrones in Alameda County**

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**
**Track B (Advanced)**
**Mentor:** Metropolitan Transportation Commission (MTC)
**Team:** Rabah Babaci & Adithya Ayanam

---

## **Project Overview**

This project evaluates **time-of-day transit accessibility** in Alameda County, with a focus on **Equity Priority Communities (EPCs)** from MTC’s Plan Bay Area 2050.

Our central research question is:

> **How does transit-based job accessibility change from AM Peak to Late Night, and do EPC tracts experience disproportionately larger losses when transit service frequencies decline?**

To answer this, we integrate:

- TIGER/Line tract geometries
- ACS 2018–2022 population estimates
- LODES 2021 Workplace Area Characteristics (WAC) jobs
- GTFS feeds for **AC Transit** and **BART**
- OpenStreetMap street networks
- The **r5py multimodal routing engine** for time-aware, transit-plus-walk travel times

We compute:

- **AM Peak (7:30–8:30 AM)** and **Late Night (22:00–23:00)** travel-time matrices
- **ISOchrones** at **15 / 30 / 45 minutes**
- **Job accessibility** from six representative EPC and non-EPC tracts
- **Percent change** in reachable jobs by time-of-day
- **Static and interactive visualizations** for StoryMap and GitHub Pages

The final narrative highlights **where, when, and for whom** accessibility shrinks after dark—and what this means for equitable transportation planning.

---

## **Quickstart**

### **1. Clone & Set Up Environment**

```bash
git clone https://github.com/rabahbabaci/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate          # Mac/Linux
# .venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

### **2. Add API Keys**

Create a `.env` file in the repository root:

```
CENSUS_API_KEY=your_census_key_here
API_511_KEY=your_511_key_here
```

### **3. Run the Analysis**

```bash
jupyter notebook notebooks/time_aware_transit_access_equity.ipynb
```

All outputs automatically save to:

- `data/processed/`
- `visualizations/`
- `results/`

The notebook is fully reproducible with **Restart & Run All**.

---

## **Repository Structure**

```
MTC-Transit_Access_Equity/
├── data/
│   ├── raw/                    # EPC, TIGER, ACS, LODES, GTFS, OSM (not committed)
│   └── processed/              # cleaned datasets, matrices, isochrones
│
├── docs/
│   └── data_dictionary.txt     # final variable definitions
│
├── notebooks/
│   └── time_aware_transit_access_equity.ipynb
│
├── results/                    # summary tables, findings
├── visualizations/             # static & interactive HTML maps/plots
├── src/                        # optional modularization
│
├── requirements.txt
├── .env
├── .gitignore
└── README.md
```

Large raw datasets (GTFS, OSM) are intentionally excluded from version control, per course policy.

---

## **Data Sources**

- **MTC Plan Bay Area 2050** — Equity Priority Communities
- **U.S. Census TIGER/Line 2022** — tract geometries
- **ACS 2018–2022** — population characteristics
- **LEHD LODES 2021 WAC** — job locations
- **511.org GTFS** — AC Transit & BART schedules
- **OpenStreetMap (Geofabrik)** — street network

All raw inputs are cleaned and harmonized in the notebook, with outputs stored under `data/processed/`.

---

## **Methods**

### **1. Data Cleaning & Preprocessing**

The notebook performs:

- GEOID standardization to 11-digit FIPS
- CRS harmonization to **EPSG:4326** for routing inputs
- Merging EPC, ACS, LODES, and TIGER datasets
- Classification of missing and “dark data” (per lecture: structural zero-population tracts, suppressed LODES cells)
- Validation of geometries
- Projected centroid calculation (EPSG:26910) for routing

All steps follow the course emphasis on transparent, reproducible data hygiene.

---

### **2. Midterm Accessibility (Proxy Model)**

Before using R5, we computed a simple 30-minute accessibility proxy:

- Euclidean centroid distances
- 40 km/h travel-time assumption
- Cumulative reachable jobs

This baseline enables comparison against the fully time-aware routing results.

---

### **3. Time-Aware Routing with r5py (Final Project)**

Steps include:

1. Download AC Transit + BART GTFS
2. Download and clip Alameda County OSM network
3. Build a unified **TransportNetwork** in R5
4. Select six representative tracts (3 EPC, 3 non-EPC)
5. Detect first valid weekday GTFS service date
6. Compute full origin–destination travel-time matrices for:

   - **AM Peak (07:30)**
   - **Late Night (22:00)**

7. Construct 15 / 30 / 45-minute convex-hull isochrones
8. Merge LODES job data and compute:

   - AM job access
   - Late-night job access
   - Percent change

---

### **4. Visualization Framework**

Visualizations were designed according to course **visual expressiveness & effectiveness** guidelines:

- Clear, interpretable color schemes
- Avoiding misleading gradients or excessive detail
- No spaghetti lines or pie charts
- Meaningful legends, scales, and labels

Two interactive deliverables (Track B requirement):

1. **Isochrone Explorer (AM vs Late × 15/30/45 min)**
2. **Side-by-side AM Peak vs Late Night Access (30 min)**

Static maps and bar charts complement the interactive tools for interpretation.

---

## **Key Results**

### **Midterm Baseline**

- EPC tracts reached **~75,000 more jobs** within 30 minutes than non-EPC tracts.
- Accessibility highest along the **Berkeley–Oakland–San Leandro** corridor.
- Eastern Alameda County shows lower access due to distance and network separation.

---

### **Final R5 Time-Aware Findings**

**When transit service frequencies drop at night:**

- Most tracts experience **significant losses** in job accessibility (–5% to –27%).
- EPC tracts—especially in **Oakland**—see steep late-night contractions due to reduced headways.
- Some southern Alameda tracts show **modest increases**, reflecting congestion-free routing in R5.
- Accessibility disparities **widen at night**, affecting workers with nonstandard schedules.

Isochrones clearly visualize spatial shrinkage at 15/30/45 minutes, particularly for EPC origins.

---

## **Limitations**

### **Missing/Dark Data**

- LODES includes imputed or suppressed workplace counts
- ACS estimates carry sampling variance
- GTFS reflects _scheduled_ service, not real-world reliability

### **Analytical**

- Tract-level results may exhibit ecological fallacy
- MAUP influences choropleth interpretation
- Convex-hull isochrones smooth local irregularities

### **Technical**

- R5 does not model fares or all agency-specific transfer rules
- Late-night GTFS definitions vary across agencies

---

## **Interactive Visualizations**

### **1. Isochrone Explorer (All Thresholds & Periods)**

`visualizations/explorer_all_thresholds.html`

Enables toggling between:

- 15 / 30 / 45-minute thresholds
- AM vs Late Night
- EPC vs non-EPC origins

### **2. AM Peak vs Late Night (30-minute comparison)**

`visualizations/main_am_vs_late_30min.html`

Shows shrinkage or expansion of reach under nighttime conditions.

Both are ready to embed in **StoryMap** or **GitHub Pages**.

---

## **Reproducibility**

- Full workflow executes with **Restart & Run All**
- All paths are relative; outputs save automatically
- Notebook handles all downloads (GTFS, OSM)
- Dependencies specified in `requirements.txt`

---

## **How This Repository Meets Track B Requirements**

✔ Cleaned & documented datasets
✔ Complete R5 routing methodology
✔ EPC vs non-EPC comparative analysis
✔ Two required interactive visualizations
✔ High-quality figures following visualization guidelines
✔ Clear narrative and interpretation
✔ Data dictionary included
✔ Fully reproducible workflow

This repository is fully aligned with the course rubric and ready for final submission.

---

## **License**

MIT License — free for academic and educational use.

---

## **Contact**

**Rabah Babaci**
**Adithya Ayanam**
**Mentor:** Metropolitan Transportation Commission (MTC)
