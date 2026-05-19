# GEOL0069 Final Project

Welcome to the GEOL0069 Final Project repository!


## Contents

- [Introduction](#Introduction)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Contributing](#contributing)


## Introduction
This is the final project for the GEOL0069 AI for Earth Observation module 
at UCL. It applies unsupervised and supervised machine learning to Sentinel-2 
satellite imagery to monitor the ongoing collapse of the Aral Sea — once the 
world's fourth largest lake.

Google Earth Engine is used to access imagery across four time periods (2015, 
2018, 2021, 2024), classifying the lake basin into four land cover types — 
open water, brine/shallow water, salt flat, and desert scrub — to track how 
each has changed over time.

Two methods are compared:
- **GMM (Gaussian Mixture Model)** — unsupervised classification, no labels required
- **CNN (Convolutional Neural Network)** — supervised, trained on water occurrence 
  labels from the JRC Global Surface Water dataset

Together these allow a direct assessment of how an unsupervised and a supervised 
approach differ in detecting water body change across a spectrally complex, 
arid environment.

## Background and Motivation

The Aral Sea, located on the border of Kazakhstan and Uzbekistan, was once 
one of the largest inland bodies of water on Earth. Soviet-era irrigation 
projects from the 1960s diverted its two feeding rivers towards cotton 
agriculture, resulting in a 90% decrease in volume and 74% reduction in 
surface area — one of the most dramatic environmental catastrophes of the 
twentieth century (Micklin, 2007).

The exposed lakebed — now the Aralkum Desert — has become one of Central 
Asia's most active dust and salt storm sources, with storms growing more 
powerful as more of the lake bottom is exposed (Indoitu et al., 2015). 
Tracking this ongoing change is directly relevant to the health of millions 
of people across the region (Encyclopaedia Britannica, 2024).

This project classifies the basin into four land cover types — open water, 
brine/shallow water, salt flat, and desert scrub — chosen because they 
represent the sequential stages of ecological collapse, from open water 
through progressive evaporation and salinisation to bare desert, allowing 
the drying process to be tracked and quantified over time (Löw et al., 2013).

Satellite remote sensing is well suited to this monitoring task — field 
surveys in the basin are logistically difficult and prohibitively expensive 
at scale. Sentinel-2, with its 10–20m resolution, 5-day revisit cycle, and 
freely accessible Copernicus archive, provides a consistent long-term record 
for systematic change detection (ESA, 2022).

## 3. Research Questions, Data & Pre-processing

This project addresses two questions:

1. How have the four land cover classes of the Aral Sea basin — open water, 
   brine, salt flat, and desert scrub — changed in spatial extent between 
   2015 and 2024?
2. How do unsupervised (GMM) and supervised (CNN) classification methods 
   compare in their ability to detect and quantify these changes?

All imagery is sourced from the Sentinel-2 Level-2A surface reflectance 
product, accessed via Google Earth Engine (GEE) through the 
`COPERNICUS/S2_SR_HARMONIZED` collection (ESA, 2015). Training labels 
for the CNN are sourced independently from the JRC Global Surface Water 
dataset, also accessed via GEE.

Three water-sensitive spectral indices are computed from the imagery:

- **NDWI** (Green − NIR) / (Green + NIR) — detects open water using 
  bands B3 and B8 (McFeeters, 1996)
- **MNDWI** (Green − SWIR) / (Green + SWIR) — separates water from 
  salt flat and bare soil using bands B3 and B11 (Xu, 2006)
- **NDVI** (NIR − Red) / (NIR + Red) — distinguishes desert scrub 
  from bare salt crust using bands B8 and B4 (ESA, 2015)

Water-focused indices are used rather than vegetation indices, as the 
primary spectral challenge in the Aral Sea basin is separating open 
water and shallow brine from dry salt flat — not distinguishing 
vegetation types.

## Methodology

*INSERT FLOW CHART*

This project implements two classification approaches and compares their 
ability to detect water body change across the Aral Sea basin.

**Gaussian Mixture Model (GMM)** — an unsupervised method requiring no 
training labels. GMM is chosen over K-means as it assigns soft probabilistic 
class memberships rather than hard boundaries, better reflecting the gradual 
transitions between land cover types in an arid environment. The model is 
fitted on 2015 imagery and applied consistently across all four years, 
ensuring pixel count changes reflect genuine land cover change rather than 
model variation.

**Convolutional Neural Network (CNN)** — trained on water occurrence labels 
from the JRC Global Surface Water dataset. Unlike the GMM, the CNN processes 
3×3 spatial patches, incorporating neighbourhood context around each pixel. 
Using an independent label source keeps the CNN entirely separate from the 
GMM, making the comparison between the two methods meaningful

## 5. Notebook Overview

**01_preprocessing.ipynb**
Fetches Sentinel-2 imagery for 2015, 2018, 2021 and 2024 via GEE, applies 
cloud masking, computes NDWI/MNDWI/NDVI, and exports GeoTIFFs to Google Drive.

**02_unsupervised_gmm.ipynb**
Loads the index stacks, applies GMM classification, and produces land cover 
maps and a water area time series for each year.

**03_supervised_cnn.ipynb**
Fetches JRC water labels, trains a CNN on 3×3 spatial patches, rolls it out 
across the full image, and compares results against the GMM.

All notebooks are adapted from GEOL0069 module skeletons — attribution is 
noted at the top of each notebook.

## Contact

[Add contact information here]
