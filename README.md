# PWD Stormwater Billing Parcels - Data Quality Assessment

<img width="2560" height="1316" alt="image" src="https://github.com/user-attachments/assets/d8a4bcef-0e08-4478-a019-a89f7d38b9ad" />

## Overview
Geospatial QA Validation Pipeline that utilizes Philadelphia Stormwater Billing parcel data, and validates both tabular and geospatial data for structural integrity, data quality checks, schema validation, and topology validation. Also generates a visualization of topology related errors (overlaps, slivers, intersections), and generates data quality reports to help decide a recommended course of action if issues are severe enough.

---

## Table of Contents
- [Dataset Overview](#dataset-overview)
- [Main Deliverables](#main-deliverables)
- [Key Findings](#key-findings)
- [Installation Requirements](#installation-requirements)
- [Usage Instructions](#usage-instructions)
- [Function Index/Reference](#function-indexreference)
- [Acknowledgements](#acknowledgements)
- [Licensing](#licensing)

---

## Dataset Overview
- **Total Records:** 547,268 parcels
- **Covers:** Philadelphia, PA
- **Coordinate System:** EPSG:4326 (WGS84) reprojected to EPSG:32618 (UTM Zone 18N)
- **Geometry Type:** Polygons (parcel boundaries)
- **Columns:** 17 attribute fields + geometry

---

## Main Deliverables

### Scripts
- `geospatial_data_quality_audit.py`
- `tabular_data_quality_schema_audit.py`
- `topo_error_map.py`
- `topo_validation.py`

### Documentation
- `README.md`
- Executive Summary
- References
- License

### Visualization
- `topology_issues_map.html.zip`
- `topology_issues_map_top9500.html`
- `webmap_preview.png`

---

## Key Findings

### Topology
- **17,009 overlapping parcels** (3.1% of dataset)
- **1 intersection**

### Geometry Validity
- **6 "Nested shells" errors** – Cannot perform spatial operations on these parcels

### Positional Issues
- **6 parcels** outside Philadelphia bounds
- **812 area outliers** (0.15%)
- **6,235 spatial outliers** (1.14%) - parcels far from city center

### Schema Mismatches
- **6 columns** with type inconsistencies

### Strengths
- No null geometries (all 547,268 parcels have valid spatial data)
- No duplicate rows
- Primary key (objectid) is unique
- Low percentage of outliers (< 2% for most checks)

---

## Installation Requirements

### Python Libraries
- `geopandas` - Geospatial data manipulation
- `pandas` - Tabular data operations
- `folium` - Interactive web mapping
- `shapely` - Geometric operations (included with geopandas)
- `warnings` - Python built-in (no install needed)
- `matplotlib` - Plotting/visualization
- `pandera` - Data validation schemas
- `numpy` - Numerical operations
- `pandas_dq` - Data quality reporting

### Environment Setup
```python
# Install all dependencies
!pip install geopandas folium matplotlib 'pandera[pandas]' pandas_dq

# Mount Google Drive (for Colab users)
from google.colab import drive
drive.mount('/content/drive')

# Verify installations
import geopandas as gpd
import pandas as pd
import folium
import matplotlib.pyplot as plt
print("All dependencies installed")
```

---

## Usage Instructions

```bash
# Install dependencies
pip install -r requirements.txt

# Run tabular validation
python tabular_data_quality_schema_audit.py

# Run geospatial validation
python geospatial_data_quality_audit.py

# Run topology validation
python topo_validation.py

# Generate error map
python topo_error_map.py
```

---

## Function Index/Reference

| Function | Parameters | Returns | Purpose |
|----------|------------|---------|---------|
| `check_geometries(gdf)` | `gdf`: GeoDataFrame | DataFrame | Checks for null, empty, and invalid geometries |
| `check_polygon_topology(gdf, sample_size, sample_every)` | `gdf`: GeoDataFrame<br>`sample_interval`: int<br>`max_parcels`: int | DataFrame | Detects overlaps, slivers, and intersections between polygons |
| `check_polygon_topology_fast(gdf, sample_interval, max_parcels)` | `gdf`: GeoDataFrame<br>`sample_interval`: int (default=1)<br>`max_parcels`: int or None | DataFrame | Optimized topology check using spatial index for large datasets |
| `generate_qa_summary(gdf, geo_report, topo_report, pos_check)` | `gdf`: GeoDataFrame<br>`geometry_report`: DataFrame<br>`topology_report`: DataFrame<br>`positional_check`: dict | Tuple (summary_df, flags_df, status_str) | Generates QA summary with severity flags and recommendations |
| `visualize_issues(gdf, geometry_report, topology_report)` | `gdf`: GeoDataFrame<br>`geometry_report`: DataFrame<br>`topology_report`: DataFrame | folium.Map | Creates interactive map showing invalid geometries and topology issues |
| `visualize_topology_issues(gdf, topology_report)` | `gdf`: GeoDataFrame<br>`topology_report`: DataFrame | folium.Map or None | Creates detailed Folium map of all topology issues with color-coded types |

---

## Acknowledgements

### Data Source
- **Philadelphia Water Department** - Stormwater billing parcel data
- **Data.gov** - Open data catalog and distribution platform

---

## Licensing

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

[![CC BY 4.0][cc-by-shield]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
