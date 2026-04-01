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

## 📉 Statistical Modeling

Logistic Regression:
logit(P(major outage)) = β₀ + β₁ × Mean Heat Index

Key Findings:
- +1°F increase → ~43–52% higher odds of major outage
- Heat significantly increases outage risk

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
Redwan  
GIS Specialist | GeoAI | Spatial Analytics
