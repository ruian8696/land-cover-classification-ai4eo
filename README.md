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
```

The imagery was filtered by:

- Study area
- Date range: June to August 2023
- Cloud cover percentage
- Sentinel-2 Scene Classification Layer for cloud masking

Summer imagery was selected because vegetation is more active during this period, making it easier to distinguish vegetation from urban and bare surfaces.

---

## Input features

The classification uses Sentinel-2 spectral bands and spectral indices.

### Sentinel-2 bands

| Band | Description |
|---|---|
| B2 | Blue |
| B3 | Green |
| B4 | Red |
| B8 | Near Infrared |
| B11 | Shortwave Infrared 1 |
| B12 | Shortwave Infrared 2 |

### Spectral indices

| Index | Purpose |
|---|---|
| NDVI | Highlights vegetation |
| NDWI | Highlights water bodies |
| NDBI | Helps identify built-up surfaces |

The final input features used by the classifier are:

```text
B2, B3, B4, B8, B11, B12, NDVI, NDWI, NDBI
```

---

## Methodology

The project follows a supervised machine learning workflow.

### 1. Data access

Sentinel-2 Surface Reflectance imagery was accessed using Google Earth Engine. The image collection was filtered by location, date, and cloud cover.

### 2. Preprocessing

Cloud masking was applied using the Sentinel-2 Scene Classification Layer. Cloud shadows, medium- and high-probability clouds, cirrus, and snow or ice pixels were removed. A median composite was then created to produce a cleaner image for classification.

### 3. Feature preparation

Useful Sentinel-2 bands were selected, and NDVI, NDWI, and NDBI were calculated. These bands and indices were combined into one classification image.

### 4. Training sample preparation

Manual training polygons were created for four land cover classes:

| Class value | Class name |
|---|---|
| 0 | Water |
| 1 | Vegetation |
| 2 | Urban / built-up area |
| 3 | Bare / open land |

These polygons were used to extract spectral and index values from the Sentinel-2 image.

### 5. Model training

The extracted samples were split into training and testing datasets. A Random Forest classifier was trained using the labelled training samples.

### 6. Classification

The trained Random Forest classifier was applied to the Sentinel-2 composite image to classify each pixel into one of the four land cover classes.

### 7. Accuracy assessment

The classification result was evaluated using testing samples. The assessment includes:

- Confusion matrix
- Overall accuracy
- Kappa coefficient
- Feature importance

---

## Repository structure

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
    ├── confusion_matrix.png
    ├── feature_importance.png
    └── workflow_figure.png
```

---

## Main outputs

The project produces the following outputs:

| Output | Description |
|---|---|
| Sentinel-2 RGB visualisation | True-colour visualisation of the study area |
| Land cover classification map | Classified map of water, vegetation, urban, and bare / open land |
| Confusion matrix | Accuracy assessment of the classification |
| Feature importance plot | Shows which bands or indices contributed most to the Random Forest model |
| Workflow figure | Summarises the full AI4EO classification workflow |

---

## Results summary

The Random Forest classifier successfully produced a land cover classification map for Central London.

The model achieved:

```text
Overall accuracy: approximately 0.56
Kappa coefficient: approximately 0.31
```

These values indicate that the classification result is moderate rather than highly accurate. The output should therefore be interpreted as a demonstration of a complete AI4EO workflow rather than a precise operational land cover product.

The classification identifies water mainly along the River Thames and vegetation in parks, gardens, and green spaces. Some areas appear to be overclassified as vegetation, which may be caused by mixed urban pixels, street trees, gardens, and the limited number of training polygons.

Bare / open land was more difficult to classify because exposed soil, construction sites, dry grass, and some built-up surfaces can have similar spectral reflectance values.

---

## Limitations

This project has several limitations:

- The training samples were manually selected and relatively limited in number.
- The classification was based on a single summer Sentinel-2 composite.
- Central London contains many mixed pixels, where buildings, roads, trees, gardens, and open surfaces occur close together.
- Bare / open land is difficult to define and separate in a dense urban environment.
- The accuracy assessment used testing samples derived from the manually labelled polygons rather than a fully independent land cover reference dataset.

---

## Future improvements

Future work could improve the classification by:

- Increasing the number and spatial distribution of training samples
- Using an independent validation dataset
- Testing imagery from different seasons
- Adding additional spectral indices
- Comparing Random Forest with other machine learning models
- Using higher-resolution imagery for complex urban areas
- Applying object-based image analysis instead of pixel-based classification

---

## Environmental cost assessment

The environmental cost of this project is relatively low compared with large-scale deep learning studies. This is because the workflow uses:

- A limited study area
- A single seasonal Sentinel-2 composite
- A Random Forest classifier rather than a deep neural network
- Cloud-based processing in Google Earth Engine
- A reduced sampling scale where possible

However, cloud-based Earth observation analysis still has environmental costs. Data centres consume electricity, and repeated processing or exporting large raster files can increase energy use.

To reduce the environmental impact, this project limits the spatial extent to Central London, uses only selected Sentinel-2 bands and indices, avoids unnecessary repeated model training, and saves only the key output figures.

---

## How to run the notebook

The main notebook is:

```text
land_cover_classification.ipynb
```

It can be run in Google Colab.

Before running the notebook, install the required packages:

```python
!pip install geemap earthengine-api -q
```

The notebook requires Google Earth Engine authentication. The Earth Engine project used in the notebook is:

```text
ee-ruian8696
```

If another user runs the notebook, they may need to replace the project ID with their own registered Google Earth Engine project.

---

## Required Python packages

The main packages used are:

```text
earthengine-api
geemap
numpy
pandas
matplotlib
```

These are listed in `requirements.txt`.

---

## Assignment components covered

| Assignment requirement | Repository component |
|---|---|
| Description of the problem | README and notebook project overview |
| Figure illustrating remote sensing / AI workflow | `results/workflow_figure.png` |
| Well documented GitHub repository | README, notebook, requirements, results folder |
| Python code | `land_cover_classification.ipynb` |
| Video explanation | To be submitted separately |
| Environmental cost assessment | README and notebook final section |

---

## Author

Rui An

---

## Acknowledgements

This project was developed for an AI for Earth Observation coding assignment. Sentinel-2 imagery was accessed using Google Earth Engine.
