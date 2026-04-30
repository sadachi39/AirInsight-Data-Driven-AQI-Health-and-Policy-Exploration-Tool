# **AirInsight: Data-Driven AQI, Health, and Policy Exploration Tool**

## **Project Overview**

AirInsight is an interactive visualization platform designed to empower users to explore current, historical, and predicted Air Quality Index (AQI) data across United States counties. While existing research often identifies air pollution as a major cause of preventable disease, it frequently lacks actionable, spatially targeted solutions. This tool bridges that gap by allowing policymakers and the public to overlay AQI data with environmental and health factors, such as electric vehicle (EV) charging infrastructure and pollution-related health incidents. Most notably, the platform includes a simulation feature to predict how policy interventions—specifically the addition of EV stations—could impact regional air quality in the future.

## **Data Architecture & ETL**

The project pipeline utilizes a robust cloud-based architecture to handle a massive, multi-source dataset:

* **Storage & Processing:** All raw data was stored in Google Cloud Platform (GCP) buckets and processed using GCP BigQuery.

* **Data Volume:** The team integrated eight distinct data sources totaling over 2.1 GB, including 3.6 million daily temperature records and 125,000+ daily AQI readings.

* **Data Cleaning & Imputation:** To address sparse monitoring coverage, missing data was imputed using EPA ecoregion-based averaging and bilinear interpolation.

* **Normalization:** Data was aggregated into monthly averages for 928 counties to ensure reliability and minimize exposure misclassifications.

## **Analytical Methodology**

### **1\. Time-Series Forecasting (Holt-Winters)**

The platform employs an additive Holt-Winters exponential smoothing model to capture level, trend, and seasonality components within the AQI data.

* **Accuracy:** The model achieved high predictive reliability with an average Root Mean Squared Error (RMSE) of 6.92 and a Mean Absolute Error (MAE) of 5.48.

* **Reliability:** Forecasts typically remain within the same EPA-defined air quality category as actual observations, providing stable insights for planning purposes.

### **2\. Policy Intervention Modeling**

To isolate the specific impact of EV infrastructure on air quality, the team implemented a Two-Way Fixed-Effects (FE) regression model.

* **The Model:**  
  $$AQI\_{it} \= \\beta\_1 EV\_{it} \+ \\gamma Temp\_{it} \+ \\alpha\_i \+ \\tau\_t \+ \\epsilon\_{it}$$  
  .

* **Key Findings:** The analysis revealed a statistically significant negative correlation between EV station density and AQI levels.

* **Impact:** The model estimates that adding one station per 100,000 residents correlates with a 0.021 to 0.049 reduction in AQI, depending on the controls used.

## **Visualization & User Interface**

Developed in Microsoft Power BI and integrated with BigQuery, the dashboard provides a three-tab interactive experience:

* **Health Indicators:** Spatially maps pollution-related health incidents alongside AQI.

* **AQI Forecasts:** Displays 12-month future projections with 95% confidence intervals via dynamic tooltips.

* **Policy Simulator:** Allows users to simulate 5%, 10%, and 25% annual increases in EV infrastructure to view projected AQI trajectories through 2029\.

