 # GEOL0069 Week 4 – Unsupervised Echo Classification: Sea Ice vs Leads

<div align="center">
  <br>
  <img src="https://github.com/YOUR-USERNAME/YOUR-REPO/raw/main/sentinel3_logo.png" alt="Sentinel-3" width="320">
  <h3>Unsupervised discrimination of sea ice and leads from Sentinel-3 SRAL waveforms</h3>
</div>

<br>

This repository contains the Week 4 assignment for **GEOL0069 Artificial Intelligence for Earth Observation** (UCL Earth Sciences).  
The work extends the provided notebook `Chapter1_Unsupervised_Learning_Methods_Michel.ipynb` (or its variant) by applying Gaussian Mixture Modelling (GMM) to classify radar echoes into **sea ice** and **leads**, computing class-averaged echo shapes with variability, and evaluating agreement against the official ESA surface-type classification.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-USERNAME/YOUR-REPO/blob/main/week4_assignment.ipynb)

<p align="right">(<a href="#top">back to top</a>)</p>

## Table of Contents

- [Context](#context)
- [Method Overview](#method-overview)
- [Key Results](#key-results)
- [Getting Started](#getting-started)
- [Repository Files](#repository-files)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

<a name="context"></a>
## Context

Sentinel-3’s SRAL instrument measures surface height via radar echoes. The shape and intensity of these echoes differ markedly between **sea ice** (diffuse scattering → broader, less peaky waveforms) and **leads** (specular reflection from open water → sharp, high-amplitude peaks with rapid rise and decay).  

Unsupervised clustering on derived features (backscatter σ⁰, peakiness, stack standard deviation) allows separation of these surface types without relying on labeled training data.

ESA provides a reference classification (`surf_type_class_20_ku` or equivalent flag), typically coded as sea ice = 1, lead/open water = 2 (after offset adjustment for binary comparison).

<a name="method-overview"></a>
## Method Overview

**Core approach**: Gaussian Mixture Model (GMM) with 2 components

- **Features**: σ⁰ (backscatter), peakiness, SSD (from RIP Gaussian fit)
- **Preprocessing**:
  - Extract valid waveforms and auxiliary variables from NetCDF
  - Compute peakiness and SSD
  - Standardize features (`StandardScaler`)
  - Remove NaN-containing rows
  - Restrict analysis to ESA-labeled sea ice and lead points
- **Model**: `GaussianMixture(n_components=2, random_state=42)`
- **Post-processing**:
  - Predict cluster labels
  - Map clusters to physical classes (based on mean waveform shape and feature statistics: higher peakiness + higher σ⁰ → leads)
  - Compute per-class **mean waveform** and **standard deviation**
  - Align selected waveforms via cross-correlation to reduce shift-induced noise
  - Generate confusion matrix against ESA flags

GMM was preferred over K-means due to its probabilistic (soft) assignment and ability to model elliptical clusters.

```python
# Example core snippet (for illustration only)
from sklearn.mixture import GaussianMixture
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(features_clean)

gmm = GaussianMixture(n_components=2, random_state=42)
gmm.fit(X_scaled)
labels = gmm.predict(X_scaled)
