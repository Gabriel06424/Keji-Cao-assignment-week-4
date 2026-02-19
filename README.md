# Keji-Cao-assignment-week-4
<!-- Back to top link -->
<a name="readme-top"></a>
<br />
<div align="center">
  <a href="https://github.com/Gabriel06424/Keji-Cao-assignment-week-4">
   <img src="images/Logo.png" alt="Logo" width="800" height="500">
  </a>
  
  <h3 align="center">Sea Ice and Lead Echo Classification using Unsupervised Learning</h3>

<p align="justify">
    This project applies unsupervised machine learning algorithms to classify radar echoes from Sentinel-3 altimetry data into sea ice and leads. The primary goal is to compare the performance of K-Means and Gaussian Mixture Models (GMM) for this task and validate the results against official ESA classifications. The project involves calculating average echo shapes with standard deviations for each class and quantifying accuracy using a confusion matrix.
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#key-objectives">Key Objectives</a></li>
        <li><a href="#methods-used">Methods Used</a></li>
      </ul>
    </li>
    <li><a href="#introduction-to-unsupervised-learning">Introduction to Unsupervised Learning</a>
      <ul>
        <li><a href="#k-means-clustering">K-means Clustering</a></li>
        <li><a href="#gaussian-mixture-models">Gaussian Mixture Models (GMM)</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
        <li><a href="#data">Data</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#results">Results</a>
      <ul>
        <li><a href="#average-echo-shapes">Average Echo Shapes</a></li>
        <li><a href="#classification-validation">Classification Validation</a></li>
      </ul>
    </li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

This repository contains the code and analysis for Week 4 assignment of the GEOL0069 AI for Earth Observation course. The core task is to classify Sentinel-3 altimetry waveforms into two categories: sea ice and leads (open water channels in ice).

### Key Objectives
*   **Classify Echoes**: Use K-Means and GMM to classify radar returns based on key features.
*   **Analyze Waveforms**: Compute and visualize the mean and standard deviation of echo shapes for each class.
*   **Validate Results**: Compare the unsupervised classifications against the official ESA labels using a confusion matrix.

### Methods Used
*   **K-Means Clustering**: A centroid-based algorithm partitioning data into k clusters.
*   **Gaussian Mixture Model (GMM)**: A probabilistic model assuming data is generated from a mixture of Gaussian distributions.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- INTRODUCTION TO UNSUPERVISED LEARNING -->
## Introduction to Unsupervised Learning

Unsupervised learning involves finding patterns in data without pre-existing labels. This is particularly useful in Earth Observation for exploratory analysis and structure discovery.

### K-means Clustering
K-means clustering is a type of unsupervised learning algorithm used for partitioning a dataset into a set of k groups (or clusters), where k represents the number of groups pre-specified by the analyst. It classifies the data points based on the similarity of the features of the data {cite: macqueen1967some}. The basic idea is to define k centroids, one for each cluster, and then assign each data point to the nearest centroid, while keeping the centroids as small as possible.

#### Why K-means for Clustering?
K-means clustering is particularly well-suited for applications where:
- The structure of the data is not known beforehand: K-means doesn't require any prior knowledge about the data distribution or structure, making it ideal for exploratory data analysis.
- Simplicity and scalability: The algorithm is straightforward to implement and can scale to large datasets relatively easily.

#### Key Components of K-means
- Choosing K: The number of clusters (k) is a parameter that needs to be specified before applying the algorithm.
- Centroids Initialization: The initial placement of the centroids can affect the final results.
- Assignment Step: Each data point is assigned to its nearest centroid, based on the squared Euclidean distance.
- Update Step: The centroids are recomputed as the center of all the data points assigned to the respective cluster.

#### The Iterative Process of K-means
The assignment and update steps are repeated iteratively until the centroids no longer move significantly, meaning the within-cluster variation is minimised. This iterative process ensures that the algorithm converges to a result, which might be a local optimum.

#### Advantages of K-means
- Efficiency: K-means is computationally efficient.
- Ease of interpretation: The results of k-means clustering are easy to understand and interpret.

### Gaussian Mixture Models
Gaussian Mixture Models (GMM) are a probabilistic model for representing normally distributed subpopulations within an overall population. The model assumes that the data is generated from a mixture of several Gaussian distributions, each with its own mean and variance {cite: reynolds2009gaussian}. GMMs are widely used for clustering and density estimation, as they provide a method for representing complex distributions through the combination of simpler ones.

#### Why Gaussian Mixture Models for Clustering?
Gaussian Mixture Models are particularly powerful in scenarios where:
- Soft clustering is needed: Unlike K-means, GMM provides the probability of each data point belonging to each cluster, offering a soft classification and understanding of the uncertainties in our data.
- Flexibility in cluster covariance: GMM allows for clusters to have different sizes and different shapes, making it more flexible to capture the true variance in the data.

#### Key Components of GMM
- Number of Components (Gaussians): Similar to K in K-means, the number of Gaussians (components) is a parameter that needs to be set.
- Expectation-Maximization (EM) Algorithm: GMMs use the EM algorithm for fitting, iteratively improving the likelihood of the data given the model.
- Covariance Type: The shape, size, and orientation of the clusters are determined by the covariance type of the Gaussians (e.g., spherical, diagonal, tied, or full covariance).

#### The EM Algorithm in GMM
The Expectation-Maximization (EM) algorithm is a two-step process:
- Expectation Step (E-step): Calculate the probability that each data point belongs to each cluster.
- Maximization Step (M-step): Update the parameters of the Gaussians (mean, covariance, and mixing coefficient) to maximize the likelihood of the data given these assignments.
This process is repeated until convergence, meaning the parameters do not significantly change from one iteration to the next.

#### Advantages of GMM
- Soft Clustering: Provides a probabilistic framework for soft clustering, giving more information about the uncertainties in the data assignments.
- Cluster Shape Flexibility: Can adapt to ellipsoidal cluster shapes, thanks to the flexible covariance structure.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

This project is designed to be run in Google Colab, which seamlessly integrates with Google Drive for data storage and GitHub for version control.

### Prerequisites
The analysis requires the following Python libraries:
*   `numpy`
*   `matplotlib`
*   `scikit-learn`
*   `netCDF4`
*   `pandas`
*   `seaborn` (for enhanced confusion matrix visualization)
*   `rasterio` (if processing Sentinel-2 imagery)

### Installation
To set up the environment in Colab, you can install the necessary packages using pip:

   ```sh
   !pip install netCDF4
   !pip install rasterio
   !pip install seaborn
