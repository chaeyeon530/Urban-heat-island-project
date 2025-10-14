# Urban-heat-island-project
# 🌆 Urban Heat Island Analysis: Seoul vs Melbourne

> **Team Project | La Trobe Exchange 2025**  
> **Team Members:** 권하림 · 이채연(Hani) · 임연지 · 김현지  
> **Supervisor:** David (La Trobe University)

---

## 🧭 Project Overview

This project investigates **urban heat island (UHI)** effects by comparing **Seoul (South Korea)** and **Melbourne (Australia)** using geospatial and environmental datasets.

We analyzed how factors such as **building density**, **green space**, **road length**, and **air pollution** correlate with **average temperature changes (2016 → 2023)**.

**Main Tools:**  
PostgreSQL + PostGIS · QGIS · R (`ggplot2`, `gstat`, `raster`)  
**Data Source:**  
KMA (Korea Meteorological Administration), OpenStreetMap, Statistics Korea, Australian Bureau of Meteorology

---

## 📂 Data Pipeline

```mermaid
flowchart TD
    A[Raw Data: KMA & OSM] --> B[Data Cleaning & Integration]
    B --> C[Spatial Join in PostGIS]
    C --> D[Interpolation (IDW Method)]
    D --> E[Zonal Aggregation by District]
    E --> F[Correlation & Visualization in R]
    F --> G[Comparison: Seoul vs Melbourne]
