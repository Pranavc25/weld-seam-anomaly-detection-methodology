# Deployment-Oriented Plan

Although the thesis was research-focused, I planned the pipeline with production-readiness in mind.

This document describes how the research prototype could be turned into a maintainable computer vision inference service.

---

## Target Architecture

```mermaid
flowchart LR
    A[Inspection System / Image Source] --> B[Preprocessing Service]
    B --> C[Model Inference Service]
    C --> D[Post-processing Module]
    D --> E[OK / NOK Decision + Heatmap]
    E --> F[Result Storage]
    E --> G[Review Dashboard]
    F --> H[Monitoring + Feedback Loop]
