# Iris Flowers Dataset — Statistical Summary

Generated from `iris_flowers.csv` (the classic Fisher/Anderson Iris dataset).

## 1. Dataset Overview

- **Total rows (samples)**: 150
- **Total columns**: 5
- **Feature columns**: SepalLength, SepalWidth, PetalLength, PetalWidth (all in centimeters)
- **Categorical column**: Name (species)

**Species breakdown:**

| Species | Count |
|---------|-------|
| Iris-setosa | 50 |
| Iris-versicolor | 50 |
| Iris-virginica | 50 |

## 2. Overall Descriptive Statistics (all species combined)

Units: centimeters.

| Feature | Mean | Median | Std Dev | Min | Max |
|---------|------|--------|---------|-----|-----|
| SepalLength | 5.843 | 5.800 | 0.828 | 4.300 | 7.900 |
| SepalWidth | 3.054 | 3.000 | 0.434 | 2.000 | 4.400 |
| PetalLength | 3.759 | 4.350 | 1.764 | 1.000 | 6.900 |
| PetalWidth | 1.199 | 1.300 | 0.763 | 0.100 | 2.500 |

## 3. Per-Species Statistics

Values are reported as **mean ± standard deviation** (cm).

| Feature | Iris-setosa | Iris-versicolor | Iris-virginica |
|---------|--------|--------|--------|
| SepalLength | 5.006 ± 0.352 | 5.936 ± 0.516 | 6.588 ± 0.636 |
| SepalWidth | 3.418 ± 0.381 | 2.770 ± 0.314 | 2.974 ± 0.322 |
| PetalLength | 1.464 ± 0.174 | 4.260 ± 0.470 | 5.552 ± 0.552 |
| PetalWidth | 0.244 ± 0.107 | 1.326 ± 0.198 | 2.026 ± 0.275 |

**Mean by species:**

| Feature | Iris-setosa | Iris-versicolor | Iris-virginica |
|---------|--------|--------|--------|
| SepalLength | 5.006 | 5.936 | 6.588 |
| SepalWidth | 3.418 | 2.770 | 2.974 |
| PetalLength | 1.464 | 4.260 | 5.552 |
| PetalWidth | 0.244 | 1.326 | 2.026 |

**Standard deviation by species:**

| Feature | Iris-setosa | Iris-versicolor | Iris-virginica |
|---------|--------|--------|--------|
| SepalLength | 0.352 | 0.516 | 0.636 |
| SepalWidth | 0.381 | 0.314 | 0.322 |
| PetalLength | 0.174 | 0.470 | 0.552 |
| PetalWidth | 0.107 | 0.198 | 0.275 |

## 4. Correlation Insight

Pearson correlation coefficients among the four numeric features:

| | SepalLength | SepalWidth | PetalLength | PetalWidth |
|---|---|---|---|---|
| SepalLength | 1.000 | -0.109 | 0.872 | 0.818 |
| SepalWidth | -0.109 | 1.000 | -0.421 | -0.357 |
| PetalLength | 0.872 | -0.421 | 1.000 | 0.963 |
| PetalWidth | 0.818 | -0.357 | 0.963 | 1.000 |

**Strongest linear relationship**: `PetalLength` and `PetalWidth`, with Pearson r = 0.963 (r² = 0.927).

This is a very strong positive correlation, meaning that as petal length increases, petal width tends to increase proportionally across the dataset. 
Intuitively this makes sense: longer petals are consistently wider petals. Petal dimensions scale together so tightly that either feature alone is nearly sufficient to describe petal size, and together they are highly discriminative between species.

## 5. Key Findings

1. **Perfect class balance**: Each of the three species (Iris-setosa, Iris-versicolor, Iris-virginica) appears exactly 50 times — no class imbalance to worry about in modeling.

2. **Petal dimensions are the most distinguishing features**: Iris-setosa has dramatically smaller petals (mean PetalLength ≈ 1.464 cm, mean PetalWidth ≈ 0.244 cm) compared to versicolor (≈ 4.260 × 1.326 cm) and virginica (≈ 5.552 × 2.026 cm). Setosa petals are essentially non-overlapping with the other two species.

3. **Sepal dimensions overlap more**: While virginica tends to have the longest sepals and setosa the widest sepals relative to length, sepal features show substantial overlap between versicolor and virginica, making them weaker standalone classifiers.

4. **Strongest pairwise correlation is PetalLength ↔ PetalWidth (r ≈ 0.963)**, confirming that petal length and width scale almost isometrically. PetalLength also correlates very strongly with SepalLength, reflecting overall plant size.

5. **Low within-species variability for setosa**: Standard deviations on petal measurements for Iris-setosa are noticeably smaller than for the other two species (e.g., PetalLength std ≈ 0.174 cm vs. 0.470 / 0.552 cm), indicating that setosa is the most morphologically uniform of the three.

6. **Practical takeaway for modeling**: A simple classifier (e.g., decision rule on PetalLength/PetalWidth, or k-NN/LDA) should easily separate setosa from the rest; distinguishing versicolor from virginica is the harder subtask and will rely on smaller differences (especially petal width and sepal shape).
