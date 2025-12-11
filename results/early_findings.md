
# 7. Early Findings & Notes

- **Spatial patterns (maps)**
  - EPC tracts cluster along the West County corridor (Berkeley → Oakland → San Leandro → Hayward), where transit services and job densities are highest.
  - Eastern Alameda County (Dublin, Pleasanton, Livermore) is overwhelmingly non-EPC and shows noticeably lower accessibility under the pilot metric.

- **Pilot accessibility results (jobs ≤ 30 minutes, centroid proxy)**
  - EPC tracts reach roughly **420k–450k jobs**, while non-EPC tracts average **340k–365k**.
  - *Interpretation:* Even in a simplified model, EPC tracts appear closer to major employment centers, which raises the question of how time-of-day service levels will change this relationship.

- **Population context**
  - EPC tracts tend to include older, denser urban neighborhoods.
  - Many non-EPC tracts in eastern Alameda County are lower-density and more auto-oriented, contributing to lower estimated accessibility.

- **Data joins & keys**
  - GEOIDs required consistent zero-padding (state–county–tract = 11 digits).
  - LODES WAC data required block → tract aggregation (`w_geocode` → first 11 digits; sum `C000`).
  - One Alameda tract shows no LODES employment (water/no-employment area), which is expected.

- **Planning takeaways (preliminary)**
  - High-access EPC corridors reflect proximity to concentrated job centers in Oakland and Berkeley.
  - Low-access non-EPC suburban tracts illustrate structural distance from employment hubs and may depend more heavily on car travel.
  - *These patterns must be re-evaluated with real transit travel times,* especially for **late-night** service where frequency drops sharply.

- **Method limitations (pilot metric only)**
  - Straight-line distances with a constant speed do not represent real transit or road travel.
  - Accessibility is overestimated compared to what we expect from network-based routing.
  - LODES 2021 may reflect post-COVID shifts in employment geography.
  - The final accessibility analysis will use **R5-based, time-specific travel times** (AM Peak and Late-Night).

- **Quality checks**
  - CRS standardized to EPSG:4326 for mapping and EPSG:26910 for distance calculations.
  - All geometries validated; EPC flags and ACS attributes merged cleanly.

## Quantitative Summary

| Group       | Tract Count | Mean Jobs ≤30 min | Median | Min  | Max  |
|------------:|------------:|------------------:|-------:|-----:|-----:|
| **EPC**     | 97          | 419k              | 422k   | 322k | 482k |
| **Non-EPC** | 282         | 344k              | 366k   | 53k  | 488k |

**Interpretation:**  
EPC tracts have access to **~75k more jobs** within 30 minutes under the centroid proxy (≈22% higher). This reflects their physical proximity to major job centers, not necessarily better transit service. The R5 analysis will test whether this advantage persists—or disappears—when using real scheduled transit travel times.

## Accessibility Distribution Interpretation
- EPC tracts show higher medians and a more compact range.
- Non-EPC tracts show wider variability and very low minimum values due to spatial distance from employment clusters.
- These early patterns help justify computing **time-aware isochrones** to understand how AM Peak vs Late-Night service affects accessibility gaps.

## Key Early Takeaway
EPC tracts show consistently high proximity-based accessibility, while non-EPC suburban tracts lag behind.  
**The next phase will evaluate whether these differences grow, shrink, or reverse when using R5-based travel times for AM Peak and Late-Night scenarios.**

**Data sources:**  
MTC Plan Bay Area 2050 (EPC), US Census TIGER/ACS 2022, LEHD LODES 2021 (WAC).

