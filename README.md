# Land Cover Classification Using Sentinel-2 and Random Forest

## Project Overview

This project focuses on land cover and land use classification using satellite imagery and machine learning. The aim is to classify different land cover types, such as water, vegetation, urban areas, and bare soil, using Sentinel-2 multispectral imagery.

## Research Problem

Land cover classification is an important task in Earth observation because it helps monitor environmental change, urban expansion, vegetation distribution, and surface water patterns. In this project, machine learning is applied to satellite imagery to automatically classify different surface types.

## Study Area

The proposed study area is Greater London, UK. London contains a mixture of dense urban areas, parks, rivers, and bare surfaces, making it suitable for land cover classification.

## Data Source

The project will use Sentinel-2 multispectral satellite imagery. The key bands are:

- B2: Blue
- B3: Green
- B4: Red
- B8: Near Infrared

NDVI will also be calculated using:

NDVI = (B8 - B4) / (B8 + B4)

## Methodology

The workflow includes:

1. Accessing Sentinel-2 satellite imagery
2. Selecting useful spectral bands
3. Creating or importing training samples
4. Training a Random Forest classifier
5. Predicting land cover classes
6. Producing a classified land cover map
7. Assessing accuracy using a confusion matrix

## Land Cover Classes

The main classes are:

- Water
- Vegetation
- Urban / built-up area
- Bare soil

## Repository Structure

```text
land-cover-classification-ai4eo/
│
├── README.md
├── requirements.txt
├── land_cover_classification.ipynb
│
├── data/
│   └── sample_data_description.txt
│
└── results/
    └── results_description.txt
