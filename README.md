# weld-seam-anomaly-detection-methodology

**Methodology & Results Documentation**  
*Unsupervised anomaly detection and localization on industrial 2D/3D welding seam surface data*

> ⚠️ **NDA Notice**  
> This project was completed at **Automation W+R GmbH, Munich** as part of my Master's thesis at **Technische Hochschule Rosenheim**.  
> The original source code, proprietary sensor data, internal tools, and company-specific implementation details cannot be shared.  
> This repository documents the **problem, methodology, architecture, evaluation approach, results, limitations, and deployment-oriented thinking** for portfolio and discussion purposes.

---

## Project Overview

Industrial welding seam inspection is a challenging quality-control task because many defects are small, rare, and visually similar to normal surface variations. Existing supervised or rule-based inspection approaches can perform well, but they often require labelled defect data, manual configuration, and adaptation for every new seam geometry or production setup.

The goal of this thesis was to investigate whether an **unsupervised deep learning approach** could detect and localize welding seam surface defects without requiring labelled defect examples during training.

I developed and evaluated a reconstruction-based computer vision pipeline using **autoencoders and classical image processing**. The workflow covered dataset preparation, preprocessing, model training, inference, residual-map generation, defect localization, evaluation, and benchmarking against an industrial **HALCON** baseline.

The best-performing approach achieved an **AUROC of 0.966** on the main evaluation case.

---

## Why This Project Matters

In industrial inspection, detecting whether a part is defective is not enough. The system also needs to show **where** the defect is located so that engineers or operators can understand the issue and take action.

This project focused on both:

- **Anomaly detection** — identifying whether a weld-seam sample is normal or defective
- **Anomaly localization** — highlighting the suspicious region on the surface image

The work explored how unsupervised learning can reduce dependency on manual defect labelling while still supporting practical quality inspection requirements.

---

## My Ownership

I was responsible for the full research and engineering workflow:

- Converted industrial 2D/3D weld-seam height-map data into model-ready image datasets
- Prepared training, validation, and test datasets from multiple welding seam segments
- Designed preprocessing workflows for ROI extraction, grayscale conversion, resizing, normalization, and augmentation
- Implemented and compared multiple autoencoder-based anomaly detection approaches
- Built the inference workflow for reconstruction, residual-map computation, anomaly scoring, and defect localization
- Evaluated model performance using AUROC, precision, recall, F1-score, accuracy, SSIM, MSE, and confusion matrices
- Benchmarked the results against an industrial HALCON-based inspection baseline
- Analyzed false positives, false negatives, localization quality, and failure cases
- Documented technical trade-offs, model behavior, limitations, and future improvement areas

---

## Industrial Problem

The inspection task involved surface defects on welding seams. Examples of relevant defect types included:

- burn-through
- porosity
- seam interruption
- excess silicate
- incorrectly recognized seams due to sensor or scanning issues

The challenge was that these defects could be subtle and visually close to normal weld splatter or surface texture. This made the problem difficult for both classical rule-based image processing and supervised learning approaches.

The key limitation of supervised inspection in this context was the need for labelled defect data. Since defects are rare and labelling is expensive, an unsupervised approach was explored.

---

## Dataset Context

The original dataset is proprietary and cannot be shared.

High-level dataset characteristics:

- Around **8,000 real industrial welding seam samples**
- 2D bitmap height-map images derived from 3D surface data
- Data captured using triangulation/profile sensor technology
- Samples collected from multiple welding seam segments
- Strong class imbalance, as defective samples were rare
- Dataset required preprocessing and careful selection of clean normal samples for training

The input data was not a ready-made public dataset. A significant part of the work involved transforming raw industrial inspection data into a structured dataset suitable for training, validation, testing, and benchmarking.

---

## Methodology

The project followed a reconstruction-based unsupervised anomaly detection strategy.

The core assumption was:

> A model trained only on normal weld-seam surfaces should reconstruct normal regions well, while defective regions should produce higher reconstruction errors.

The pipeline consisted of the following stages:

```mermaid
flowchart TD
    A[2D/3D Weld-Seam Surface Data] --> B[Dataset Preparation]
    B --> C[Preprocessing]
    C --> D[ROI Extraction]
    D --> E[Patch Generation]
    E --> F[Autoencoder Training on Normal Samples]
    F --> G[Image / Patch Reconstruction]
    G --> H[Residual Map + SSIM / MSE Error]
    H --> I[Thresholding + Morphological Processing]
    I --> J[Anomaly Score + Defect Localization]
    J --> K[Evaluation + HALCON Benchmark]
