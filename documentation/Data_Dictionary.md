# Data Dictionary

## Overview

This document describes the variables included in the datasets used in the Delhi Air Quality, Agricultural Fires and Meteorological Analysis (2015–2025).

The project contains four principal processed datasets:

1. `research_daily.csv`
2. `regression_data.csv`
3. `dynamic_data.csv`
4. `nasa_weather_daily.csv`

The datasets were created by collecting, processing, and combining daily air quality, satellite-based vegetation-fire, and meteorological observations.

## Data Acquisition Through API

The meteorological dataset was retrieved directly from the NASA POWER API using Python rather than downloaded as a separate CSV file.

The API returned the requested daily meteorological observations in JSON format. The original response was stored separately, while the meteorological parameters were extracted and converted into a Pandas DataFrame named `nasa_data`.

The following variables were retrieved:

- T2M – Temperature at 2 metres
- RH2M – Relative humidity at 2 metres
- WS10M – Wind speed at 10 metres
- WD10M – Wind direction at 10 metres
- PRECTOTCORR – Corrected precipitation

The original API response is stored in:

`data/raw/nasa_power_weather.json`

The extracted meteorological dataset is stored in:

`data/processed/nasa_weather_daily.csv`

The meteorological observations were subsequently aligned with the AQI and vegetation-fire datasets using observation dates.

## 1. research_daily.csv

This dataset combines daily Delhi AQI observations, Punjab–Haryana vegetation-fire detections, and meteorological variables using their dates.

| Variable | Description | Unit |
|---|---|---|
| DATE | Date of observation | Date |
| Delhi_AQI | Daily Air Quality Index for Delhi | AQI points |
| PH_Fire_Detections | Daily VIIRS fire detections in Punjab and Haryana | Number of detections |
| T2M | Temperature at 2 metres | °C |
| RH2M | Relative humidity at 2 metres | % |
| WS10M | Wind speed at 10 metres | m/s |
| WD10M | Wind direction at 10 metres | Degrees |
| PRECTOTCORR | Corrected precipitation | mm/day |

This dataset contains 3,719 date-aligned observations.

## 2. regression_data.csv

This dataset contains the observations used for the main October–November regression analysis. It includes the original variables and additional features created for statistical modelling.

The main variables include:

| Variable | Description |
|---|---|
| Delhi_AQI | Daily Delhi Air Quality Index |
| PH_Fire_Detections | Daily Punjab–Haryana fire detections |
| PH_Fire_Lag4 | Fire detections from four calendar days earlier |
| Log_PH_Fire_Lag0 | Log-transformed fire detections at lag 0 |
| Log_PH_Fire_Lag2 | Log-transformed fire detections at lag 2 |
| Log_PH_Fire_Lag4 | Log-transformed fire detections at lag 4 |
| Log_PH_Fire_Lag6 | Log-transformed fire detections at lag 6 |
| T2M | Temperature at 2 metres |
| RH2M | Relative humidity at 2 metres |
| WS10M | Wind speed at 10 metres |
| WD10M | Wind direction at 10 metres |
| PRECTOTCORR | Corrected precipitation |
| Wind_Sin | Sine-transformed wind direction |
| Wind_Cos | Cosine-transformed wind direction |
| Year | Calendar year |
| Month | Calendar month |
| AQI_Lag1 | Previous calendar day's AQI, where available |

The logarithmic fire variables were created to represent fire activity on a transformed scale. Wind direction was represented using sine and cosine transformations because it is a circular variable.

The dataset contains 667 observations used in the October–November regression analysis.

## 3. dynamic_data.csv

This dataset contains the observations used in the final dynamic regression model.

It includes the following main variables:

| Variable | Description |
|---|---|
| Delhi_AQI | Current-day Delhi Air Quality Index |
| Log_PH_Fire_Lag4 | Log-transformed Punjab–Haryana fire detections from four calendar days earlier |
| AQI_Lag1 | Previous calendar day's Delhi AQI |
| T2M | Temperature at 2 metres |
| RH2M | Relative humidity at 2 metres |
| WS10M | Wind speed at 10 metres |
| Wind_Sin | Sine-transformed wind direction |
| Wind_Cos | Cosine-transformed wind direction |
| PRECTOTCORR | Corrected precipitation |
| Year | Calendar year |
| Month | Calendar month |

The dataset also contains additional derived variables used for model specification and diagnostic analysis.

The dynamic regression model incorporates previous-day AQI to account for persistence in daily air quality.

The final dynamic regression model used 663 observations and achieved an R-squared of approximately 0.7797.

## 4. nasa_weather_daily.csv

This dataset contains the daily meteorological observations retrieved from the NASA POWER API and converted into a Pandas DataFrame.

The dataset includes the following variables:

| Variable | Description | Unit |
|---|---|---|
| DATE | Date of meteorological observation | Date |
| T2M | Temperature at 2 metres | °C |
| RH2M | Relative humidity at 2 metres | % |
| WS10M | Wind speed at 10 metres | m/s |
| WD10M | Wind direction at 10 metres | Degrees |
| PRECTOTCORR | Corrected precipitation | mm/day |

The dataset covers the period from 26 September 2015 to 30 November 2025.

The original NASA POWER API response is preserved separately in `data/raw/nasa_power_weather.json`.

The processed meteorological dataset was used as a source for the combined daily analysis dataset.

## Important Notes

1. **AQI:** AQI represents the daily Air Quality Index and should not be interpreted as a direct measurement of the concentration of a single pollutant.

2. **Fire detections:** VIIRS satellite detections are not necessarily unique fires and do not independently confirm that each detection represents agricultural residue burning.

3. **Meteorological data:** NASA POWER provides gridded meteorological estimates rather than direct measurements from individual Delhi monitoring stations.

4. **Lag variables:** A lagged variable represents an earlier observation. For example, a four-day fire lag associates the current AQI observation with fire detections from four calendar days earlier.

5. **Date alignment:** The datasets were aligned by observation date before analysis. Some observations were excluded where required variables were unavailable. Lagged variables were constructed using calendar dates rather than simply shifting rows within the October–November subset.

6. **Study period:** The main analysis covers 2015–2025.

7. **Regression interpretation:** Regression coefficients describe conditional statistical associations within the specified model. They do not independently establish causal relationships.