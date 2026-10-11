# AgriSat-TN 🌾

Satellite-based classification of **Paddy vs. Banana** and land parcels in **Ambasamudram and Cheranmahadevi taluks, Tirunelveli, Tamil Nadu** using Sentinel-2 and Google Earth Engine.

Built with a **FastAPI backend** that connects directly to Google Earth Engine and a modern **React (Vite) + Leaflet** frontend dashboard.

---

## Architecture Overview

```text
AgriSat-TN/
├── backend/
│   ├── main.py              # FastAPI app with EE endpoints (/api/tiles, /api/stats, /api/accuracy)
│   └── requirements.txt     # fastapi, uvicorn, earthengine-api, geopandas, etc.
├── frontend/                # Vite + React web application
│   ├── src/
│   │   ├── App.jsx          # Leaflet map, Recharts bar chart, parcel stats, accuracy metrics
│   │   └── index.css        # Dashboard styling
│   └── package.json
├── data/                    # GeoJSON polygons with 'class' property (0: paddy, 1: banana, 2: other, 3: water)
│   └── sample_training.geojson
├── gee_auth.py              # Earth Engine authentication logic
├── classify.py              # Sentinel-2 composite generation, NDVI, Random Forest classifier
└── README.md
```

---

## 1. Running the FastAPI Backend

Open a terminal or PowerShell:

```powershell
cd C:\Users\abira\.gemini\antigravity\scratch\AgriSat-TN\backend

# Activate the virtual environment
..\venv\Scripts\Activate.ps1

# Start the FastAPI server with reload
uvicorn main:app --reload --port 8000
```
> The API will start on **`http://localhost:8000`**.  
> Interactive OpenAPI documentation is available at **`http://localhost:8000/docs`**.

---

## 2. Running the React Frontend

Open a second terminal or PowerShell:

```powershell
cd C:\Users\abira\.gemini\antigravity\scratch\AgriSat-TN\frontend

# Start Vite dev server
npm run dev
```
> The frontend will launch at **`http://localhost:5173`**.

---

## Features

- **Interactive Satellite Map**: Centered on Ambasamudram (`8.70, 77.45`, zoom 11) using Leaflet.
- **Dynamic Tile Layer Fetching**: Direct Sentinel-2 true-color RGB and Random Forest classified tile streaming from Google Earth Engine.
- **Layer Toggles**: Switch between the Sentinel-2 RGB composite and classification overlay.
- **Classification Legend**:
  - 🟩 **Paddy (0)**: `#008000` (Green)
  - 🟨 **Banana (1)**: `#FFFF00` (Yellow)
  - ⬜ **Other (2)**: `#808080` (Gray)
  - 🟦 **Water (3)**: `#0000FF` (Blue)
- **Area Statistics**: Computed in hectares using `ee.Image.pixelArea()`, visualized via a **Recharts bar chart** and summary table.
- **Model Evaluation Card**: Real-time display of **Overall Accuracy**, **Kappa Coefficient**, and 4x4 Confusion Matrix.
- **Sidebar Filters**: Select year (`2025`, `2024`, `2023`) and adjust cloud cover threshold slider (`5%` - `40%`).
