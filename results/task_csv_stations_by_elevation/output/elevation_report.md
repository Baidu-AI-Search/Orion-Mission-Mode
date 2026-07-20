# Idaho Weather Stations — Elevation Analysis

**Data source:** `idaho_weather_stations.csv`  

**Stations analyzed:** 213

## Summary Statistics

| Metric | Elevation (feet) |
|---|---|
| Minimum | 995 |
| Maximum | 9,150 |
| Mean | 4,859 |
| Median | 4,920 |
| Range | 8,155 |

## Top 10 Highest-Elevation Stations

| Rank | Station Name | Elevation (ft) | County | Managing Agency |
|---|---|---|---|---|
| 1 | MEADOW LAKE SNOTEL | 9,150 | Lemhi | NRCS |
| 2 | VIENNA MINE PILLOW | 8,960 | Camas | NRCS |
| 3 | MILL CREEK SUMMIT SNOTEL | 8,800 |   | NRCS |
| 4 | GALENA SUMMIT SNOTEL | 8,780 | Blaine | NRCS |
| 5 | DOLLARHIDE SUMMIT SNOTEL | 8,420 | Custer | NRCS |
| 6 | FRANKLIN BASIN PILLOW | 8,170 | Franklin | NRCS |
| 7 | HILTS CREEK SNOTEL | 8,000 | Custer | NRCS |
| 8 | HOWELL CANYON SNOTEL | 7,980 |   | NRCS |
| 9 | LOST-WOOD DIVIDE PILLOW | 7,900 | Blaine | NRCS |
| 10 | BEAR CANYON SNOTEL | 7,900 | Custer | NRCS |

## Bottom 10 Lowest-Elevation Stations

| Rank | Station Name | Elevation (ft) | County | Managing Agency |
|---|---|---|---|---|
| 1 | DWORSHAK FISH HATCHERY | 995 | Clearwater | NWS |
| 2 | KAMIAH | 1,212 | Lewis | NWS |
| 3 | KOOSKIA | 1,280 | Lewis | NWS |
| 4 | OROFINO | 1,320 | Clearwater | NWS |
| 5 | LEWISTON W.B  AP | 1,436 | Nez Perce | NWS |
| 6 | FENN RANGER STATION | 1,585 | Idaho | NWS |
| 7 | PORTHILL | 1,775 | Boundary | NWS |
| 8 | RIGGINS | 1,800 | Idaho | NWS |
| 9 | BROWNLEE DAM | 1,844 | Adams | NWS |
| 10 | BONNERS FERRY 1 SW | 1,860 | Boundary | NWS |

## Elevation Distribution by Managing Agency: NWS vs. NRCS

| Agency | Number of Stations | Average Elevation (ft) |
|---|---|---|
| NWS (National Weather Service) | 143 | 3,993 |
| NRCS (Natural Resources Conservation Service — SNOTEL/SNOW) | 70 | 6,628 |
| Difference (NRCS − NWS) |  | **+2,635 ft** |

**Interpretation:** NRCS-managed stations sit, on average, **2,635 feet higher** than NWS stations in Idaho. This is expected because NRCS operates the SNOTEL/SNOW-TELEMETRY network, which is purpose-built to monitor mountain snowpack and therefore intentionally places stations at high elevations in the headwaters of river basins. NWS cooperative observer (COOP) stations, by contrast, tend to be located near populated communities, airports, and agricultural areas in the valleys and plains, where people live and where surface-weather observations are most needed for forecasts and records. As a result, NWS sites are clustered at lower elevations while NRCS sites skew strongly toward Idaho's mountain zones.

## Average Elevation by County (≥3 stations)

> *Note: 5 stations (BEAR SADDLE SNOTEL, HOWELL CANYON SNOTEL, MILL CREEK SUMMIT SNOTEL, MOOSE CREEK SNOTEL, SQUAW FLAT PILLOW) had a blank/unknown county field in the source file and were excluded from county-level averages.*

- **Highest average elevation:** **Franklin County** — average **6,793 ft** across 3 stations.
- **Lowest average elevation:** **Canyon County** — average **2,367 ft** across 3 stations.

### Top 5 Counties by Average Station Elevation

| County | Stations | Avg Elevation (ft) |
|---|---|---|
| Franklin | 3 | 6,793 |
| Blaine | 11 | 6,677 |
| Custer | 10 | 6,583 |
| Bear Lake | 4 | 6,511 |
| Caribou | 5 | 6,182 |

### Bottom 5 Counties by Average Station Elevation

| County | Stations | Avg Elevation (ft) |
|---|---|---|
| Canyon | 3 | 2,367 |
| Lewis | 5 | 2,665 |
| Latah | 3 | 2,820 |
| Ada | 5 | 2,914 |
| Gooding | 3 | 3,237 |

## Summary

Across Idaho's 213 weather stations, elevations range dramatically from **995 ft** at DWORSHAK FISH HATCHERY in Clearwater County to **9,150 ft** at MEADOW LAKE SNOTEL in Lemhi County — a vertical spread of more than 8,155 feet. The mean elevation (4,859 ft) is nearly identical to the median (4,920 ft), indicating the distribution is **roughly symmetric** around the center, with the lowest-elevation stations (valley NWS sites around ~1,000–2,000 ft) balanced by a cluster of very high NRCS SNOTEL sites above 7,000 ft. This pattern reflects Idaho's dramatic topography — broad agricultural lowlands along the Snake River Plain and deep river canyons (Clearwater, Salmon, Snake) flanked by high ranges such as the Sawtooth, Salmon River, Bitterroot, Lost River, and Tetons — and the dual mission of the observing network: NWS stations concentrate where people live and farm in the valleys and plains, while NRCS stations climb into the mountains to track the snowpack that feeds the state's rivers and reservoirs. County averages mirror this geography, with high-elevation mountain counties (Franklin, Blaine, Custer, Bear Lake) hosting the tallest stations and low-elevation Snake River Plain / Palouse counties (Canyon, Lewis, Latah, Ada) hosting the lowest.
