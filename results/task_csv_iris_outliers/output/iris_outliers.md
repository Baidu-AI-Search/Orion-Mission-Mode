# Iris Flowers Dataset – Outlier & Unusual Observation Report

**Dataset:** `iris_flowers.csv` (150 samples × 5 columns)  
**Numeric features:** `SepalLength`, `SepalWidth`, `PetalLength`, `PetalWidth` (all in cm)  
**Species:** *Iris-setosa* (n=50), *Iris-versicolor* (n=50), *Iris-virginica* (n=50)  
**Row numbering:** 0-based (as in the CSV, excluding the header row).

---

## 1. Outlier Detection Method

I used a **two-tiered univariate approach** combined with a **multivariate check**, so that single-feature extremes and multi-feature atypical samples are both surfaced:

1. **Tukey's IQR (Interquartile Range) fences** – the classic boxplot rule:
   - Lower fence = Q1 − 1.5 × IQR
   - Upper fence = Q3 + 1.5 × IQR
   - Any observation outside the fences is flagged.
   - This is robust to non-normality and works well on small samples (n=50/species).

2. **Z-score (modified, sample SD)** – flags values with |z| > 3 (i.e. more than 3 standard deviations from the mean).
   - Used as a second opinion; with n=150 (or 50 within-species), z>3 corresponds to the most extreme observations.
   - Note: z-scores are sensitive to the very outliers they are trying to detect, so I do **not** use them as the sole criterion.

3. **Mahalanobis distance (within-species, multivariate)** – flags samples that are unusual when all four features are considered jointly:
   - Computed within each species (using that species' mean vector and covariance matrix).
   - Significance tested against a χ²(4) distribution; p < 0.01 treated as "highly unusual", p < 0.05 as "mildly unusual".

4. **Centroid-distance ratio (mislabel check)** – measures each sample's Euclidean distance to its labeled species centroid and to the centroids of the other two species. Samples closer to a *different* species centroid (distance ratio > 1.2) are listed as potential mislabel candidates.

**Decision rule for "outlier" in the tables below:** flagged by IQR **and/or** z > 3.

---

## 2. Overall Outliers (across the full 150-sample dataset)

The overall IQR fences are extremely wide because the three species are very different (especially setosa's tiny petals vs. virginica's large ones). Consequently almost nothing is an outlier when species are lumped together — except on `SepalWidth`.

| Feature | Q1 | Q3 | IQR | Lower fence | Upper fence | Outlier rows (IQR) | Outlier rows (|z|>3) |
|---|---|---|---|---|---|---|---|
| SepalLength | 5.1 | 6.4 | 1.3 | 3.15 | 8.35 | **none** | none |
| SepalWidth  | 2.8 | 3.3 | 0.5 | 2.05 | 4.05 | **15, 32, 33, 60** | **15** |
| PetalLength | 1.6 | 5.1 | 3.5 | −3.65 | 10.35 | none | none |
| PetalWidth  | 0.3 | 1.8 | 1.5 | −1.95 | 4.05 | none | none |

### Specific overall outlier values

| Row | Species | SepalLength | SepalWidth | PetalLength | PetalWidth | Why flagged |
|---|---|---|---|---|---|---|
| **15** | Iris-setosa | 5.7 | **4.4** | 1.5 | 0.4 | SepalWidth 4.4 > 4.05 fence (z = +3.11 vs. global) |
| 32 | Iris-setosa | 5.2 | **4.1** | 1.5 | 0.1 | SepalWidth 4.1 > 4.05 fence (z = +2.42) |
| 33 | Iris-setosa | 5.5 | **4.2** | 1.4 | 0.2 | SepalWidth 4.2 > 4.05 fence (z = +2.65) |
| **60** | Iris-versicolor | 5.0 | **2.0** | 3.5 | 1.0 | SepalWidth 2.0 < 2.05 fence (z = −2.43) |

Rows 32 and 33 sit just barely outside the global upper fence and are *not* flagged by z > 3, so they are borderline. Rows 15 and 60 are the most extreme on `SepalWidth` in opposite directions.

Boxplot of every feature split by species (whiskers extend to 1.5×IQR; dots are out-of-fence points):
![image](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784181099624364188/d7dce3cc5d82f6dc459bd077852fe290_boxplot_by_species.png?responseContentDisposition=attachment%3B%20filename%3D%22boxplot_by_species.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T05%3A55%3A10Z%2F-1%2Fhost%2F17ca2339acb6e4cf40e818643c6daf15414d32300b84419a1cdd5611b082a28e)
---

## 3. Within-Species Outliers

Outliers detected **inside each species group** are often more informative because they reveal individuals that are extreme *for their own kind* even if they look normal globally.

### 3.1 Iris-setosa (n=50)

Setosa is a very tight cluster, especially on petals. Even small deviations are flagged.

| Feature | Mean | SD | IQR lower | IQR upper | Outlier rows | Values (within-sp z-score) |
|---|---|---|---|---|---|---|
| SepalLength | 5.006 | 0.352 | 4.20 | 5.80 | — | none |
| SepalWidth | 3.418 | 0.381 | 2.30 | 4.50 | — | none (row 15 = 4.4 is inside the within-setosa fence) |
| PetalLength | 1.464 | 0.174 | 1.137 | 1.838 | **13, 22, 24, 44** | 13→1.1 (z=−2.10); 22→1.0 (z=−2.67); 24→1.9 (z=+2.51); 44→1.9 (z=+2.51) |
| PetalWidth | 0.244 | 0.107 | 0.05 | 0.45 | **23, 43** | 23→0.5 (z=+2.39); **43→0.6 (z=+3.32, also z>3)** |

**Notable setosa individuals:**
- **Row 22** (5.1, 3.7? actually 4.6, 3.6, 1.0, 0.2) – shortest petal in the whole dataset (1.0 cm).
- **Row 43** (5.0, 3.5, 1.6, 0.6) – widest petal among setosa by a wide margin (0.6 cm vs. species max normally 0.4–0.5); the only setosa that reaches z>3 in any feature.
- **Row 23** (5.1, 3.3, 1.7, 0.5) – second-widest petal for a setosa.

### 3.2 Iris-versicolor (n=50)

Versicolor is the most "central" species and shows very few univariate outliers.

| Feature | Mean | SD | IQR lower | IQR upper | Outlier rows | Values |
|---|---|---|---|---|---|---|
| SepalLength | 5.936 | 0.516 | 4.55 | 7.35 | — | none |
| SepalWidth | 2.770 | 0.314 | 1.81 | 3.71 | — | none (row 60 = 2.0 sits just *inside* the within-versicolor fence of 1.81) |
| PetalLength | 4.260 | 0.470 | 3.10 | 5.50 | **98** | **98→3.0** (z=−2.68) – the only versicolor petal below 3.1 |
| PetalWidth | 1.326 | 0.198 | 0.75 | 1.95 | — | none |

**Notable versicolor individual:**
- **Row 98** (5.1, 2.5, 3.0, 1.1) – shortest sepal and *by far* the shortest petal of any versicolor (all other versicolor petals are 3.3–5.1). It is also the smallest versicolor overall and sits surprisingly close to the setosa/virginica boundary.

### 3.3 Iris-virginica (n=50)

Virginica has the widest spread and shows several within-species extremes.

| Feature | Mean | SD | IQR lower | IQR upper | Outlier rows | Values |
|---|---|---|---|---|---|---|
| SepalLength | 6.588 | 0.636 | 5.21 | 7.91 | **106** | **106→4.9** (z=−2.65) – shortest sepal in the species |
| SepalWidth | 2.974 | 0.322 | 2.24 | 3.74 | **117, 119, 131** | 117→3.8 (z=+2.56); 131→3.8 (z=+2.56); 119→2.2 (z=−2.40) |
| PetalLength | 5.552 | 0.552 | 3.94 | 7.04 | — | none (species range 4.5–6.9 is continuous) |
| PetalWidth | 2.026 | 0.275 | 1.05 | 3.05 | — | none |

**Notable virginica individuals:**
- **Row 106** (4.9, 2.5, 4.5, 1.7) – *extremely* small sepal (4.9) and short petal (4.5) for a virginica; its measurements look much more like a large versicolor.
- **Row 118** (7.7, 2.6, 6.9, 2.3) – longest petal in the entire dataset (6.9 cm), among the longest sepals (7.7).
- **Row 131** (7.9, 3.8, 6.4, 2.0) – the longest sepal in the dataset (7.9 cm) and an unusually wide sepal (3.8) for a virginica.
- **Row 119** (6.0, 2.2, 5.0, 1.5) – very narrow sepal and a petal width (1.5) at the low extreme for virginica.

---

## 4. Unusual Observations (multivariate / atypical combinations)

Using within-species **Mahalanobis distance** (χ² with 4 d.f.), the following are the most atypical samples for their species when *all four features are considered together*:

### Strongly unusual (p < 0.01)
| Row | Species | SL | SW | PL | PW | Mahal² | p-value | Interpretation |
|---|---|---|---|---|---|---|---|---|
| **118** | Iris-virginica | 7.7 | 2.6 | 6.9 | 2.3 | 13.67 | 0.0084 | A "giant" virginica – longest petal in the dataset, very long sepal, but narrow sepal. The *single most statistically extreme point in the dataset under the within-species Gaussian model.* |

### Mildly unusual (0.01 ≤ p < 0.05)
| Row | Species | SL | SW | PL | PW | Mahal² | p-value | Why odd |
|---|---|---|---|---|---|---|---|---|
| 41 | Iris-setosa | 4.5 | 2.3 | 1.3 | 0.3 | 12.63 | 0.013 | Narrow sepal for a setosa (2.3) combined with short sepal (4.5). |
| **43** | Iris-setosa | 5.0 | 3.5 | 1.6 | 0.6 | 12.00 | 0.017 | Unusually wide petal (0.6) – the most extreme setosa petal. |
| 68 | Iris-versicolor | 6.2 | 2.2 | 4.5 | 1.5 | 12.49 | 0.014 | Narrow sepal (2.2) – same pattern as Row 60. |
| **98** | Iris-versicolor | 5.1 | 2.5 | 3.0 | 1.1 | 10.29 | 0.036 | Overall very small; petal length 3.0 is uniquely short for versicolor. |
| 22 | Iris-setosa | 4.6 | 3.6 | 1.0 | 0.2 | 11.20 | 0.024 | Shortest petal in dataset (1.0), short sepal, wide sepal – an unusual shape combination. |
| 24 | Iris-setosa | 4.8 | 3.4 | 1.9 | 0.2 | 9.60 | 0.048 | Long-petaled setosa with short sepal and narrow petal. |
| **131** | Iris-virginica | 7.9 | 3.8 | 6.4 | 2.0 | 10.83 | 0.029 | Longest sepal in dataset, very wide sepal – unusual shape (big everything except petal width). |

### Other interesting near-boundary cases (flagged on centroid-distance check, p > 0.05)

These samples sit in the overlap zone between versicolor and virginica and warrant a closer look. I measured each sample's Euclidean distance to all three species centroids and flagged those closer to a *different* centroid than their labeled one (ratio > 1.2):

| Row | Labeled species | Closest centroid | Distance to own | Distance to closest | Ratio | Measurements |
|---|---|---|---|---|---|---|
| **77** | versicolor | **virginica** | 1.15 | 0.65 | **1.77** | 6.7, 3.0, 5.0, 1.7 |
| **106** | virginica | **versicolor** | 2.07 | 1.16 | **1.79** | 4.9, 2.5, 4.5, 1.7 |
| 52 | versicolor | virginica | 1.22 | 0.90 | 1.35 | 6.9, 3.1, 4.9, 1.5 |
| 138 | virginica | versicolor | 0.98 | 0.76 | 1.30 | 6.0, 3.0, 4.8, 1.8 |
| 119 | virginica | versicolor | 1.24 | 0.95 | 1.30 | 6.0, 2.2, 5.0, 1.5 |
| 121 | virginica | versicolor | 1.20 | 0.99 | 1.21 | 5.6, 2.8, 4.9, 2.0 |

Scatter plot of petal dimensions with the most unusual observations highlighted. (Note: setosa forms the tight bottom-left cluster; points on the versicolor/virginica boundary are the natural hybrids/borderline cases.)
![image](https://skillagent.bj.bcebos.com/sandbox_exports/20260716/1784181099624364188/80c402d64211cd35e072d3f31b77bb25_scatter_petals.png?responseContentDisposition=attachment%3B%20filename%3D%22scatter_petals.png%22&authorization=bce-auth-v1%2FALTAKVQiEIjpxWERFF0pX5kWYx%2F2026-07-16T05%3A55%3A10Z%2F-1%2Fhost%2Fb42c5301dcbea8f7c4865590b736cf48da2fca141a615e01575394fd8a83cdd0)
---

## 5. Summary

### Counts

| Level | How counted | # outliers / unusual obs. |
|---|---|---|
| Overall univariate (IQR ∪ z>3) | full 150 rows × 4 features | **4 rows** on one feature (SepalWidth): rows 15, 32, 33, 60 |
| Within-species univariate (IQR ∪ z>3) | per species | **13 species-feature flags**, covering **11 distinct rows**: 13, 22, 23, 24, 43, 44 (setosa); 98 (versicolor); 106, 117, 119, 131 (virginica) |
| Multivariate (p < 0.01, Mahalanobis within species) | joint distribution | **1 row** — Row 118 (virginica) |
| Multivariate (p < 0.05) | joint distribution | **7 distinct rows** — 22, 24, 41, 43 (setosa); 68, 98 (versicolor); 118, 131 (virginica) |
| Potential mislabels (centroid ratio > 1.2) | cross-species distance | **6 rows** — 52, 77 (versicolor); 106, 119, 121, 138 (virginica) |

If we union across all methods, **18 distinct rows** (12% of the dataset) are flagged in at least one way. Of those, roughly **5 rows are genuinely "noteworthy"** (rows 15, 43, 98, 106, 118, 131) — the rest are borderline IQR fence-huggers.

### Which features/species are most affected?

- **SepalWidth** is the only feature with global outliers (4 of the 4 overall univariate outliers). It is known from the iris literature to have the most measurement noise.
- **PetalLength / PetalWidth** are extremely clean globally but show within-species extremes *only in setosa* (setosa petals are so tightly clustered that 1.0 cm or 0.6 cm already count as outliers).
- **SepalLength** shows essentially no outlier activity except **Row 106** (virginica, SL = 4.9), which is dramatically short for its species.
- **By species:**
  - *Iris-setosa*: 6 within-species univariate flags (mostly petal extremes) and 4 mild multivariate outliers — these are natural extremes within a very tight cluster, not data errors.
  - *Iris-versicolor*: cleanest species; only **Row 98** stands out.
  - *Iris-virginica*: most variable; contains the dataset's single most extreme point (Row 118) and several borderline/mislabel-suspect cases near the versicolor boundary.

### Are any samples likely mislabeled?

- **Strongest mislabel candidate: Row 106** (labeled *Iris-virginica*, SL=4.9, SW=2.5, PL=4.5, PW=1.7). It is the most extreme outlier in virginica on `SepalLength`, sits much closer to the versicolor centroid (distance ratio 1.79), and its measurements (especially sepal length 4.9 and petal length 4.5) are squarely in versicolor range. This is the classic "possible mislabeled virginica" often discussed in analyses of Fisher's Iris data (it corresponds to sample 107 in 1-based numbering).
- **Second candidate: Row 77** (labeled *Iris-versicolor*, 6.7, 3.0, 5.0, 1.7). Petal length 5.0 and petal width 1.7 are at the very top of versicolor and overlap virginica; distance ratio to virginica centroid is 1.77. Could be a large versicolor or a borderline/virginica-adjacent individual.
- **Rows 52, 119, 121, 138** are milder borderline cases living in the natural overlap between versicolor and virginica; they are more consistent with natural between-species continuity than with mislabeling.
- **Rows flagged as multivariate outliers in setosa (22, 23, 24, 41, 43, 44)** are *not* mislabels — setosa is so well separated from the other species on petals that these are simply shape/size extremes within setosa.
- **Row 118 (the largest virginica)** is a genuine extreme of the species, not a mislabel.

**Bottom line:** The Iris dataset is remarkably clean. The only case that warrants a label review is **Row 106**; the other flagged points are best treated as legitimate natural variation (many are famous "edge cases" of this 90-year-old dataset).

---

*Report generated from `iris_flowers.csv` using IQR fences, z-scores (|z|>3), within-species Mahalanobis distance (χ² test), and cross-species centroid distance ratios.*
