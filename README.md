# GEOL0069 Week 4 Assignment  
Unsupervised Classification of Sentinel-3 Radar Echoes: Sea Ice vs. Leads

<div align="center">
  <br>
  <img src="https://sentinels.copernicus.eu/documents/247904/1877131/Sentinel-3_SRAL_Product_Overview" alt="Sentinel-3 SRAL Concept" width="480">
  <h4>Discriminating sea ice and leads using Gaussian Mixture Modelling on altimetry waveforms</h4>
</div>

This repository presents the Week 4 deliverable for module **GEOL0069 – Artificial Intelligence for Earth Observation** (UCL).  
The analysis extends the provided notebook `Chapter1_Unsupervised_Learning_Methods_Michel.ipynb` (variant 2) by applying GMM clustering to separate sea ice and lead echoes, computing representative waveform statistics (mean + std), performing waveform alignment, and evaluating performance via confusion matrix against the official ESA surface-type flags.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-USERNAME/YOUR-REPO/blob/main/week4_assignment.ipynb)

<p align="right">(<a href="#top">back to top</a>)</p>

## Table of Contents

- [Background](#background)
- [Approach](#approach)
- [Main Results](#main-results)
- [Setup & Execution](#setup--execution)
- [Files in Repository](#files-in-repository)
- [Acknowledgments & References](#acknowledgments--references)
- [Contact](#contact)

<a id="background"></a>
## Background

Sentinel-3 SRAL (Synthetic Aperture Radar Altimeter) acquires Ku-band waveforms that carry surface information through their shape and intensity.  

- **Leads** (open-water fractures) produce sharp, high-backscatter specular reflections.  
- **Sea ice** generates diffuse, lower-amplitude returns with broader trailing edges.  

These physical differences allow unsupervised clustering on derived features (σ₀, peakiness, stack standard deviation) to separate the two classes without labeled training data. The ESA product provides a reference binary flag (`surf_type_class_20_ku` adjusted to 0/1) for validation.

<a id="approach"></a>
## Approach

**Model**: GaussianMixture (n_components=2)  
**Features**: backscatter coefficient (σ₀), peakiness, SSD (from RIP Gaussian fit)  
**Preprocessing**:
- Extract valid 20 Hz measurements
- Compute peakiness and SSD
- Standardize features
- Remove NaNs
- Restrict to ESA-labeled sea ice (0) and lead (1) points

**Workflow**:
1. Fit GMM → predict hard cluster labels
2. Assign physical classes (higher σ₀ / sharper peak → leads)
3. Compute mean waveform and standard deviation per class (with ±σ shading)
4. Align individual waveforms via cross-correlation to reduce range-bin jitter (especially important for sea ice)
5. Generate confusion matrix vs. ESA flags

GMM was selected over K-means for its probabilistic assignments and better handling of varying cluster shapes/covariances.

<p align="right">(<a href="#top">back to top</a>)</p>

<a id="main-results"></a>
## Main Results

### 1. Mean & Standard Deviation Waveforms per Class

Mean echo shapes clearly separate the specular lead signature (sharp peak, high power) from the diffuse sea-ice response (lower amplitude, broader shape). Shaded regions show ±1 standard deviation.

![Mean and std per class](mean_std_plot.png)

### 2. Waveform Alignment Demonstration

Individual echoes are shifted to align peaks using cross-correlation. The figure compares original (blue) vs. aligned (red) waveforms for selected sea-ice and lead examples, reducing apparent noise in statistics.

![Waveform alignment examples](alignment_examples.png)

### 3. Histogram of Waveforms per Predicted Cluster

Distribution of all waveforms assigned to each GMM cluster (leads = 1, sea ice = 0). Leads show tighter, higher-amplitude concentration; sea ice exhibits wider spread.

![Lead cluster histogram](lead_histogram.png)  
![Sea ice cluster histogram](seaice_histogram.png)

### 4. Confusion Matrix – GMM vs. ESA Official Classification

Near-perfect agreement is achieved (accuracy ≈ 99.7%). Only 46 total misclassifications out of 12,195 valid points.

![Confusion matrix](confusion_matrix.png)

### 5. Feature Space Visualization (GMM clusters)

Scatter plot in reduced feature space showing clear separation between predicted classes.

![GMM feature scatter](gmm_scatter.png)

<p align="right">(<a href="#top">back to top</a>)</p>

<a id="setup--execution"></a>
## Setup & Execution

Run the analysis in **Google Colab** (recommended).

**Required packages**:

```bash
!pip install netCDF4 cartopy
