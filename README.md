# **Transit Access Equity: Time-Aware Isochrones in Alameda County**

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**
**Track B (Advanced)**
**Mentor:** Metropolitan Transportation Commission (MTC)
**Team:** Rabah Babaci & Adithya Ayanam

---

## **Project Overview**

This project evaluates **time-of-day transit accessibility** in Alameda County, with a focus on **Equity Priority Communities (EPCs)** as defined in MTC’s _Plan Bay Area 2050_.
The core research question:

### **How does transit-based job accessibility change from AM Peak to Late Night, and do EPC tracts experience disproportionately greater losses when service frequency declines?**

To answer this, we integrate:

- Census TIGER tract geometries
- ACS 2018–2022 population data
- LODES 2021 Workplace Area Characteristics (WAC) job counts
- AC Transit + BART GTFS feeds
- OpenStreetMap multimodal networks
- The **r5py** routing engine for time-aware, schedule-based travel times

We compute:

- **AM Peak (7:30–8:30)** and **Late Night (22:00–23:00)** travel-time matrices
- **Isochrones** for 15 / 30 / 45 minutes
- **Job accessibility metrics** for six representative EPC and non-EPC tracts
- **Interactive + static visualizations** comparing peak vs. late-night access

This repository provides a complete, reproducible workflow demonstrating **where**, **when**, and **for whom** transit job access contracts after dark.

---

## **Quickstart**

### **1. Clone the repository and set up environment**

```bash
git clone https://github.com/rabahbabaci/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate       # Mac/Linux
# .venv\Scripts\activate        # Windows

pip install -r requirements.txt
```

### **2. Add API keys**

Create a `.env` file in the project root:

```
CENSUS_API_KEY=your_census_api_key_here
API_511_KEY=your_511_api_key_here
```

### **3. Run the full workflow**

```bash
jupyter notebook notebooks/time_aware_transit_access_equity.ipynb
```

All outputs are created automatically in:

- `data/processed/`
- `visualizations/`
- `results/`

---

## **Repository Structure**

> _Note: File lists are not shown, only what belongs in each directory (per course policy)._

```
MTC-TRANSIT_ACCESS_EQUITY/
├── data/
│   ├── raw/                     # Unprocessed inputs (not committed):
│   │   # GTFS feeds, OSM extracts, ACS tables, LODES WAC, EPC layers
│   │
│   └── processed/               # Cleaned & analysis-ready datasets:
│       # tract geometries, merged ACS/LODES, travel-time matrices,
│       # AM & Late-Night isochrones, job accessibility summary
│
├── docs/
│   └── data_dictionary.txt      # Definitions for all fields used in analysis
│
├── notebooks/
│   └── time_aware_transit_access_equity.ipynb
│       # Complete Track B workflow (Restart & Run All compatible)
│
├── results/
│   # summary tables, early findings, interpretive material
│
├── visualizations/
│   # Interactive HTML + static PNGs:
│   # - Isochrone explorer (AM vs Late, 15/30/45 min)
│   # - Animated accessibility chart
│   # - Final paired map (AM access vs % Late-Night change)
│   # - Percent-change bar chart and midterm proxy figures
│
├── src/                          # Optional modular Track B code:
│   # routing utilities, convex hull helpers, data cleaning modules
│
├── .gitignore
├── .env                          # Not committed
├── LICENSE
├── requirements.txt
└── README.md
```

---

## **Data Sources**

- **MTC Plan Bay Area 2050** — Equity Priority Communities
- **US Census TIGER/Line 2022** — tract boundaries
- **ACS 2018–2022** — population estimates
- **LEHD LODES 2021 WAC** — workplace job counts
- **511.org GTFS** — AC Transit + BART
- **OpenStreetMap (Geofabrik NorCal)** — multimodal network data

All raw datasets are cleaned, standardized, validated, and stored in `data/processed/`.

---

## **Methods**

### **1. Data Cleaning & Integration**

- Standardize all GEOIDs to 11-digit FIPS
- Merge EPC, ACS, TIGER, and LODES WAC tables
- Repair geometries and unify CRS (EPSG:4326 for storage; EPSG:26910 for spatial ops)
- Evaluate and classify missing/dark data:

  - ACS sampling variability
  - LODES suppression
  - GTFS schedule-only reliability

Cleaning steps follow course guidelines emphasizing reproducibility and transparency.

---

### **2. Midterm Proxy Accessibility Model**

Before R5 routing, we computed a simplified 30-minute accessibility measure using:

- Centroid-to-centroid Euclidean distance
- 40 km/h assumed speed
- Cumulative jobs within 30 minutes

This established baseline spatial patterns for later comparison.

---

### **3. Time-Aware Routing (Final Phase)**

Using **r5py**, we:

1. Downloaded & clipped GTFS + OSM
2. Built a multimodal transport graph
3. Selected **6 representative tracts** (3 EPC, 3 Non-EPC)
4. Computed **two full travel-time matrices**:

   - AM Peak (7:30–8:30)
   - Late Night (22:00–23:00)

5. Generated 15 / 30 / 45-minute convex-hull isochrones
6. Merged job data to calculate:

   - Jobs reachable
   - Percent change (Late vs AM)

---

### **4. Visualization Strategy**

All visuals prioritize **expressiveness and effectiveness**:

- Clear legends, no unnecessary gradients
- EPC vs Non-EPC visual distinction
- Proper color associations (red/blue)
- Interactivity for time-threshold comparison

Final interactive deliverables include:

- **Isochrone Explorer** (`explorer_all_thresholds.html`)
- **Animated Accessibility Chart** (`animated_accessibility_slider.html`)

Static figures include:

- AM vs Late paired map
- 30-min percent change bar chart
- Midterm proxy visuals (choropleth, bar chart, violin)

---

## **Key Findings**

### **Midterm (Proxy Model)**

- EPC tracts initially appear to reach _significantly more jobs_ (~75k difference).
- Higher accessibility along the Oakland–San Leandro corridor.

### **Final (Time-Aware R5 Results)**

- **Late-night accessibility declines sharply**, with some tracts losing 20–30% of reachable jobs.
- **EPC tracts often experience the steepest losses**, reflecting reliance on frequent trunk lines that do not run late.
- Some non-EPC suburban tracts see slight improvements due to congestion-free conditions.
- Time-of-day patterns reveal **hidden inequalities** not visible in static, all-day models.

---

## **Limitations**

- **LODES**: suppressed counts and imputed values
- **ACS**: sampling noise in low-pop tracts
- **GTFS**: scheduled service ≠ actual reliability
- **Tract-level MAUP** and generalization issues
- Isochrones use convex hulls, which may over-approximate reach

Despite these limitations, the trend patterns are robust.

---

## **Interactive Visualizations**

### **1. Isochrone Explorer (AM vs Late, 15/30/45 min)**

`visualizations/explorer_all_thresholds.html`

- Toggle thresholds
- Compare EPC vs Non-EPC
- Solid (AM) vs dashed (Late Night) polygons

### **2. Animated Job Accessibility Chart**

`visualizations/animated_accessibility_slider.html`

- Shows jobs reachable by tract across all thresholds
- AM vs Late Night frames
- Highlights nonlinear night-time change

---

## **Reproducibility**

- Fully reproducible **Restart & Run All** notebook
- All file paths are **relative**
- Environment locked via `requirements.txt`
- Data dictionary included (`docs/data_dictionary.txt`)
- Raw data intentionally excluded under course policy

---

## **License**

MIT License — unrestricted academic and research use.

---

## **Contact**

**Rabah Babaci**
**Adithya Ayanam**
Mentor: **Metropolitan Transportation Commission (MTC)**
