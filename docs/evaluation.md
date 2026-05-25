# Evaluation & Failure Analysis

This document summarizes the evaluation strategy, key results, and failure analysis from the thesis.

---

## Evaluation Questions

The evaluation was designed around three practical questions:

1. Can the model separate normal and defective weld-seam samples?
2. Can the model localize suspicious regions on the surface image?
3. How does the developed method compare with an existing industrial HALCON-based inspection baseline?

---

## Metrics Used

| Metric | Purpose |
|---|---|
| AUROC | Measures separation between normal and anomalous samples |
| Precision | Measures reliability of predicted anomalies |
| Recall | Measures ability to detect actual anomalies |
| F1-score | Balances precision and recall |
| Accuracy | Measures overall classification correctness |
| Confusion Matrix | Helps analyze false positives and false negatives |
| SSIM | Measures structural similarity between input and reconstruction |
| MSE | Measures pixel-level reconstruction error |

The evaluation was not limited to a single score. Reconstruction quality, residual maps, localization output, false positives, false negatives, and threshold behavior were also analyzed.

---

## Results Summary

The best-performing setup achieved:

| Result | Value |
|---|---|
| Main evaluation AUROC | **0.966** |
| Benchmark reference | Industrial HALCON-based baseline |
| Task | Unsupervised anomaly detection and localization |
| Data type | 2D/3D weld-seam surface data |

A second seam type with similar characteristics also showed strong performance:

| Metric | Value |
|---|---|
| AUROC | 0.94 |
| Precision | 0.875 |
| Recall | 0.933 |
| F1-score | 0.903 |
| Accuracy | 0.90 |

---

## Interpretation

The results showed that reconstruction-based unsupervised learning can be a promising direction for industrial weld-seam inspection, especially when labelled defect data is limited.

The strongest result, AUROC 0.966, showed good separation between normal and defective samples on the main evaluation case. The comparison against an industrial HALCON baseline helped validate the feasibility of the approach.

---

## Failure Analysis

A major part of the project was understanding where the model struggled.

Observed challenges included:

- false positives caused by surface texture variation
- reduced localization quality near image boundaries
- sensitivity to patch size and stride during sliding-window inference
- difficulty separating weld splatter from actual defects
- weaker generalization when clean normal samples were limited
- threshold sensitivity across different seam geometries

---

## Why Failure Analysis Matters

In industrial inspection, a high metric alone is not enough. A model must be robust, explainable, and consistent across changing production conditions.

Failure analysis helped identify:

- where the model was reliable
- where additional preprocessing was needed
- when threshold calibration became unstable
- which seam types required more clean reference data
- what would be required before production deployment

---

## Engineering Takeaway

The project showed that model performance should be evaluated not only through AUROC or accuracy, but also through:

- localization quality
- false-positive behavior
- generalization across seam types
- sensitivity to preprocessing and patch size
- practical usability of anomaly heatmaps
