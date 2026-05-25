# Model Trials

This document summarizes the main model directions tested during the thesis.

The goal was not to test only one architecture, but to compare different reconstruction-based strategies for unsupervised anomaly detection and localization.

---

## Trial 1 — Autoencoder with Skip Connections

### Goal

The first trial used an autoencoder architecture with skip connections to preserve spatial information during reconstruction.

### Why it was tested

For defect localization, reconstruction quality alone is not enough. The model must preserve enough spatial detail so that residual maps can highlight suspicious regions.

Skip connections were tested because they can help retain low-level image details during decoding.

### What it showed

This trial provided a useful baseline for reconstruction-based anomaly detection. It helped establish the first evaluation workflow and showed how reconstruction errors could be used for anomaly scoring.

However, localization quality still required improvement, especially for fine-grained or subtle defects.

---

## Trial 2 — Pre-trained Encoder with Autoencoder Decoder

### Goal

The second trial explored whether a pre-trained encoder could improve feature representation and reconstruction behavior.

### Why it was tested

Pre-trained encoders can sometimes generalize better because they already contain learned visual features from large datasets. This trial tested whether transfer-learned visual features would help with industrial weld-seam surface data.

### What it showed

The experiment was useful for comparing feature extraction strategies, but the approach was not the strongest direction for this specific data type. Industrial height-map surface data differs from natural images, so pre-trained features did not automatically solve the localization challenge.

---

## Trial 3 — Patch-Based Autoencoder

### Goal

The third trial used randomly cropped patches from normal weld-seam images.

### Why it was tested

Patch-based learning was tested to improve localization. Full-image reconstruction can lose fine local defect details, while patch-based inference allows the model to focus on smaller surface regions.

### What it showed

This was the most relevant direction for localization because it preserved local defect information better and allowed residual maps to highlight suspicious sub-regions.

Patch-based inference also made it easier to analyze the relationship between patch size, stride, reconstruction error, and localization quality.

---

## Trial Comparison

| Trial | Main Idea | Why It Was Tested | Main Learning |
|---|---|---|---|
| Trial 1 | Autoencoder with skip connections | Preserve spatial details | Good baseline, but localization needed improvement |
| Trial 2 | Pre-trained encoder + decoder | Test transfer-learned features | Useful comparison, but not ideal for industrial height-map data |
| Trial 3 | Patch-based autoencoder | Improve local reconstruction | Best direction for defect localization |

---

## Final Direction

The final approach focused on patch-based reconstruction and residual-map analysis because the project required both:

- image-level anomaly detection
- localized defect output

For industrial inspection, localization is critical because operators and engineers need to know where the defect is, not only whether a sample is abnormal.
