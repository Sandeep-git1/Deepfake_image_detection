# Deepfake_image_detection

# Deepfake Detection using Residual Noise Texture and Machine Learning

This repository presents a forensic analysis pipeline for detecting AI-generated images (deepfakes) using explainable, texture-based features derived from image noise. 
The approach replicates and extends the methodology proposed in:

**"Forensics Analysis of Residual Noise Texture in Digital Images for Detection of Deepfake"**  
Arthur Méreur, Antoine Mallet, Rémi Cogranne, Minoru Kuribayashi  
IEEE ICASSP 2025

---

## Project Overview

The original paper proposed a detection pipeline that does **not rely on deep learning**, but instead uses handcrafted features derived from **residual noise textures**. This implementation replicates that methodology and further explores an alternate classification pipeline using **raw grayscale pixel data** and **machine learning classifiers** like Random Forest and SVM.

---

## What This Implementation Adds

### 1. Full Replication of Paper's Method
- **Noise Residual Extraction** using the **Laplacian filter**
- **Fractal Dimension (FD)** estimation using box counting
- **Texture Complexity (TC)** computation based on log-transformed self-similarity
- Binary classification using **Linear SVM**
- Evaluation using **ROC AUC**, accuracy, precision, recall, and F1-score

### 2. Additional Pipeline: Raw Grayscale-Based Classification
- Extracts flattened grayscale image vectors (64×64 resolution)
- Applies **feature scaling** (StandardScaler)
- Trains and compares:
  - **Random Forest**
  - **SVM (Linear, RBF)**
  - **Logistic Regression**

### 3. Performance Analysis and Feature Comparison
- Visualizes FD and TC histograms
- Plots ROC curve
- Stores all residuals and features for reproducibility
- Offers a modular folder structure (scripts + notebooks + results)

---

## Dataset

Created my own dataset with 1700 photos each (i.g. 3400 total photos, AI + Real). 


Preprocessed images are resized to **512×512 grayscale** and stored under `preprocessed/`. Residuals are stored as `.npy` files under `residuals/`.

---

## Feature Extraction Summary

| Feature              | Technique                             |
|----------------------|----------------------------------------|
| Fractal Dimension    | Box-counting of Laplacian residuals    |
| Texture Complexity   | Log-transformed block-wise similarity  |
| Residuals            | Discrete Laplacian filter (3×3 kernel) |
| Raw Pixels (Alt)     | Flattened 64×64 grayscale input        |

---

## Classifiers and Results

### A. FD + TC (Linear SVM – Paper-Style)
| Metric         | Value   |
|----------------|---------|
| Accuracy       | 0.53    |
| ROC AUC Score  | 0.61    |
| Recall (Fake)  | 0.88    |
| Recall (Real)  | 0.22    |

This pipeline reproduces the original paper's logic using explainable features. The model struggled with real image recall but detected fakes well — as expected for a lightweight, interpretable proof-of-concept.

### B. Raw Grayscale + ML Classifiers (Added by This Work)

| Classifier           | Accuracy | Precision (Real) | Precision (Fake) | Recall (Fake) |
|----------------------|----------|------------------|------------------|---------------|
| Random Forest        | 0.86     | 0.85             | 0.87             | 0.82          |
| RBF SVM              | 0.85     | 0.84             | 0.86             | 0.82          |
| Linear SVM           | 0.64     | 0.70             | 0.59             | 0.75          |
| Logistic Regression  | 0.66     | 0.69             | 0.63             | 0.67          |

These models significantly outperformed the paper-style approach and proved that **texture signals in raw grayscale images** can be exploited via classical ML with strong results.

---


Author
Sandeep Patro
M.Tech – ICT (Software Systems), India

