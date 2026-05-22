# Land Cover Classification Using Sentinel-2 and Random Forest

## Project overview

This project applies an AI for Earth Observation (AI4EO) workflow to classify land cover and land use types in Central London using Sentinel-2 multispectral satellite imagery and a Random Forest machine learning classifier.

The main objective is to distinguish four land cover classes:

- Water
- Vegetation
- Urban / built-up area
- Bare / open land

Land cover classification is an important Earth observation application because it helps monitor the spatial distribution of surface materials and land use patterns. In an urban environment such as London, this type of analysis can support environmental monitoring, urban planning, green space assessment, and studies of surface change.

The workflow demonstrates how satellite imagery, spectral indices, manually labelled training samples, and machine learning can be combined to produce a land cover classification map.

---

## Research problem

Urban areas contain a complex mixture of buildings, roads, vegetation, water bodies, gardens, parks, and exposed surfaces. These land cover types can be difficult to classify because many pixels contain mixed materials, especially at Sentinel-2 spatial resolution.

This project addresses the following research question:

**Can Sentinel-2 multispectral imagery and a Random Forest classifier be used to classify major land cover types in Central London?**

The project focuses on demonstrating a complete and reproducible AI4EO classification workflow rather than producing an operational land cover product.

---

## Study area

The study area is **Central London, United Kingdom**.

Central London was selected because it contains a mixture of different land cover types, including:

- The River Thames
- Dense urban and built-up areas
- Parks and green spaces
- Bare or open surfaces

A relatively small study area was used to reduce computational cost and make the workflow easier to run in Google Colab and Google Earth Engine.

---

## Data source

The project uses **Sentinel-2 Surface Reflectance imagery** accessed through **Google Earth Engine**.

The Sentinel-2 image collection used is:

```text
COPERNICUS/S2_SR_HARMONIZED
