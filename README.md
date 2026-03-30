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

## Author

Redwan Kabir
