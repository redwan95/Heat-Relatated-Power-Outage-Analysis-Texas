# 🌡️ Texas Heatwave–Power Outage Analysis (2014–2021)

## 📌 Overview
This project investigates the relationship between **extreme heat (heatwaves)** and **power outages** across Texas counties from **2014 to 2021**.

It integrates:
- **EAGLE-I outage data** (county-level, hourly)
- **Open-Meteo historical weather data** (temperature & humidity)
- Derived **Heat Index (HI)** as a human-impact metric

The workflow identifies **heatwave events**, links them to **power outages**, and evaluates **risk escalation using statistical modeling**.

---

## 🎯 Objectives

1. Detect heatwave events using an adaptive, data-driven threshold  
2. Quantify outage impacts during heatwaves (frequency, magnitude, duration)  
3. Map spatial vulnerability across Texas counties  
4. Model outage severity risk as a function of heat intensity  

---

## 📂 Data Sources

### 🔌 Power Outages
- EAGLE-I Dataset
- Hourly outage counts at county level

### 🌡️ Weather Data
- Open-Meteo Archive API
- Central Texas proxy (Lat: 31.0, Lon: -99.0)

---

## ⚙️ Methodology

### 1. Data Processing
- Filter Texas outages
- Aggregate to hourly peaks
- Fetch weather and compute Heat Index

### 2. Daily Aggregation
- Compute daily max/mean HI
- Outage metrics per day

### 3. Heatwave Detection
- Adaptive threshold selection using quantiles
- Final threshold: **HI ≥ 100°F**

### 4. Event Construction
- ≥2 consecutive heatwave days (fallback: 1 day)

### 5. Event Metrics
- Duration, max customers, total outages, HI stats

### 6. County Aggregation
- Event counts, outage exposure, duration

---

## 📊 Outputs

### Tables
- tx_heatwave_events_2014_2021.csv
- tx_heatwave_outage_events_2014_2021.csv
- tx_heatwave_outage_county_summary_2014_2021.csv

### Visualizations
- Distribution plots (JPEG)
- County maps (PNG)

---
## Data Sources

| Dataset | Provider | Temporal Coverage | Spatial Resolution | Variables Used |
|---------|----------|-------------------|--------------------|----------------|
| **EAGLE-I Outages** | U.S. DOE Environment for Analysis of Geo-Located Energy Information | 2014–2021 | County (FIPS) | `fips_code`, `county`, `state`, `sum` (customers without power), `run_start_time` |
| **Open-Meteo Archive** | Open-Meteo Historical Weather API | 2014-01-01 to 2021-12-31 | Single point (31.0°N, 99.0°W — central Texas), replicated to all counties | `temperature_2m` (°C), `relativehumidity_2m` (%) |
| **TIGER/Line Shapefiles** | U.S. Census Bureau (2018 vintage) | Static | County polygons (500k generalized) | Geometry, `STATEFP`, `COUNTYFP` |

### Data Volume Summary

| Metric | Value |
|--------|-------|
| Raw EAGLE-I records (Texas) | 6,726,628 |
| Hourly outage records (after aggregation) | 2,043,277 |
| Open-Meteo hourly weather observations | 70,128 (single site) |
| Weather records (replicated × 254 counties) | 17,812,512 |
| Daily records (all months, all counties) | 742,188 |
| Unique Texas counties in dataset | 254 |

---

🏗️ Pipeline Architecture


tx-heatwave-outage-analysis/
├── README.md
├── tx_heatwave_outage_analysis.py
├── LICENSE
│
├── eaglei_outages/                  # Input data (not tracked in git)
│   ├── eaglei_outages_2014.csv
│   ├── eaglei_outages_2015.csv
│   ├── eaglei_outages_2016.csv
│   ├── eaglei_outages_2017.csv
│   ├── eaglei_outages_2018.csv
│   ├── eaglei_outages_2019.csv
│   ├── eaglei_outages_2020.csv
│   └── eaglei_outages_2021.csv
│
├── data/
│   └── cb_2018_us_county_500k.zip   # Auto-downloaded shapefile
│
├── outputs/
│   ├── tables/
│   │   ├── tx_heatwave_events_2014_2021.csv
│   │   ├── tx_heatwave_events_2014_2021.xlsx
│   │   ├── tx_heatwave_outage_events_2014_2021.csv
│   │   ├── tx_heatwave_outage_events_2014_2021.xlsx
│   │   ├── tx_heatwave_outage_county_summary_2014_2021.csv
│   │   ├── tx_heatwave_outage_county_summary_2014_2021.xlsx
│   │   └── tx_major_outage_logit_threshold_summary.csv
│   │
│   └── figures/
│       ├── tx_heatwave_outage_event_distributions.jpg
│       ├── tx_heatwave_outage_map_n_events.png
│       ├── tx_heatwave_outage_map_customers.png
│       ├── tx_heatwave_outage_map_duration.png
│       ├── tx_major_outage_logit_P90_severity.png
│       ├── tx_major_outage_logit_P95_severity.png
│       └── tx_major_outage_logit_Fixed_500_customers.png


## Requirements

- Python 3.10+
- pandas
- numpy
- matplotlib
- geopandas
- requests
- openpyxl
- statsmodels

📂 Files Generated

All outputs are saved under the outputs/ directory and organized as follows:

📊 Tables
- tx_heatwave_events_2014_2021.csv — All detected heatwave events
- tx_heatwave_events_2014_2021.xlsx
- tx_heatwave_outage_events_2014_2021.csv — Heatwave events with associated outages
- tx_heatwave_outage_events_2014_2021.xlsx
- tx_heatwave_outage_county_summary_2014_2021.csv — County-level aggregated metrics
- tx_heatwave_outage_county_summary_2014_2021.xlsx
- tx_major_outage_logit_threshold_summary.csv — Logistic regression threshold comparison

📈 Figures
Distribution Plots
- tx_heatwave_outage_event_distributions.jpg — 3-panel histogram of event characteristics

Spatial Maps
- tx_heatwave_outage_map_n_events.png — Heatwave event frequency by county
- tx_heatwave_outage_map_customers.png — Total customers affected
- tx_heatwave_outage_map_duration.png — Mean outage duration

Statistical Modeling (Logistic Regression)
- tx_major_outage_logit_P90_severity.png — Model using 90th percentile threshold
- tx_major_outage_logit_P95_severity.png — Model using 95th percentile threshold
- tx_major_outage_logit_Fixed_500_customers.png — Model using fixed outage threshold

## 📉 Statistical Modeling

Logistic Regression:
logit(P(major outage)) = β₀ + β₁ × Mean Heat Index

---

## 🚀 How to Run

```bash
pip install pandas numpy matplotlib geopandas requests openpyxl statsmodels
```

```python
event_metrics_all, events_with_outage, daily, daily_hw, county_summary = main()
logit_threshold_summary = run_major_outage_logit_robustness(event_metrics_all)
```

---

## 📦 Project Structure

```
├── data/
├── outputs/
├── notebooks/
├── src/
└── README.md
```

---

## 🔮 Future Work
- Use spatial weather grids
- Add infrastructure data
- Extend to national scale

---

## 🧑‍💻 Author
S M Redwan Kabir
GIS Specialist | GeoAI | Spatial Analytics
