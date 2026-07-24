# Solar Energy Yield Prediction & Forecasting

This project was developed to analyze, clean, and model the production data of a photovoltaic park, with the goal of estimating and forecasting the total energy yield using Deep Learning algorithms.

## Project Objectives

1. **Data Extraction (ETL):** Restoring an industrial database from a backup (`.bak`) in an isolated Docker environment (SQL Server) and querying the raw data.
2. **Data Cleaning (Pruning):** Removing anomalies, sensor errors, and downtime periods from the SCADA data.
3. **Machine Learning Modeling:** Training and comparing MLP, LSTM, and GRU architectures to map the relationship between solar irradiation and generated energy.
4. **Forecasting:** Simulating an optimal summer meteorological scenario to demonstrate the park's capacity to generate over 210,000 kWh/day.

## Technologies Used

* **Language:** Python 3
* **Data Manipulation:** Pandas, NumPy, Scikit-Learn
* **Deep Learning:** TensorFlow / Keras
* **Visualization:** Matplotlib
* **Databases:** Microsoft SQL Server, PyODBC, Docker
* **Versioning:** Git & GitHub

## Data Pipeline

The data was extracted from 4 main tables (`Train`, `LoggersTRENDS`, `MeteoTRENDS`, `ALARMHISTORY`) sampled at the second level, which were then aggregated into daily totals. To provide high-quality data to the AI models, a strict **Data Pruning** process was applied:

* **Weekend Filtering:** Removing non-working days.
* **Maintenance Filtering:** Removing days with production close to 0 (system offline).
* **Outlier Detection (IQR):** Eliminating hardware spikes (false-positive values incorrectly reported by sensors) using the Interquartile Range statistical method.
* **Extrapolation:** Scaling the measured production from 3 pairs of inverters to the total capacity of the park (28 pairs).

## AI Models and Forecasting

Three neural network architectures were developed to predict production:

* **MLP (Multi-Layer Perceptron):** Proved to be the most efficient for this dataset, successfully generalizing the direct mathematical relationship between solar irradiation and energy yield.
* **LSTM & GRU (Recurrent Networks):** Used to capture potential temporal dependencies of the production.

**Forecasting Scenario (2 Weeks):**

Since there was no weather API providing future data, the forecast for the next 10 working days was achieved through multivariate data simulation. A synthetic weather forecast (historical maximum solar irradiation + summer margin) was injected into the models. The simulation successfully demonstrated that, under optimal conditions, the park reaches and exceeds the estimated production target of **210,000 - 240,000 kWh / day**.