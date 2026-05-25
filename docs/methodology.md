# Methodology

This document explains the technical workflow behind the industrial weld-seam anomaly detection project.

The goal was to investigate whether an unsupervised deep learning approach could detect and localize surface defects on industrial 2D/3D weld-seam height-map data without requiring labelled defect samples during training.

---

## 1. Problem Setup

The inspection problem involved detecting and localizing surface defects on welding seams. The relevant defect types included burn-through, porosity, seam interruption, excess silicate, and incorrectly recognized seams caused by sensor or scanning issues.

The existing supervised or rule-based inspection approaches could perform well, but they required labelled defect data and configuration effort for each new seam geometry or welding process. Since industrial defects are rare and labelling is time-consuming, I explored an unsupervised reconstruction-based approach.

The main idea was:

> Train the model on normal weld-seam surfaces and detect defects by identifying regions with high reconstruction error during inference.

---

## 2. Data Context

The original data is proprietary and cannot be shared.

High-level characteristics:

- Industrial 2D bitmap height-map images derived from 3D surface data
- Data captured using triangulation / profile sensor technology
- Around 8,000 weld-seam samples
- Multiple welding seam segments
- Strong class imbalance, with defective samples being rare
- Clean normal samples required careful selection before training

This was not a ready-to-use public dataset. A major part of the project was converting raw industrial inspection data into structured training, validation, and test datasets.

---

## 3. Dataset Preparation

The dataset preparation workflow included:

- reviewing available weld-seam samples
- selecting clean normal samples for training
- separating validation and test samples
- preparing defective samples for evaluation
- organizing data across different seam types
- ensuring consistent input format for model training and inference

The goal was to create a reliable dataset structure that allowed consistent model comparison and reproducible evaluation.

---

## 4. Preprocessing

Preprocessing was necessary because real industrial surface data contains noise, irrelevant background regions, boundary artifacts, and variation between seam geometries.

Key preprocessing steps:

- Region of Interest extraction
- grayscale conversion
- resizing
- normalization
- data augmentation
- contour-based processing
- patch extraction

Tools and methods:

`Python` `OpenCV` `NumPy` `thresholding` `contour analysis` `morphological operations`

---

## 5. Model Training

The project used a reconstruction-based unsupervised anomaly detection strategy.

The model was trained only on normal samples. During inference, defective regions were expected to reconstruct poorly, producing higher residual errors.

Training focused on:

- learning normal weld-seam surface patterns
- preventing overfitting to training samples
- comparing reconstruction quality across model variants
- testing both full-image and patch-based approaches

---

## 6. Inference Pipeline

The inference workflow converted model output into inspection-relevant anomaly information.

Steps:

1. Load trained model
2. Preprocess test image
3. Reconstruct image or image patches
4. Compute residual map between input and reconstruction
5. Calculate anomaly score
6. Apply thresholding
7. Refine suspicious regions using morphological operations
8. Generate localized defect output
9. Evaluate detection and localization quality

---

## 7. Methodology Overview

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
