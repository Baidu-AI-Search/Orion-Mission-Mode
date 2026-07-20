# US Cities Density & Population Concentration Report

**Dataset:** `us_cities_top1000.csv` — the 1,000 largest US cities by population
**Columns analyzed:** City, State, Population, lat, lon
**Total cities in dataset:** 1,000
**Total population represented:** 131,132,443
**Distinct states + DC represented:** 51 (all 50 states + District of Columbia)

---

## 1. Population Concentration by State

"Average population per city" in each state is computed as the total population of that state's cities that appear in the top-1000 list divided by the number of such cities. States whose smaller cities don't clear the top-1000 threshold will naturally show higher averages, but the ranking still reveals meaningful patterns about urban scale.

### Top 10 States — Highest Average City Population

| Rank | State | Cities in Top 1000 | Total Pop (in dataset) | Avg Pop per City |
|------|-------|--------------------|-------------------------|------------------|
| 1 | District of Columbia | 1 | 646,449 | 646,449 |
| 2 | New York | 17 | 9,933,332 | 584,314 |
| 3 | Hawaii | 1 | 347,884 | 347,884 |
| 4 | Alaska | 1 | 300,950 | 300,950 |
| 5 | Nevada | 6 | 1,481,832 | 246,972 |
| 6 | Kentucky | 5 | 1,079,181 | 215,836 |
| 7 | Nebraska | 4 | 807,304 | 201,826 |
| 8 | Pennsylvania | 13 | 2,598,080 | 199,852 |
| 9 | Arizona | 25 | 4,691,466 | 187,659 |
| 10 | Texas | 83 | 14,836,230 | 178,750 |

**Key takeaway:** The very top of the list is dominated by single-city states/territories (DC, Hawaii, Alaska) where only the dominant metro area makes the cut. Among states with multiple represented cities, **New York** stands out dramatically — its cities average ~584K residents, propelled by New York City itself. Sun Belt giants **Arizona**, **Texas**, and **Nevada** also show high averages, reflecting large, sprawling metro areas.

### Bottom 10 States — Lowest Average City Population

| Rank | State | Cities in Top 1000 | Total Pop (in dataset) | Avg Pop per City |
|------|-------|--------------------|-------------------------|------------------|
| 1 | Vermont | 1 | 42,284 | 42,284 |
| 2 | West Virginia | 2 | 99,998 | 49,999 |
| 3 | Delaware | 2 | 108,891 | 54,446 |
| 4 | Wyoming | 2 | 122,076 | 61,038 |
| 5 | Maine | 1 | 66,318 | 66,318 |
| 6 | South Carolina | 12 | 812,734 | 67,728 |
| 7 | Montana | 4 | 277,392 | 69,348 |
| 8 | North Dakota | 4 | 281,945 | 70,486 |
| 9 | Mississippi | 6 | 427,944 | 71,324 |
| 10 | Utah | 19 | 1,440,569 | 75,819 |

**Key takeaway:** These are mostly rural/mountain states with small, widely dispersed populations. **Vermont** barely clears the top-1000 threshold with only Burlington represented. Notably, **South Carolina** (12 cities) and **Utah** (19 cities) make the bottom-10 despite sending many cities to the list — a sign of relatively flat city-size hierarchies with many mid-sized communities.

---

## 2. Single-City Dominance

For states with **5 or more** cities in the dataset, we measured the share of the state's total dataset population concentrated in its single largest city. This is a classic "primate city" indicator.

### Top 5 States Where the Largest City Dominates Most

| Rank | State | Largest City | Largest City Pop | State Total (in dataset) | % in Largest City | # Cities |
|------|-------|--------------|------------------|---------------------------|--------------------|----------|
| 1 | New York | New York City | 8,405,837 | 9,933,332 | **84.6%** | 17 |
| 2 | Maryland | Baltimore | 622,104 | 954,852 | **65.2%** | 7 |
| 3 | Pennsylvania | Philadelphia | 1,553,165 | 2,598,080 | **59.8%** | 13 |
| 4 | New Mexico | Albuquerque | 556,495 | 953,296 | **58.4%** | 7 |
| 5 | Kentucky | Louisville | 609,893 | 1,079,181 | **56.5%** | 5 |

**Key takeaway:**
- **New York is the textbook primate-city state:** NYC alone holds nearly 85% of the population of all New York State cities in the dataset — and it outpaces the second-largest (Buffalo) by an enormous margin.
- **Baltimore**, **Philadelphia**, **Albuquerque**, and **Louisville** each dominate their states in a classic core-city-without-a-peer pattern. These states lack a true "second city" of comparable scale.
- Note that Rhode Island, Delaware, Hawaii, Alaska, DC, Maine, Vermont, Wyoming, West Virginia, and South Dakota were excluded from this calculation because they have fewer than 5 cities in the top 1000; several of these would show near-100% primacy if the threshold were relaxed.

---

## 3. State Representation

All 50 states plus the District of Columbia appear in the top-1000 list (**51 jurisdictions total**).

### States with the Most Cities in the Top 1000

| Rank | State | # of Cities |
|------|-------|-------------|
| 1 | California | 212 |
| 2 | Texas | 83 |
| 3 | Florida | 73 |
| 4 | Illinois | 52 |
| 5 | Massachusetts | 36 |
| 6 | Ohio | 33 |
| 7 | Michigan | 31 |
| 8 | Washington | 28 |
| 9 | Arizona | 25 |
| 10 | Minnesota | 24 |

**Key takeaway:** **California** alone supplies over a fifth of all top-1000 cities (212 of 1000), a staggering reflection of its size and decentralized metro structure. The "big three" Sun Belt states — CA, TX, FL — together contribute **368 cities** (36.8% of the list). The Rust Belt/Upper Midwest is also well represented (IL, OH, MI, MN), reflecting the historic legacy of many industrial-era mid-sized cities.

### States with the Fewest Cities (1 or 2)

| State | # of Cities |
|-------|-------------|
| Alaska | 1 |
| Hawaii | 1 |
| District of Columbia | 1 |
| Maine | 1 |
| Vermont | 1 |
| Delaware | 2 |
| South Dakota | 2 |
| West Virginia | 2 |
| Wyoming | 2 |

**Key takeaway:** Five jurisdictions have only a single city in the top 1000 — including DC (which *is* a single city by definition), and sparsely-populated states like Alaska (Anchorage), Hawaii (Honolulu), Vermont (Burlington), and Maine (Portland). Four more states have exactly two cities. These states are characterized by low total population, low urbanization, or both.

---

## 4. Population Inequality Within States

For states with **10 or more** cities in the dataset, we calculated the ratio of the largest city's population to the smallest city's population (higher ratio = more unequal city-size distribution). A coefficient of variation (CV) is also shown for robustness.

### States with the Most Unequal (Most Top-Heavy) Distributions

| Rank | State | # Cities | Largest City Pop | Smallest City Pop | Largest ÷ Smallest | CV |
|------|-------|----------|------------------|-------------------|--------------------|-----|
| 1 | New York | 17 | 8,405,837 | 37,659 | **223.2×** | 3.45 |
| 2 | California | 212 | 3,884,307 | 37,101 | **104.7×** | 2.24 |
| 3 | Illinois | 52 | 2,718,782 | 37,240 | **73.0×** | 3.17 |
| 4 | Texas | 83 | 2,195,914 | 37,093 | **59.2×** | 1.82 |
| 5 | Arizona | 25 | 1,513,367 | 37,130 | **40.8×** | 1.63 |

**Key takeaway:** These states are each dominated by a single mega-city (NYC, LA, Chicago, Houston, Phoenix) that towers over a long tail of much smaller communities. In **New York**, the NYC-to-smallest-city ratio is an extraordinary **223-to-1** — the state is essentially one giant metropolis plus a collection of small/mid-sized cities (Buffalo, Rochester, Syracuse, Yonkers, etc.). **Illinois** shows the same Chicago-centric pattern.

### States with the Most Equal (Flattest) Distributions

| Rank | State | # Cities | Largest City Pop | Smallest City Pop | Largest ÷ Smallest | CV |
|------|-------|----------|------------------|-------------------|--------------------|-----|
| 1 | South Carolina | 12 | 133,358 | 37,647 | **3.5×** | 0.52 |
| 2 | Connecticut | 15 | 147,216 | 40,347 | **3.6×** | 0.44 |
| 3 | Arkansas | 10 | 197,357 | 40,167 | **4.9×** | 0.56 |
| 4 | Iowa | 13 | 207,510 | 40,566 | **5.1×** | 0.57 |
| 5 | Utah | 19 | 191,180 | 36,956 | **5.2×** | 0.53 |

**Key takeaway:**
- **Connecticut** has the lowest CV (0.44) of any state with 10+ cities — its cities (Bridgeport, New Haven, Stamford, Hartford, Waterbury, etc.) are remarkably similar in size, a legacy of its decentralized, multi-nucleated settlement pattern.
- **South Carolina** shows a similarly flat hierarchy with Charleston/Columbia only modestly larger than mid-tier cities like Greenville and Mount Pleasant.
- **Utah**, **Iowa**, and **Arkansas** represent different flavors of "many-mid-sized-cities" patterns — Utah's strong Mormon settlement heritage of evenly-spaced towns, Iowa's grid of county-seat cities, and Arkansas's dispersed small metros.

---

## 5. Regional Summary

States are grouped using the four standard US Census regions (with DC placed in the Southeast, per Census convention):

- **Northeast:** CT, ME, MA, NH, RI, VT, NJ, NY, PA
- **Midwest:** IL, IN, MI, OH, WI, IA, KS, MN, MO, NE, ND, SD
- **Southeast:** DE, FL, GA, MD, NC, SC, VA, DC, WV, AL, KY, MS, TN, AR, LA, OK, TX
- **West:** AZ, CO, ID, MT, NV, NM, UT, WY, AK, CA, HI, OR, WA

| Region | Cities in Top 1000 | % of All Cities | Total Pop (in dataset) | % of All Pop | Avg City Size |
|--------|--------------------|-----------------|-------------------------|---------------|---------------|
| **West** | 348 | 34.8% | 45,814,296 | 34.9% | 131,650 |
| **Southeast** | 307 | 30.7% | 41,422,799 | 31.6% | 134,928 |
| **Midwest** | 231 | 23.1% | 24,408,828 | 18.6% | 105,666 |
| **Northeast** | 114 | 11.4% | 19,486,520 | 14.9% | 170,934 |

**Key takeaways:**

1. **The West and Southeast dominate in sheer volume.** Together they account for 65.5% of top-1000 cities and 66.5% of the population represented — a clear marker of where American urban growth has been concentrated in recent decades (California + Texas + Florida alone drive much of this).

2. **The Northeast has by far the highest average city size (~171K)** despite contributing the fewest cities. This is driven by the Northeast's older, denser settlement pattern of a few very large cities (NYC, Philadelphia, Boston, etc.) rather than a broad spread of mid-sized ones. It's the most "bimodal" region: a handful of giants, fewer small representatives.

3. **The Midwest has the lowest average city size (~106K)** despite a high number of cities (231). This reflects the region's characteristic pattern of many older industrial cities (Akron, Toledo, Fort Wayne, Grand Rapids, etc.) in the 50K–200K range, without a West/South-style sunbelt mega-city other than Chicago.

4. **The Southeast and West are near-identical in average city size (~132–135K)** but achieve that differently: the Southeast has Texas + Florida pulling the average, while the West is anchored by California. Both are characterized by extensive suburbanization and a broad distribution of city sizes.

---

## Summary Insights

- **Population is extremely concentrated at the top:** New York City alone (8.4M) accounts for ~6.4% of the entire 1000-city dataset. The top-10 cities (NYC, LA, Chicago, Houston, Phoenix, Philadelphia, San Antonio, San Diego, Dallas, San Jose) together represent a large share of total population.
- **California is in a category of its own**, with more than 1 in 5 top-1000 cities — more than the entire Northeast region.
- **Two distinct urban models emerge across states:**
  - *Primate-city states* (NY, IL, MD, PA, NM, KY) where one dominant metro overshadows all others.
  - *Decentralized states* (CT, SC, UT, IA, AR) where cities are more evenly sized.
- **Regional patterns mirror historical and economic development paths:** the Northeast's dense old core, the Midwest's network of industrial mid-sized cities, the Sun Belt's sprawling growth across Southeast and West.

---

*Report generated from analysis of `us_cities_top1000.csv`. Region classification follows US Census Bureau definitions.*
