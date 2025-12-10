
# 7. Early Findings & Notes

- **Spatial patterns (maps)**
  - EPC tracts cluster along the West County corridor (Berkeley → Oakland → San Leandro → Hayward), close to major job centers and dense transit service.
  - Eastern Alameda County (Dublin, Pleasanton, Livermore) is overwhelmingly non-EPC and shows lower accessibility in the pilot metric.

- **Pilot accessibility results (jobs ≤30 minutes, centroid proxy)**
  - EPC tracts reach roughly **420k–450k jobs**, while non-EPC tracts average around **340k–365k**.
  - *Interpretation:* even with a simplified straight-line model, EPC areas appear more connected to employment clusters.

- **Population context**
  - EPC tracts tend to include more established, older urban neighborhoods with moderate population levels, while many non-EPC tracts in eastern Alameda County are lower-density and auto-oriented.

- **Data joins & keys**
  - GEOIDs required consistent zero-padding (state–county–tract = 11 digits).
  - LODES WAC data needed block → tract aggregation (`w_geocode` → first 11 digits, sum `C000`).
  - One Alameda tract has no reported LODES jobs (water/no-employment area), which is expected and handled in the merge.

- **Planning takeaways**
  - Strong accessibility in EPC corridors highlights where transit or EV charging integration would serve the most people.
  - Lower-access non-EPC suburban tracts point to potential gaps for first/last-mile improvements or demand-response services.

- **Method limitations (pilot metric only)**
  - Straight-line distance with constant 40 km/h speed ignores actual road network and transit schedules.
  - LODES data (2021) may reflect post-COVID changes in job locations.
  - Final accessibility results will use **R5-based, time-specific travel times** rather than centroid approximations.

- **Quality checks**
  - CRS standardized to EPSG:4326 for mapping and EPSG:26910 for distance calculations.
  - All geometries validated; EPC tags and population attributes merged cleanly across sources.

**Quantitative Summary**

| Group | Tract Count | Mean Jobs ≤30 min | Median | Min | Max |
|------:|------------:|------------------:|-------:|----:|----:|
| **EPC**     | 97  | 419k | 422k | 322k | 482k |
| **Non-EPC** | 282 | 344k | 366k | 53k  | 488k |

**Interpretation:**  
EPC tracts can access **~75k more jobs** within 30 minutes than non-EPC tracts (≈22% higher), suggesting their proximity to major job corridors.

**Boxplot interpretation**
- EPC tracts show higher medians and a more compact range.
- Non-EPC tracts show a wider spread and much lower minimums, reflecting suburban isolation.

**Key early takeaway**  
EPC tracts show consistently strong job accessibility even under a simple model, while non-EPC suburban areas lag behind.  
The final phase will evaluate whether these gaps persist once we use **network-based, time-aware isochrones** for AM peak vs. late night.

**Data sources:**  
MTC Plan Bay Area 2050 (EPC), US Census TIGER/ACS 2022, LEHD LODES 2021 (WAC).

