# Methodology

## 1. Project Approach

This project uses a quantitative data-analysis approach to investigate the relationship between Delhi's Air Quality Index (AQI), vegetation-fire activity in Punjab and Haryana, meteorological conditions, and Diwali during 2015–2025.

Python was used for data collection, cleaning, processing, statistical analysis, regression modelling, and visualisation.

The analysis was conducted in several stages, beginning with data collection and preparation, followed by exploratory analysis, time-lagged analysis, regression modelling, and diagnostic testing.

## 2. Data Collection

Four main data sources were used.

### 2.1 Delhi Air Quality Data

Daily Delhi AQI observations and monitoring-station information were collected for the study period.

The data were processed to obtain daily AQI observations and examine changes in air quality over time.

Monitoring-station information was also examined to understand changes in the availability of AQI observations and station coverage throughout the study period.

### 2.2 NASA FIRMS Vegetation-Fire Data

Satellite-based fire detections were obtained from NASA's Fire Information for Resource Management System (FIRMS).

The analysis used Visible Infrared Imaging Radiometer Suite (VIIRS) Suomi National Polar-orbiting Partnership (SNPP) 375-metre fire detection data covering the available study period.

Fire detections were filtered using the vegetation-fire classification (type = 0) before geographical aggregation. The confidence classifications provided in the satellite dataset were also examined during descriptive analysis.

Geographical filtering was performed using state-level administrative boundaries to identify detections within Punjab and Haryana.

The retained fire detections were aggregated by date to produce daily Punjab–Haryana fire counts.

For the main regression analysis, vegetation-fire detections were examined during October and November, when fire activity in the region was particularly prominent.

It should be noted that satellite fire detections represent detected thermal anomalies and do not necessarily correspond to unique individual fires.

### 2.3 NASA POWER Meteorological Data

Daily meteorological data were retrieved directly from the NASA POWER API using Python.

The following parameters were collected:

Variable       Description                    Unit
---------------------------------------------------
T2M            Temperature at 2 metres         °C
RH2M           Relative humidity at 2 metres   %
WS10M          Wind speed at 10 metres         m/s
WD10M          Wind direction at 10 metres     Degrees
PRECTOTCORR    Corrected total precipitation   mm/day

The API request used the geographical coordinates of Delhi (latitude 28.6139 and longitude 77.2090) and covered the period from 26 September 2015 to 30 November 2025.

The original API response was stored in JSON format, and the extracted observations were converted into a Pandas DataFrame for further processing.

NASA POWER provides gridded meteorological estimates rather than direct measurements from individual Delhi monitoring stations.

The API request used UTC as its time standard. The retrieved dates were subsequently used for date-based alignment with the other datasets.

### 2.4 Geographical Boundaries

State-level administrative boundaries were used to identify geographical locations within Punjab, Haryana, and India.

Geographical filtering was performed through spatial joining to retain fire detections located within the relevant administrative boundaries.

The boundaries were used to distinguish detections within Punjab and Haryana from those occurring elsewhere in India.

## 3. Data Cleaning and Preparation

The collected datasets were processed separately before being combined.

The main preparation steps included:

    - Converting date fields into a consistent datetime format.
    - Checking for missing values and duplicate observations.
    - Filtering satellite fire detections geographically.
    - Retaining the selected vegetation-fire classification.
    - Aggregating fire detections by date.
    - Aligning AQI, fire, and meteorological observations using their dates.
    - Examining AQI monitoring-station availability.
    - Checking the availability and consistency of observations.
    - Creating analysis-ready datasets.

Four principal processed datasets were prepared:

    1. research_daily.csv
       Combined daily AQI, fire detections, and meteorological observations.

    2. regression_data.csv
       October-November observations prepared for regression analysis.

    3. dynamic_data.csv
       Final dataset used in the dynamic regression model.

    4. nasa_weather_daily.csv
       Separately processed daily meteorological observations retrieved
       from NASA POWER.

The October-November regression dataset initially contained 667
observations. The previous-calendar-day AQI variable was subsequently
constructed using the full daily date sequence. Observations without
the required previous-day AQI value were excluded from the final
dynamic regression dataset.

Consequently, the final dynamic regression dataset contained
663 observations.

Missing observations were examined during data preparation, and the
final regression sample was restricted to observations with the
variables required for model estimation.

## 4. Exploratory Data Analysis

Exploratory Data Analysis (EDA) was conducted to understand the distribution, seasonal behaviour, and temporal patterns of the collected variables.

The analysis included:

- Annual AQI trends.
- Monthly and seasonal AQI patterns.
- Annual and monthly vegetation-fire detection counts.
- Comparison of Punjab–Haryana fire activity with national fire detections.
- Meteorological trends and their relationship with AQI.
- Correlation analysis between fire activity and Delhi AQI.

Graphs and descriptive statistics were used to identify patterns and potential relationships before regression modelling.

## 5. Time-Lagged Analysis

Time-lagged analysis was performed to investigate whether earlier vegetation-fire activity was associated with subsequent changes in Delhi AQI.

Fire detection variables were constructed using different calendar-day lags, including 0, 2, 4, and 6 days.

A four-day lagged fire variable was selected for the primary regression model.

The fire variable was logarithmically transformed to represent fire activity on a transformed scale.

The analysis used calendar dates to construct the lag variables before restricting the regression sample to October and November. This avoided treating the last day of October and the first day of November as consecutive observations when intervening dates were missing from the seasonal sample.

## 6. Diwali Analysis

AQI variations around Diwali were examined using event-window analysis.

The analysis included:

    - Event Windows: Three-day, seven-day, and ten-day
      before-and-after comparisons.

    - Descriptive Analysis: Calculation of mean, median, and
      standard deviation of AQI changes.

    - Statistical Tests: Paired t-test and Wilcoxon signed-rank
      test to evaluate before-and-after differences.

    - Relative-Day Analysis: Examination of average AQI patterns
      across days surrounding Diwali.

    - Comparative Analysis: Assessment of AQI variations across
      different event-window lengths and study years.

The results were interpreted cautiously, as observed AQI changes
may also be influenced by meteorological conditions, other pollution
sources, and seasonal variations. No direct causal relationship
with Diwali-related activities was assumed.

## 7. Dynamic Regression Modelling

A multiple linear regression model was developed to examine the conditional association between Punjab–Haryana fire activity and Delhi AQI while accounting for previous-day AQI, meteorological variables, and calendar effects.

The primary model was specified as:

Delhi AQI = β₀

+ β₁ × (Log of Punjab-Haryana Fire Detections, 4 Days Earlier)

+ β₂ × (Previous Day's Delhi AQI)

+ β₃ × Temperature

+ β₄ × Relative Humidity

+ β₅ × Wind Speed

+ β₆ × Sine of Wind Direction

+ β₇ × Cosine of Wind Direction

+ β₈ × Precipitation

+ Year Effects

+ Month Effects

+ Error Term (ε)

Where:

    - **Delhi AQI:** Current-day Delhi Air Quality Index, used as the dependent variable.
    - **Log of Punjab–Haryana Fire Detections (4-Day Lag):** Log-transformed Punjab–Haryana fire detections recorded four calendar days earlier.
    - **Previous Day's Delhi AQI (1-Day Lag):** Delhi's AQI from the previous calendar day, included to account for persistence in air quality.
    - **Temperature:** Temperature measured at 2 metres above the surface.
    - **Relative Humidity:** Relative humidity measured at 2 metres above the surface.
    - **Wind Speed:** Wind speed measured at 10 metres above the surface.
    - **Wind Direction (Sine and Cosine):** Sine and cosine transformations of wind direction, used to represent its circular nature.
    - **Precipitation:** Corrected total precipitation.
    - **Year Effects:** Categorical controls accounting for differences across calendar years.
    - **Month Effects:** Categorical controls accounting for differences between October and November.
    - **Error Term (ε):** Represents the variation in Delhi's AQI that is not explained by the independent variables included in the regression model.

The final dynamic regression model contained 663 observations and estimated 20 parameters, including the intercept and categorical effects.

The logarithmic transformation was applied to the four-day-lagged fire variable to represent fire activity on a transformed scale. Previous-day AQI was included to account for persistence in daily air quality, while year and month effects were incorporated as categorical variables to control for calendar-related differences.

Model coefficients were evaluated using calendar-aware Heteroskedasticity and Autocorrelation Consistent (HAC) standard errors, with a maximum lag of seven calendar days. This approach accounted for heteroskedasticity and temporal dependence while respecting the actual calendar-day distances between observations, including the gaps between October and November observations across different years.

The model achieved an R-squared of approximately 0.7797, indicating that approximately 78% of the variation in Delhi AQI within the estimation sample was explained by the included variables.

## 8. Statistical Inference and Model Diagnostics

Several statistical procedures were used to evaluate the regression model.

### 8.1 Calendar-Aware HAC Standard Errors

Heteroskedasticity and Autocorrelation Consistent (HAC) standard errors were calculated using a calendar-aware approach.

The regression observations were restricted to October and November, creating gaps between seasonal periods.

Therefore, observations were matched using their actual calendar dates rather than treating consecutive rows as consecutive days.

A maximum lag of seven calendar days and Bartlett weights were used for the HAC covariance calculation. No small-sample correction was applied.

### 8.2 Multicollinearity

Variance Inflation Factor (VIF) was calculated to examine potential multicollinearity among the explanatory variables.

### 8.3 Heteroskedasticity

The Breusch–Pagan test was used to examine whether the regression residual variance was constant.

### 8.4 Residual Normality

The Jarque–Bera and Shapiro–Wilk tests were used to assess the normality of the regression residuals.

### 8.5 Functional-Form Specification

The Ramsey RESET test was used to investigate potential functional-form misspecification.

### 8.6 Sensitivity and Influence Analysis

Influential observations were examined using regression influence diagnostics.

A sensitivity analysis was also conducted after excluding the five most influential observations to evaluate whether the estimated fire–AQI association remained statistically significant.

Additional model specifications, including a quadratic fire-variable specification, were examined as supplementary analyses.

## 9. Interpretation of Results

The regression coefficients were interpreted as conditional statistical associations between the explanatory variables and Delhi AQI.

The analysis does not establish that vegetation fires directly cause changes in Delhi's air quality.

The results were interpreted alongside the limitations of satellite fire detections, meteorological estimates, AQI monitoring coverage, and the observational nature of the data.

## 10. Limitations of the Methodology

The analysis has several limitations:

    - Satellite fire detections do not necessarily represent unique fires.
    - VIIRS detections cannot independently confirm that every detected fire represents agricultural residue burning.
    - NASA POWER meteorological data are gridded estimates rather than direct observations from individual Delhi monitoring stations.
    - AQI monitoring-station coverage changed during the study period, potentially affecting comparisons across years.
    - Changes in satellite observation and data-processing characteristics may affect the comparability of fire detections over time.
    - Other pollution sources and environmental factors were not fully represented in the regression model.
    - The regression model may not capture all possible nonlinear relationships or interactions between explanatory variables.
    - Statistical associations should not be interpreted as direct causal relationships.

## 11. Software and Libraries

The analysis was conducted using Python in Jupyter Notebook.

The principal libraries used were:

    - Pandas
    - NumPy
    - Matplotlib
    - Seaborn
    - Statsmodels
    - Requests
