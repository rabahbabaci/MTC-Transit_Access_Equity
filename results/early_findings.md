
# 7. Early Findings & Notes

- **Spatial pattern (map)**
  - EPC tracts cluster along the West County corridor (Berkeley–Oakland–San Leandro–Hayward) near I-80/I-880 and major transit lines; fewer EPC tracts in the east (Castro Valley–Dublin–Pleasanton–Livermore).
  - EPC areas align with older, denser neighborhoods and job centers; non-EPC tracts dominate suburban/eastern parts of the county.

- **Accessibility pilot (jobs ≤ 30 min, 40 km/h proxy)**
  - Sampled EPC tracts averaged ~380 k reachable jobs, vs ~330 k for non-EPC (~+15 % gap).
  - *Interpretation:* EPC tracts are generally closer to employment clusters, even with a simple centroid-distance proxy.

- **Population context**
  - Many EPC tracts have moderate to high population; pairing high population with higher job accessibility suggests strong demand for transportation and services.

- **Data joins & keys**
  - Consistent 11-digit GEOID was crucial. We rebuilt GEOIDs via zero-padding (state = 2, county = 3, tract = 6) to merge TIGER tracts + EPC + ACS + LODES without mismatches.
  - LODES raw WAC required block→tract aggregation (`w_geocode` → first 11 chars; sum `C000`).

- **Map takeaways for planning**
  - EPC corridors overlap areas where EV charging + transit integration could serve many residents and jobs.
  - Non-EPC eastern tracts show lower accessibility—candidates for first/last-mile or targeted job access strategies.

- **Limitations (methods)**
  - Travel time proxy uses straight-line centroid distance and constant speed (no network or congestion effects).
  - LODES year = 2021 (post-COVID shifts possible).
  - EPC flag simplified (class labels). Threshold sensitivity could be tested.

- **Quality checks**
  - CRS harmonized to EPSG:4326 for storage; EPSG:26910 for distance.
  - Visual inspection shows clean tract coverage and EPC highlighting where expected (west county urban core).

**Quantitative Results**

| Group | Tract Count | Mean Jobs ≤ 30 min | Median | Min | Max |
|:------|-------------:|------------------:|-------:|----:|----:|
| **EPC** | 97 | **419 217** | **421 795** | 321 849 | 482 405 |
| **Non-EPC** | 282 | **344 052** | **365 831** | 53 305 | 488 078 |

*Interpretation:* EPC tracts can reach ≈ 75 000 more jobs within 30 minutes than non-EPCs (~22 % higher accessibility), reflecting stronger links to employment along the western corridor (Berkeley–Oakland–San Leandro–Hayward).

**Boxplot Interpretation**
- EPC tracts show higher medians and tighter spread → consistently high accessibility.  
- Non-EPC tracts show wider variance → mixed access and suburban isolation.

**Key Takeaway**
EPC-designated tracts exhibit substantially higher job access within 30 minutes, highlighting transit and charging-equity needs in lower-access (non-EPC) areas.

**Data sources:** MTC Plan Bay Area 2050 (EPC), US Census TIGER/ACS 2022, LODES 2021 (LEHD).
