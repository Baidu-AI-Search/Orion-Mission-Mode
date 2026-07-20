# Geographic Distribution of the 1,000 Largest U.S. Cities

*Analysis based on `us_cities_top1000.csv` (1,000 cities with columns City, State, Population, lat, lon).*

Total population represented: **131,132,443** across 1,000 cities (average 131,132 per city; median 68,207).

---

## 1. Geographic Center of Population

| Centroid type | Latitude | Longitude | Approx. location |
|---|---|---|---|
| **Population-weighted** | **36.9857° N** | **−96.5109° W** | Near Tulsa, northeastern Oklahoma |
| **Unweighted (simple mean)** | 37.3382° N | −96.4830° W | South-central Kansas, near Wichita |

**Difference:** weighted − unweighted = **−0.35° latitude, −0.03° longitude**.

**Interpretation.** The two centroids sit roughly 39 km (≈24 miles) apart — the population-weighted center is shifted slightly **south and marginally west** of the geometric mean of city locations. The offset is small because the top-1,000 cities are broadly distributed, but the southward shift indicates that very large cities in the Sun Belt (e.g., Houston, San Antonio, Dallas, Phoenix, Los Angeles, San Diego) pull the population center slightly toward the south. The near-zero longitudinal difference reflects that the country's heaviest population clusters (the Northeast megalopolis, Florida, Texas, and Southern California) roughly balance each other east-west around −96.5° W — a longitude that coincides remarkably well with the official U.S. population center (which has been migrating southwest toward Texas/Oklahoma for decades). The unweighted centroid being farther north shows that the **Great Plains, Midwest, and Northeast have more *cities* in the dataset, while Southern/Southwestern metros punch above their weight in *population***.

---

## 2. Geographic Extremes

| Direction | City | State | Latitude | Longitude | Population |
|---|---|---|---|---|---|
| **Northernmost** | Anchorage | Alaska | 61.2181° N | −149.9003° W | 300,950 |
| **Southernmost** | Honolulu | Hawaii | 21.3069° N | −157.8583° W | 347,884 |
| **Easternmost** | Portland | Maine | 43.6615° N | −70.2553° W | 66,318 |
| **Westernmost** | Honolulu | Hawaii | 21.3069° N | −157.8583° W | 347,884 |

Notes:
- Honolulu is both the southernmost *and* westernmost city among the top 1,000 — Hawaii's mid-Pacific location places it farther west than any mainland or Alaskan city large enough to make the list.
- In the contiguous 48 states, the westernmost large city in the dataset is in coastal Washington/California (longitudes near −122° to −124°), but those are still ~35° east of Honolulu.
- Portland, ME edges out East Coast cities like Boston (−71.06°) and New York (−74.00°) as the easternmost major city.

---

## 3. Latitude Band Analysis

Cities are grouped into 5° latitude bands:

| Latitude band | City count | % of cities | Total population | Avg. city population |
|---|---:|---:|---:|---:|
| Below 30°N (South / Florida, South TX, SoCal low desert, HI) | 96 | 9.6% | 12,853,738 | 133,893 |
| 30–35°N (Deep South, Southern CA, AZ, NM) | 279 | 27.9% | 38,580,127 | 138,280 |
| 35–40°N (Mid-South, TN/NC/CA central, SoCal, OK, AR) | 257 | 25.7% | 36,242,762 | 141,022 |
| 40–45°N (Northeast, Midwest, Great Plains, PNW) | 316 | 31.6% | 38,062,264 | 120,450 |
| Above 45°N (Far North, New England north, Upper Midwest, AK) | 52 | 5.2% | 5,393,552 | 103,722 |

**Key takeaways:**
- **Most cities:** the **40–45°N band** (316 cities), a wide swath that contains New York, Chicago, Philadelphia, Boston, Detroit, Seattle, Portland, Minneapolis, and most of the Northeast/Midwest urban core.
- **Most total population:** the **30–35°N band** (≈38.6 million residents), narrowly edging out 40–45°N. This is the Sun Belt powerhouse — it includes Los Angeles, Houston, Dallas, San Antonio, San Diego, Phoenix, Austin, Jacksonville, Charlotte, Memphis, and many of the fastest-growing metros.
- **Largest average city size:** also the **35–40°N band** (141,022), which sweeps in the Bay Area, LA exurbs, Denver, Las Vegas, Nashville, and the Research Triangle.
- The **above-45°N band** is the smallest in city count and has the smallest average size. It includes Seattle (47.6°N), Spokane, Tacoma, Portland OR (45.5°N), Fargo, Anchorage, and several smaller Upper Midwest and New England cities — reflecting sparser settlement (outside the Puget Sound outliers) at higher latitudes. Minneapolis (44.98°N) and Boston (42.4°N) fall just below the 45° cutoff.

---

## 4. East–West Split (dividing line: −95° W)

The meridian −95° W runs roughly through eastern Kansas/Tulsa, OK — a commonly used proxy separating the Eastern U.S. from the Great Plains/West.

| Half | # of cities | % of cities | Total population | Average city size |
|---|---:|---:|---:|---:|
| **East** (lon > −95°) | 546 | 54.6% | 67,016,952 | 122,742 |
| **West** (lon ≤ −95°) | 454 | 45.4% | 64,115,491 | 141,224 |

**Takeaways:**
- The East contains **~92 more cities** than the West — i.e., the eastern half of the country has a denser, more distributed network of mid-sized cities.
- The two halves are surprisingly close in total population (≈2.9 M more people in the East, a gap of only ~2%), but the **western cities are larger on average (~15% bigger mean population)**.
- This is the classic signature of Western urbanization: fewer, larger, more-spaced-apart metropolitan areas (Los Angeles, Phoenix, San Diego, Dallas, Houston, San Antonio, San Jose, San Francisco, Seattle, Denver, Las Vegas, Portland) vs. a more crowded Eastern landscape of many smaller industrial/post-industrial cities packed closer together.

---

## 5. State Geographic Spread (states with 10+ cities)

For each state with at least 10 of the top-1,000 cities, "geographic spread" is computed simply as max − min of latitude and longitude among that state's cities (in degrees). The combined diagonal (Euclidean distance in degrees between the NE-most and SW-most corners of the bounding box) gives a single ranking of how geographically sprawling the state's city network is.

### Top 10 most geographically spread-out states

| Rank | State | # cities in top 1000 | Latitude span (°) | Longitude span (°) | Diagonal spread (°) |
|---:|---|---:|---:|---:|---:|
| 1 | **Texas** | 83 | 9.32 | 12.50 | **15.59** |
| 2 | **California** | 212 | 7.95 | 7.22 | 10.73 |
| 3 | **Florida** | 73 | 4.97 | 7.16 | 8.72 |
| 4 | **Tennessee** | 17 | 1.51 | 7.70 | 7.84 |
| 5 | **Washington** | 28 | 3.11 | 5.66 | 6.46 |
| 6 | **New York** | 17 | 2.50 | 5.45 | 6.00 |
| 7 | **Missouri** | 16 | 2.68 | 5.33 | 5.97 |
| 8 | **Iowa** | 13 | 1.27 | 5.82 | 5.96 |
| 9 | **Arizona** | 25 | 3.65 | 4.35 | 5.68 |
| 10 | **North Carolina** | 22 | 1.87 | 5.19 | 5.51 |

### Observations
- **Texas** is the overwhelming leader: with 83 cities in the top 1,000 spanning more than 9 degrees of latitude (≈640 miles) and 12.5 degrees of longitude (≈700 miles at that latitude), it is the most geographically dispersed large-state urban network in the country — from Brownsville in the Rio Grande Valley up to Amarillo in the Panhandle, and from Beaumont on the Louisiana border to El Paso in the far west.
- **California** comes second. Despite having by far the *most* top-1,000 cities (212 — more than 1 in 5 of the entire dataset), its cities are compressed along a coast-hugging north–south axis, so its diagonal spread is only ~2/3 that of Texas.
- **Florida** ranks third: a long peninsula gives it ~7° of longitude span (Pensacola to Key West/Jax area) but a relatively narrow latitude range.
- **Tennessee** is an interesting standout — it's a narrow state latitudinally but stretches a surprising ~7.7° of longitude from Memphis in the southwest corner to the Tri-Cities (Johnson City/Kingsport/Bristol) in the northeast — a longer east-west span than Washington, New York, or Arizona despite being a physically smaller state.
- **Iowa** similarly appears in the top 10 almost entirely because of its east-west extent (~5.8°), reflecting how grid-aligned Plains states pack in many mid-sized cities across a wide longitude band.
- Compact or smaller states with 10+ cities (e.g., Massachusetts, New Jersey, Connecticut, Maryland, Ohio, Indiana) cluster at the bottom of the spread ranking, with diagonals under ~3–4°, reflecting the tighter geographic packing of older Eastern urban networks.

---

## Summary

- The **population-weighted center** of the 1,000 largest U.S. cities sits in northeastern Oklahoma (~37.0°N, 96.5°W), only ~40 km from the unweighted geometric mean in south-central Kansas — a small but telling southward tug from Sun Belt megacities.
- Geographic extremes are dominated by the two non-contiguous states: **Anchorage** (north) and **Honolulu** (south and west), with **Portland, ME** at the eastern edge.
- Latitudinally, the **40–45°N** band contains the most cities, while the **30–35°N** Sun Belt band holds the most people — a clear signal of post-war southward population shift.
- The East has more cities; the West has bigger ones — almost a dead heat in total population.
- **Texas** has the most geographically sprawling network of large cities of any U.S. state, with California second and Florida third.
