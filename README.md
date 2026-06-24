# Satellite India AQI & HCHO Hotspot Analytics (Sentinel-AQI)

Sentinel-AQI is a unified Geospatial AI and Remote Sensing software suite designed for the continuous prediction of surface Air Quality Index (AQI) and unsupervised identification of Formaldehyde (HCHO) hotspots over India boundaries using free satellite observations.

This project was built for a national-level college hackathon, incorporating a zero-cost API stack.

---

## 🚀 Key Features & Innovation
1.  **Continuous Surface AQI Coverage**: Maps columnar tropospheric densities from Sentinel-5P to human-breathing ground air levels using Planetary Boundary Layer Height (PBLH) integration. This bypasses sparse physical monitoring telemetry.
2.  **Volatile HCHO Cluster Isolation**: Integrates DBSCAN (Density-Based Spatial Clustering of Applications with Noise) to automatically extract contiguous organic plumes over India districts.
3.  **Biomass Causation Models**: Correlates crop stubble fires (captured via high-temperature radiance pixels in NASA EOS MODIS/VIIRS FIRMS) to Formaldehyde intermediate spikes to isolate organic combustion from urban traffic.
4.  **AI Forecast Alerts**: Compares Random Forest and non-linear XGBoost classifiers, mapping features to prospective 72-hour risk grids (Low, Medium, High).
5.  **Interactive Full-Stack GIS Dashboard**: Rich visualization built in Streamlit and React, displaying interactive tooltips, custom pollutant color scales, and real-time ML simulations.

---

## 📁 Directory Structure
```text
project/
│
├── data/                             # Raw CSVs, spatial GeoJSON boundaries, and local caches
├── notebooks/                        # Jupyter notebooks for satellite raster EDA and model validation
├── models/                           # Saved Joblib/XGBoost weights
├── scripts/
│   ├── gee_satellite_harvest.js      # GEE Code Editor JS-script for NO2, SO2, CO, O3, HCHO, & MODIS Fires
│   ├── aqi_processor.py              # Python module with India CPCB equations & vertical downscaling
│   └── ml_pipeline.py                # Regressors, DBSCAN clustering, and Risk alert training pipeline
│
├── dashboard/                        # Asset templates and Streamlit configuration
├── reports/
│   ├── ppt_outline_presentation.md   # Presentation Slide Deck content guidelines for judges
│   ├── demo_script_guide.md          # Chronological presenter script guiding the live dashboard demo
│   └── hackathon_guide.md            # Architecture diagram and free hosting deployment instructions
│
├── app.py                            # Production python Streamlit Dashboard script
├── server.ts                         # Production node/express client serving geospatial Gemini APIs
├── requirements.txt                  # Python environments setup manifest
├── package.json                      # Node environment and build triggers script
└── README.md                         # Main project documentation (This file)
```

---

## 📡 Free Data Sources & Ingestion
These free options are utilized in this workspace:
*   **Sentinel-5P (TROPOMI)**: Daily tropospheric columns of $NO_2$, $SO_2$, $CO$, $O_3$, and $HCHO$ available via Copernicus dataspace (Earth Engine Collections: `COPERNICUS/S5P/OFFL/L3_*`).
*   **Active Fire Indices**: NASA EOS MODIS and VIIRS FIRMS thermal anomalies (Earth Engine Collection ID: `FIRMS`, band `T21` representing high temperature vegetation clearance).
*   **Admin Boundary Maps**: FAO GAUL (Global Administrative Unit Layers) boundaries matching Indian state and district divisions.
*   **Ground Verification**: CPCB (Central Pollution Control Board) real-time station records accessed via open API portals for model evaluation.

---

## 🧮 Theoretical Background & Core Equations

### 1. Column-to-Surface Downscaling Proxy
Satellite sensors measure Vertical Column Density ($VCD$, in $mol / m^2$), representing the accumulation throughout the entire atmosphere. To map this is breathing air, we use the Planetary Boundary Layer Height Limit ($PBLH$, in $meters$):

$$\text{Surface Concentration } (\mu g/m^3) = \frac{VCD \times \text{Molar Mass } (g/mol) \times 10^6}{\text{Planetary Boundary Layer Height } (meters)}$$

Where:
*   Molar Mass is $30.03$ for $HCHO$ and $46.005$ for $NO_2$.
*   $PBLH$ varies seasonally from $\approx 500m$ in winter (boundary compression triggering severe smog) to $\approx 2500m$ in convective summers.

### 2. CPCB India Breakpoint AQI Formula
The India AQI category is defined by the maximum sub-index of primary pollutants using piecewise linear equations:

$$I_p = \frac{I_{hi} - I_{lo}}{B_{hi} - B_{lo}} (C_p - B_{lo}) + I_{lo}$$

Where:
*   $C_p$: Surface concentration of pollutant $p$.
*   $B_{hi}, B_{lo}$: CPCB official breakpoint bounds enclosing $C_p$.
*   $I_{hi}, I_{lo}$: Associated subindex bounds representing that breakpoint.

---

## 🛠️ Installation & Execution Guide

### Running the Python Streamlit Dashboard locally:
1. Ensure Python 3.9+ is configured. Then clone this directory:
   ```bash
   pip install -r requirements.txt
   ```
2. Launch the responsive portal:
   ```bash
   streamlit run app.py
   ```

### Running the Node React Full-Stack Interface:
This app includes a complete full-stack React interface with a built-in server and an integrated Gemini Geospatial Assistant.
1. Run dependencies installation:
   ```bash
   npm install
   ```
2. Start development mode showing the live server on [http://localhost:3000](http://localhost:3000):
   ```bash
   npm run dev
   ```

---

## 🏆 Presentation Slides Blueprint
Our folder `/reports/` contains complete outlines for your hackathon pitch:
*   `/reports/ppt_outline_presentation.md` yields a 7-minute, 7-slide structure focusing on the core science and DBSCAN parameters.
*   `/reports/demo_script_guide.md` provides custom speaker narratives for showing live sliders during the judge demo.
