Perfect 🔥 — here’s your **final, fully polished `README.md`** that covers **every aspect of your CYPLAN 101 Track B project** — from setup and execution to report generation, deliverables, and reproducibility.

It’s written for both your **MTC mentor** and **future portfolio reviewers**, so anyone can clone, run, and generate the full report end-to-end.

---

````markdown
# 🚆 Transit Access Equity: Time-Aware Isochrones in the Bay Area

**CYPLAN 101 — Introduction to Urban Data Analytics (Fall 2025)**  
**Track B (Advanced)** | **Mentor:** Metropolitan Transportation Commission (MTC)  
**Team:** Rabah Babaci & Adithya Ayanam

---

## ⚡ Quickstart

1. **Clone the repo**
   ```bash
   git clone https://github.com/<your-username>/MTC-Transit_Access_Equity.git
   cd MTC-Transit_Access_Equity
   ```
````

2. **Create a virtual environment**

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate      # Mac/Linux
   .venv\Scripts\activate         # Windows
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Add your Census API key**
   Create `.env` in the repo root:

   ```bash
   CENSUS_API_KEY=your_api_key_here
   ```

5. **Run the notebook**

   ```bash
   jupyter notebook notebooks/transit_access_equity_midterm.ipynb
   ```

   👉 All outputs save automatically to `/data/processed/`, `/visualizations/`, and `/results/`.

---

## 🧭 Project Overview

This project examines **transit accessibility equity** across Alameda County, California, focusing on **Equity Priority Communities (EPCs)** from _MTC Plan Bay Area 2050_.
Using census and employment data, we estimate how many **jobs are reachable within 30 minutes** and compare patterns between EPC and non-EPC tracts.

**Key Questions**

- How does transit accessibility vary by location and time of day?
- Which EPC areas experience the largest job access gaps?
- How can time-aware metrics inform equitable transit and EV planning?

---

## 🗂️ Repository Structure

```
MTC-Transit_Access_Equity/
├── data/
│   ├── raw/                 # Original datasets (EPC, TIGER, ACS, LODES)
│   └── processed/           # Cleaned & merged GeoJSON files
├── visualizations/          # Interactive map + charts
├── results/                 # Early findings + final PDF report
├── docs/                    # Data dictionary & documentation
├── notebooks/               # Main Jupyter notebook (midterm + future)
├── src/                     # Helper scripts (future Track B work)
├── requirements.txt         # Python dependencies
└── README.md                # This file
```

---

## ⚙️ Environment Setup

> Requires Python 3.12+ and a valid Census API key.

### 1️⃣ Clone & activate

```bash
git clone https://github.com/<your-username>/MTC-Transit_Access_Equity.git
cd MTC-Transit_Access_Equity
python3 -m venv .venv
source .venv/bin/activate
```

### 2️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 3️⃣ Run analysis

Open:

```bash
notebooks/transit_access_equity_midterm.ipynb
```

Run cells 1–12 sequentially to reproduce the full midterm analysis.

---

## 🧾 Report Generation

The notebook automatically creates outputs in `/data/processed/`, `/visualizations/`, and `/results/`.
To generate the final PDF report (`results/report.pdf`):

### Option 1 — Export from Jupyter or VS Code (Recommended)

1. Open the notebook: `notebooks/transit_access_equity_midterm.ipynb`
2. Run **all cells** (Sections 1–12).
3. Go to **File → Save and Export As → PDF**.
4. Save to `results/report.pdf`.

### Option 2 — Command-line (Reproducible)

Install the web PDF exporter and Chromium renderer:

```bash
pip install "nbconvert[webpdf]"
python -m playwright install chromium
```

Then run:

```bash
jupyter nbconvert notebooks/transit_access_equity_midterm.ipynb \
    --to webpdf \
    --output-dir results \
    --output report.pdf
```

### Optional — HTML version

```bash
jupyter nbconvert notebooks/transit_access_equity_midterm.ipynb \
    --to html --output-dir results --output report.html
```

---

## 📊 Key Outputs

| Output           | Path                                   | Description                            |
| ---------------- | -------------------------------------- | -------------------------------------- |
| Cleaned dataset  | `data/processed/alameda_final.geojson` | Merged tract-level dataset             |
| Interactive map  | `visualizations/epc_map.html`          | EPC (red) vs non-EPC (blue) tracts     |
| Boxplot          | `visualizations/jobs30_boxplot.png`    | Distribution of jobs within 30 min     |
| Choropleth       | `visualizations/jobs30_map.png`        | Spatial variation of accessibility     |
| Bar chart        | `visualizations/jobs30_bar_chart.png`  | Summary comparison by EPC status       |
| Findings summary | `results/early_findings.md`            | Markdown analysis summary              |
| Final report     | `results/report.pdf`                   | 4–5 page CYPLAN 101 midterm submission |
| Data dictionary  | `docs/data_dictionary.txt`             | Field definitions for datasets         |

---

## 💡 Method Summary

- **Datasets:** MTC Plan Bay Area 2050 (EPC) | TIGER/Line 2022 | ACS 2018–2022 | LODES 2021
- **Libraries:** Python | GeoPandas | Folium | Matplotlib | r5py (for final phase)
- **Metric:** Jobs reachable within 30 min (centroid distance @ 40 km/h proxy)
- **Equity focus:** EPC vs non-EPC tracts in Alameda County

---

## 📈 Midterm Findings

- EPC tracts can reach ≈ **75 000 more jobs** within 30 minutes than non-EPCs (~22 % higher accessibility).
- Highest accessibility clustered along the **West County corridor** (Berkeley → Oakland → Hayward).
- Non-EPC eastern tracts show lower job density and limited transit reach.
- Findings validated through choropleth, boxplot, and bar chart visuals.

---

## ⏭️ Next Steps (Final Phase — Track B)

1. Integrate 511 GTFS feeds + OpenStreetMap extracts.
2. Build R5py transport network for time-aware accessibility.
3. Generate 15 / 30 / 45 min isochrones (AM, Mid, PM, Late).
4. Compare EPC vs non-EPC accessibility gaps.
5. Visualize results with Kepler.gl or Plotly animation.

---

## 🤝 Collaboration Workflow

- **main** branch = stable release.
- Develop each section on feature branches (e.g., `feature/section3-cleaning`).
- Merge via Pull Request after review.
- Commit message style:

  ```
  Add Section X: <short description>
  ```

---

## 🧩 Reproducibility Badge

To re-execute and export the entire pipeline automatically:

```bash
bash run_all.sh
```

_(See optional `run_all.sh` script for one-command execution.)_

---

## 🧾 License

**MIT License** — Open for educational and research use.

---

## 📚 References

- Metropolitan Transportation Commission — _Plan Bay Area 2050 EPC Data_
- US Census Bureau — _TIGER/Line 2022_, _ACS 2018–2022 5-Year Estimates_
- 511 SF Bay Open Data — _GTFS Feeds_
- OpenStreetMap — _Network & POI Data_
- LEHD LODES (2021) — _Workplace Area Characteristics_
- r5py Documentation — [https://r5py.readthedocs.io](https://r5py.readthedocs.io)

---

## 📧 Contact

**Rabah Babaci** — [LinkedIn](https://linkedin.com/in/rabahbabaci)
**Adithya Ayanam** — UC Berkeley, CYPLAN 101
**Mentor:** Metropolitan Transportation Commission (MTC)

```

```
