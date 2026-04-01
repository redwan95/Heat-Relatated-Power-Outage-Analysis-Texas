# Texas Heatwave – Power Outage Relationship Analysis (2014–2021)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Research Question](#research-question)
- [Data Sources](#data-sources)
- [Methodology](#methodology)
  - [Pipeline Architecture](#pipeline-architecture)
  - [Heat Index Computation](#heat-index-computation)
  - [Adaptive Heatwave Threshold Selection](#adaptive-heatwave-threshold-selection)
  - [Heatwave Event Construction](#heatwave-event-construction)
  - [Logistic Regression Robustness Checks](#logistic-regression-robustness-checks)
- [Outputs](#outputs)
  - [Tabular Outputs](#tabular-outputs)
  - [Visualizations](#visualizations)
- [Key Results](#key-results)
  - [Heatwave–Outage Event Statistics](#heatwaveoutage-event-statistics)
  - [Logistic Regression Summary](#logistic-regression-summary)
- [Installation and Usage](#installation-and-usage)
  - [Prerequisites](#prerequisites)
  - [Install Dependencies](#install-dependencies)
  - [Data Preparation](#data-preparation)
  - [Run the Analysis](#run-the-analysis)
- [Configuration Parameters](#configuration-parameters)
- [File Structure](#file-structure)
- [Technical Details](#technical-details)
  - [Software Versions Tested](#software-versions-tested)
  - [API Rate Limits](#api-rate-limits)
  - [Computational Performance](#computational-performance)
- [License](#license)

---

## Project Overview

This project quantifies the statistical relationship between **extreme heat events
(heatwaves)** and **electric power outages** across all **254 Texas counties** from
**2014 through 2021**. The analysis pipeline ingests county-level power outage
records from the U.S. Department of Energy's **EAGLE-I** system and hourly
meteorological observations from the **Open-Meteo Archive API**, constructs
heatwave events using an adaptive percentile-based threshold, and evaluates the
heat–outage link through event-level metrics, choropleth mapping, and logistic
regression modeling.

The entire workflow runs in a **single Python script** (or Colab notebook) and
produces publication-ready tables, maps, distribution plots, and statistical
summaries.

---

## Research Question

> **Does increasing heat intensity (measured by heat index) during heatwave events
> significantly elevate the probability and severity of power outages across Texas
> counties?**

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

### Pipeline Architecture

# Texas Heatwave – Power Outage Relationship Analysis (2014–2021)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Research Question](#research-question)
- [Data Sources](#data-sources)
- [Methodology](#methodology)
  - [Pipeline Architecture](#pipeline-architecture)
  - [Heat Index Computation](#heat-index-computation)
  - [Adaptive Heatwave Threshold Selection](#adaptive-heatwave-threshold-selection)
  - [Heatwave Event Construction](#heatwave-event-construction)
  - [Logistic Regression Robustness Checks](#logistic-regression-robustness-checks)
- [Outputs](#outputs)
  - [Tabular Outputs](#tabular-outputs)
  - [Visualizations](#visualizations)
- [Key Results](#key-results)
  - [Heatwave–Outage Event Statistics](#heatwaveoutage-event-statistics)
  - [Logistic Regression Summary](#logistic-regression-summary)
- [Installation and Usage](#installation-and-usage)
  - [Prerequisites](#prerequisites)
  - [Install Dependencies](#install-dependencies)
  - [Data Preparation](#data-preparation)
  - [Run the Analysis](#run-the-analysis)
- [Configuration Parameters](#configuration-parameters)
- [File Structure](#file-structure)
- [Limitations and Caveats](#limitations-and-caveats)
- [Technical Details](#technical-details)
  - [Software Versions Tested](#software-versions-tested)
  - [API Rate Limits](#api-rate-limits)
  - [Computational Performance](#computational-performance)
- [Citation](#citation)
- [License](#license)

---

## Project Overview

This project quantifies the statistical relationship between **extreme heat events
(heatwaves)** and **electric power outages** across all **254 Texas counties** from
**2014 through 2021**. The analysis pipeline ingests county-level power outage
records from the U.S. Department of Energy's **EAGLE-I** system and hourly
meteorological observations from the **Open-Meteo Archive API**, constructs
heatwave events using an adaptive percentile-based threshold, and evaluates the
heat–outage link through event-level metrics, choropleth mapping, and logistic
regression modeling.

The entire workflow runs in a **single Python script** (or Colab notebook) and
produces publication-ready tables, maps, distribution plots, and statistical
summaries.

---

## Research Question

> **Does increasing heat intensity (measured by heat index) during heatwave events
> significantly elevate the probability and severity of power outages across Texas
> counties?**

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


### Pipeline Architecture

# Final Analysis Report  
## Heat Intensity and Power Outage Relationship in Texas (2014–2021)

## Abstract

This project examines the relationship between extreme heat and power outages in Texas from 2014 to 2021. It combines EAGLE-I outage records, hourly weather data, adaptive heatwave identification, county-level summaries, maps, and logistic regression checks to measure how outage activity changes with heat intensity.

## Introduction

Extreme heat can place stress on electric infrastructure through higher cooling demand, equipment strain, and broader grid instability. This analysis asks whether more intense heatwave events are associated with stronger outage impacts across Texas counties.

The notebook is designed as a reproducible pipeline. It loads outage data, pairs it with weather conditions, defines heatwave events adaptively, and then evaluates both event-level and county-level outage patterns.

## Data

### Outage Data

The analysis uses annual EAGLE-I outage files for Texas from 2014 through 2021. These files include county, state, timestamp, and outage customer counts.

### Weather Data

Hourly weather data is retrieved from the Open-Meteo archive and used to compute:
- temperature in Fahrenheit,
- relative humidity,
- dew point,
- and heat index.

### Geographic Data

County boundary data is used to create county maps that visualize heatwave-outage burden.

## Methodology

### Outage Processing

Outage records are filtered to Texas and aggregated to hourly county-level outage counts. This reduces the raw file volume and aligns outage data with hourly weather observations.

### Weather Processing

Hourly weather is pulled for the full study period and matched to all Texas counties. Heat index is calculated from temperature and humidity, and dew point is also estimated.

### Daily Aggregation

Hourly merged records are aggregated to daily county-level metrics, including:
- daily maximum temperature,
- daily mean temperature,
- daily maximum heat index,
- daily mean heat index,
- hours above heat index thresholds,
- daily outage totals,
- and maximum outage customers.

### Adaptive Heatwave Definition

Instead of using a fixed temperature threshold, the notebook evaluates warm-season heat index quantiles and selects a threshold that balances:
- enough heatwave days,
- enough outage overlap,
- and practical event identification.

This approach makes the heatwave definition data-driven and more suitable for the Texas climate context.

### Event Labeling

Heatwave days are grouped into contiguous events. The notebook first prioritizes multi-day events and then falls back to one-day events if needed to ensure valid heatwave-outage identification.

### Event Metrics

For each heatwave event, the analysis computes:
- start and end date,
- duration,
- mean and maximum heat index,
- maximum outage customers,
- total outage counts,
- and whether the event had outage activity.

### County Summary

County-level summaries track:
- number of heatwave events,
- number of heatwave events with outages,
- total peak customers during outage-linked events,
- and mean event duration.

### Robustness Checks

To test whether the heat-intensity relationship is stable, the notebook runs logistic regression models using several major-outage thresholds, including percentile-based and fixed-cutoff definitions.

## Results

The notebook identified thousands of heatwave events across Texas counties during the study period and a substantial subset with outage overlap. The adaptive threshold approach selected a high heat-index cutoff that still retained enough warm-season heatwave days and outage-linked heatwave days for analysis.

The regression results show a positive association between event heat intensity and the probability of a major outage outcome under multiple severity definitions. This suggests that hotter heatwave events are more likely to be associated with stronger outage impacts.

County-level outputs and maps also reveal spatial variation in outage burden, indicating that some counties experience larger or more persistent heat-related impacts than others.

## Figures and Tables

The notebook generates:
- distribution plots for outage severity and event duration,
- county-level maps for heatwave-outage metrics,
- and regression comparison visuals.

It also exports CSV and Excel tables for downstream use.

## Discussion

The analysis supports the idea that heat intensity matters for outage risk. The adaptive heatwave design is useful because it avoids relying on an arbitrary fixed threshold and instead tailors the event definition to observed conditions in Texas.

Several factors may influence the observed patterns:
- county infrastructure differences,
- local grid resilience,
- event duration,
- and potential spatial variability in weather exposure.

Because the notebook focuses on a single state and a specific set of inputs, the findings should be interpreted as a Texas-specific empirical result rather than a universal rule.

## Limitations

- Weather is represented using a central Texas archive location replicated across counties.
- Outage data availability depends on the annual EAGLE-I files.
- The analysis is observational, so it does not establish causality.
- County-level variation may reflect both climate and infrastructure differences.

## Conclusion

This project shows a meaningful relationship between heat intensity and power outage outcomes in Texas during 2014–2021. The combination of adaptive heatwave detection, event-based outage metrics, county mapping, and logistic robustness checks provides a strong reproducible framework for climate and grid resilience analysis.

## Requirements

- Python 3.10+
- pandas
- numpy
- matplotlib
- geopandas
- requests
- openpyxl
- statsmodels

## Files Generated

- `txheatwaveevents20142021.csv`
- `txheatwaveevents20142021.xlsx`
- `txheatwaveoutageevents20142021.csv`
- `txheatwaveoutageevents20142021.xlsx`
- `txheatwaveoutagecountysummary20142021.csv`
- `txheatwaveoutagecountysummary20142021.xlsx`
- Plot figures
- County map figures
- Logistic regression figures
tx-heatwave-outage-analysis/
│
├── README.md                                            # This documentation
├── tx_heatwave_outage_analysis.py                       # Main analysis script
├── LICENSE                                              # MIT License
│
├── eaglei_outages/                                      # Input data directory
│   ├── eaglei_outages_2014.csv                          #   (not tracked in git)
│   ├── eaglei_outages_2015.csv
│   ├── eaglei_outages_2016.csv
│   ├── eaglei_outages_2017.csv
│   ├── eaglei_outages_2018.csv
│   ├── eaglei_outages_2019.csv
│   ├── eaglei_outages_2020.csv
│   └── eaglei_outages_2021.csv
│
├── cb_2018_us_county_500k.zip                           # Auto-downloaded shapefile
│
│── ── Output Tables ──
├── tx_heatwave_events_2014_2021.csv                     # All heatwave events
├── tx_heatwave_events_2014_2021.xlsx
├── tx_heatwave_outage_events_2014_2021.csv              # Heatwave events with outages
├── tx_heatwave_outage_events_2014_2021.xlsx
├── tx_heatwave_outage_county_summary_2014_2021.csv      # County-level summary
├── tx_heatwave_outage_county_summary_2014_2021.xlsx
├── tx_major_outage_logit_threshold_summary.csv          # Logit comparison table
│
│── ── Output Figures ──
├── tx_heatwave_outage_event_distributions.jpg           # 3-panel histograms
├── tx_heatwave_outage_map_n_events.png                  # Map: event frequency
├── tx_heatwave_outage_map_customers.png                 # Map: customers affected
├── tx_heatwave_outage_map_duration.png                  # Map: mean duration
├── tx_major_outage_logit_P90_severity.png               # Logit plot: P90 threshold
├── tx_major_outage_logit_P95_severity.png               # Logit plot: P95 threshold
└── tx_major_outage_logit_Fixed_500_customers.png        # Logit plot: fixed 500

## Author

Redwan Kabir
