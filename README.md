# Land Cover Classification Using Sentinel-2 and Random Forest

## Project overview

This project applies an AI for Earth Observation (AI4EO) workflow to classify land cover and land use types in Central London using Sentinel-2 satellite imagery and a Random Forest machine learning classifier.

In simple terms, the project uses satellite images to identify what different parts of the land surface are made of. The aim is to classify the study area into four main land cover classes:

- Water
- Vegetation
- Urban / built-up area
- Bare / open land

Land cover classification is important because it helps us understand how the Earth's surface is used and how it changes over time. In an urban area such as London, this type of analysis can support environmental monitoring, urban planning, green space assessment, and studies of urban surface change.

This project is not intended to produce a perfect operational land cover product. Instead, it demonstrates a complete and reproducible AI4EO workflow, including satellite data access, image preprocessing, feature extraction, manual training sample creation, Random Forest classification, accuracy assessment, result interpretation, and environmental cost assessment.

---

## Research problem

Urban areas are difficult to classify from satellite images because they contain many different surface materials close together. For example, one small area of Central London may contain buildings, roads, trees, gardens, water, shadows, and exposed ground.

Sentinel-2 pixels can also contain mixed materials. This means that a single pixel may include part of a road, part of a tree, and part of a building. This makes urban land cover classification more challenging.

This project addresses the following research question:

**Can Sentinel-2 multispectral imagery and a Random Forest classifier be used to classify major land cover types in Central London?**

The project focuses on demonstrating the full workflow rather than producing a highly accurate final land cover map.

---

## Study area

The study area is **Central London, United Kingdom**.

Central London was selected because it contains a useful mixture of land cover types, including:

- The River Thames
- Dense urban and built-up areas
- Parks and green spaces
- Gardens and street vegetation
- Bare or open surfaces

A relatively small study area was used to reduce computational cost and make the workflow easier to run in Google Colab and Google Earth Engine.

---

## Data source

This project uses **Sentinel-2 Surface Reflectance imagery** accessed through **Google Earth Engine**.

Sentinel-2 is a European Space Agency satellite mission that provides optical images of the Earth's surface. It records reflected sunlight in several different wavelength bands. These bands are useful because different land surfaces reflect light differently.

The Sentinel-2 image collection used in this project is:

```text
COPERNICUS/S2_SR_HARMONIZED
```

The imagery was filtered by:

- Study area
- Date range: June to August 2023
- Cloud cover percentage
- Sentinel-2 Scene Classification Layer for cloud masking

Summer imagery was selected because vegetation is usually more active during this period, making it easier to distinguish vegetation from urban and bare surfaces.

Raw satellite imagery is not stored in this repository. The data are accessed directly through Google Earth Engine when the notebook is run.

---

## What the Sentinel-2 bands mean

Sentinel-2 images are made of several spectral bands. Each band records reflected light in a different part of the electromagnetic spectrum.

This project uses the following Sentinel-2 bands:

| Band | Simple explanation | Why it is useful |
|---|---|---|
| B2 | Blue light | Helps represent the visible colour of the surface |
| B3 | Green light | Useful for identifying vegetation and creating natural colour images |
| B4 | Red light | Important for vegetation analysis because plants absorb red light |
| B8 | Near-infrared | Very useful for detecting vegetation because healthy plants reflect strongly in near-infrared |
| B11 | Shortwave infrared 1 | Useful for separating built-up surfaces, dry soil, and vegetation moisture differences |
| B12 | Shortwave infrared 2 | Useful for distinguishing dry surfaces, bare ground, built-up areas, and some soil or moisture differences |

The visible bands are B2, B3, and B4. These are similar to what human eyes can see and can be combined to make a true-colour image.

B8, B11, and B12 are not visible to the human eye, but they are very useful in remote sensing. For example, vegetation reflects strongly in B8, while built-up areas and bare soil often show stronger differences in B11 and B12.

---

## Spectral indices used

Spectral indices are simple calculations using satellite bands. They help highlight certain surface types more clearly.

This project uses three spectral indices:

| Index | Formula | Simple explanation |
|---|---|---|
| NDVI | (B8 - B4) / (B8 + B4) | Highlights vegetation |
| NDWI | (B3 - B8) / (B3 + B8) | Highlights water bodies |
| NDBI | (B11 - B8) / (B11 + B8) | Helps identify built-up surfaces |

### NDVI

NDVI stands for **Normalized Difference Vegetation Index**. It is commonly used to identify vegetation.

Healthy vegetation usually absorbs red light and reflects near-infrared light. Therefore, high NDVI values usually indicate vegetation such as trees, grass, parks, or gardens.

### NDWI

NDWI stands for **Normalized Difference Water Index**. It is used to highlight water bodies.

Water usually has low reflectance in near-infrared wavelengths, so NDWI can help identify rivers, lakes, and other water surfaces.

### NDBI

NDBI stands for **Normalized Difference Built-up Index**. It is used to help identify built-up surfaces.

Built-up areas, bare surfaces, and dry materials often behave differently in shortwave infrared and near-infrared wavelengths. This makes NDBI useful for separating urban areas from vegetation and water.

The final input features used by the classifier are:

```text
B2, B3, B4, B8, B11, B12, NDVI, NDWI, NDBI
```

---

## Machine learning method: Random Forest

This project uses a **Random Forest classifier**.

Random Forest is a supervised machine learning method. This means that the model needs labelled examples before it can classify the full image.

In this project, manual training polygons are created for four land cover classes:

| Class value | Class name |
|---|---|
| 0 | Water |
| 1 | Vegetation |
| 2 | Urban / built-up area |
| 3 | Bare / open land |

The Random Forest model learns the spectral characteristics of these labelled examples. It then applies this learning to classify every pixel in the Sentinel-2 image.

Random Forest is suitable for this project because:

- It can use many input bands and indices at the same time
- It works well with tabular remote sensing data
- It is easier to run than many deep learning models
- It is less computationally expensive than large neural networks
- It provides feature importance, which helps interpret which bands or indices were most useful

---

## Methodology

The project follows a supervised land cover classification workflow.

### 1. Data access and study area definition

Sentinel-2 Surface Reflectance imagery was accessed using Google Earth Engine. The image collection was filtered by location, date range, and cloud cover percentage. A rectangular study area covering Central London was defined in the notebook.

### 2. Sentinel-2 preprocessing

Clouds and cloud shadows can reduce classification accuracy. Therefore, cloud masking was applied using the Sentinel-2 Scene Classification Layer.

The following unwanted pixels were removed:

- Cloud shadows
- Medium-probability clouds
- High-probability clouds
- Cirrus clouds
- Snow or ice pixels

A median composite was then created from the filtered image collection. This helps produce a cleaner image by reducing the influence of clouds, shadows, and unusual pixel values.

### 3. Feature preparation

Several Sentinel-2 bands were selected, and NDVI, NDWI, and NDBI were calculated. These original bands and spectral indices were combined into one classification image.

A true-colour Sentinel-2 RGB image was exported as:

```text
results/rgb_satellite_image.png
```

This image provides a visual reference of the study area before classification.

### 4. Training sample preparation

Manual training polygons were created for the four land cover classes. These polygons represent known examples of water, vegetation, urban / built-up area, and bare / open land.

The labelled polygons were used to extract spectral band and index values from the Sentinel-2 image. The extracted samples were then split into training and testing datasets.

### 5. Random Forest classification

A Random Forest classifier was trained using the labelled training samples. The trained classifier was then applied to the Sentinel-2 composite image. Each pixel was assigned to one of the four land cover classes based on its spectral and index values.

The final land cover classification map was exported as:

```text
results/land_cover_map.png
```

### 6. Accuracy assessment and model interpretation

The classification result was evaluated using testing samples. The assessment includes:

- Confusion matrix
- Overall accuracy
- Kappa coefficient
- Feature importance

The confusion matrix shows where the model classified pixels correctly and where it confused one class with another.

Overall accuracy gives the proportion of correctly classified testing samples.

Kappa coefficient gives an additional measure of classification agreement, accounting for agreement that could occur by chance.

Feature importance shows which Sentinel-2 bands or spectral indices contributed most to the Random Forest model.

A workflow figure was also created to summarise the full AI4EO classification process.

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
    ├── results_description.txt
    ├── rgb_satellite_image.png
    ├── land_cover_map.png
    ├── confusion_matrix.png
    ├── feature_importance.png
    └── workflow_figure.png
```

---

## Main outputs

The project produces the following outputs:

| Output | File name | Description |
|---|---|---|
| Sentinel-2 RGB visualisation | `rgb_satellite_image.png` | True-colour Sentinel-2 image of the study area before classification |
| Land cover classification map | `land_cover_map.png` | Final classified map of water, vegetation, urban / built-up area, and bare / open land |
| Confusion matrix | `confusion_matrix.png` | Accuracy assessment of the classification result |
| Feature importance plot | `feature_importance.png` | Relative importance of Sentinel-2 bands and spectral indices in the Random Forest classifier |
| Workflow figure | `workflow_figure.png` | Diagram summarising the AI4EO classification workflow |

---

## Results summary

The Random Forest classifier produced a land cover classification map for Central London using Sentinel-2 multispectral satellite imagery.

The classification result identifies water mainly along the River Thames. Vegetation is mostly associated with parks, gardens, green spaces, and areas with higher NDVI values. Urban / built-up land is expected to represent roads, buildings, and dense built surfaces across Central London. Bare / open land represents exposed or sparsely vegetated surfaces, although this class is more difficult to define in a dense urban environment.

The model achieved approximately:

```text
Overall accuracy: 0.56
Kappa coefficient: 0.31
```

These values indicate that the classification result is moderate rather than highly accurate. The result should therefore be interpreted as a demonstration of a complete AI4EO workflow rather than a highly precise operational land cover product.

---

## Limitations

This project has several limitations.

First, the training data were manually selected and relatively small in number. This means the classifier may not fully represent the spectral variability of each land cover class.

Second, the classification was based on a single summer Sentinel-2 composite. This does not capture seasonal variation in vegetation, bare surfaces, or urban reflectance.

Third, Central London contains many mixed urban pixels. A single Sentinel-2 pixel may include a mixture of roofs, roads, trees, grass, and shadows. This can make it difficult to separate vegetation from urban surfaces, especially where street trees, gardens, and buildings occur close together.

Fourth, bare / open land is difficult to classify in a dense urban environment. Exposed soil, construction sites, dry grass, and some built-up surfaces can have similar spectral reflectance values, especially in visible and shortwave infrared bands.

Finally, the accuracy assessment uses testing samples derived from the manually labelled polygons rather than a fully independent land cover reference dataset. Therefore, the accuracy result should be treated as an internal assessment rather than a fully independent validation.

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
- Separating different urban subclasses, such as roads, buildings, and construction sites

Higher-resolution imagery would be particularly useful for Central London because many urban land cover features are smaller than the spatial resolution of Sentinel-2 pixels.

---

## Environmental cost assessment

The environmental cost of this project is relatively low compared with large-scale deep learning studies. This is because the workflow uses:

- A limited study area
- A single seasonal Sentinel-2 composite
- A Random Forest classifier rather than a deep neural network
- Cloud-based processing in Google Earth Engine
- A reduced sampling scale where possible
- Only a small number of exported output figures

However, cloud-based Earth observation analysis still has environmental costs. Data centres consume electricity, and repeated processing, exporting large raster files, and unnecessary model training can increase energy use.

To reduce the environmental impact, this project limits the spatial extent to Central London, uses selected Sentinel-2 bands and spectral indices, avoids unnecessary repeated model training, and saves only the key output figures. Most processing is carried out in Google Earth Engine, which avoids downloading and storing large volumes of raw satellite imagery locally.

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

The notebook will:

1. Access Sentinel-2 imagery from Google Earth Engine
2. Define the study area
3. Apply cloud masking
4. Create a median composite
5. Calculate spectral indices
6. Create manual training samples
7. Train a Random Forest classifier
8. Produce a land cover classification map
9. Assess accuracy
10. Export output figures to the `results/` folder

---

## Required Python packages

The main packages used are:

```text
earthengine-api
geemap
numpy
pandas
matplotlib
jupyter
```

These are listed in `requirements.txt`.

---

## Assignment components covered

| Assignment requirement | Repository component |
|---|---|
| Description of the problem | README and notebook project overview |
| Figure illustrating remote sensing / AI workflow | `results/workflow_figure.png` |
| Well documented GitHub repository | README, notebook, requirements file, data folder, and results folder |
| Well documented Python code | `land_cover_classification.ipynb` |
| Video recording explaining the code | To be submitted separately |
| Environmental cost assessment | README and notebook final section |

---

## References

- Breiman, L. (2001). Random Forests. *Machine Learning*, 45, 5–32. https://doi.org/10.1023/A:1010933404324

- European Space Agency. (2015). *Sentinel-2 User Handbook*. ESA Standard Document, Issue 1, Revision 2.

- Google Earth Engine Developers. (n.d.). *Harmonized Sentinel-2 MSI: MultiSpectral Instrument, Level-2A Surface Reflectance*. Earth Engine Data Catalog.

- McFeeters, S. K. (1996). The use of the Normalized Difference Water Index in the delineation of open water features. *International Journal of Remote Sensing*, 17(7), 1425–1432. https://doi.org/10.1080/01431169608948714

- Rouse, J. W., Haas, R. H., Schell, J. A., and Deering, D. W. (1974). Monitoring vegetation systems in the Great Plains with ERTS. In *Third Earth Resources Technology Satellite-1 Symposium*, NASA SP-351, 309–317.

- Zha, Y., Gao, J., and Ni, S. (2003). Use of normalized difference built-up index in automatically mapping urban areas from TM imagery. *International Journal of Remote Sensing*, 24(3), 583–594. https://doi.org/10.1080/01431160304987

---

## Licence

This repository is created for educational coursework purposes. The code and documentation may be reused for learning and non-commercial academic work with appropriate acknowledgement.

---

## Author

Rui An

---

## Acknowledgements

This project was developed for an AI for Earth Observation coding assignment. Sentinel-2 imagery was accessed using Google Earth Engine.
