# Iris Flower Classification — Rule-Based Analysis Report

## 1. Dataset Overview

The analysis uses the classic **Iris flowers dataset** (150 samples, 50 per species):

- **Features**: `SepalLength`, `SepalWidth`, `PetalLength`, `PetalWidth` (all in cm)
- **Target**: `Name` — one of *Iris-setosa*, *Iris-versicolor*, *Iris-virginica*

---

## 2. Feature Analysis — What Best Distinguishes the Species?

Summary statistics per species (min / mean ± std / max, in cm):

| Feature       | Iris-setosa          | Iris-versicolor      | Iris-virginica       |
|---------------|----------------------|----------------------|----------------------|
| SepalLength   | 4.3 / 5.01±0.35 / 5.8 | 4.9 / 5.94±0.52 / 7.0 | 4.9 / 6.59±0.64 / 7.9 |
| SepalWidth    | 2.3 / 3.42±0.38 / 4.4 | 2.0 / 2.77±0.31 / 3.4 | 2.2 / 2.97±0.32 / 3.8 |
| **PetalLength** | **1.0 / 1.46±0.17 / 1.9** | **3.0 / 4.26±0.47 / 5.1** | **4.5 / 5.55±0.55 / 6.9** |
| **PetalWidth**  | **0.1 / 0.24±0.11 / 0.6** | **1.0 / 1.33±0.20 / 1.8** | **1.4 / 2.03±0.27 / 2.5** |

Key observations:

- **PetalLength** and **PetalWidth** are by far the most discriminative features:
  - *Iris-setosa* has petals **< 2 cm long** and **< 0.7 cm wide** — completely separated from the other two species.
  - Between *versicolor* and *virginica*, petal dimensions overlap somewhat, but *virginica* tends to have larger petals (PetalLength mostly > 4.5 cm, PetalWidth mostly ≥ 1.5 cm).
- **SepalLength** is somewhat useful (virginica > versicolor > setosa on average) but has substantial overlap between versicolor and virginica.
- **SepalWidth** is the least useful alone: setosa has the widest sepals on average, but versicolor and virginica heavily overlap.

**Conclusion**: A simple, high-accuracy classifier can be built using just **PetalLength** and **PetalWidth**.

---

## 3. Classification Rules (Decision-Tree Style)

```
IF  PetalLength < 2.5 cm          →  Iris-setosa
ELSE IF PetalWidth  < 1.7 cm      →  Iris-versicolor
ELSE                              →  Iris-virginica
```

**Rationale**:

1. **Rule 1 (Setosa)**: All setosa samples have PetalLength ≤ 1.9 cm, while the other two species have PetalLength ≥ 3.0 cm. The threshold 2.5 cm sits in the clear "gap" between clusters.
2. **Rule 2 (Versicolor)**: Among the non-setosa group, versicolor almost always has PetalWidth < 1.7 cm.
3. **Rule 3 (Virginica)**: Anything remaining (large petals, wide petal) is virginica.

This set uses only two features and three threshold comparisons — easy to memorize and apply by hand.

---

## 4. Accuracy Evaluation

Applied to the full 150-sample dataset:

| Rule | Condition                                  | Predicted as          | # Predicted | # Correct | Rule accuracy |
|------|--------------------------------------------|-----------------------|-------------|-----------|---------------|
| R1   | PetalLength < 2.5                          | Iris-setosa           | 50          | 50        | 100.0%        |
| R2   | PetalWidth < 1.7 (after R1)                | Iris-versicolor       | 52          | 48        | 92.3%         |
| R3   | else                                       | Iris-virginica        | 48          | 46        | 95.8%         |

Per-species accuracy:

| Species          | Correct / Total | Accuracy |
|------------------|-----------------|----------|
| Iris-setosa      | 50 / 50         | 100.0%   |
| Iris-versicolor  | 48 / 50         | 96.0%    |
| Iris-virginica   | 46 / 50         | 92.0%    |

**Overall accuracy: 144 / 150 = 96.0%**

> Note: The counts above reflect the cascading decision order — R1 fires first on setosa-sized petals, then R2 and R3 partition the rest. The "predicted" counts for R2/R3 sum to 100 because only the non-setosa samples reach those rules.

---

## 5. Confusion Matrix

Rows = actual species; Columns = predicted species.

|                   | Pred: setosa | Pred: versicolor | Pred: virginica |
|-------------------|:-----------:|:----------------:|:---------------:|
| **Actual: setosa**     | 50 |  0 |  0 |
| **Actual: versicolor** |  0 | 48 |  2 |
| **Actual: virginica**  |  0 |  4 | 46 |

Observations from the matrix:

- **Setosa is perfectly classified** — zero errors.
- All six errors are **confusions between versicolor and virginica**, which are known to be the hardest pair in this dataset because their petal-size ranges overlap.

---

## 6. Misclassification Analysis

There are **6 misclassified samples**, all lying near the versicolor/virginica decision boundary (PetalWidth ≈ 1.4–1.8 cm):

| # (row index) | SepalL | SepalW | PetalL | PetalW | Actual species    | Predicted         | Why difficult |
|------:|:------:|:------:|:------:|:------:|-------------------|-------------------|---------------|
| 70  | 5.9 | 3.2 | 4.8 | 1.8 | Iris-versicolor | Iris-virginica | PetalWidth 1.8 is above the 1.7 threshold, yet it is actually versicolor (atypically wide petals for versicolor). |
| 77  | 6.7 | 3.0 | 5.0 | 1.7 | Iris-versicolor | Iris-virginica | PetalWidth exactly at the threshold with a long petal (5.0 cm) that is more virginica-like. |
| 119 | 6.0 | 2.2 | 5.0 | 1.5 | Iris-virginica  | Iris-versicolor | PetalWidth 1.5 is versicolor-typical, even though PetalLength 5.0 suggests virginica. |
| 129 | 7.2 | 3.0 | 5.8 | 1.6 | Iris-virginica  | Iris-versicolor | Long sepals/petals (very virginica), but PetalWidth only 1.6 cm triggers the versicolor rule. |
| 133 | 6.3 | 2.8 | 5.1 | 1.5 | Iris-virginica  | Iris-versicolor | Narrow petals (1.5 cm) paired with otherwise virginica-sized measurements. |
| 134 | 6.1 | 2.6 | 5.6 | 1.4 | Iris-virginica  | Iris-versicolor | PetalWidth only 1.4 cm — the narrowest-petaled virginica in the dataset; almost any PetalWidth-only rule will miss it. |

**Why they are hard**: These six samples sit in the overlap zone between versicolor and virginica in petal dimensions. They are genuine "borderline" individuals that even a simple threshold model cannot distinguish without adding more features (e.g., SepalLength) or accepting some errors. Using only two simple thresholds keeps the rule set memorable while still achieving 96% accuracy — a reasonable trade-off between simplicity and performance.

---

## 7. Summary

- Two features, **PetalLength** and **PetalWidth**, are sufficient to separate the three Iris species with very high accuracy.
- A three-line rule set — `PetalLength < 2.5 → setosa; else PetalWidth < 1.7 → versicolor; else virginica` — achieves **96.0% overall accuracy**.
- *Iris-setosa* is linearly separable from the other two and is classified perfectly.
- All errors (6/150) occur between *versicolor* and *virginica* on boundary cases with atypical petal widths.
