# Flood Impact Assessment on Infrastructure – Lookoja 

This repository contains a complete **Google Earth Engine (GEE)** workflow to map flood extent and assess its impact on population, land cover, buildings, schools, and  health facilities. The analysis focuses on a flood event in **Lokoja** (2022).

##  Overview

The project:
- Defines an Area of Interest (AOI) around the flood‑affected area, the area within the confluence of the River Niger and the River Benue.
- Downloads and processes **Sentinel‑1**, **Sentinel‑2**, and **Landsat‑8** imagery (pre‑ and post‑flood).
- Extracts flood masks using **thresholding** and **Random Forest** classification.
- Combines the three sensor‑based flood masks into a final high‑confidence flood extent.
- Downloads **GRID3** infrastructure data (schools, health facilities), road, and **Google Open Buildings**.
- Quantifies flood impact on:
  - Land cover (ESA WorldCover 2021)
  - Population (WorldPop 2020/2022)
  - Buildings, schools, health facilities
- Exports final rasters and vectors to Google Drive.

- ##  Area of Interest

- The AOI is a rectangle covering part of Lokoja, Kogi State:
[6.685181, 7.676968] to [6.853409, 7.890588]

##  Required Libraries

Install the dependencies (preferably in a fresh environment):

```bash
pip install earthengine-api geemap geopandas pandas shapely requests
Note: You also need a Google Earth Engine account (sign up at earthengine.google.com).

Workflow

1. Initialisation
Authenticate and initialise Earth Engine.

Set flood parameters (pre‑flood: 2022‑08‑01 to 2022‑09‑10; flood: 2022‑09‑14 to 2022‑11‑18).

2. Satellite data download & preprocessing
Sentinel‑2 (optical): cloud masking, resampling to 10 m, spectral indices (NDVI, NDWI, MNDWI, EVI).

Sentinel‑1 (SAR): speckle filtering (focal median), VV/VH polarisation.

Landsat‑8 (optical): cloud masking, scaling, indices.

3. Flood mapping
Thresholding (weak labels):

S2: NDWI > 0.1

S1: VV < -15 (dB)

L8: NDWI > 0.3

Machine Learning (Random Forest):

Stratified random sampling (250 points per class, 500 total).

Train on threshold‑derived labels.

Classify full images → flood masks for each sensor.

Ensemble: Reproject all ML masks to 10 m and take the maximum (union) → final_flood_mask

4. Thematic layers
WorldPop (2020, 2022): population density.

ESA WorldCover 2021: land cover classes.

JRC Global Surface Water: permanent water (seasonality ≥ 10 months).

SRTM and Copernicus DEM (GLO‑30).

Google Open Buildings (v3).

GRID3 infrastructure (public ArcGIS REST services):

Schools

Health facilities (v2.0)

5. Impact assessment
Land cover – flooded area per class (km²).

Population – number of people in flooded pixels (WorldPop 2022).

Buildings – count of intersecting building polygons.

Schools / health facilities – count of points intersecting flood extent.

Roads – total length (km) of flooded road segments.

6. Export
All composite images, flood masks, and vector layers (AOI, infrastructure) can be exported to Google Drive as GeoTIFF, Shapefile, or GeoJSON.

Key Results (for this AOI)
Metric	Value
Total flooded area	~140.9 km²
Affected population	~34,551 people
Buildings affected	9,844 out of 85,253
Schools affected	6 out of 94
Health facilities affected	6 out of 84
Roads flooded	324 km out of 1,343 km
Land cover breakdown (flooded area per class):

Permanent water bodies: 44.0 km²

Trees: 28.9 km²

Grassland: 22.1 km²

Cropland: 20.1 km²

Shrubland: 12.6 km²

Built‑up: 1.5 km²

wetland: 3.9 km²

Bare/sparse: 1.0 km²

How to Run the Notebook

Clone this repository.

Install the required libraries.

Open notebook/main.ipynb in Jupyter / VS Code.

Run cells sequentially.

The first run will ask you to authenticate Earth Engine (follow the link).

All intermediate and final results are visualised inline.

To export results to Google Drive, uncomment and run the export_* cells.

Repository Structure

Flood_Mapping/
├── notebook/
│   └── main.ipynb          # full analysis notebook
├── environment.yaml        # conda environment specification
├── .gitignore              # excludes large/temporary files
└── README.md

License

This project is open‑source (MIT). Data sources retain their own licences:

GRID3, WorldPop, ESA WorldCover, JRC GSW, USGS SRTM, Copernicus DEM, Google Open Buildings.

Contributors:
Racheal Onocheta
Saheedat Ajike Akanbi
Idris Ibrahim
Ubong Usoro


Acknowledgements
Google Earth Engine team

GRID3 (grid3.org) for infrastructure data

WorldPop (worldpop.org)

ESA (WorldCover)

JRC (Global Surface Water)



