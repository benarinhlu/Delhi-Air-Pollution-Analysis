# Delhi Air Quality, Agricultural Fires and Meteorological Analysis (2015–2025)

## Project Overview

This project investigates the relationship between Delhi's Air Quality Index (AQI), vegetation-fire activity in Punjab and Haryana, meteorological conditions, and Diwali during 2015–2025.

Using Python, statistical analysis, and data visualisation, the project explores seasonal patterns, time-lagged relationships, and the association between agricultural fire activity and Delhi's air pollution.

The project combines daily AQI observations, satellite-based vegetation-fire detections, and meteorological data to examine environmental factors associated with changes in Delhi's air quality.

## Project Objectives

- Analyse annual and seasonal patterns in Delhi's AQI.
- Examine vegetation-fire activity in Punjab and Haryana.
- Investigate time-lagged relationships between fire detections and Delhi AQI.
- Evaluate the association between meteorological conditions and air quality.
- Examine AQI variations around Diwali.
- Develop and evaluate a multiple linear regression model.

## Tools and Technologies

- Python
- Jupyter Notebook
- Pandas and NumPy
- Matplotlib and Seaborn
- Statsmodels
- Requests (NASA POWER API data retrieval)

## Data Sources

The project uses data from the following sources:

- **Delhi AQI:** Daily Air Quality Index observations and monitoring-station information. The source archive is available through [UrbanEmissions AQI Bulletins](https://github.com/urbanemissionsinfo/AQI_bulletins).
- **NASA FIRMS:** VIIRS satellite-based vegetation-fire detections from the Suomi-NPP satellite. Data are available through the [NASA FIRMS Download Portal](https://firms.modaps.eosdis.nasa.gov/download/).
- **NASA POWER:** Daily meteorological data retrieved through the [NASA POWER Daily API](https://power.larc.nasa.gov/docs/services/api/temporal/daily/). Variables include temperature, relative humidity, wind speed, wind direction, and precipitation.
- **Geographical Boundaries:** State-level administrative boundaries used to identify and filter fire detections within Punjab, Haryana, and India.

The original NASA POWER API response is preserved in JSON format. The extracted meteorological observations are incorporated into the processed analysis dataset.

### Download the Raw Datasets

The original raw datasets are hosted separately on Google Drive because some files are too large to store directly in the GitHub repository.

**[Download the Raw Datasets from Google Drive](https://drive.google.com/drive/folders/1uPchM1jaAB1d_LwzOi87WaxkWMZyQrpQ?usp=sharing)**

The folder contains:

- Four Delhi AQI source CSV files.
- The NASA FIRMS VIIRS vegetation-fire archive CSV.
- The original NASA POWER weather API response in JSON format.

Download the files and place them in the project's `data/raw/` directory before running the notebook.

The processed datasets used in the analysis are available in the `data/processed/` directory of this repository.

## Key Findings

- Punjab and Haryana recorded **628,988 VIIRS vegetation-fire detections** during October and November across the study period.
- Fire activity showed substantial seasonal and annual variation, with particularly high detection counts during May, October, and November.
- A four-day lagged fire variable showed a statistically significant positive association with Delhi AQI in the primary dynamic regression model.
- The primary dynamic regression model used **663 observations** and achieved an R-squared of approximately **0.780**.
- The estimated coefficient for the four-day lagged log-transformed fire variable was **5.4047**, with a calendar-aware HAC standard error of **1.3170** and a p-value of approximately **0.000041**.
- Temperature, relative humidity, and wind speed also showed statistically significant conditional associations with Delhi AQI.
- Diwali-related AQI variations differed depending on the event window examined.
- Sensitivity analysis showed that the positive fire–AQI association remained statistically significant after excluding the five most influential observations.

These findings represent statistical associations within the analysed data and should not be interpreted as evidence of direct causation.

## Statistical Methods

The project applies the following statistical techniques:

- Descriptive statistical analysis
- Annual and seasonal trend analysis
- Time-series visualisation
- Pearson correlation analysis
- Time-lagged correlation analysis
- Multiple linear regression
- Dynamic regression incorporating previous-day AQI
- Calendar-aware Heteroskedasticity and Autocorrelation Consistent (HAC) standard errors
- Variance Inflation Factor (VIF)
- Breusch–Pagan test for heteroskedasticity
- Jarque–Bera and Shapiro–Wilk tests for residual normality
- Ramsey RESET test for functional-form specification
- Sensitivity analysis
- Influence and residual diagnostics

## Project Structure

```text
Delhi-Air-Pollution-Analysis/
│
├── AQI_Analysis.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── raw/
│   │   ├── AllIndiaBulletins_Master.csv
│   │   ├── AllIndiaBulletins_Master_2024.csv
│   │   ├── AllIndiaBulletinsMaster2025.csv
│   │   ├── AllIndiaBulletinsMaster2026_openrefined.csv
│   │   ├── VIIRS_fire_archive.csv
│   │   └── nasa_power_weather.json
│   │
│   └── processed/
│       ├── research_daily.csv
│       ├── regression_data.csv
│       └── dynamic_data.csv
│
├── figures/
│
├── results/
│
└── documentation/
    ├── Data_Dictionary.md
    └── Methodology.md
```

**Note:** The raw datasets are hosted on Google Drive and are not stored directly in this GitHub repository. The project structure above shows the expected local directory arrangement.

## Limitations

- Satellite fire detections do not necessarily represent unique fires or directly measure crop-residue burning.
- NASA POWER meteorological variables are gridded estimates rather than direct observations from individual Delhi monitoring stations.
- AQI is influenced by multiple emission sources and meteorological conditions.
- The number of Delhi AQI monitoring stations changed over the study period, which may affect comparisons across years.
- Satellite detection and processing consistency should be considered when interpreting long-term fire trends.
- The observational analysis identifies statistical associations and does not establish causation.
- Regression diagnostics indicate potential heteroskedasticity, non-normal residuals, and functional-form limitations.
- The analysis focuses on selected environmental variables and does not account for every possible source of Delhi's air pollution.

## Conclusion

This project examines the relationship between Delhi's air quality, Punjab–Haryana vegetation-fire activity, meteorological conditions, and Diwali over the period 2015–2025.

The dynamic regression analysis identifies a statistically significant positive association between four-day lagged vegetation-fire activity and Delhi AQI, even after accounting for previous-day AQI, meteorological variables, and calendar effects.

The findings demonstrate the value of combining environmental datasets, time-lagged analysis, and statistical modelling to investigate urban air pollution.

This project is an exploratory data-analysis and statistical-modelling portfolio project developed using Python.