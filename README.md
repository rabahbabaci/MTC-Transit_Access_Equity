# **Transit Access Equity: Time-Aware Isochrones in Alameda County**

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**
**Track B (Advanced)**
**Mentor:** Metropolitan Transportation Commission (MTC)
**Team:** Rabah Babaci & Adithya Ayanam

---

## **Live Project Website**

A complete, interactive narrative of this project—including maps, animated charts, and interpretive text—is available at:

**[https://rabahbabaci.github.io/MTC-Transit_Access_Equity/](https://rabahbabaci.github.io/MTC-Transit_Access_Equity/)**

---

## **Project Overview**

This project evaluates **time-of-day transit accessibility** in Alameda County, California, with a focus on **Equity Priority Communities (EPCs)** as defined in MTC’s _Plan Bay Area 2050_. Rather than treating transit access as a static condition, we examine how accessibility changes across the day as service frequency and coverage vary.

### **Research Question**

**How does transit-based job accessibility change from AM Peak to Late Night, and do EPC tracts experience disproportionately greater losses when service frequency declines?**

To answer this question, we integrate:

- Census TIGER tract geometries
- ACS 2018–2022 population estimates
- LODES 2021 Workplace Area Characteristics (WAC) job counts
- AC Transit and BART GTFS schedules
- OpenStreetMap multimodal networks
- The **r5py** routing engine for time-aware, schedule-based travel times

We compute:

- **AM Peak (7:30–8:30 AM)** and **Late Night (10:00–11:00 PM)** travel-time matrices
- **Isochrones** for 15 / 30 / 45 minutes
- **Job accessibility metrics** for six representative EPC and non-EPC census tracts
- **Interactive and static visualizations** comparing peak and late-night access

Together, these components provide a reproducible workflow for understanding **where**, **when**, and **for whom** transit job access contracts after dark.

---

## **Quickstart**

### **1. Clone the repository and set up the environment**

```bash
git clone https://github.com/rabahbabaci/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity

python3 -m venv .venv
source .venv/bin/activate       # Mac/Linux
# .venv\\Scripts\\activate      # Windows

pip install -r requirements.txt
```

> **Note:** Java (JDK 21) is required for `r5py`. Ensure that `java --version` runs successfully before executing the notebook.

---

### **2. Add API keys**

Create a `.env` file in the project root:

```
CENSUS_API_KEY=your_census_api_key_here
API_511_KEY=your_511_api_key_here
```

These keys are used to programmatically fetch Census data and GTFS feeds. The `.env` file is intentionally excluded from version control.

---

### **3. Run the full workflow**

```bash
jupyter notebook notebooks/time_aware_transit_access_equity.ipynb
```

The notebook is designed to run **top-to-bottom** using _Restart & Run All_. All outputs are generated automatically in:

- `data/processed/`
- `visualizations/`
- `results/`

---

## **Repository Structure**

> _File lists are not shown in full; only directory purpose is documented, in accordance with course policy._

```
MTC-TRANSIT_ACCESS_EQUITY/
├── data/
│   ├── raw/                     # Unprocessed inputs (not committed):
│   │   # GTFS feeds, OSM extracts, ACS tables, LODES WAC, EPC layers
│   │
│   └── processed/               # Cleaned & analysis-ready outputs:
│       # tract geometries, merged ACS/LODES tables,
│       # travel-time matrices, AM & Late-Night isochrones,
│       # job accessibility summaries
│
├── docs/
│   └── data_dictionary.txt      # Definitions for all fields used in analysis
│
├── notebooks/
│   └── time_aware_transit_access_equity.ipynb
│       # Complete Track B workflow (Restart & Run All compatible)
│
├── results/
│   # Summary tables and interpretive outputs
│
├── visualizations/
│   # Interactive HTML + static PNGs:
│   # - Isochrone explorer (AM vs Late Night, 15/30/45 min)
│   # - Animated accessibility chart
│   # - Paired AM-access vs Late-Night-change map
│   # - Percent-change bar chart and midterm proxy figures
│
├── src/                          # Optional modular Track B utilities
│   # routing helpers, convex hull logic, data preparation modules
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
- **US Census TIGER/Line 2022** — census tract boundaries
- **ACS 2018–2022** — population estimates
- **LEHD LODES 2021 WAC** — workplace job counts
- **511.org GTFS** — AC Transit and BART schedules
- **OpenStreetMap (Geofabrik Northern California)** — multimodal network data

All raw datasets are cleaned, standardized, validated, and **derived outputs** are stored in `data/processed/`.

---

## **Methods**

### **1. Data Cleaning & Integration**

- Standardize all GEOIDs to 11-digit FIPS codes
- Merge EPC, ACS, TIGER, and LODES WAC tables
- Repair geometries and unify CRS (EPSG:4326 for storage; EPSG:26910 for spatial operations)
- Identify and document sources of uncertainty, including:

  - ACS sampling variability
  - LODES suppression and imputation
  - GTFS schedule-only representations of service

Cleaning steps follow course guidelines emphasizing **reproducibility**, **interpretability**, and **transparency**.

---

### **2. Midterm Proxy Accessibility Model**

Before time-aware routing, we computed a simplified 30-minute accessibility measure using:

- Centroid-to-centroid Euclidean distance
- A constant assumed speed of 40 km/h
- Cumulative jobs reachable within 30 minutes

This proxy captures broad spatial patterns but ignores network structure and service schedules. **It is included for comparison only; all substantive conclusions in this project are based on time-aware R5 results.**

---

### **3. Time-Aware Routing (Final Phase)**

Using **r5py**, we:

1. Downloaded and clipped GTFS and OSM data
2. Built a multimodal transport network
3. Selected **six representative census tracts** (three EPC, three non-EPC)
4. Computed two full travel-time matrices:

   - **AM Peak (7:30–8:30 AM)**
   - **Late Night (10:00–11:00 PM)**

5. Generated 15 / 30 / 45-minute convex-hull isochrones
6. Merged job data to calculate:

   - Jobs reachable within each threshold
   - Percent change in access from AM Peak to Late Night

The six tracts are **illustrative rather than statistically representative**, chosen to span EPC and non-EPC conditions while keeping runtime feasible and results interpretable.

---

### **4. Visualization Strategy**

All visualizations are designed to maximize **expressiveness and effectiveness**:

- Clear legends and consistent color encodings
- Explicit EPC vs non-EPC distinctions
- Interactivity to support comparison across time windows and thresholds

Final interactive deliverables include:

- **Isochrone Explorer** (`visualizations/explorer_all_thresholds.html`)
- **Animated Accessibility Chart** (`visualizations/animated_accessibility_slider.html`)

Static figures include paired maps, percent-change bar charts, and midterm proxy comparisons.

---

## **Key Findings**

### **Midterm (Proxy Model)**

- EPC tracts initially appear to reach substantially more jobs, particularly along the Oakland–San Leandro corridor.
- These patterns reflect proximity rather than actual service availability.

### **Final (Time-Aware R5 Results)**

- **Late-night accessibility declines sharply**, with some tracts losing 20–30% of reachable jobs.
- **EPC tracts often experience the steepest losses**, reflecting reliance on frequent trunk lines that do not run late.
- Some non-EPC suburban tracts show small gains due to congestion-free conditions, but these gains occur on top of lower baseline access.
- Time-of-day analysis reveals **inequities that static, all-day metrics conceal**.

---

## **Limitations**

- **LODES:** suppressed and imputed job counts
- **ACS:** sampling noise in low-population tracts
- **GTFS:** scheduled service does not capture reliability or delays
- **Spatial aggregation:** MAUP and ecological fallacy at the tract level
- **Isochrone construction:** convex hulls may over-approximate reach

Despite these limitations, the relative patterns across time windows and community types are robust.

---

## **Reproducibility**

- Fully reproducible **Restart & Run All** notebook
- All file paths are relative
- Environment locked via `requirements.txt`
- Data dictionary included (`docs/data_dictionary.txt`)
- Large raw datasets are intentionally excluded and fetched or referenced within the notebook

This structure prioritizes transparency by making modeling choices, assumptions, and transformations visible at each step.

---

## **License**

MIT License — unrestricted academic and research use.

---

## **Contact**

**Rabah Babaci**
**Adithya Ayanam**
Mentor: **Metropolitan Transportation Commission (MTC)**

---
